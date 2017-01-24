## Proposition Logic

* A propositional formula can be thought of a curcuit, with wires as propositions, and gates making formulae.
* A countable set is the smallest infinite set. S is a countably inifinite set if its element can be enumerated as a0, a1, a2, . Set of reals is not countable. N is countable. I is countable, with ordering for instance like 0, -1, 1, -2, ..... Rationals are countable. 
* A set of finite strings over an alphabet is countable. Real numbers cannot be represented as a set of finite strings over numbers, hence it's not countable.



 Fix \P, a countable set of propsitions {p0, p1, ..}. Syntax of prop. logic 


 Let WFF be the smalles set such that, p in P is in WFF if \phi and \phi' are in WFF, then (\phi /\ \phi'), (\phi \/ \phi') and (\not phi) are also in WFF. 

#### Grammar for WFF - 
```
WFF ::= p | (p1 \/ p2) | \not p

where p, p1 and p2 are in WFF.
```

Let W be the set of all sets that satisfied the property above, then the union of the sets is also a WFF, and is the smallest set. 

#### Semantics of Propositional logic - 

* Model : World that determines basic facts.

* Valuation : A valuation v is a function from propositions to True/False.

* A formula is vaild if it's true in all models.

* A formula is satisfiable if it's true in some model.


SAT problem is decidable, hence to check validity, negate formula and hand it to SAT solver to check for validity.
