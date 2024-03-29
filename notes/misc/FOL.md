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

*\underline{Proof 1:}* A generalization of the proof of the recursion theorem.
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

The forward direction
$C^{\star} \subseteq C_{\star}$ can be shown by showing $C_{\star}$ is
*inductive*. As $C^{\star}$ is the *intersection* of all *inductive* sets,
it must be contained in $C_{\star}$.

The other direction can be showing
$C_i \subseteq C^{\star}$ by induction on $i$, where $C_i$ is defined as above.
For $i=0$, $C_0 = B$, thus $C_0 \subseteq C^{\star}$ For $i \geq 1$,
if $C_{i-1} \subseteq C^{\star}$, then $C_{i} = \{ g(x, y) \mid x, y \in C_i \}
\cup \{f(x) \mid x \in C_i \}$. Since $C_i \subseteq C^{\star}$ and $C^{\star}$
is closed under $f, g$, $C^{i+1} \subseteq C^{\star}$.

*Formal Definition of Recursion Theorem:*
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
said language. Both the existence and uniqueness of the function follows from
this theorem.

*\underline{Corollary 1.1:} Proof of existence of $\overline{h}$ using approximating functions*

Let $\overline{h}$ be a union of approximating functions. Say a function
$\nu$ is *acceptable* iff its domain is a subset of $C$, its range is
a subset of $V$, and for $x, y$ in $C$.

 i. If $x \in B$, then $\nu(x) = h(x)$.
 ii. If $f(x, y)$ in domain of $\nu$, then so are $x, y$.
     Moreover, $\nu(g(x, y)) = G(\nu(x), \nu(y))$. Similarly,
     if $f(x)$ belongs to the domain of $\nu$, then so does $x$,
     and $\nu(f(x)) = F(\nu(x))$.

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

For any $x \in B$, from definition of acceptable functions, we know
that for $\nu_1, \nu_2 \in K$, $\nu_1(x) = \nu_2(x) = h(x)$. Thus,
for $x \in B$, all acceptable functions defined at x agree. Thus $B \subseteq S$.

Next, we show that $S$ is closed under $f, g$. Consider any $x, y \in S$.
We need to show that $f(x), g(x, y) \in S$. Since $x, y \in S$,
For any acceptable functions $\nu_1, \nu_2$ defined at $x$ and $(x, y)$,
we know $\nu_1(x) = \nu_2(x)$ from definition of S.
Since $\nu_1(f(x)) = F(\nu_1(x))$ and $\nu_2(f(x)) = F(\nu_2(x))$.
Thus, if $\nu_1, \nu_2$ agree on $x$, then $\nu_1$ and $\nu_2$ also agree at $f(x)$.
Thus, if $x \in S$, then $f(x) \in S$, by definition of S. Similarly,
for any $x, y \in S$, $\nu_1(g(x, y)) = \nu_2(g(x, y)) = G(\nu_1(x), \nu_1(y)) =
G(\nu_2(x), \nu_2(y))$. Thus, for $x, y \in S$, $g(x,y) \in S$. Hence $S$
is inductive. Since $S \subseteq C$, and $C$ is the smallest inductive set,
$S = C$. Since $\overline{h}$ is the union of all *acceptable* sets,
it's defined over $C$, and is single valued.

To show (2), we already have shown that $\fdomain(S) = C$.
To show that $f(x) \in \fdomain(\overline{h})$, then $x \in
\fdomain(\overline{h})$, we consider an acceptable function $\nu$
with $f(x) \in \fdomain(\nu)$. Thus $x \in \fdomain(\nu)$, implying
$x \in \fdomain(\overline{h})$.


<!---
For any two acceptable functions $\nu_1, \nu_2 \in K$, and $x \in B$, by
definition of acceptable functions, $\nu_1(x) = \nu_2(x) = h(x)$.
Next, we show that $\overline{h}$ is closed under $f, g$.
For any $f(x, y) \in S$, we know if $f(x, y) \in \dom(\nu_1(x)) \in
\fdomain{\nu_1}$, so are $x, y$. Moreover, since $f$ and $g$
are disjoint and functions, $f(x, y) \in \dom(\nu_2(x)) \in
\fdomain{\nu_2}$.
--->

Compactness
===========

Given a finite/infinite set of formulas $\Gamma$, $\Gamma$
is *finitely-satisfiable* iff every finite-subset $\Gamma^0 \subseteq \Gamma$
is *satisfiable*. Compactness gives *satisfiablility* and *finite-satisfiability* are
equivalent.

