---
title: Software Model Checking
subtitle: Notes on Model Checking
author: Ranjit Jhala
header-includes: |
  \include{commands}
---


Preliminaries
-------------

A simple program is defined via a transition relation as $P = (X, L, l_0, t)$ where
$X$ is the set of variables, $L$ set of control locations with $l_0 \in L$
initial locations and $T$ the set of transitions. Given $(l, \rho, l') \in T$,
$l, l'$ are the source and end locations of the trasition and $\rho$ is a
constraint over free variables from $X \cup X'$, where variables from $X$
represent values from $l$ and variables from $X'$ represent values from $l'$.

The labels and transitions define a control flow graph (CFG).
For a given program, its CFG captures its control flow.
A program can be converted to its simple program representation as:

 - Assignment `x := e` becomes $x' = e \wedge \underset{y \in X / \{x\}}{\bigwedge}y =
   y'$

A state $s$ is an of valuation variables (X), represented as $s = v.X$.
Given $(s, s') \in v.X \times v.X'$, say $(s, s') \vDash \rho$ if the
valuations satisfy the constraint.

A *finite computation* is defined as $\langle l_0, s_0\rangle, \langle l_1, s_1
\rangle, \dots \langle l_k, s_k \rangle \in (L \times v.X)^{*}$ where
$l_0 \in L_0$ (initial location)
and for each $i \in \{0, \dots, k-1\}$  there exists a
transition $(l_i, \rho, l_{i+1}) \in T$ such that $(s_i, s_{i+1}) \vDash \rho$

Similarly an *infinite computation* is defined $\langle l_0, s_0\rangle, \langle l_1, s_1
\rangle, \dots  \in (L \times v.X)^{\omega}$


### Properties

Intuitively, safety properties state "bad things don't happen".
Liveness properties state "good things eventually happen".
Mathematically, a property $\Pi \subset (L \times v.X)^{\omega}$,
is basically defined as a set of traces and a traces $\sigma$
is said to satify $\Pi$ iff $\sigma \in \Pi$.

A safety property is one such that for every infinite computation
$\sigma \in \Pi$, and for every prefix $\sigma'$, there exists
a $\beta \in (L \times v.X)^{\omega}$ such $\sigma' \beta \in \Pi$.
Contrapositively, a trace $\sigma'$ is unsafe if there exists no trace $\beta$
that makes it safe (i.e. $\sigma . \beta \in \Pi$).
In other words, $\Pi$ is a safety property iff and only if it's prefix
closed.

The **verification problem** takes as input a program P and property $\Pi$
and return true if all traces of P are in $\Pi$ and false otherwise.
It can also be stated in terms of reachability: given a program P and
a location $\epsilon$, the verification problem return true iff
no exeuctions reach $\epsilon$.

### Stateful Search

Let $\finitePgm$ be the class of programs where each variable
ranges over a finite domain. The verification problem has
an algorith with the following steps:

1. Takes as input the property and the error state to avoid.
2. Maintain queues reach and worklist for reached states and
   states to work on.
2. Add all initial states to the queue of states to explore.
3. Dequeue a state $(l, s)$, and if it's not in reach, add it.
4. For each $(l, \rho, l') \in T$, add all states $s' \in \text{Post}(s, \rho)$
   to worklist.
5. Check if $\epsilon \in$ reach.

State space explosion is the main problem that must be handled.
There are two main ares of research:

1. Collapse states into equivalent classes and perform the search
   on one candidate from each class. Requires showing that
   any bug in the original system can also be intercepted in the
   reduced system. Involves techiniques like Partial order reduction,
   symmetric reduction, etc.

2. Compositional approaches involve proving safety on smaller sub
   programs to compose a proof for the main program. Basically
   corresponds to the Hoare logic composition rule, which say

   {A1} P1 {G1}     {A2} P2 {G2}        G1 -> A2
   ---------------------------------------------
            {A1} P1 ; P2 {G2}








