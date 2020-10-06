Equational Logic
----------------

Formally reasoning about equalities.
Equations are axioms of the form (\forall X, l = r), where X is a set of
variables in the equality.

Generally, `T(F, X)` is a set of terms that can be formed using F (the set of
functional terms using arities) and X, a set of variables. `\sigma` is a
substitution of the variables to terms, and variables in a term can be
instantiated using `\sigma`.

Apart from the equality axioms described above, there are also some proof rules
  - Reflexitivity - for any term x, x = x
  - Symmetry - for any x & y, x = y -> y = x
  - Transitivity - for any x, y & z, x = y & y = z -> x = z
  - Congruence - for i = 1,2...,n , t_i = t'_i -> f(t_1, t_2, ..., t_n) =
    f(t'_1, t'_2, ..., t'_n)
  - Subsititutivity - for any \sigma, t1 = t2 -> \sigma(t1) = \sigma(t2)

Given equational axioms E, and a set of terms T(F, X), Theory(E) is the set
of all derivable theorems using axioms and the aforementioned proof rules.

In other words, E |- (x = y) iff (x = y) \in Theory(E) .