Note that *compactness* isn't obvious. Compactness doesn't say that each
finite subset is satisfiable by the same model, but that as long as
all finite subsets are satisfiable (by models that may be different), there
exists some model that also satisfies the inifinite set.

**Proof of Compactnes**

If $\Gamma$ is *satisfiable*, then the satisfying assignment $\nu$ also
satisfies every finite subset $\Gamma^0$. Thus, if $\Gamma$ is satisfiable,
it's also finitely satisfiable.

Next, we show that *finite satisfiability* implies *satisfiability*.
If $\Gamma$ is finite, then compacntess trivially holds.

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
v_k ] \}$. A relation is said to be *definable* if  there is a formula
$\varphi$ that defines it. In the example above, the set $\{b\}$ would
not be *definable* as there is no formula that defines it.
For natural numbers, there are *uncountably infinite* relations, but
*countably-infinite* formulas, so there must be some relations not definable.

Logical Implication
===================

Get $\Gamma$ be a set of sentences, and $\varphi$ some sentence.
We say $\Gamma \vDash \varphi$ iff for every structure $\structure$
for the language and for every function $s : V \to \mid \structure \mid$,
if $\structure$ satisfies every sentence of $\Gamma$ with $s$, then
$\structure$ also satisfies $\varphi$ with $s$.

Definability of a Class of Structures
=====================================

*Structures* such as graphs, groups, etc., are essentially models
that satisfy a set of formulas $\Sigma$, also referred to as axioms.
Let $\Mod{\Sigma}$ be the set of all *models* for the language, i.e.,
the set of all structures in which every member of $\Sigma$ is true.

A class $\class$ is an *elementary class* iff $\class = \Mod{\tau}$
for some sentence $\tau$. $\mathcal{K}$ is an elementary class
in the wider sense if $\mathcal{K} = \Mod{\Sigma}$.

Examples of Class of Structures
-------------------------------

Assume a language with parameters $\forall$, and $E$, a binary predicate.
A graph is then a structure $\structure = (V, E^{\structure})$ where
$V$ is a non-empty set of nodes. The edge relation $E^{\structure}$
is *irreflexive* and *symmetric*. This can be expressed by the axiom:

$$ \forall x . \left(\left(\neg \left(x E x\right) \right) \wedge \left(\forall y . \left(x E y\right) \implies \neg \left(y E x \right)\right)\right) $$

For a directed graph, the symmetric can be dropped, and for
a graph with loops, irreflexivity can be dropped.

Homomorphisms
=============

Given structures $\structure, \mathfrak{B}$, a function from
$h: \structure \to \mathfrak{B}$ is said to be a homomorphism if:

 - For every predicate symbol $P$, we have $\langle a_1, a_2, \dots, a_n\rangle \in P^{\structure}$
   $\langle h(a_1), h(a_2), \dots, h(a_n) \rangle \in P^{\mathfrak{B}}$.

 - For every constant $a \in \mid \structure \mid$, we have $h(a) \in \mid
 \mathfrak{B} \mid$.

 - For every function symbol $f$, we have:
   $h(f^{\structure}(a_1, \dots, a_n)) = f^{\mathfrak{B}}(h(a_1), \dots h(a_n))$.

Thus, homomorphisms are structure-preserving transformations.
If $h$ is one-to-one then it's called an *isomorphism* of $\mathfrak{U}$
into $\mathfrak{B}$ if there is an isomorpism of $\mathfrak{U}$ onto
$\mathfrak{B}$, (i.e. $range(h) = \mid \mathfrak{B} \mid$), then they are said to be *isomorphic*, written $\mathfrak{U}
\cong \mathfrak{B}$.

Say the language has symbols $+, .$, where $\structure = (\mathbb{N}; +, .)$
Let $\mathfrak{B} = (\{ e, o \}, +_{\mathfrak{B}}, ._{\mathfrak{B}})$.
Define $h: \mathbb{N} \to \{e, o \}$ that maps even numbers to e and odd numbers
to o. Furthermore, map addition of two even numbers to even, and so on.
This defines a homomorphism from $\mathfrak{U}$ onto $\mathfrak{B}$, but not an
isomorphism.

Consider the set of positive integers $\mathbb{P}$ with ordering $<_P$.
Let $\mathbb{N}$ be the set of natural numbers with order $<_N$.
Then, there is an *isomorphism* $h$ is from $(\mathbb{P}, <_P)$ onto $(\mathbb{N}, <_N)$
defined as $h(n) = n - 1$.

