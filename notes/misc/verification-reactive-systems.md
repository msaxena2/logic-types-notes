Reactive Systems Verification
=============================

Some notes based on Verfication of Reactive Systems.

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




