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

Other Categories
----------------

Consider category of *injective functions* -
  * Objects are sets. Morphism are injective functions.
  * To prove the above is a category, need to prove the following
    - Given objects A, B, and C, prove that the identity morphism
      is an injective function. Holds as the identity function is
      bijective.
    - Prove that for any f: A -> B & g: B -> C, where f, and
      g are injective, f;g = g o f is also injective.
      Can be proven by contradicition on the definition of injective
      functions. A function f:A -> B is injective iff for any x,y in A
      x != y -> f(x) != f(y). Assume h: A -> C = g o f is not injective.
      That is, there exist x,y in A such that x != y -> h(x) = h(y).
      This is a contradicition since g o f (x) != g o f (y) for any
      x, y in A, where x != y, since both g and f are injective.


More interstingly, consider category of *monoidal algebraic structures* -
  * An object M is defined as (M, :: : M, M -> M) . The relation
    `::` is associative and has an identity element `empty`, which is also in M.
    (To make it easier to understand, consider `::` to be the list cons
    operator, with identity being the `empty` list)
  * Morphism are function that maintain algebraic structure. If `f : A -> B` is a
    morphism, then f(l_1A ::_A l_2A) = f(l_1A) ::_B f(l_2A) = l_1B ::_B l_2B.
    `f(empty_A) = empty_B`. It can also easily be proven using definition of
    the functions that h = f;g = g o f also maintains monoidal structure.

Isomorphism
-----------

Given A,B and `f: A -> B` and `g: B -> A`, A and B
are said to be isomorphic if `f;g = id_a` and `g;f = id_b`.
The arrows are said to be isomorphic. For function,
for any x in A, y in B, g o f (x) = x and f o g (y) = y.

Diagrams
--------

DAGs where objects are nodes and arrows are edges. A Diagram
in a category need not be the entire category itself.
Formally, given a category `C`, a diagram is a mapping
from the graph's node to objects in C, and arrows to
morphisms in C. More formally given a diagram d,
d(i) = X, where X is an object in C, and d(i -> j) = X -> Y
where i -> j is an edge in the diagram, and X -> Y is a
morphism in C.

Note - at first thought, it may appear that a diagram
is just a representation of a Category as a DAG. This intuition
is misjudged. While a category may be represented in its
entirety using a diagram, it's more often that case that we
talk of a "diagram in a category". Thus, we're only representing
a part of the category in the diagram. This becomes important
when we talk of cones and limits.

Commutativity of Diagrams
-------------------------

Important concept in category theory. A diagram is said to
commute iff for any two nodes in the diagram, all paths
from A to B are equal.

For example,

```
    X
   / \
  A   B
   \ /
    Y
```

where arrows point from left to right is said
to commute iff A -> X; X -> B = A -> Y ; Y -> B = B.


Cones
-----

Given a category *C* and a diagram d(N,E) in said category,
a cone is a pair (C, {x_i}, i \in N), with the property -
  * x_i: C -> d(i) is a morphism in *C*
  * For an i -> j edge in d, x_i; d(i) -> d(j) = x_j.

A cone can be thought of as an operation on a diagram.
When the diagram is thought of as a planar graph, the cone
can be thought of as something outside the plane, with
morphisms to edges in the plane.


Limits
------

A limit of a diagram can be thought of as the maximal cone.
Formally, a limit is a cone (C, {y_i}) s.t. for any other
cone (C', {x_i}), i \in Nodes of d, there exists a unique
morphism *h* from C' -> C, s.t. h;y_i = x_i. Any two limits
in a diagram are isomorphic.






