Notes on Category Theory
========================

Category theory borrows notation from set theory.
The source of an arrow is called a "domain", while the destination called
"co-domain". The intuition behind set theory as category theory is objects
are sets and an arrow f: A -> B represents a total function from set A to
set B. Function composition arises naturally from composition of arrows.
Moreorver, the identity arrow is the identity function `f(x) = x`.

Issues
------

Need to prove that the identity arrow is indeed the identity function.
Why can a function from A -> A not be an identity, given the domain and
co-domain of the function are the same set? The identity arrow
must have the following property:
```
  f : A -> B ; id_A = id_B ; f : A -> B
```
Where f can be any function from A -> B.
So, f(i_a(x)) = i_b(f(a)) for any x \in A. Emitting the proof,
it's the case that only when id_A and id_B are identity functions
an A & B, the condition above holds.

