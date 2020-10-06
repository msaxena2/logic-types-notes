---
title: Dimensions of Object Oriented Language Design
subtitle: Notes on OO Language Design
author: Peter Wagner
header-includes: |
  \include{commands}
---

Synopsis
-------

The dimension considered in this paper are:

 1. Objects
 2. Classes
 3. Inheritence
 4. Strong typing
 5. Concurrency
 6. Persistence


Objects, Classes and Inheritence
--------------------------------

Objects are autonomous entities with state.
Unlike functions, which for each input
return the same output, objects can behave
differently as they evolve over time.

A language is **object oriented** if it supports
objects. Objects are a neccesity but not sufficient
on their own to call a language objected oriented.

An object-based language is **object oriented** if objects
belong to classes and class hierarchies can be defined
via inheritence.

That is, **Object Oriented = Objects + Classes + Inheritence**

### Class

A class is a template (like a cookie cutter) for creating new objects.

### Inheritence

A mechanism for sharing resources among classes.


Note that the class of *Object Oriented (OO)* languages is
stricter than class of *object based (OB)* languages. That is,
a OO language is an OB language, but not vice versa.


### Classical Language

An OB language is classical or Class Based (CB) if every object has a class.

 - CB languages are a subset of OB languages.
 - OO languages are a subset of CB languages.


Data Abstraction and Strong Typing
----------------------------------

**Data Abstraction** is an object whose state is
only visible through operation.

**Strong Typing** A language is strongly typed
if type compatability of all expressions can be
determined at compile time.

The class of **Strong Typed Object Oriented Languages (STOO)** is
narrower than **OO**. Note *STOO requires support for abstraction*.
Although, both strong typing and abstraction are related concepts
that promote modularity, it is possible for an OO language to
be strongly typed but not support data abstraction.

The accepted notion is that *Strongly Typed Object Oriented Languages*
should be the norm of


Consistency and Orthogonality
-----------------------------

A collection of language features is said
to be **consistent** if there exists a model
of a language that realizes them. In other
words, if the features can coexist in a
language, then they're consistent.
The five features of STOO
(objects, classes, inheritence, abstraction, strong typing)
languages are consistent (shown by multiple languages).

For a collection of language features is said
to be **orthogonal** if for every subset,
there is a language that contains the subset
features but not features in the complementary
subset

Inheritence
-----------

The paper makes an attempt to orthogonalize
inheritence from classes and objects. That is,
inheritence is a special case of delegation,
which is orthogonal to objects and classes.
This is
obtained via a concept called *delegation*

Delegation
----------

Allows *the delegation* of performing of an
operation to an ancestor. A key feature
here is that the self reference in the
ancestor object is dynamically set to the
delegating object. This dynamic binding
of the self reference in the ancestor to
the delegator makes the ancestor share
identity with the delegator, which is
key feature of inheritence.

*Delegation* is a form of resource sharing
that allows shared resources to be viewed
as belonging to the entity on behalf of which
they are executed. Inheritence is a special
case of delegation thus are considered in the
same dimension as delegation.


Abstration
----------

Abstraction is a mechanism by which
an interface for an entity dictates accesses
to the entity by other entities.


**Four orthogonal (independent) dimensions of strongly
typed OO languages**:

 1. Objects
 2. Abstraction
 3. Delegation
 4. Types

In other words, it's possible to have languages
that contain any subset of the above (and none from
the complement). But those languages are not *OO*.


Design Alternatives for Delegation
----------------------------------

 1. classless delegation v/s inheritence:
    Deligation in an instance heirarchy (like decorators)
    in classless delegation v/s delegation via class hierarchy
    in inheritence

 2. Stict v/s Non strict:
    Behavioral compatiblity with ancestors enforced v/s not enforced.
    In strict, the realtion is "is-a" while in Non strict it's
    "like"

 3. Single v/s Multiple
    There is no concensus here - some languages support one or the other
    or a mixture.

 4. Abstract Interface vs Code Sharing


Interesting combinations of Orthogonal Features
-----------------------------------------------

### Classless object-based languages without delegation

#### Actors

**Actor = Objects + Abstraction + Concurrency - Classes - Inheritence - Typing**

Actors are objects with an address and a behavior.
The address has a unbounded buffer for storing linear
sequence of messages (communications). The actor is
defined in terms of responses to communications.

An actor's state is independent of its successors.
It can designate a successor (same address) as
as a parent to process an incoming communication
to the actor.

### Classless object-based languages with delegation

#### Prototypes

A *prototype* is both an object and a template.
Other objects may delegate responsibility to
the prototype, and it will provide some default
implementation of intended functionality.

An example is Javascript, where instead of classes,
one can directly create objects that serve as prototypes
of other objects. This allows for delegation (obviously
not via inheritence)

Example:

```.js

  var foo = {one: 1};

  // Sets prototype of bar as foo
  var bar = Object.create(foo);
  bar.two = "cat";


  bar.one; // 1
  bar.two; // cat
```

Object Based Concurrency
------------------------

*Process based languages* are object
based languages that support concurrent
execution of objects called processes.

Object based languages are constrained to only allow
sequential execution of objects.


A **Thread** is an object with a
thread control block - locus of control and stack
that represent's the thread's state.

Processes can be:

 - Sequential - single thread of control
 - Quasi-concurrent - At most one active thread of control
 - Concurrent - Mutiple active threads of control


Persistent Languages
--------------------

Object based languages with persistency
and concurrency can serve as persistent
languages.

Conclusion
----------

Thus in total, 6 orthogonal dimensions are recognized
in Objected Oriented Language design. They are:
 1. Objects
 2. Types
 3. Delegation
 4. Abstration
 5. Concurrency
 6. Persistence


Based on these, one can come with different classes:

 1. Object based have entities with state
 2. Object Oriented have classes, abstract and inheritence (delegation).
 3. Classless with delegation - Prototypes
 4. Classless Object based with delegation and concurrent - actors
 5. Classless Object based with concurrency and persistency - databases

