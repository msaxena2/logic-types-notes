---
title: Abstract Interpretation
subtitle: Notes on Abstract Interpretation (POPL'77)
author: Patric Cousot
---


Preliminaries
-------------

The language used in the paper resembles flowcharts
and modeled by a '71 paper by Scott and Strachey.


Semantics expressed in terms of `nodes` and `arcs`
between them. The `arcs` denote the transition system.
A Node can be an Assignment, Test/Conditional, Junction,
Entry or Exit node. A Start node has no predecessor,
end has no succcessor, assignment has 1 predecessor and 1 successor,
Test has one predecessor and 2 successors, junction has
multiple predecessor and 1 successor.


A `state` is an entity in `Arcs X Env`. An arc is simply
a transition in the program represented above as a flowchart


The transition function `n-state` is defined as

```
n-state(s) =
  let n = cs(s) and e = env(s) in
    case n in
      Assignment => <a-succ(n), e[val [[expr(n)]](e) / id(n)]>
      Test => cond(val [[test(n)]](e), <a-succ-t(n), e>, <a-succ-f(n)), e>)
      Junction => <a-succ(n), e>
      Exit => s
```

A computation sequence of length n ($\geq 0$)
is defined as `n-succ(n-succ(....(n-succ(s))))`
The terminating state is defined as the lfp of `n-succ`.


Static Semantics of Programs
----------------------------

Operational semantics are concerned with sequence of states
(states described above). However it's often enough to reason
about sets of states associated with each program point
to prove certain properties about programs.

Contexts
--------

Context `Cq` for some arc `q` of program `P` is the set of all possible
environments associated with `q` in all possible computation
sequences of `P`. Formally:

$$ Cq = \{ e \mid (\exists n \geq 0, \exists i_s \in \text{I-states} \mid \langle
q, e \rangle \in \text{n-state}^n(i_s)\}$$


Intuitively, the context for arc $q$ is the set of all states
in all executions going through the arc. Recall a state
is a tuple in sort *Arc X Env*, and an execution is simply
a sequence of states.

A **Context Vector** is simply a mapping from Arcs to Contexts.

Semantics relate context of an `arc(r)` with an arc `q` adjacent
to `r` (`origin(r) = end(q)`) as `Cv(r) = n-context(r, Cv)` where
`n-context` is defined as:

```
  n-context(cv, r) =
    case origin(r) in
      Entries => { \bot$ }
      (Assignment | Test | Junction) =>
        // All next states of previous arc q
```



