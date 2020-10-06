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




