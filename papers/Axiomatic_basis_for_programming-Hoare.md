---
title: Axiomatic Basis For Programming
subtitle: Notes on Hoare Logic
author: C. A. R. Hoare
---


Synopsis
--------

Foundational paper from 1969 that
talks about deductive reasoning.
Specifically, "All consequences of
execution it (the program) can be found
out from the text of the program itself
by means of deductive reasoning".

Notation
--------

Also know as a Hoare triple, writen `P {Q} R`, where
P is `pre-condition`, Q is `program` and R is `post-condition`

Is read as "If assertion P is true
before the execution of program Q, then
assertion R will be true on completion"


Axioms
------

### Assignment:

```
|- P0 { x = f; } P

```
P0 is obtained by substitution f for x in P.

Intuition above is - "Any expression P(x) that is true about the value
of x after the assignment is made must also be true of the value
of f (i.e. P(f) must be true) before the assignment to x is made".
Thus, in the axiom P0 is obtained by substituting f for x in P.

### Rules of Consequence

#### Strongest Post (strongest post implies post)
```
If |- P {Q} R and |- R -> S  then P {Q} S
```

#### Weakest Pre (pre implies weakest pre)

```
If |- P {Q} R and |- S -> P then S {Q} R
```
Can also be thought of as, "if P is known to be a
pre condition for program Q to produce result R, then
so is any other condition that implies P"

### Rule of Composition

```
If |- P {Q1} R1 and  |- R1 {Q2} R then  |- P {Q1 ; Q2} R
```

### Rule of iteration

```

If |- P /\ B {S} P  then |- P {while B do S} P /\ (!B)

```

