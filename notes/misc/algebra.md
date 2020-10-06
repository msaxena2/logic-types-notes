Notes on Simple Algebras
========================

An algebra can simply be thought of as mathematical structures.
The study of properties about these structures is the study of modern abstract
algebra.

Simple Definition of an Algebra
-------------------------------

Quite simple, an algebra is a pair $\mathcal{A} = (A, \__A)$, where $A$ the set
of elements of the algebra, and $_A$, also known as the interpretation, is a
function $\Sigma \to A$, or a way of interpreting symbols in a signature (of a
logical theory) into $A$. So, in some sense, an algebra for a given logical
theory, is simply a model that satisfies the axioms of the theory and provides
and interpretation for the symbols in the signature of said theory. The Logical theory
of Monoids, for instance, with axioms outlining associtiavity and identity,
would have multiple algebras that satisfy the theory. Any such algebra
$\mathcal{M} = (M, \__M)$ would also, of course, lead to $\mathcal{M} \vDash
T_{\text{Monoids}}$. An algebraic structure like a cons-list would be one
algebra that serves as a model for the theory of Monoids.


Term Algebra
------------

Simple algebra with elements that represent basically syntax trees of a Theory.
Consider an unsorted theory $\Sigma = ({{0}, {s_}, {_+_}), then the term algebra's
elements would (by definition) be -
   - 0, s(0), ....
   - s(0 + s(s(0 + ))) ....

Which are just syntax trees over the signature - they have no semantic meaning.

The term algebra always exists (known as initiality), and is isomorphic to any other term algebra for
the same signature. They also satisfy properties -
  - No junk. Nothing but the terms exist in the algebra's carrier set.
  - No confusion. For a sensible theory, there's only one way to interpret an
    inhabitant of the theory's signature into the algebra.


With these properties, a term algebra becomes useful. Given any other algebra,
say the Canonical algebra, there will only be a unique homomorphism between the
algebras. This is important for semantics of "evaluation" of terms in tools like
Maude. For a sort-decreasing, weakly confluent and weakly terminating theory,
the semantics are simply given by the "unique" homomorphism from the term
algebra to the canonical algebra. The canonical algebra so to say, is like the
term algebra, only has terms that cannot be reduced any further by equations in
the theory. This means the homomorphism acts as the "evaluation" function, and
renders semantics to maude's "reduce" command.

The main point here is that for a theory in a logic, the models are basically
algebras, where the structure of the inhabitants is dictated by the
axioms/lemmas/theorems.
