---
title: Semantics and Pragmatics of Real-Time Maude
author: Peter Olveczky and Jose Meseguer
numbersections: true
header-includes: |
  \include{commands}
---

Real Time Maude
===============

Real-time Maude is an extension to full maude with support for modeling timed systems.


### Equational Theory of Time
  The equational theory $\TIME$ is defined as an order commutative monoid
  $(\Time, 0, \leq)$ with a monus operator  $\dotdiv$ .

### Timed Rewrite Theory
Given an equational theory $\mathcal{R} = \left( \Sigma, E, \varphi, R \right)$, a
  real time rewrite theory $\mathcal{R}_{\phi, \tau}$ is a tuple
  $\left(\mathcal{R}, \phi, \tau \right)$, where
  \begin{itemize}
    \item $\phi: \TIME \to (\Sigma, E)$ is an equational theory
      morphism for the theory $\TIME$ to the underlying theory $\mathcal{R}$,
      i.e., $\phi$ interprets $\TIME$ in $\mathcal{R}$.
    \item $(\Sigma, E)$ defines operator $\{\_\}: \State \to \System$ that takes
      a system state to the global top-level state. The sort $\System$ has no
      subsorts.
    \item $\tau$ is an assignment $\tau_l$ of sort $\phi(\Time)$ to every rewrite rule
      of the form $ l: \{t\}  \rightarrow \{t'\}\ \text{if}\ \psi$. This is
      abbreviated as $l: \{t\} \xrightarrow{\tau_l} \{t'\}\ \text{if}\ \psi$.
      $\tau_l$ denotes the duration of rewrite, called a tick rule, and may
      contain variables outside $t$, $t'$.
  \end{itemize}

Specifying Systems in Real-Time Maude
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

Tick Rules
----------
Tick rules operate over terms of sort $\System$, and signifying the passage of
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