More generally, given $\structure, \mathfrak{B}$ for some language, where $| \mathfrak{B}|
\subseteq | \structure |$. The identity map from $|\mathfrak{B}|$ into
$|\mathfrak{U}|$ is an isomorphism if:

 - For each predicate parameter $P$, $P^{\mathfrak{B}}$ is the restriction
   of $P^{\mathfrak{U}}$.
 - For each function symbol $f$, $f^{\mathfrak{B}}$ is the restriction of
   of $f^{\mathfrak{U}}$, and $c^{\mathfrak{U}} = c^{\mathfrak{B}}$ for each
   constant $c$.

If these conditions are met, then $\mathfrak{B}$ is a *substructure* of
$\mathfrak{U}$

**Theorem 2:** Let $h$ be a homomorphism form $\mathfrak{B}$ into
$\mathfrak{U}$, and let $s$ map the set of variables $\Var$ into $\mathfrak{B}$. Then,

 a. For any term $t$, $h(\overline{s}(t)) = \overline{h \circ s}(t)$, where
   $\overline{s}(t)$ is composed in $\mathfrak{B}$, and $\overline{h \circ s}(t)$
   is computed in $\mathfrak{U}$.
 b. For a quantifier free formula $\alpha$ not containing the equality symbol:
    $$\vDash_{\mathfrak{B}} \overline{s}(\alpha) \text{ iff } \vDash_{\mathfrak{U}} \overline{h \circ s}(\alpha)$$
 c. If $h$ is an isomorphism, then the restriction on equality may be removed.
 d. If $h$ is onto, then the restriction on $\alpha$ being quantifier free may be removed.

*\underline{Proof 2.a:}*

Need to show for any term $t$, $h(\overline{s}(t)) = \overline{h \circ s}(t)$.
Since $h \circ s$ is a map from the variables $\Var$ to $| \mathfrak{B} |$,
there exists a unique homomorphism $\overline{h \circ s}$ s.t.

 - $\overline{h \circ s}(v) = h \circ s(v)$ for $v \in \Var$
 - $\overline{h \circ s}(f(a_1, \dots, a_n)) = f^{\mathfrak{B}}(\overline{h \circ s}(a_1), \dots, \overline{h \circ s}(a_n))$


By induction on the term $t$.

*Base Case:* If $t$ is variable $v$, then $h(\overline{s}(v)) = h(s(v)) = h \circ
s(v) = \overline{h \circ s}(v)$, by definition of $\overline{h \circ s}$ above.

If $t$ is a constant $c$, then let $\overline{s}(c) = c^{\structure}$.
By definition, $h(c^{\structure}) = c^{\mathfrak{B}}$. By definition of
$\overline{h \circ s}(c) = c^{\mathfrak{B}}$.


*Inductive Case:* Let $t=f(a_1,\dots,a_n)$. We have:

$$
\begin{aligned}
= &\  h(\overline{s}(f(a_1,\dots,a_n))) &  \\
= &\  h(f^{\structure}(\overline{s}(a_1),\dots,\overline{s}(a_n))) & \;(\text{by definition of s}) \\
= &\  f^{\mathfrak{B}}(h(\overline{s}(a_1)), \dots, h(\overline{s}(a_n))) & \; (\text{by definition of h}) \\
= &\  f^{\mathfrak{B}}(\overline{h \circ s}(a_1)), \dots, \overline{h \circ s}(a_n)) & \; (\text{by inductive hypothesis}) \\
= &\  \overline{h \circ s}(f(a_1,\dots, a_n)) & \; (\text{by defintion of }\ \overline{h \circ s}) \\
\end{aligned}
$$

*\underline{Proof 2.b:}*

Let $\alpha$ be quantifier-free, and not contain any equalities.

*Base Case:* Let $\alpha$ be an atomic formula $f(a_1, \dots, a_n)$.
$$
\begin{aligned}
            & \vDash_{\mathfrak{U}} f(a_1, \dots, a_n)[s] \\
\text{iff}\ & \langle \overline{s}(a_1), \dots, \overline{s}(a_n) \rangle \in f^{\mathfrak{U}} &\ \text{(by definition of satisfaction)} \\
\text{iff}\ & \langle h(\overline{s}(a_1)), \dots, h(\overline{s}(a_n)) \rangle \in f^{\mathfrak{B}} &\ \text{(by defintion of } h) \\
\text{iff}\ & \langle \overline{h \circ s}(a_1)), \dots, \overline{h \circ s}(a_n) \rangle \in f^{\mathfrak{B}} &\ \text{(using proof 2.a)} \\
\text{iff}\ & \vDash_{\mathfrak{B}} f(a_1, \dots, a_n)[h \circ s] &\ (\text{by definition of satisfaction}) \\
\end{aligned}
$$

