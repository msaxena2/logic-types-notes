---
title: A Theory of Type Polymorphism for Programming
subtitle: Notes on Milner's paper from '77
author: Robin Milner
header-includes:
  \include{commands}
---


Synopsis
--------

Untyped languages (especially at the time of the paper's publication)
provide a lot of flexibility in realms such as structure processing.
However, a common pitfall of these languages were bugs due to
issues arising out of improper usage of "types". This paper
introduces the concept of polymorphic types that allow
for flexibility of untyped languages without the pitfalls.


Polymorphism
------------

There's "ad hoc" polymorphism that deals with overloaded functions
and "parametric" polymorphism, as described by Strachey.
The polymorphic type system from this paper was introduced
in this paper was used in ML (the meta language for LCF).

Fully determined types in the work are referred to as "monotypes"
while polymorphic types are also called "polytypes". Polytypes
are different as they admit type variables.


Example Language
----------------

Consider a simple language defined as:

```{.haskell}
 type Exp = x
          | (ee')
          | \x . E
          | f (\x . E) \\ least fixed point
          | let x = e in e'
          | if (B) then e else e'
```

#### Denotational Semantics

The domains are CPOs (complete partial orders with an upper bound).
Say $B_i$ is the set of basic domains. Say $V = B_0 + B_1 + \dots$ is
the set of all "values" (union of basic domains).
Let $F = V \to V$ be functions between domains.

Say $\eval \in \Exp \to \Env \to V$

