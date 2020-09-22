---
title: Rewriting Logic as a Framework for Concurrency
subtitle: Notes on concurrent system in rewriting logic
author: Jose Meseguer
header-includes: |
  \include{commands}
---


Synopsis
--------

The work presents rewriting logic as a basis for
expressing various models of concurrency. Note
that the motivation is *not* to come up with a
universal model that other models can be translated
to, but to express other models in one common model
as naturally (i.e., avoiding translation) as much
as possible.

Preliminaries
-------------

### Rewriting Logic

A signature in rewriting logic is given by the order
sorted equational theory $(\Sigma, E)$. The equations
partition terms into equivalence classes.

Sentences of rewriting logic are sequents of the form
$\equivClass{t}{E} \to \equivClass{t'}{E'}$, where $\equivClass{t}{E}$
is the set of $E$-equivalent terms.

A rewrite theory is given by $\mathcal{R} = (\Sigma, E, L, R)$
where $(\Sigma, E)$ is the signature, $L$ is a set of labels on
rules and $R$ are axioms (rewrite sequents of the form shown above).
