---
title: Modularity for Timed Automata and Hybrid Systems
author: Rajeev Alur and Thomas Henzinger
numbersections: true
header-includes: |
  \usepackage{mathabx}
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
