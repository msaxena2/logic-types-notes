---
title: Towards a Unified Theory of Test Data Selection
author: John Goodenough
header-includes: |
  \include{commands}
---

Synopsis
-------

This paper attempts to prove that
well structured tests can demonstrate
the absence of errors in programs.
In essence the fundamental questions
the paper attempts to address are:

 1. What are the sources of errors in programs?
 2. How can test data be selected to ensure failure
    from sources above do not manifest in programs.


Preliminaries
-------------

Given program `F` with domain `D`.
For $d \in D$, OK(d) means that F(d)
is an acceptable result. OK(d) can
be naturally extended to OK(T) for $T \subseteq D$, where
OK(T) means OK(d) for all $d \in T$.
A test T is said to be `through` when
COMPLETE(T, C) holds, where C is a predicate descibing
a data selection criterion.

The criterion C must be
reliable and meaninful. C is **reliable**
if for every T s.t. COMPLETE(T, C),
OK(T) must hold. In other words,
an error free program when run on a test selected
via the criterion must give an acceptable output.
C is **valid** if doesn't forbid the selection
of data that may reveal errors.

Formally,

 1. SUCCESSFUL(T) = $(\forall t \in T)$ OK(t)
 2. RELIABLE(C) =  $\forall T,T' \subseteq D$ (COMPLETE(T, C) $\wedge$
    COMPLETE(T', C)) $\implies$ (SUCCESSFUL(T) $\equiv$ SUCCESSFUL(T'))
 3. VALID(C) = $(\forall d \in D)$ !OK(d) $\implies \exists T \subseteq D$
    (COMPLETE(T, C) $\wedge$ !SUCCESSFUL(T))

In other words:

 * A successful suite is one where all elements get expected results
 * A reliable criterion is one such that any data sets T, T' chosen using
   it, i.e. COMPLETE(T, C) and COMPLETE(T', C) must only have test inputs
   where output is expected.
 * A criterion is valid if for any input value where the test fails,
   there must be a test suite $T \subseteq D$ s.t. it is complete w.r.t.
   C (as in COMPLETE(T, C) holds), but doesn't satisfy $SUCCESSFUL(T)$


The fundamental theorem is as follows:

If there exists $T \subset D$ and $C$ such that:

 1. COMPLETE(T, C)
 2. RELIABLE(C)
 3. VALID(C)
 4. SUCCESSFUL(T)

then $\forall d \in D$, OK(d), which essentially
shows that given a RELIABLE and VALID criterion
and test data that is complete w.r.t. to the criterion
is enough to establish the correctness of the entire
program.

Now, the entire input domain can be chosen
as the test input, in which case, using the
main theorem is equilvalent to establishing
program correctness via enumerative model
checking.

But, finding a valid and reliable criterion,
and a dataset that is useful (as in terms of size)
such that COMPLETE(t, C) is not a trivial task.
But it is an alternative method of looking at
formal correctness.

Types of Errors
--------------

Errors are mainly of two kinds: Performance and Logical.

Logical error can be classified into:

 1. construction - failure to implement specification
 2. specification - failure to capture intended design in specification
 3. design - failure to satisfy understood requirement
 4. requirement - failure to understand real requirment


Each of these logical errors manifest in the implementation.
So, errors can be categorized based on implementation:

 1. Missing control flow: Failure to test condition (like
    missing check for zero before division)

 2. Inappropriate Path selection: The path condition itself
    has a bug redirecting flow along unexpected branch

 3. Inappropriate or missing action: Missing a statement,
    or writing a wrong statment, like A + B instead of A * B.


These classification are important when deciding
criteria for test selection. Specifically, the question
that must be addressed is "Will all construction, specification,
design and requirement" errors always be detected by exercising
programs with data from the criterion"?


General method of test selection
--------------------------------

The authors discovered, in a published
algorithm for formatting text according
to a set of rules that been proven correct,
several bugs.


The authors then corrected the specs
for the program, bugs in it. They then
show how a method based purely on
program structure cannot yeild reliable
tests.

The paper goes through an exercise of
analyzing a program with bugs by
constructing a table with both conditional
and regular statements. Then,
the paper lists out rules that trigger certain
conditions and statments based on
how the conditions or statment must be reached.

Based on these rules, the paper suggests tests
that cover all paths, statements and some loops.


Developing Effective Program Tests
----------------------------------

### Sources for conditions (for tests)

 - Requirements program must satisfy
 - Specification
 - General characteristics of implementation
   such as data structures used
 - The internal structure of the program itself


## Condition Table Method

Each column is a logically possible combination
of conditions that may occur during program execution

### Deriving condition table for input domain

If inputs are characters like NULL, BLANK, OTHER and UNDEFINED, then
for the last 2 characters in the input stream, there are 16 entries in the
  condition table.

Formal Definitions of Completeness, Validity and Reliabilitily
--------------------------------------------------------------

If a condition c is a comibination of clauses, then
we have given $T \subseteq D$-
$$\text{COMPLETE}(T, C) =
  \left(\left(\forall c \in C \exists t \in T . c(t)\right) \wedge \left(\forall t \in T  \exists c \in C . c(t)\right)\right)$$


Thus, one can come up with a natural partitioning of
the test cases as follows:

Let $C'\subseteq C$, then one can come up with an equivalence class
for a clause $C'$ as
$$E(C')  = \{ d \in D \mid (\forall c \in C' c(d))  \wedge (\forall c' \in C - C'
\neg c(d))\}$$



































