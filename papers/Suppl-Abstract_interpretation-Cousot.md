---
title: Abstract Interpretation
subtitle: Notes on Abstract Interpretation (POPL'77)
author: Patric Cousot
header-includes: |
  \include{commands}
  \usepackage{mathtools}
  \usepackage{stmaryrd}
  \usepackage{dirtytalk}
---


Synopsis
--------

Program executions are computations in some universe of
objects. An execution can also be used to describe
computation in another universe of objects to
derive answers about the original computation .

E.g. The rule of signs can be used to derive the sign
of the result of -1443 * 32. Numbers can be abstracted
away and represented by their sign, and rules of
operations over signs will give for the example above:
-(+) * (+) => (-) * (+) => (-) .

Another example, typing a program involves execution
in an abstract domain of types to derive the final
type without actually needing to run the program concretely.



Preliminaries
-------------

### Relations

A binary relation $R$ on set $S$ is basically a member of $\powerset{S \times S}$.

A **pre-order** $\sqsubseteq$ on a set $S$ is
a *reflexive* ($\forall x \in S . s \sqsubseteq s$)  and *transitive*
($\forall x, y, z \in S . (x \sqsubseteq y) \wedge (y \sqsubseteq z) \implies (x \sqsubseteq z)$)
relation.

A **partial-order** is an *anti-symmetric pre-order*

A **linear order** is a *total partial-order* i.e.
($\forall x, y \in S . (x \sqsubseteq y) \vee (y \sqsubseteq x)$)

A **chain** of set $S$ is a subset $X \subseteq S$
such that $\sqsubseteq$ is a *linear order* on $X$

A **Complete Partial Order (CPO)** is a partial order
such that every increasing chain has a *least upper bound*.
A *Strict CPO* is one where every chain also has an infimum.

### POSET

A *reflexive*, *anti-symmetric* and *transitive*  relation

- Anti-symmetric defined as $a \leq b \wedge b \leq a \implies a = b$

### Lattice

A POSET $L$ such that $\forall a, b \in L$ both
$a \sqcup b$ (lub) and $a \sqcap b$ (glb) exist.

Note that lub, glb defined for finite subsets,
but may not be defined for infinite subsets.

#### Complete Lattice

For any subset $L' \subseteq L$, glb, lub are defined.
In other words, even inifinite subsets have glb, lub defined.

##### Example

For any set $S$, $(\mathcal{P}(S), \varnothing, \subseteq, \cup, \cap)$
for a complete lattice.

## Relations on POSETs

Given a poset $P(\leq)$ and $Q(\preceq)$, A relation $\varphi \in P \to Q$
is monotone iff $\forall x, y \in P . x \leq y \implies \varphi(x) \preceq
\varphi(y)$. Given a cpo, a relation $\varphi$ is order preserving if
it's monotone and preserves the least upper bound ($\sqcup \{x_1, x_2, \dots \} =
\vee \{\varphi(x_1), \varphi(x_2), \dots \}$).


## Tarski's Theorem

States that for a complete lattice $L$ and
monotone order preserving function $\varphi$,
the fixed points of $\varphi$ form a complete lattice.


### Applications of Tarski's Theorem

Given an $\omega$ upper continuous mapping $\varphi$.
The least fixed point ($\lfp$) is given by:

$$\lfp(\varphi) = \glb_{i \in \Nat}(\varphi^{n}(\bot))$$

where $\glb$ is the greatest lower bound operation

### Galois Connections


### Approximation of Concrete Properties by Abstract Properties

For a given program $P$, let $\concrete{P}(\concrete{\preceq})$ be
a POSET representing the set of concrete
properties. Say $\concrete{p}_1 \concrete{\preceq} \concrete{p}_2$ to mean $\concrete{p}_1$ is
*more precise* than $\concrete{p}_2$. Similary,
$\abstr{P}(\abstr{\preceq})$ be the POSET of abstract
properties. Precision can basically mean the set
of states satisfied by the property is smaller.
For example say $\concrete{p}_1$ is the property $=0$
and $\concrete{p}_2$ is $\geq 0$,
then $\concrete{p}_1 \concrete{\preceq} \concrete{p}_2$.
Say $\abstr{p}_1 = \alpha(\concrete{p}_1)$ and $\concrete{p_1} \concrete{\preceq} \concrete{p}_2$
Then $\abstr{p}_2$ is less precise than $\abstr{p}_1$.


#### Definitions

The pair $(\alpha, \gamma)$ forms a Galois connection iff

$$ \forall \concrete{p} \in \concrete{P}, \abstr{p} \in \abstr{P}.
  (\alpha(\concrete{p}) \abstr{\preceq} \abstr{p}) \Leftrightarrow
    (\concrete{p} \concrete{\preceq} \gamma(\abstr{p})) $$

