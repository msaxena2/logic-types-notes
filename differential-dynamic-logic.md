Differential Dynamic Logic
==========================

Differential Dynamic Logic (dL) deals with hybrid systems, or systems with
behavior dictated by both continuous and discrete dynamics.

Syntax
------

\phi ::= Real Arithmetic Predicate
       | [\alpha] \psi
       | <\alpha> \psi
       | \phi -> \psi
       | ^(and), ~(not), e.t.c.
       | \exists x \phi
       | \forall x \phi

Semantics
---------

Let v be the set of all variables. A state omega is
mapping : v -> R, from variables to reals. Given a formula
\phi, we say \omega \entails \phi if \phi is true in given
state \omega. \omega[[t]] gives the interpretation of a term
t in state omega.

The semantics of a hybrid program a are given as a transition
relation. [[a]] is the set of states reachable from \omega on
running program a.

Given a state v
 - (\omega, v) \in [[x := t]] if v(x) = t and \omega and v agree on all
   other varibles.
 - (\omega, v) \in [[x := * ]] if v and \omega agree on all variables except x
 - (\omega, v) \in [[?Q]] iff \omega = v and \omega \entails Q
 - (\omega, v) \in [[x' = t ^ Q ]] iff there is a solution \phi:[0,r] -> Q s.t.
   \phi(0) = \omega, \phi(r) = v and \phi \entails x'= t ^ Q. Here t is a term
   and Q is an evolution domain. Note, \phi is not a dL state.
   I think \phi \entails x'= t ^ Q is overoaded notation saying that for every
   point p in the evolution domain, \phi(p) (which is a dL state) \entails x' =
   t & Q .
 - [[A \cup B]] = [[A]] \cup [[B]].
 - [[A; B]] = {(\omega, v) : (\omega, u) \in [[A]], (u,v) \in [[B]].
 - [[A*]] = [[A]]*, transitive, reflexive closure of A.

Interpretation of dL formulas in state \omega
 - \omega \entails \forall x \phi iff for all states v that agree with \omega
   on every variable except x, v \entails \phi
 - \omega \entails [\alpha]\psi iff for every (omega, v) in [[\alpha]],
   v \entails \psi.
 - \omega \entails <\alpha>\psi iff there exists a (omega, v) in [[\alpha]],
   v \entails \psi.





