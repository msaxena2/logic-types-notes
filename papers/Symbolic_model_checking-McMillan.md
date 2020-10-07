---
title: Symbolic Model Checking
subtitile: Notes on symbolic model checking
header-includes: |
  \include{commands}
---

Boolean Representation
----------------------

Given a set of varibles S, any element in $2^S$
can be thought of as an evaluation where variables
in the element are true while those absent are false.
Now, for any $v \in S$, we can identify the
set of sets containing v (i.e. valuation of v is true)
as $\{ s \mid v \in s \wedge s \in 2^S \}$


The vector functional $\lambda \bar{v} f$.
when applied to $\bar{a}$, it yeilds $f[\bar{a} / \bar{v}]$.

The vector functional $\tau$ is closed if for every $\bar{a} \in \SortBool^n$
f($\bar{a}$) is either true or false. A formula containing just
variables from $S$ will always be closed.
Furthermore, $\tau$ uniquely characterizes the subset of vectors
from $\SortBool^n$ where it is true.

For any set of states $\hat{p}$, there is a unique
representation $p$ defined as $p = \lambda v_1, v_2, \dots, v_n p$
where $\mathit{p}$ is a set of boolean vectors, and $\hat{p}$
is a set of subsets of $S$.
So essentially, given a vector in $\SortBool^n$,
you can come up with a unique representation
$p = \lambda \bar{v} p$ where p is a set of subsets of $S$
(the set of variables). This formula characterizes the
set of states (i.e. boolean vectors) where it holds.


One can similary have a relation $R = \lambda (\bar{v}, \bar{v'}) R$
s.t. it characterizes relations between pairs of states.

Intuitively, a state in this setting is not a valuation but
a n-valued vector. The lambda representation allows
associating the mapping with the vector itself.

Atomic propositions can be represented as
$\lambda \bar{v} . v_i$ for states where the i-th
component in the boolean vector is true.

The labeling $L(l_i) = \lambda \bar{v} . v_i$.

Symbolic Models
---------------


For CTL, we need quantification as in -

Given some formula $f$, $\exists \bar{v} . f$
means:

$$ \exists v_1, v_2, \dots v_n . \lambda f = \bigvee_{\bar{\alpha} \in
\SortBool^n} \lambda v_1, v_2, v_3, \dots, v_n . f(\bar{\alpha})$$


Now, EFp, which says that there exists a path such that
along it p eventuall holds. Or, it identifies the set of
states $v_1, \dots, \dots, v_n$ such that there exists
a state $(v_1', \dots, v_n')$ in $R((v_1, \dots, v_n), (v_1', \dots, v_n'))$
such that $(v_1', \dots, v_n') \in p$
This can be represented as:

$$ \exists \bar{v}, \bar{v}' . R(\bar{v}, \bar{v}') \wedge p(\bar{v}')$$

Note that $p(\bar{v}')$ ensures that variables $v$ in $\lambda \bar{v} . p$
are properly substituted. The existential quantification outside
still allows reasoning via SMT.