Some more properties of the Galois connections:

$$ \forall \concrete{p} \in \concrete{P} . \concrete{p} \concrete{\preceq} \gamma(\alpha(\concrete{p}))$$
$$ \forall \abstr{p} \in \abstr{P} . \alpha(\gamma(\abstr{p})) \abstr{\preceq} \abstr{p}$$

The Galois connectiontion $(\alpha, \gamma)$ can be used as:

$$ (\textit{\textbf{Concretization}})\ \ \gamma(\abstr{p}) \in \concrete{P}$$
$$ (\textit{\textbf{Abstraction}})\ \  \alpha(\concrete{p}) \in \abstr{P}$$

Thus, $\alpha$ is a monotone function in $\concrete{P}(\concrete{\preceq})
 \to \abstr{P}(\abstr{\preceq})$
and $\gamma$ is a monotone function in  $\abstr{P}(\concrete{\preceq}) \to
\concrete{P}(\concrete{\preceq})$


For a state $x$, abstraction followed by concretization results
in a state that's less than $x$.

We know since $L_2$ is a lattice, $\alpha(x) \leq \alpha(x)$.
Since $(\alpha, \gamma)$ is a Galois connection, we know
$\alpha(x) \leq y \Longleftrightarrow x \leq \gamma(y)$, which
gives us $x \leq \gamma(\alpha(x))$.

$(\alpha, \gamma)$ form a Galois connection iff they are monotone, i.e.
$x \leq_1 [\leq_2] y \implies \alpha(x) [\gamma(x)] \leq_2 [\leq_1] \alpha(y) [\gamma(y)]$,
which gives us $(\gamma \circ \alpha)(x) \leq_1 x$ and $(\alpha \circ \gamma)(y)
\leq_2 y$

This means that *abstraction* followed by *concretization* leads to
less information than what you started with since
$(\overbrace{\gamma}^{\textit{abstraction}} \circ
\underbrace{\alpha}_{\textit{concretization}})(x) \leq_1 x$

#### Composition of Galois Connections

Say $(\alpha_1, \gamma_1)$ and $(\alpha_2, \gamma_2)$ are Galois connections.

Then $(\alpha_1 \circ \alpha_2, \gamma_1 \circ \gamma_2)$ is also a Galois
connection.

#### Partitioning and Local Invariants

Given a set $S$ of states, properties can also be
represented via the sets of states on which they
hold. In other words, $\concrete{P} = \powerset{S}$.
Given $\concrete{p}$, an asbtraction can be defined
as $\abstr{p}[c] = \{ \rho \mid \langle c, \rho \rangle \in \concrete{p}\}$
for all control states ($c \in C$).
On the other hand the concretization can be defined as $\gamma(\abstr{p}) = \{ \langle c, \rho \rangle \mid \rho \in
  \abstr{p}[c]\}$ for all $c \in C$.



Computation of Abstract Semantics
---------------------------------

Abstract semantics can be computed from concrete semantics,
which presents a big advantage for abstract interpretation.


Consider following example language:

$$ Expr ::= Id \mid \mathbb{N} \mid Expr + Expr  \mid Expr \geq Expr $$
$$ Stmt ::=  \mid Id := Expr \mid \say{begin} \mid \say{end} \mid \say{if}  Expr  \say{goto} \textit{PC} $$


##### Sorts

 * $\textit{PC} \equiv \mathbb{N}$

 * $\textit{Env} \equiv Id \to \mathbb{N}$. Also written $\rho$

 * $\textit{State} \equiv (\textit{PC} \times \textit{Env})$


#### Semantics of Expressions

 * $\llbracket x \rrbracket_{\rho} = \rho(x)$

 * $\llbracket E_1 + E_2 \rrbracket_{\rho} = \llbracket E \rrbracket_{\rho} +
   \llbracket E_2 \rrbracket_{\rho}$


#### Semantics of Statments

$\textit{pc}, \rho \to \textit{pc} + 1, \rho'$ defined as:

 * $\textit{pc}(Stmt) = x := E$ then $\rho' = \rho[\llbracket E \rrbracket_{\rho} / x]$

 * $\textit{pc}(Stmt) = "if" E "goto" n$ then $\textit{pc} = \textit{pc} + 1$ if $\llbracket E \rrbracket_{\rho} = \top$ else $\textit{pc} = n$

 * ... and so on


#### Collecting Semantics

Semantics that collects the set of all possible traces

The set of all traces of all states order by inclusion forms
a lattice


 * For the language in previous section, $\mathbb{Z}$ forms
   the domain. $\mathcal{P}(\mathbb{Z})$ gives the collecting semantics
   with $\subseteq$.

 * The abstract lattice simply becomes $\mathcal{P}(\mathbb{Z})$ with $\leq
   \equiv \subseteq$.


