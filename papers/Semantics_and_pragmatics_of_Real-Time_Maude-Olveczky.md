---
title: Semantics and Pragmatics of Real-Time Maude
author: Peter Olveczky and Jose Meseguer
numbersections: true
header-includes: |
  \usepackage{mathabx}
---

Real Time Maude
===============

Real-time Maude is an extension to full maude with support for modeling timed systems.


### Equational Theory of Time
  The equational theory $\text{TIME}$ is defined as an order commutative monoid
  $(\textit{Time}, 0, \leq)$ with a monus operator  $\dotdiv$ .

### Timed Rewrite Theory
Given an equational theory $\mathcal{R} = \left( \Sigma, E, \varphi, R \right)$, a
  real time rewrite theory $\mathcal{R}_{\phi, \tau}$ is a tuple $\left(\mathcal{R}, \phi, \tau \right)$, where

 - $\phi: \text{TIME} \to (\Sigma, E)$ is an equational theory
    morphism for the theory $\text{TIME}$ to the underlying theory $\mathcal{R}$,
    i.e., $\phi$ interprets $\text{TIME}$ in $\mathcal{R}$.
 - $(\sigma, e)$ defines operator $\{\_\}: \textit{State} \to \textit{System}$ that takes
    a system state to the global top-level state. The sort $\textit{System}$ has no
    subsorts.
 - $\tau$ is an assignment $\tau_l$ of sort $\phi(\textit{Time})$ to every rewrite rule
    of the form $l: \{t\}  \rightarrow \{t'\}\ \text{if}\ \psi$. This is
    abbreviated as $l: \{t\} \xrightarrow{\tau_l} \{t'\}\ \text{if}\ \psi$.
    $\tau_l$ denotes the duration of rewrite, called a tick rule, and may
    contain variables outside $t$, $t'$.

Specifying systems in Real-Time Maude
--------------------------------------

Modules in Real-Time Maude are called timed modules and specified as
`tmod .. endtm` or `tomod .. endtom`.
Real-time Maude provides a functional module `TIME` with a sort `Time` as
follows:
```
fmod TIME is
  sorts Time NzTime . subsort NzTime < Time .
  op zero   : -> Time .
  op _plus_ : Time Time -> Time .
  --- definition for plus, monus, lte, gte
endfm
```

Any timed module implicitly includes said module, s.t. that morphism $\phi$
maps $\textit{TIME}$ to $\text{Time}$. maps 0 to `zero`, `_+_` to $\text{plus}$
, `_<_` to `_gt_`, etc.

Real-Time Maude doesn't assume a model of time. The model itself is left to the
user, but does provide a `NAT-TIME-DOMAIN` module that defines the domain to be
natural numbers. For dense time, another `POSRAT-TIME-DOMAIN` is provided that
models time as non-negative rationals.

### Tick Rules

Tick rules operate over terms of sort $\textit{System}$, and signifying the passage of
time when the rule application takes place.

Tick rules tend to take one of the form:

 - rule [l]: $t \rightarrow t'$ in time $x$ if $\psi$ /\\ $x$ le $u$ /\\ $\psi'$ [nonexec] .
 - rule [l]: $t \rightarrow t'$ in time $x$ if $\psi$ /\\ $x$ lt $u$ /\\ $\psi'$ [nonexec] .
 - rule [l]: $t \rightarrow t'$ in time $x$ if $\psi$ [nonexec] .
 - rule [l]: $t \rightarrow t'$ in time $x$ [nonexec] .

Where $x$ doesn't occur in $t$ or $t'$ and isn't instantiated in $\psi$.
$u$ should occur in $t$ or be instantied in $\psi$, denoting the maximum amount
of time in one tick step. Tick rules where the duration contain a variable that
does not occur on the left hand side and is not initialized by mathcing
equations are called *time-nondeterministic*.

Real-Time Maude assumes application of a tick rule in which time advances by 0
does not change the state. A rule is said to be *admissible* if:

 - Its zero-time tick rules doesn't change the state.
 - Is either *time-deterministic* or *time-nondeterministic* with any of the
   above forms.

The execution of *admissible* tick rules is supported in Real-Time Maude. However
*admissible time-nondeterministic* rules isn't supported directly by Maude,
since the duration variable can be instantiated in many ways. Thus these rules
are tagged with the attribute `[nonexec]`. Real-Time Maude provides sampling
strategies for the execution of these rules.

### Defining Initial States

Real-Time Maude allows operators of sort `GlobalState` to initialize state.
These operators must reduce to a term of form `{t}`.

Specifying Systems in Real-Time Maude
=====================================

Usually, such systems specify these operators:

 - `delta: Configuration TimeConfiguration` to
   model the passage of time on the system configuration.
 - `mte: ConfigurationTimeInf` for the *maximum
   time elapse* from a configuration.

The following functions distribute over the configuration:

```
  vars NeC, NeC': NeConfiguration .
  var R : Time .
  eq delta(none, R) = none .
  eq delta(NeC NeC, R) = delta(NeC, R) delta(NeC', R) .

  eq mte(none) = Inf .
  eq mte(NeC NeC') = min(mte(NeC) mte(NeC')) .
```

Followed by the tick rule:

```
  crl [tick]:
    { SYS:Configuration } => { delta(SYS:Configuration , R:Time) }
    in time R:Time if R:Time le mte( SYS:Configuration ) .
```


### Formal Analysis in Real-Time Maude

Real-Time Maude translated Timed Modules in untimed modules.
However, there exist situations not handled in untimed maude:

 - Control of duration of computation, instead of number of rewrites.
 - Direct execution of time-nondeterministic rules.
 - Search with time bounds - Can the system reach a state in some time?
 - Reducing infinite state to finite state model checking by bounding on time.

#### Time Sampling Strategies

For dense time, Real-Time Maude allows one to set strategies to dictate how
a *time-nondeterminisitic* rule application will advance time. For example `(
set tick def r ) . ` tries to advance time by `r` units. If the tick rule
has an upper bound u, then the strategy tries to advance by r, or u, whichever
is lower. Note that this highlights the limitation of Real-time Maude analysis.
A *time-nondeterminisitic* rule will still advance by a deterministic time
bound. This means that Real-Time Maude may miss interleavings from not using
time values from a true *nondeterministic* assignment. Thus, counter-examples
from Real-Time Maude are *true* counterexamples, but a satisfaction only means
that the property holds for all **states visited**.

#### Timed Rewrite

Real-Time Maude provides the following command

```
 (trew [n] in mod : t in time <= r .)
```
to perform a timed rewrite (for atmost n steps) with time bound r. Real-Time
Maude also provides a `tfrew` command for rule-fair and position-fair rewriting
with a certain time bound.

#### Timed Search

Real-Time Maude's timed search command is responsible for exploring all system
behavior in bounded time. The form of the command is:

```
 (tsearch t0 arrow pattern with no time limit .)
 (tsearch t0 arrow pattern in time ~r .)
 (tsearch t0 arrow pattern in time-interval between ~' r and ~'' r' .)

```
where:

 - `pattern` can either be a term `t` or a ground-irreducible term `t`
with a semantic condition involving the variables in `t`.
 - `~` can either be `<`,`<=`, `>`, `>=`.
 - `~'` can either be `>` or `>=` and `~''` can either be `<` or `<=`.
 - `r` and `r'` are ground terms of sort `Time`.
 - `arrow` can be one of `=>1`, `=>*`, `=>+` signifying `pattern` can be reach
   in $1$, $\ge 0$, or $> 1$ step(s) respectively.

Semantics of Real-Time Maude
============================

Real-Time Maude uses Maude to execute commands. It uses a *reductionist*
approach - theory preserving transformations change the real-time theory to a
semantically-equivalent Maude theory, and commands are translated into ordinary
Maude commands.

Clocked Transformations
-----------------------

The *ordinary theory* for a *clocked theory* basically adds the operator `_in_`
to specify time durations, with the equation:
```
  eq (C:ClockedSystem in R1:Time) in R2:Time
   = C:ClockedSystem in (R1:Time + R2:Time) .
```

Say $\mathcal{R_{\phi, \tau}}$ is the clocked system, and $\mathcal{R_{\phi,\tau}}^C$ is the ordinary
system. It can be shown that:

$\mathcal{R_{\phi,\tau}} \vdash t \xrightarrow{r} t'$ iff
$\mathcal{R_{\phi,\tau}}^C \vdash t\ \text{in}\ \text{time}\ r' \rightarrow t'\ \text{in}\ \text{time}\ r' +_{\phi} r$

For the time sampling strategies, the following rules replace the timed rules,
assuming the strategy to set tick as $r$.


 - $t \rightarrow t' \text{ in time } x \wedge \psi \wedge x \leq u \wedge \psi'$ becomes
   $t\rightarrow t' \text{ in time } x := \text{ if } u \leq r \text{ then } u \text { else } r \text{ fi } \wedge \psi \wedge x \leq u \wedge \psi'$.
 - $t \rightarrow t' \text{ in time } x \wedge \psi \wedge x < u \wedge \psi'$ becomes
   $t\rightarrow t' \text{ in time } x := r  \wedge \psi \wedge x < u \wedge \psi'$.
 - $t \rightarrow t' \text{ in time } x \wedge \psi$ becomes
   $t\rightarrow t' \text{ in time } x := r  \wedge \psi$.
 - $t \rightarrow t' \text{ in time } x$ becomes
   $t\rightarrow t' \text{ in time } x := r$.


Real-Time Maude Examples
========================

Simple Clock Example
--------------------

Consider a clock which 'ticks' when running, resets to zero after 24,
and stopped when its battery dies.

```
  rl [tickWhenRunning]
    {clock(R)} => {clock{R + R')} in time R' /\ if R' < 24 - R .
  rl [tickWhenStopped]
    {clock-stopped(R)} => {clock-stopped(R)} in time R' .
  rl [reset]
    clock(24) => clock(0) .
  rl [batteryDies]
    clock(R) => clock-stopped(R) .
```

Before running any analysis, one needs to set the sampling time, for example `(set tick def 1 .)`.
One can then run commmands such as `trew {clock(0)} in time <= 99 .` to get
stopped-clocked(24) to simulate one behavior, or `tsearch {clock(0)} =>*
{clock(R)} such that R > 24 in time 99 . ` to get `no-solutions`.


