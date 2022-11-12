---
title: Modularity for Timed Automata and Hybrid Systems
author: Rajeev Alur and Thomas Henzinger
numbersections: true
header-includes: |
  \include{commands}
---

This paper addresses the problem of analysis of `open` systems,
or systems with a component of interaction with the environment.
A physical system may stop time *finitely* to perform actions,
but cannot do so *inifinitely*, i.e., it may not perform
*inifintely* many actions in a finite amount of time, or
exhibit *zenoness*.

**Machine Closure**:
The property that a system cannot exhibit inifite
transitions in a finite amount of time. For real time
*machine closure* translates to non-zenoness: every
finite trajectory can be extended to a divergent trajectory.
However, machine closure is not closed under parallel
composition. For example, consider system $P$ that emits an
event at time $0$, and waits for response. If response
arrives at time $\delta_1$, it emits an event an $\delta_1 +
\frac{1}{2}$, and waits. If it receives another response at
$\delta_2$, it responds at time $\delta_2 + \frac{1}{4}$,
and so on. System $Q$ waits for response. If it receives one at
time $\epsilon_1$, it responds at time $\epsilon_1 +
\frac{1}{2}$, and so on. While both $P$ and $Q$ are non
zeno, their parallel composition $P \parallel Q$ emits
events at $0, \frac{1}{2}, 1, \frac{5}{4}, \frac{3}{2},
\dots$, resulting in a system exhibit zeno behavior.
Thus for live open systems, the condition to consider is
*receptiveness*. A module is *receptive* iff it has a
strategy to generate an infinite live run in a two-player
game against the environment. Receptiveness is closed under
composition. First described in the context of I/O automata,
this paper addresses *receptiveness* in the context of
timed and hybrid automata. As timed automata are models for
closed systems, this works extends this model to *reactive
modules* to obtain *timed modules* for open reactive systems.
The two main contributions of note are:

  1. Extension of *assume-guarantee* principle for modular
     reasoning to open system reactive modules with clock
     variables.
  2. A fixed-point characterization of two-player game to
     the *receptivness game* to obtain a symbolic procedure for
     determining reactiveness of timed modules.

While the paper also extends results to hybrid automata
(beyond timed automata), discussion of hybrid systems
is omitted here.

Timed Modules
=============

Reactive Modules
----------------

A discrete-reactive system is described using a set of
events that change their values in a sequence of rounds.
Such systems, and their interactions, are modeled using
objects called *reactive modules*.

A reactive module $P$ has:

 - A finite set $X_p$ of variables. A state of the modules
   is a valuation of these variables. Events can be
   represented by toggling boolean variables.

 - The set $X_p = \ctr(X_p) \cup \extl(X_p)$ of *control* and
   *external* variables respectively. The set $\ctr(X_p)$ is
   updated by the module, while $\extl(X_p)$ is updated from
   the external world.

 - $\ctr(X_p) = \priv(X_p) \cup \intf(X_p)$ of *private*
   and *interface* variables respectively. The set
   $\obs(X_p)$ of *observable* variables is defined as
   $\intf(X_p) \cup \extl(X_p)$.

An *observation* of $P$ is a valuation of $\extl(X_p)$.


### Concurrency Models

A reactive module can model both asynchronous and
synchronous system. This document focuses on *pure* asynchronous sytems.

### Latched vs Updated Values

In each round, variables at the start of the round (before
udpate) and end (after update) are referred to as latched
and update respectively. The primed version of a variable is
used to depict updated values

### Initialization vs Update Actions.

The first initializes the set $X_p$, while subsequent rounds
updates it. Given two sets $X$ and $Y$ of variables,
an action $\alpha$ is a binary relation between valuations $X$
and $Y$. The action is *executable* if for every evaluation
$s$ of $X$, the number of valuations $t$ for $Y$ s.t. $(s,
t) \in \alpha$ is finite and non-zero.

An *atom* $A$ for set $X$ of variables consists of a
declaration and a body. The declaration partions $X$
into set $\ctr(X_A) \cup \textit{read}(X_A) \cup \awaited(X_A)$ of
*ctrl*, *read*, and *awaited*. The body consists of
an action $\textit{Init}_A$ from $\awaited(X_A')$ to $\ctr(X_A')$.
and an executable action $\textit{Update}_A$ from $\textit{read}(X_A) \cup
\awaited(X_A)$ to $\ctr(X_A')$.

The initial action assigns initial values to the control
variables as a non-deterministic function of the initial
values of awaited variables. The update action assigns
updated values to the controlled variable as a
non-deterministic function of the latched values of the
read varibles and udpate values of the awaited
variables. Thus, in a subround, the controlled variables
can be only updated after the awaited variables have been
updated. If for set $A$ of variables, $y \in \ctr(A)$ and
$x \in \awaited(A)$, then we say $y \succ_A x$.

A Reactive Module $P$ consists of a declaration and a body.
The declaration is a finite set $X_P$ of variables. The body
is a set $\mathcal{A}_P$ of $X_P$ atoms s.t.

 - $\bigcup_{A \in \mathcal{A}_P} \ctr(X_A) = \ctr(X_P)$.
 -  $\forall A, B \in \mathcal{A}_P . \ctr(X_A) \cap
    \ctr(X_B) = \emptyset$
 - Transitive closure $\succ_{P} = (\bigcup_{A_X
   \in \mathcal{A}_P} \succ_{A_X})^+$ is asymmetric.



