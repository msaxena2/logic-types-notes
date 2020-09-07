---
title: Abstract Interpretation
subtitle: Notes on Abstract Interpretation (POPL'77)
author: Patric Cousot
header-includes: |
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

### Galois Connections

#### Definitions

Let $L_1(\leq_1)$ and $L_2(\leq_2)$ be POSETS.
$(\alpha, \gamma)$ is a Galois connection iff $\alpha \in L_1 \to L_2$
and $\gamma \in L_2 to L_1$, and

$$ \forall x \in L_1, y \in L_2, \alpha(x) \leq_2 y \Longleftrightarrow x \leq_1 \gamma(y)$$

$L_1 \xrightleftharpoons[\gamma]{\alpha} L_2$ is a Galois connection where
$L_1$ is the concrete lattice and $L_2$ is the abstract lattice.

#### Properties

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






