---
title: Miscellaneous Notes on First Order Logic
header-includes: |
  \include{header.tex}
---

Based on a Mathematical Introduction to Logic by Herbert Enderton.


Preliminaries
=============

Inductive Principle
-------------------

Assume a set $C$ generated from some set $B$ (of base elements)
using a set $\mathcal{F}$ (of functions). If a set $S \subseteq C$ is
inductive, i.e., $B \subseteq S$, and closed under $\mathcal{F}$, then
$S = C$.

Structures/Models
-----------------

Given a FO-Signature $\Sigma$, and a *structure* or *model* $\structure = \left(\domain, \__{\structure}\right)$
s.t.

 - For every constrant $c \in \Sigma$, there is some $c_{\structure} \in
 \domain$.
 - For every *n-ary* relation symbol $R \in \Sigma$, there is some
  $R^{\structure} \subseteq \domain^n$.
 - For every *n-ary* functional symbol $f$, there is some
 $f^{\structure} : \domain^n \to \domain$.

Say $s: \text{Var} \to \domain$ is a *valuation* from the set of
$\Sigma$-variables $\text{Var}$ to objects in the domain of computation.
We write formula $\varphi$ is satisfied in $\structure$
with valuation $s$ as $\vDash_{\structure} \varphi[s]$.

Formally, to define satisfaction, define extended-valuation $\overline{s}$ recursively as:

 - $\overline{s}(x) = s(x)$ for $x \in \text{Var}$.
 - $\overline{s}(c) = c_{\structure}$ for constant $c$.
 - $\overline{s}\left(f\left(t_1, \dots, t_n\right)\right) = f_{\structure}\left(\overline{s}\left(t_1\right), \dots \overline{s}\left(t_n\right)\right)$
   for some *n-ary* functional symbol $f$.

**Theorem 1.** For valuation s, there always exists a unique
*extended-valuation* $\overline{s}$.

*Proof:* A generalization of the proof of the recursion theorem.
Consider set $U$ (of all expressions), and set $B \subseteq U$.
Functions $f: U \to U$ and $g: U \times U \to U$ are defined over $U$.
Let set $C$ be a set *generated* from B. The set $C$ can be defined
either *bottom-up* or *top-down* as:

 - **(Top Down)** $C^{\star} = \bigcap_{C_i}$  where $C_i$ is inductive, i.e. *closed*
   under $f, g$, and $B \subseteq C_i$.
 - **(Bottom Up)** $C_{\star} = \bigcup_{i\in \mathbb{N}}(C_i)$, where:
    * $C_0 = B$.
    * $C_i = \left\{ f(x) \mid x \in C_{i-1} \right\} \cup \left\{g(x,x) \mid x
    \in C_{i-1} \right\} \cup C_{i-1}$.

It's well-known that $C = C^{\star} = C_{\star}$. The forward direction
$C^{\star} \subseteq C_{\star}$ can be shown by showing $C_{\star}$ is
*inductive*. As $C^{\star}$ is the *intersection* of all *inductive* sets,
it must be contained in $C_{\star}$. The other direction can be showing
$C_i \subseteq C^{\star}$ by induction on $i$, where $C_i$ is defined as above.

Consider set $C$, generated *freely* from $f, g$, i.e., $f, g$ are *one-to-one*
and their ranges are *disjoint*. Furthermore, consider set $V$, and functions
$F: V \to V, G: V \times V \to V, h: B \to V$,
then there exists a *unique* homomorphism $\overline{h}$ s.t.

 - $\overline{h} = h(x)$ for $x \in B$.
 - $\overline{h}(f(x)) = F(\overline{h}(x))$ for $x \in C$.
 - $\overline{h}(g(x, y)) = G(\overline{h}(x, y))$ for $x, y \in C$.

Using the above, it becomes obvious that given an interpretation $\nu$ over variables in a
propositional/First-order language, there exists a unique homomorphism/
extended interpretation $\overline{\nu}$ into $\{\top,\bot\}$ for the WFFs in
said language. Both the existence and uniqueness of the function is given
by the theorem.

*Proof of existence using approximating functions*

Let $\overline{h}$ be a union of approximating functions. Say a function
$\nu$ is *acceptable* iff its domain is a subset of $C$, its range is
a subset of $V$, and for $x, y$ in $C$.

 i. If $x \in B$, then $\nu(x) = h(x)$.
 ii. If $f(x, y)$ in domain of $\nu$, then so are $x, y$.
     Moreover, $\nu(f(x, y)) = F(\nu(x), \nu(y))$. Similarly,
     if $g(x)$ belongs to the domain of $\nu$, then so does $x$,
     and $\nu(g(x)) = G(\nu(x))$.

Define $K$ to be the set of all *acceptable* functions, and
say $\overline{h} = \bigcup(K)$.

Thus, $\langle x, z \rangle \in \overline{h}$ iff $z = \nu(x)$
for some acceptable $\nu$.

To show that $\overline{h}$ satisfies our requirements, we must establish:

1. $\overline{h}$ is a function.
2. $\overline{h} \in K$, i.e., $h$ itself is *acceptable*.
3. $\overline{h}$ is defined throughout $C$.
4. $\overline{h}$ is *unique*.

To show (1), say $S = \{ x \in C \mid \overline{h}(x) = z \text{ for at most 1 z} \}$.
$S = \{ x \in C \mid \text{ all acceptable functions agree at x } \}$.

For $x \in B$, from definition of acceptable functions, we know
for any $\nu_1, \nu_2 \in K$, $\nu_1(x) = \nu_2(x) = h(x)$.
Thus, $B \subseteq S$. Next, by definition of *acceptable* functions,
we know that if $\nu(x) \in S$


<!---
For any two acceptable functions $\nu_1, \nu_2 \in K$, and $x \in B$, by
definition of acceptable functions, $\nu_1(x) = \nu_2(x) = h(x)$.
Next, we show that $\overline{h}$ is closed under $f, g$.
For any $f(x, y) \in S$, we know if $f(x, y) \in \dom(\nu_1(x)) \in
\fdomain{\nu_1}$, so are $x, y$. Moreover, since $f$ and $g$
are disjoint and functions, $f(x, y) \in \dom(\nu_2(x)) \in
\fdomain{\nu_2}$.
--->

Definability
============

Suppose a FOL *signature* $\Sigma = \{ E \}$, where $E$ intuitively
is a binary relation signifying an *edge* on the model.
We use the word *structure* or *model* interchangeably. Consider
$\Sigma$-*structure* $\structure= (\{a, b, c\}, E_{\structure})$, where
$E_{\structure} = \{ (a, b), (a, c) \}$. Now, the set $\{b, c\}$ is
*definable* in $\structure$ as one can write a *Well Formed Formula (WFF)*
that exactly captures it. Said *WFF* would be $\exists v_2 . E(v_2, v_1)$.
This notion is formally captured as:

Given *structure* $\structure$ and formula $\varphi$,
with free variables $v_1,\dots, v_k$, define a *k-ary* relation on $\structure$
as $\{ (u_1, \dots, u_k)\;\mid \; \vDash_{\structure} \varphi[u_1 / v_1, \dots, u_k /
v_k ] \}$


