Notes on Simply Typed Lambda Calc
=================================

Given  `0 |- \ x : o . \ f : o -> o -> o -> o . f x x : ?`


Note that f is a lambda variable bound by the inner lambda. So the lambda can be
thought of as a function that accepts a function.

`f x x` must have the type `o -> o`. `\x . \f .  f x x` is therefore going to
have the type - `o -> (o -> o -> o -> o) -> (o -> o)`.

`0 |- \ x : o . \ f : o -> o -> o -> o . f x x : o -> (o -> o -> o -> o) -> (o -> o)`

Lambda abstraction proof rule, we get -
  - { x : o } |- \f : o -> o -> o -> o . f x x : (o -> o -> o -> o) -> (o -> o)

Lambda abstraction again, we get -
  - { x : o, f : o -> o -> o -> o } |- f x x : ( o -> o )

Using application proof rule, -
```

   Subproof                 Axiom
  -----------------------   ---------- App
 - X |- f x : o -> o -> o & X |- x : o
```

The subproof can easily be proved using the application proof rule -
```
    Axiom                     Axiom
    ------------------------- -----------
    X |- f : o -> o -> o -> o  X |- x : o
    ------------------------------------- App
  - X |- f x : o -> o -> o
```
