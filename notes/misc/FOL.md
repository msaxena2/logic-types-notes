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

It's well-known that $C = C^{\star} = C_{\star}$.

Consider set $C$, generated *freely* from $f, g$, i.e., $f, g$ are *one-to-one*
and their ranges are *disjoint*. Furthermore, consider set $V$, and functions
$F: V \to V, G: V \times V \to V, h: B \to V$,
then there exists a *unique* homomorphism $\overline{h}$ s.t.

 - $\overline{h} = h(x)$ for $x \in B$.
 - $\overline{h}(f(x)) = F(\overline{h}(x))$ for $x \in C$.
 - $\overline{h}(g(x, y)) = G(\overline{h}(x, y))$ for $x, y \in C$.

It's obvious that given an interpretation $\nu$ over variables in a
propositional/First-order language, there exists a unique homomorphism/
extended interpretation $\overline{\nu}$ into $\{\top,\bot\}$ for the WFFs.


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


