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
