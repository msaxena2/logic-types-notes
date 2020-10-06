Category Theory Notes
=====================

Functors
--------

In the category hask, types are objects, and arrows are
functions between the types. Functors take objects and
arrows between categories. So, given a type a , f a, where `f`
is a functor takes a to a different category. A nuance is
that `f (a -> b)` is not the same as `f a -> f b`. `f (a -> b)
can mostly be thought of as the primitive function type, also known as
an exponential object in the category, fed to the
functor, while f a -> f b is the arrow obtained by feeding arrow (a -> b)
to the functor. An example is the function type `a -> b -> c`, which is an
arrow that takes an `a`, and return an "exponential object" `b -> c`.


Applicatives
------------

Primitives -

Monoids - Two morphisms for every object

Associativity
