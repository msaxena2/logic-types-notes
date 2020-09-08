---
title: Automated Deduction for Verification
subtitile: Notes on deductive verification by Natrajan Shankar

---


Synopsis
--------

This paper presents a holistic overview of automated deduction
and likely directions for future research.

The goal of verification is to establish/decide the validity of a statement.
If a proof exists in a sound proof sytem, then it's valid and if the proof
system is complete, then a proof can be found for any statment.
If a statement is not valid, then a counterexample can be found. In other words,
its negation is satisfiable.

The validity problem is decidable if a program can determine whether a given
statement is valid. Some logics are "semi-deciable", that is, the program
may not terminate on statements that are not valid.


Mathematical Logic
------------------

A logic consists of a formal language, formal semantics and a formal proof
system. The semantics defines the intended meaning of symbols and expressions
in the logic. The semantics fix the meaning of certain symbols (like logical
symbol) and allow flexible definition of others. Proof rules allow deriviation
of new valid statements from existing ones.

### Propositional Logic

A proposition is a declarative statement. An atomic proposition is one that is
not related to any other. Logical connectives /\, \/, ! allow combination of
propisitons. A formula is said to be true ($\top$) if there exists an assignment
under which the formula evaluates to true.


#### Normal Forms

Forms like NNF, CNF make formulas easier to handle.


#### Intuitionistic Propositional Logic

Classical logic admits LEM p \/ (!p) and double negation (!!p) => p.
Intuitionistic logic doesn't. In other words, absence of evidence for (!p)
doesn't count as evidence for p.

#### Proof Systems for Propositional Logic

##### Hilbert Style

Inference rules have zero or more premises and a conclusion.
Proof is a tree of application of proof rules. Example,
proof rule for modus ponenes written as:

 |- A |- A => B
 --------------
     |- B

##### Gentzen Style

Conditional judgements that assert derivability of
consequent formula from some assumption formulas.

###### Gentzen Sequent Calculus


A sequent looks like:

    A |- C
    ------

Conjunction of the formulas in A(ntecedent) implies
disjunction of the formulas in C(onsequent). If a sequent
is not valid, then there exists an evaluation s.t.
all of the formulas in A are true and all of the formulas
in C are false.

### Modal Logics

Formulas can express necessity, eventuality.
Models are kripke structures. A kripke structure consists
of a set of worlds and an accessibility relation on them.

[]p holds in world w if p holds in all worlds accessible
from w. (semantics of []p).

*K* axiom (distributivity) `[](p => q) => []p => []q`

*T* accessibility relation is reflexive, given by axiom `[]p => p`.

*S4* accessibility relation is reflexive and transitive
yields `[]p => [][]p`.

*S5* accessibility relation is equivalence, given by adding `p => []<>p`


#### Applications of Modal Logics

 - `CTL, CTL*, LTL` are all modal logics.

 - Logical curcuits modeled via modal logics.





