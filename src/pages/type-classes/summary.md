## Summary

In this chapter we took a first look at type classes.
We implemented our own `Printable` type class using plain Scala
before looking at two examples from Cats---`Show` and `Eq`.

We saw the components that make up a type class:

- A `trait`, which is the type class

- Type class instances, which are implicit values.

- Type class usage, which uses implicit parameters.


We have also seen the general patterns in Cats type classes:

 - The type classes themselves are generic traits
   in the [`cats`][cats.package] package.

 - Each type class has a companion object with,
   an `apply` method for materializing instances,
   one or more *construction* methods for creating instances,
   and a collection of other relevant helper methods.

 - Default instances are provided via objects
   in the [`cats.instances`][cats.instances] package,
   and are organized by parameter type rather than by type class.

 - Many type classes have *syntax*
   provided via the [`cats.syntax`][cats.syntax] package.

In the remaining chapters of Part I
we will look at several broad and powerful type classes---`Semigroup`,
`Monoid`, `Functor`, `Monad`, `Semigroupal`, `Applicative`, `Traverse`, and more.
In each case we will learn what functionality the type class provides,
the formal rules it follows, and how it is implemented in Cats.
Many of these type classes are more abstract than `Show` or `Eq`.
While this makes them harder to learn,
it makes them far more useful for solving general problems in our code.


[Implicit parameters: dynamic scoping with static types][implicits] introduces implicit parameters, which provided inspiration for Scala 2's implicit parameters and evolved into Scala 3's given instances and using clauses. It's interesting to see the differences between implicit parameters as proposed here and how they are implemented in Scala.

[implicits]: https://dl.acm.org/doi/abs/10.1145/325694.325708
