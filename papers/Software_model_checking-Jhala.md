---
title: Software Model Checking
subtitle: Notes on Model Checking
author: Ranjit Jhala
header-includes: |
  \include{commands}
  \usepackage{stmaryrd}
  \usepackage{graphicx}
---

Synopsis
--------

The main goal of Software Model Checking is to
prove properties about program computations.


Preliminaries
-------------

A simple program is defined via a transition relation as $P = (X, L, l_0, t)$ where
$X$ is the set of variables, $L$ set of control locations with $l_0 \in L$
initial locations and $T$ the set of transitions. Given $(l, \rho, l') \in T$,
$l, l'$ are the source and end locations of the trasition and $\rho$ is a
constraint over free variables from $X \cup X'$, where variables from $X$
represent values from $l$ and variables from $X'$ represent values from $l'$.

The labels and transitions define a control flow graph (CFG).
For a given program, its CFG captures its control flow.
A program can be converted to its simple program representation as:

 - Assignment `x := e` becomes $x' = e \wedge \underset{y \in X / \{x\}}{\bigwedge}y =
   y'$

A state $s$ is an of valuation variables (X), represented as $s = v.X$.
Given $(s, s') \in v.X \times v.X'$, say $(s, s') \vDash \rho$ if the
valuations satisfy the constraint.

A *finite computation* is defined as $\langle l_0, s_0\rangle, \langle l_1, s_1
\rangle, \dots \langle l_k, s_k \rangle \in (L \times v.X)^{*}$ where
$l_0 \in L_0$ (initial location)
and for each $i \in \{0, \dots, k-1\}$  there exists a
transition $(l_i, \rho, l_{i+1}) \in T$ such that $(s_i, s_{i+1}) \vDash \rho$

Similarly an *infinite computation* is defined $\langle l_0, s_0\rangle, \langle l_1, s_1
\rangle, \dots  \in (L \times v.X)^{\omega}$


### Properties

Intuitively, safety properties state "bad things don't happen".
Liveness properties state "good things eventually happen".
Mathematically, a property $\Pi \subset (L \times v.X)^{\omega}$,
is basically defined as a set of traces and a traces $\sigma$
is said to satify $\Pi$ iff $\sigma \in \Pi$.

A safety property is one such that for every infinite computation
$\sigma \in \Pi$, and for every prefix $\sigma'$, there exists
a $\beta \in (L \times v.X)^{\omega}$ such $\sigma' \beta \in \Pi$.
Contrapositively, a trace $\sigma'$ is unsafe if there exists no trace $\beta$
that makes it safe (i.e. $\sigma . \beta \in \Pi$).
In other words, $\Pi$ is a safety property iff and only if it's prefix
closed.

The **verification problem** takes as input a program P and property $\Pi$
and return true if all traces of P are in $\Pi$ and false otherwise.
It can also be stated in terms of reachability: given a program P and
a location $\epsilon$, the verification problem return true iff
no exeuctions reach $\epsilon$.

### Stateful Search

Let $\finitePgm$ be the class of programs where each variable
ranges over a finite domain. The verification problem has
an algorith with the following steps:

1. Takes as input the property and the error state to avoid.
2. Maintain queues reach and worklist for reached states and
   states to work on.
2. Add all initial states to the queue of states to explore.
3. Dequeue a state $(l, s)$, and if it's not in reach, add it.
4. For each $(l, \rho, l') \in T$, add all states $s' \in \text{Post}(s, \rho)$
   to worklist.
5. Check if $\epsilon \in$ reach.

State space explosion is the main problem that must be handled.
There are two main ares of research:

1. Collapse states into equivalent classes and perform the search
   on one candidate from each class. Requires showing that
   any bug in the original system can also be intercepted in the
   reduced system. Involves techiniques like Partial order reduction,
   symmetric reduction, etc.

2. Compositional approaches involve proving safety on smaller sub
   programs to compose a proof for the main program. Basically
   corresponds to the Hoare logic composition rule, which says

```
            {A1} P1 {G1}     {A2} P2 {G2}     G1 -> A2
            ------------------------------------------
                        {A1} P1 ; P2 {G2}
```


#### Sytematic State Space Exploration

All non determinism is factored into two sources: input from environment
and scheduling choices. In contrast to exhaustive exploration, systematic
exploration proceeds by iterating over the space of all possible schedules
and simply executing the process under each schedule. The advantage is
that the need for a formal executable semantics is eliminated.
Mostly used in conjunction with a test harness. While a regular test
simply executes the program with some fixed schedule, a systematic
exploration based tool would execute the program with the same input
but with all possible schedules. Challeneges include a mechanism
that's needed to keep track of states, generation of schedules,
discovering executions where states are the same regardless of schedule.

#### Stateless Search

The key idea behind "stateless search" is to
not store the set of visited states while performing the search.


### Symbolic Concrete Model Checking

Instead of the enumerative approach that relies on
enumerating concrete states, symbolic model checking
uses an *abstract data type* symreg to represent states.
An interpretation function $\llbracket \_ \rrbracket : \text{symreg} \to
\powerset{v.X}$. The advantage is amplified by advances in constraint solvers.



#### Example: Propositional Logic

Consider finite domains. To simplify, we assume n boolean variables.
A boolean vector of length n can represent the state space.
A symbolic state is represented via a function that
returns true iff the representative vector is in the
symbolic state. This basically boils down to the Mathching Logic's representation
of states, where for given concrete state $c$ and symbolic pattern
$\varphi$, $c$ is in the symbolic state iff $\rho(c) \in \rho(\varphi)$,
where $\rho$ is the interpretation into the finite domain.

Note that the focus here is on the representation of states. An enumerative
representation will require $2^n$ states, but a symbolic representation
requires just $\top$ to represent the set of all states.

#### Example: First Order Logic

In this case we're dealing with programs with infinite domains.
A symbolic region's representation involves a formula $varhpi$ with free variables
that correspond to program variables. The interpretation is given $\llbracket
\varphi \rrbracket = \{s \mid  s \vDash \varphi \}$.

### Invariants and Synthesis

The set of reachable states $R$ is defined as the smallest set closed under the
Post operation. In other words $\Post(R) \subseteq R$. Safety w.r.t to a label
$\epsilon$ is guaranteed as long as $\epsilon$ does not appear in $R$.
Instead of calculating $R$, one could calculate $R'$ such that (1) set of
initial states is contained in $R'$ and (2) $R'$ is closed under the Post
operation. If $\epsilon$ doesn't appear in $R'$, then the program is still
safe, even though the $R'$ may be an over approximation of $R$.


## Abstraction

Model checking in infinite state reachability analysis, even
in the case of symbolic states may not terminate.
Abstraction provides trade-off b/w precision
and efficiency. Historically, model chekcing has focused
on precision and static analysis on efficiency.

An abstract domain $(\mathcal{L}, \gamma, \abspre, \abspost)$
where -

 1. L is the abstract domain lattice and $\gamma : L \to 2^{v.X}$ is a concretization function
 2. $\abspre, \abspost: L \times T \to L$
 3. $\forall l, l'$ and $\rho$, $\gamma(\abspost(l), rho) \subseteq \text{Post}(l, rho)$

The lattice element $l$ represents an abstract view of the
concrete states $\gamma(l)$. Furthermore, we have
$\gamma(\text{Reach}(l)) \subseteq \text{Reach}^{\sharp}(l)$

The model checking algorithm is essentially the same as the
enumerative case, with the
main insight being the use of *abstract* lattice elements instead
of symbolic regions/concrete values.

Note that since Reach $\subseteq \text{Reach}^{\sharp}$,
it's always true that if abstract model checking
returns true, then the concrete program is also true.
But, if it returns false, it may be the case that the
actual program may be true since the unsafe
abstract region might be present in $\text{Reach}^{\sharp}$
but be outide Reach.


## Polyhedral Domains

The abstract domains are n-dimensional polyhedra
for programs with n-variables.


## Predicate Abstraction

The abstract domain is parameterized over
$\Pi$, a fixed set of predicates, and ordered
by implication. Given region $\psi$, the goal is to find a
the smallest (in the implication ordering) region
that represents $\psi$ using a boolean combination of
formulas from $\Pi$.

$$ \text{Abs}(\psi, \Pi) = \bigwedge\{\phi \mid \Pi \wedge \psi \implies \phi \}$$

$\text{Abs}(\psi, \Pi)$ can be calculated recursively with cases:

 1. true if $\Pi = \varnothing$ and $\psi$ is SAT
 2. false if $\Pi = \varnothing$ and $\psi$ is UNSAT
 3. $(p \wedge \text{Abs}(\psi \wedge p, \Pi')) \vee
     (\neg p \wedge \text{Abs}(\neg p \wedge \psi, \Pi'))$ where $\Pi = \Pi'
     \cup \{p\}$

Then, an SMT solver can discharge formulas.

## Abstraction Refinement

Abstraction based model checking is sound, but not complete.
The problem arises from the case when a property doesn't
hold in the abstract domain, but is true in the concrete
domain. Refinement is a way of addressing this issue.
Counter Example Guided Abstraction Refinement (CEGAR)
uses conterexamples to refine the domains.

### CEGAR

The input to CEGAR is a path in the CFG (from the original
abstraction) where the  property doesn't hold.
A trace-formula is constructed
from the path, and checked for SAT. If unsat,
attempt to find the "unsat core" - atomic formulas
whose conjunction gives unsat.
Once found, they are added to the set of tracket
formulas.

To construct the trace formula, variables
in the trace's constraints are renamed. For example,
suppose trace is $\langle l_1, (1, \_)\rangle, \langle l_2, (\_, 2)\rangle \dots \langle l_k, (x, y)\rangle$,
where  $(l_11, x' = x + 1 \wedge y'= y + 2, l_2) \in T$
then the trace formula would look like
$x_2 = x_1 + 1 \wedge x_1 = 1 \wedge y_2 = 2 \wedge y_2 = y_1 + 1$.
The trace formula can be checked for satisfaction, and if unsat,
then the constraint obtained by dropping suffixes:
$x' = x + 1 \wedge y' = y + 1$ can be used to refine the abstraction.

## Procedural Abstraction

Given F, f0, where F is a set of functions and $f0$ is
the starting function. A procedure $f$ is defined
as $(L^f, X^f, l_0^f,T^f)$, exactly along the lines
of regular IMP. The set of instructions is extended
by including all IMP instructions (intra) and
adding CALL and RETURN (inter) instructions.

### Model checking with Procedures

Even if the variables are interpreted in finite
domains, enumerative techniques won't since
the call stack itself can be infinite. So
graph based reachability algorithms cannot be used.

### Summaries for Procedures

The key insight to analyzing procedural programs
is that the behavior of the entire program can be
summed up using input-outputs of the procedures.
If all variables have finite domains, the size of summaries,
or sets of pair of input parameter values and output
expression values is finite.
Computing a summary works as follows:

 1. Recall that for each procedure f,
    there exists a unique $x_0^f$ in $X^f$,
    the input parameter for $f$.

 2. The input state for a procedure where all variables
    *except* $x_0^f$ are set to 0. The input of a state
    $s \in v.X^f$ is the input state for $f$ where
    $x_0^f$ has the value $s(x_0^f)$.

 3. When call and return statements are encountered,
    the summary can be used instead of the method.


## Concurrency and Recursion

The reachability problem for concurrent IMP+PROC
is undecidable. This stems from the fact that
executions of procedural programs are isomorphic to
Context Free Languages (CFL). To deicde whethere a
configuration c1,c2 is reachable, the language $L_1 \cup L_2$
must be non empty, which is known to be undecidable

## Heap Data Structures

Potentially unbounded data structures represent
a big challenge. The data in these structures
have potential relations that must be reasoned about.
Thus, the tool needs to handle generalizations
over the data to reason about the structure and
instantiation to reason about the data itself.

The class imp+heap extends imp with an unbounded heap
with the property

```
  read(write(memory, index, value), index') = read(memory, index')
```
when `index =/= index'`

and

```
  read(write(memory, index, value), index') = value
```
when `index = index'`


Approaches to handling heaps:

 1. Alias analysis: Use pointers to determine whether
  two objects point to the same heap.
 2. Shape analysis is the study of determining whether
  observed heap values correspond to a class of data
  structures such as lists, trees, etc.


### Separation Logic

Extends classical hoare logic to support reasoning over
heaps by adding two operators: separating conjunction (`*`)
and the separating implication (magic wand `-*`).
For instance an assertion  of the form `A * B` says
that there is one part of heap that satisfies A while another
**separate** part that satisfies B.


Liveness and Termination
------------------------

## LTL

Temporal logics like LTL use Buchi automaton to check for
liveness properties


## Termination

Establishing termination can be established using a decreasing ranking function
on the states. That is a relation > such that for any pair of
successive states $s_i, s_{i+1}$, $s_i > s_{i+1}$
































