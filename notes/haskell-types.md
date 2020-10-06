Types in Haskell
================

Naively, a type is a set. A "proof" of the type
is an inhabitant of the set. 

Type Classes
------------

A "TypeClass" defines behavior on types. So, a 
monoid typeclass enforces the fact that every 
monoidal strucuture must obey certain properties.
For instance, the List type is, simply put a type.
A "monoid" is not a type. It's a typeclass. An instance
of this typeclass defines "algebraic" operations for the
particular "List" algebraic data type. The abstract mechanism
of talking about strcutures instead of types is  
an algebra. The algebraic data type, along with operations
on the data forms an algebra.

TypeClasses allow us to define operations on a algebraic data
type generically. So, it's an adhoc mechanism for overloading 
functions, as it's performing ad-hoc polymorphism over functions.
An instance of a functor typeclass will capture an algebra of
functors, e.t.c. Typeclasses allow us to overload operations
over data, not the data itself. This is where "type families"
come into the picture.define 


Type Families
-------------

Given a type, allows for ad-hoc polymorphism of the type itself. 
It's a partial function a the type level. Applying the function to
type indices (parameters to the function) leads to a type.
It allows more flexibility in types - No concreteness of concrete 
types and no opaqueness of parametric types.

Typeclasses and families have interface definitions. A type families 
interface definition declares the kind, or number of type indices that the 
type family takes. 

E.g. A case when parametric types aren't enough. 

Consider the type of Lists. For a List of Char, the Cons representation of a
List makes sense. For a list of parenthesis `()`, only the number of elements  
in the list is actually needed. A simple parametric data type cannot be used to
achieve this. We want to be able to programmatically decide a data type based on  
the type parameters. Hence, type families.

// Here's a confusion about Kinds. 

```haskell
class AList a where
  data MyList a :: * 

instance AList Char where

-- We're using the type Family here to define the data type 
  data MyList Char = Empty | Cons (Char) (MyList a)
```





