Reactive Systems Verification
=============================

Some notes based on Verfication of Reactive Systems from the following sources:

 - Temporal Verification or Reactive Systems (Safety) by Amir Pnuelli and Zohar Manna.
 - Verification of Reactive Systems by Klaus Schneider.

Generalogy of verification
--------------------------

### Modal Logic

Modal logic formulas $\varphi$ are defined inductively as:

$$
    \varphi ::= p \mid \neg p \mid p \vee p \mid \langle p \rangle
$$

Other formulas, such as $[\varphi] \equiv \neg \langle \neg \varphi \rangle$

Modal logic, unlike propositional logic, is interpreted in models called
*Kripke Structures*, defined as $\mathcal{K} = (S, I, \mathcal{R})$, where
$S$ is the set of worlds, $I$ the set of initial worlds, and $\mathcal{R}
\subseteq S \times S$ an accessibility relation s.t. $(s, s') \in \mathcal{R}$
signifies $s'$ is *accessible* from $s$.

### Dynamic Logic

Dynamic logic adds to model logic, *programs* or *actions*. Given a Kripke
structure $\mathcal{K} = (S, I, \Sigma, \mathcal{R}, \rho)$, where $S$ is the
set of words, $I$ the set of intial worlds, $\Sigma$ a set of actions,
$\mathcal{R} \subseteq S \times \Sigma \times S$,
a transition relation on $S$ s.t. $(s, a, s') \in \mathcal{R}$ means
$s'$ is accessible from $s$ on action $a$, $\rho: S \rightarrow 2^{P}$ an
interpretation function defining propositions that hold in a world $s \in S$,
where $P$ is the set of atomic propositions.

The set of programs is defined as:
$$\alpha ::= p \in P \mid \alpha_1 ; \alpha_2 \mid \alpha_1 \cup \alpha_2$$

Let $\overline{\rho} \subseteq S \times \alpha \times S$ be recursively defined
as:

 - $(s, a, s') \in \overline{\rho}$ iff $(s, a, s') \in \rho$
 - $(s, \alpha_1 ; \alpha_2, s') \in \overline{\rho} \iff \exists s'' \in S
   . (s, \alpha_1, s'') \in \overline{\rho} \wedge (s'', \alpha_2, s') \in \overline{\rho}$.
 - $(s, \alpha_1 \cup \alpha_2, s') \in \overline{\rho}$ iff
   $(s,\alpha_1, s') \in \overline{\rho} \vee (s, \alpha_2, s') \in \overline{\rho}$


The set of dynamic logic formulas are given by:
$$ \varphi ::= p \in P \mid \varphi \vee \varphi \mid \neg \varphi \mid \langle\alpha\rangle\varphi $$

Other formulas, such as $[\alpha]\varphi \equiv \neg\langle\alpha\rangle\neg\varphi$.

Given $s \in S$, we say:

- $s \vDash p$ iff $p \in \rho(s)$
- $s \vDash \neg \varphi$ iff $s \not\vDash \varphi$
- $s \vDash \langle\alpha\rangle\varphi \iff \exists (s, \alpha, s') \in
  \overline{\rho}$ s.t. $s' \vDash \varphi$.


Synchronous Programming in Esterel
==================================

Esterel is a state-chart formalism based language for synchronous programming.
Applications that maintain permanent interaction with their environment are
called Reactive Programs. These include real-time controllers, OS drivers,
communication protocol emitters and receivers, among others. Such programs
comprise three layers:

 1. An interface for *interfacing* with the external world.
 2. A reactive kernel containing the core programming logic.
 3. A data handling layer performing classical computation requested by the
    kernel.


### Deterministic Reactive Programs

A program is deterministic if it produces identical output sequence for
identical inputs. Sequential programs are obviously deterministic. But,
determinism doesn't mean sequentiality. Concurrent programs may be expressed
in deterministic subsystem that cooperate deterministically. Deterministic
concurrency is the key for modular development in Esterel.

Synchrony Hypothesis
--------------------

States that reactions (statements in the language) occur instantaneous, i.e.
take no time. This is a strong statement, and is understood to mean that the
environment itself stays invariant when a reaction executes. Statements take
time iff they explicitly say so. This allows one to write statements such as

```esterel
every 1000 MILISECOND do emit SECOND end
```

This means that a second is emitted every thousandth milisecond. This cannot
occur in a completely asynchronous setting. Moreover, this interpretation is
more natural for human comprehension - the user of a stop watch doesn't take
internal reaction time into account, as long as he's convinced that the watch
reacts instantly to his commands.

Esterel serves not only as a system specification language, but yields a
highly optimized machine-executable code, with performance similar to purposely
hand-written.

Esterel Semantics
-----------------

Statements are broadly classified into:

 - **Instantaneous**: standard imperative statements such as assignments,
 sequencing, conditionals, signal emission.
 - **Temporal**: trigger `await .. do` or watchdogs `do ... watching event`.


### Compilation to Automata

An interpreter based on the execution semantics would be effecient, but not
efficient enough for real-time applications. Thus, programs are compiled to
sequential automata that use Brzozowski's derivatives.

### Esterel Module Structure

A module is of the form:

```esterel
  mod MODNAME :
    declarations
    body
```

declarations define external objects used by the module: data
objects to be implemented in the data handling layer, and signals
and sensor that define the reactive interface. The are interdependent
as signals carry payloads with types declared in the data declarations.


#### Data declarations

Declarations the types, constant functions, and procedures that manipulate
data. Complex data manipulation is not handled with esterel, and delegated to
manipulation logic specified in another programming language.

#### Interface Declarations

Signals have instantaneous *ticks* that serve as interrupts for control-related
statements. Such signals may also have persistent value that can be accessed at
any time in an Esterel program. It is assumed that the value of a signal can
only change when a tick occurs, i.e. it remains invariant otherwise. For
sensors, the value is not broadcast, but is *accessible* at any time. An example
of such a sensor is a passive external device such as a thermometer.

An interface declaration may take the form:

```esterel
  input S (type);
```
for an input signal, or

```esterel
  emit S (exp);
```
to emit a signal with data `exp`. One may write

```esterel
  emit S(1) || emit S(2);
```

In such a case, a *commutative, associative* combination function must already
be declared, and the parallel combination operator `||` desugars to

```esterel
  emit S (comb (v1, comb(v2, ..., comb(vi-1, vi))));
```


#### Relation Declarations

Such declarations enforce either *incompatibility*, as in
$S_1 \neq S_2 \neq S_3$ signifies the signals are mutually exclusive or
$S_1 => S_2$ signifies $S_2$ will be present whenever $S_1$ is present.

### Esterel Instructions

Esterel *variables* and *signals* differ in the semantics only in
the manner in that only *signals* can be shared. Expressions and statements
manipulate aforementioned *variables* and *signals*, and are declared locally
as needed. The distinction between expressions ($<, <=, >=$) and statements
($x := exp, halt$) is as expected in any programming language.

#### Intuitive Semantics

A module *reacts* to an incoming input event by updating local variables and
emitting other events. This *reaction* is assumed to be instantaneous. Reactions
only occur on input events - the module is inactive between events.

The signals that constitute the event obey the sharing law, i.e.

 - A signal has a fixed value in a reaction: absent or present. It is present
   if the event is an input signal, or be emitted by a program as a local or
   output signal.
 - The value of the signal is unique in each reaction. If present, the value
   is the current input value, or is the combination of all values if the signal
   is local or emitted. If absent, the value is the same as the previous
   reaction. The value is invariant during a reaction. At first emission, the
   value is *indeterminate*.

The semantics of statements rely on four notions:

 1. The context of each statement in a program dictates the instant the
    statement starts executing.
 2. The internal execution of the statement determines when the statement
    terminates, if it ever does.
 3. If the instant of termination is the same as the one when it starts, the
    statement is said to instantaneous. Most statements are instantaneous.

The actual semantics can be described as:

 * The body start execution on reception of the first event.
 * `nothing` exits immediately.
 * `halt` performs no action, and never terminates.
 * Assignment and procedure calls immediately update memory and exit.
 Long running procedures must not be modeled as assignments. Instead, they must
 be started using events, and the result waited on using signals and values.
 * `emit` first evaluates the value, emits the signal with said value, and
   terminates.
 * The body of a loop starts when the loop starts, and on termination, is
   instantly restarted. To exit a loop, a `trap` must be used,
   defined as:
   ```esterel
   trap T in Statment end
   // where Statement may contain
   exit T
   ```
  * Watchdog statement `do Statement watching Signal` gives a time limit
    to the statement. The limit is the next reaction `Signal` is present. If the
    body terminates before `Signal` occurs, the watchdog statement also
    termintates. Otherwise it terminates as soon as `Signal` occurs.
  * a `Trap T` determines the exit point for its body. If the body terminates,
    or exits a trap, or an outer trap, the entire trap terminates.


Fair Transition Systems
=======================

Assume $\mathcal{V}$ a set of variables, and a language over
variables comprising expressions, statements, formulas and assertions.
Define a state $s$ a mapping from $u \in V$ to a value $s[u]$ in u's domain.
Let $\Sigma$ be set of all such states.

Define *fair transition system* $\langle V, \Theta, \mathcal{T}, \mathcal{J},
       \mathcal{C} \rangle$, where:

  * $V \subseteq \mathcal{V}$ a finite set of data and control variables.
  * $\Theta$ the initial condition satisfied by initial states.
  * $\mathcal{T}$ a set of transitions $\tau:\Sigma \to 2^{\Sigma}$
    is a transition function over states. For $s \in \Sigma$, $\tau(s) \subseteq
    \Sigma$. Say $\tau$ is enabled on state $s$ if $\tau(s) \neq \emptyset$.
  * $\mathcal{J} \subseteq \mathcal{T}$ a set of *weakly just* transitions.
    The justice requirement states that for $\tau \in \mathcal{J}$, computations
    where $\tau$ is continually enabled but not taken are not admissible.
  * $\mathcal{C} \subseteq \mathcal{T}$ a set of *compassionate* or
  *strongly-fair* transitions. For $\tau \in \mathcal{C}$, computations
  where $\tau$ is enabled infinitely many times but taken only finitely many
  times are not admissible.

For each $\tau \in \mathcal{T}$, define $\rho_{\tau}(V, V')$ to be a first
order formula s.t. for any $s \in \Sigma$ and $s' \in \tau(s)$,
$s' \in \tau(s)$ if $\rho_{\tau}(V, V')[s(V) / V, s'(V)/V']$ holds.


Computations
------------

Given system P with aforementioned components defined.
Let $\sigma = s_0, s_1, \dots,$, an infinite sequence of states. We say
$\tau \in \mathcal{T}$ is enabled at position $k$ of $\sigma$ if $\tau$ is
enabled at $s_k$, i.e., $\tau(s_k) \neq \emptyset$. $\tau$ is taken
at position $k$ if $s_{k+1} \in \tau(s_k)$. Multiple transitions may be taken
at the same point, i.e., for $\tau_1, \tau_2$ and $s_k, s_{k+1}$, both
$\rho_{\tau_1}[s_k[V]/V, s_{k+1}[V]/V']$ and $\rho_{\tau_2}[s_k[V]/V, s_{k+1}[V]/V']$
hold.

$\sigma$ is said to be a *P-computation* if the following are satisfied:

- *Initiality*: $s_0$ is initial.
- *Consecution*: For $j = 0, 1, \dots,$, $s_{j + 1} \in \tau(s_j)$ for $\tau \in
\mathcal{T}$.
- *Justice*:  For each $\tau \in \mathcal{J}$, it is not the case that $\tau$
  is continually enabled beyond some position $j$ in $\sigma$, i.e.,
  for every $k \geq j$, $\tau$ is enabled at $k$ but not taken.
  In other words, an enabled transition must be eventually taken.
- *Compassion*: For every $\tau \in \mathcal{C}$, it is not the case that
  $\tau$ is enabled at infinitely many positions, but taken at finitely many
  positions. In other words, a transition that is enabled infinitely often is
  taken infintely often.

*Justice* and *compassion* are referred to as *fairness requirements*.
Computation $\sigma$ that satifies *initiality* and *concecution* but not
*fairness* is called a *run* of P. While every *computation* is a *run*,
vice-versa may not hold. Every *finite* prefix of P is also a prefix of a computation.
This holds since it's not possible to know in a finite prefix whether the
complete run is fair or not. Every state in a run of P is P-accessible.

### Variants

Let $U \subseteq \mathcal{V}$ be a set of variables. State $\hat{s}$ is a
*U-variant* of state $s$ if $\hat{s}$ and $s$ agree on all variables
$\mathcal{V} \setminus U$, i.e. $\hat{s}[x] = s[x]$ for all $x \in (\mathcal{V}
\setminus U)$. Sequence  $\hat{\sigma} = \hat{s_1}, \hat{s_2}, \dots$ is a
*U-variant* of $\sigma$ if $\hat{s_i}$ is a *U-variant* of $s_i$ for $i \in 0,
1, \dots$.

Recall $V \subseteq \mathcal{V}$ are system variables, i.e., variables
manipulated by the program text, or holding control information such as program
locations. Suppose $U \cap V = \emptyset$. Since the transition relation
$\rho_{\tau}(V, V')$ is only determined by the set $V$, we get:

 * If $s_{i + 1}$ is a $\tau$-successor of $s_i$, then every *U-variant*
 $\hat{s}_{i+1}$ is a $\tau$-successor of every *U-variant* $\hat{s}_i$ of $s_i$.
 * If $\sigma = s_1, s_2, \dots,$ is a computation, then every *U-variant* $\hat{\sigma} =
 \hat{s}_1, \hat{s}_2, \dots$ is also a computation.
 * If $s_i$ is P-accessible, then so is $\hat{s}_i$.


Simple Programming Language (SPL)
---------------------------------

Consists of assignments, `await c` that awaits until condition is true,
$\alpha \Leftarrow e$ for sending $e$ on channel $\alpha$, $\alpha \Rightarrow
u$, for receiving into variable $u$ on channel $\alpha$, `request r` which
awaits until $r \geq 0$, and `release r` which increases r by 1. Together
the permit implementation of semaphores.

Apart from these, SPL has schematic statements such as `noncritical` to denote
non-critical activity in mutual exclusion, and `critical` to denote mutual
exclusion. `produce x (consume x)` represents production (consumption) of activity in producer-consumer
programs. Schematic statements don't modify program variables, except for the
variable used in `produce`.

Compond statements operate over sub-statements. These include
`if C then S1 else S2`, where `S1`, `S2` are sub-statements. Concatenation
`S1;...,Sk` results in sequential execution of sub-statements.
Selection `S1 or S2 or ... Sk` results in a *non-deterministic selection* of
one sub-statement from set of enabled sub-statements and its execution.
`while c do S` evaluates `c` and executes `S` if said evaluation is `T`. The loop
repeats when S terminates. If `c` is `F`, the statement terminates.


Cooperation statements are responsible for parallel execution. Given
$S_1, S_2, \dots, S_k$,
    $$ l: [l_1:\hat{S}_1;\hat{l}_1]\ || \dots ||\ [l_k:\hat{S}_k;\hat{l}_k]; \hat{l}: $$

First, label $l$ is executed, resulting in setting up $S_1,\dots,S_k$ for
execution, while moving from $l$ to $l_1,\dots,l_k$. At the end of all parallel
executions, a final *exit* step moves $\hat{l}_1, \dots, \hat{l}_k$ to
$\hat{l}$. Each label $\hat{l}_i$ can be viewed as an empty statement marking
the end of execution of statement $S_i$.

Grouped statements are compound statements that are executed atomically, i.e,
without interference from other parallel executions. Such statements are
specified as $\langle S \rangle$.


### Programs

A program consists of declarations, followed by a cooperation statement in which
processed may be named.


$$ P :: \left[\text{declaration}; \left[P_1 :: \left[l_1:S_1;\hat{l}_1\right]\right]\ || \dots ||\
        \left[P_k :: \left[l_k:S_k;\hat{l}_k\right]\right]\right] $$

$P_1,\dots,P_k$ are referred to as top-level processes. Processed within
$S_1,\dots,S_k$ are referred to as internal processes.

The *declaration* consists of a sequence of declaration statements of the form:
`mode variable, ..., variable: type` where $\varphi_i$.

Each declaration specifies the mode of the declarations, which can be `in,
local, or out`, a list identifiers, and the type of aforementioned
variables, and $\varphi_i$ the constraints on their initial values .
`in` variables cannot be modified, while `local` and `out` variables are
differentiated to help comprehensability, but are similar semantically.

Channel declarations may be of the form:

$$
\text{mode } \alpha_1, \alpha_2, \dots, \alpha_n: \text{ channel of type }
$$
$$
\text{mode } \alpha_1, \alpha_2, \dots, \alpha_n: \text{ channel[1] of type where } \varphi_i
$$

The former declaration corresponds to a *synchronous channel* where the sender
and receiver execute their statements on the same step.

The latter is an *asynchronous channel* declaration, where the message may be
buffered before the receiver processes it. An initial value for the buffer may
be specified, which is by default assumed to be empty.

### Labels

It is assumed that all statements in the program have labels, but only
relevant ones may be presented for brevity. Labels identify positions of control
in the program. Due to parallel execution, different labels may identify the
same control location.

To deal with redundancy of distinct labels identifying the same location,
an equivalence relation $\sim_L$ is defined as:

 * For $l:[l_1:S_1; l_2:S_2, \dots, l_k:S_k]$, $l \sim_L l_1$. k$.
 * For $l:[l_1:S_1\ \text{or}\  l_2:S_2\ \text{or}\  \dots\ \text{or}\ l_k:S_k]$, $l \sim_L l_1
 \sim_L l_2 \sim_L \dots \sim_L l_k$.
 * For block $l: [\text{declarations};\ l_1:S_1]$, $l \sim_L l_1$.
 * For cooperative statements $l:[[l_1:S1;\hat{l}_1]\ ||\ [l_2:S_2;\hat{l}_2]];\hat{l}$,
   $l$ is neither equivalent to $l_1$ or $l_2$.

A location is defined to be the equivalence class of labels $[l]$ s.t.
for $l_1,l_2 \in [l]$ $l_1 \sim_L l_2$.
