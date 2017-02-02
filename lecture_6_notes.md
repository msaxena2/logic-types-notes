## Lecture 6 Notes

#### Checking Satisfiability of a formula is satisfiable
* Truth table technique. For every evaluation, v: P(\phi) -> {T, F}, check phi is satisfiable. Checking satisfiablity is in NP, efficiency is not expected. Linear Space algorithm.

* \Phi is satifiable iff \phi(p |-> T) \/ \phi(p |-> F). Not quite same as the truth table. 

##### Algorithm

SAT(\phi) 


if \phi = T return T

else if \phi = F return F

else recurse over \phi[p |-> t]

Then, certain reductions can be performed over the formula like - p /\ T = p.




### CNF Conjunctive Normal Form

Formula = C1 /\ C2 /\ C3 ... /\ Cn

each Ci = d1 \/ d2 .... \/ dn


Any formula can be normalized to CNF.