*Inductive Case:* Suppose $t = \neg \alpha$. We know,
$\vDash_{\mathfrak{U}} \neg \alpha[s]$ iff $\not{\vDash_{\mathfrak{U}}} \alpha[s]$.
By inductive hypothesis, $\not{\vDash_{\mathfrak{U}}} \alpha[s]$ iff
$\not{\vDash_{\mathfrak{B}}} \alpha[h \circ s]$. Thus, by definition
of satisfaction for $\neg \varphi$, we have $\vDash_{\mathfrak{B}} \neg \alpha[h \circ s]$.

We omit the $\to$ case - a similar argument can be employed.

*\underline{Proof 2.c:}*
If $h$ is one-to-one, then $\alpha$ may contain equality.
Suppose $\alpha \equiv \alpha_1 = \alpha_2$. We have,

$$
\begin{aligned}
            & \vDash_{\mathfrak{B}} \alpha_1 = \alpha_2[h \circ s] & \\
\text{iff}\ & \overline{h \circ s}(\alpha_1) = \overline{h \circ s}(\alpha_2) & \\
\text{iff}\ & h(\overline{s}(\alpha_1)) = h(\overline{s}(\alpha_2)) &\ \text{ using 2.a } \\
\text{iff}\ & \overline{s}(\alpha_1) = \overline{s}(\alpha_2) &\ \text{ since h is one-to-one} \\
\text{iff}\ & \vDash_{\mathfrak{U}} \alpha_1 = \alpha_2[s]
\end{aligned}
$$

*\underline{Proof 2.d}:*

Assuming $\alpha \equiv \forall x . \varphi$, we need to show:

$$\vDash_{\mathfrak{U}} \forall x. \varphi[s] \text{ iff } \vDash_{\mathfrak{B}} \forall x . \varphi [ h \circ s] $$

The proof follows as an extension of arguments from *2.b*.

From definition of $\forall$ satisfaction, we have
$\vDash_{\mathfrak{U}} \varphi[s]$ iff for any $u \in |U|$,
$\vDash_{\mathfrak{U}} \varphi[s(x \mid u)]$. By inductive hypothesis,
we know $\vDash_{\mathfrak{U}} \varphi[s(x \mid u)]$ iff
$\vDash_{\mathfrak{B}} \varphi[(h \circ s)(x \mid h(u))]$.
Thus, we get $\vDash_{\mathfrak{U}} \forall x. \varphi [s]$ iff
for all $u \in |U|$, $\vDash_{\mathfrak{B}} \varphi[(h \circ s)(x \mid h(u))]$,
which can be rephrased as: for all $b \in R_h$,
$\vDash_{\mathfrak{B}} \varphi[(h \circ s)(x \mid b)]$ where
$R_h = \left\{ h(u) \mid u \in U \right \}$.
If $h$ is onto, $R_h = |B|$. Thus,
$\vDash_{\mathfrak{U}} \forall x . \varphi [s]$ iff
for all $b \in |B|$, $\vDash_{\mathfrak{B}} \varphi[(h \circ s)(x \mid b)]$. By
definition of $\forall$ satisfaction, we have
$\vDash_{\mathfrak{U}} \forall x .  \varphi [s]$ iff
$\vDash_{\mathfrak{B}} \forall x . \varphi[h \circ s]$.


Definability using Automorphisms
--------------------------------

Recall that set $A$ is set to be definable in $\structure$ iff there is a WFF
$\varphi(x_1, \dots, x_n)$ s.t. $A = \left\{ \langle a_1, \dots, a_n
\rangle \in |\structure|^n
\mid\ \vDash_{\structure} \varphi(a_1, \dots, a_n) \right \}$, where $x_1, \dots, x_n$
are free in $\varphi$.

Suppose $h: |\structure| \to |\structure|$ is an automorphism. Let $A$
be definable using WFF $\varphi(x_1, \dots, x_n)$. For $\langle a_1, \dots a_n
\rangle \in A$, we have $\vDash_{\structure} \varphi(a_1, \dots, a_n)$ iff
$\vDash_{\structure} \varphi(h(a_1), \dots, h(a_n))$. Thus, if
$\langle h(a_1), \dots, h(a_n) \rangle \not \in A$, we have a contradiction.
Thus, to establish non-definability, we need to find such an automorphism
that leads to a contradiction.

Deductive System
================

A system that allows us to establish systematically a
statement of the form $\Gamma \vDash \varphi$ by constructing
a *proof*. A proof has to be finitely long, and if the set of
hypothesis $\Sigma$ is inifinite, they cannot all be used.
By compactness, we can use some finite set $\Sigma_0 \subseteq \Sigma$
such that $\Sigma_0 \vDash \tau$. To enable proof checking to occur,
it must also be the case the set of formulas provable without
hypothesis is enumerable.

Formal Deductions
-----------------

Let $\Lambda$ be the set of axioms. Then for a set $\Gamma$ of
formulas, the theorems of $\Gamma$ are the set of formulas
that can be derived from $\Lambda \cup \Gamma$ using finite
applications of rule of inference. If $\varphi$ is a theorem
of $\Gamma$ (written $\Gamma \vdash \varphi$), then a sequence of
formulas describing how $\varphi$ is obtained from $\Lambda \cup \Gamma$
using rules of inference is called a *deduction* of $\varphi$ from
$\Gamma$.

*Definition of a deduction:* A deduction of $\varphi$ from $\Gamma$
is a finite sequence $\langle \alpha_0, \alpha_1, \dots, \alpha_n \rangle$
of formulas such that:

 - $\varphi = \alpha_n$ and for each $k \leq n$, either:
   * $\alpha_k$ is in $\Gamma \cup \Lambda$, or
   * $\alpha_k$ is obtained by using modus ponens on two earlier
   formulas in the sequence, that is form some $i,j < k$, $\alpha_j$
   is $\alpha_i \to \alpha_k$.

If such a deduction exists, $\varphi$ is said to be deducible from
$\Gamma$.

A set $S$ of formulas is said to be closed under modus ponens if
whenever $\alpha$ and $\alpha \to \beta \in S$, then $\beta \in S$.

Given a WFF $\varphi$, say $\varphi$ is a generalization of $\psi$
for some $n \geq 0$ and variables $x_1, \dots, x_n$ $\varphi = \forall x_1
\forall x_2, \dots, \forall x_n \psi$. If $n = 0$, the WFF $\varphi$ is
considered to be a generalization of itself.

The set $\Lambda$ of logical axioms can then be defined, for
variables $x, y$  are variables and $\alpha, \beta$ are WFFs as:

 - Tautologies
 - $\forall x\ \alpha \to \alpha_{t}^{x}$ (substitute $t$ for $x$).
 - $\forall x(\alpha \to \beta) \to (\forall x\ \alpha \to \forall x\ \beta)$

If equality is also a part of the langauge:

 - $x = x$
 - $x = y \to (\alpha \to \alpha')$ where $\alpha'$ is obtained by replacing
   $\alpha$ in zero or more places by y, and $\alpha$ is atomic.

Substitution $\forall x\ \alpha \to \alpha_{t}^{x}$ can also be defined recursively:

 - For atomic formula $\alpha$, $\alpha_{t}^{x}$ is just replacement of $t$
 for $x$.
 - $(\neg \alpha)_{t}^{x} = \neg (\alpha_{t}^{x})$
 - $(\alpha \to \beta)_{t}^{x} = (\alpha_{t}^{x} \to \beta_{t}^{x})$
 - $(\forall y\ \alpha)_{t}^{x} = (\forall y\ \alpha_{t}^{x})$ if $x \neq y$
    and $\forall y\ \alpha$ otherwise.

Note that $\forall x\ \varphi \to \varphi_{t}^{x}$ may not always hold.
For instance, consider the following example, when $\alpha$ is $\neg \forall y\ x = y$.
We get $\forall x\ (\neg \forall y\ x = y) \to \neg \forall y\ y = y$, which
is false. Thus, while making substitutions, it's important to avoid
*variable capture* or the introduction of unintended quantification.

The following rules apply to evaluate substitutability:

 - For atomic formula, $t$ is always substitutable for $x$ in $\alpha$.
 - For $\neg \alpha$, $t$ is substitutable for $x$ if it's substitutable for $x$
 in $\alpha$.
 - For $\alpha \to \beta$, $t$ is subsitutable if it's substitutable in both
 $\alpha$ and $\beta$.
 - For $\forall y\ \alpha$, $t$ is substitutable for $x$ in $\alpha$ if
   $x$ does not occur free in $\alpha$, or, $y$ doesn't occur in $t$ and
   $t$ is substitutable for $x$ in $\alpha$. This accounts for the possibility
   of variable capture.



