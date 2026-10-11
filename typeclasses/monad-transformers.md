# Monad Transformers

## 📘 Typeclasses: Monad Transformers & Semigroupal

> _“Monad Transformers simplify working with nested monadic types. Semigroupal enables combining values inside contexts into tuples.”_

These type classes help you manage complexity when dealing with multiple layers of effects or context — such as `Option[List[A]]` or `Either[Error, Option[A]]`.

***

### 🔄 Monad Transformers

#### Definition

> _Higher-kinded types that provide a convenient way to work with nested monadic values._

They “transform” one monad into another, allowing you to sequence operations across multiple contexts without manual unwrapping.

#### Example: `OptionT[List, A]`

A common transformer is `OptionT`, which wraps `List[Option[A]]` and lets you treat it like a single monadic value:

```scala
import cats.data.OptionT
import cats.instances.list._    // Provides Monad[List]
import cats.syntax.flatMap._    // Imports flatMap syntax
import cats.syntax.functor._    // Imports map syntax

val nested: List[Option[Int]] = List(Some(1), None, Some(3))

val optionT = OptionT(nested)  // Wraps List[Option[Int]] into OptionT[List, Int]

val result = optionT.map(_ * 2).value  // List(Some(2), None, Some(6))
```

> ✅ With `OptionT`, you can use `.map` and `.flatMap` directly on the inner `Option` — without manually looping over the `List`.

#### Key Benefit

* Avoids “pyramid of doom” with nested `match` or `flatMap`.
* Enables uniform API for deeply nested structures.
* Works with any monad (e.g., `EitherT`, `ReaderT`, `StateT`).

***

### 🧩 Semigroupal

#### Definition

> _A higher-kinded type class that allows you to combine (tuple) elements inside contexts._

Unlike `Monoid`, which combines values of the same type, `Semigroupal` combines _different_ values inside the same context — producing a tuple.

#### Example: Combining Two Options

```scala
import cats.Semigroupal
import cats.instances.option._   // Provides Semigroupal[Option]

val optionSemi = Semigroupal[Option]

val result = optionSemi.product(Some(123), Some("star"))  // Some((123, "star"))

val failed = optionSemi.product(Some(123), None)         // None
```

> 💡 This is especially useful when you need to combine multiple optional or effectful values — if any is `None`, the whole result becomes `None`.

#### Syntax Sugar (Using `|@|`)

```scala
import cats.syntax.semigroupal._   // Imports |@| operator

val combined = (Some(123) |@| Some("star")).tupled  // Some((123, "star"))
val failed = (Some(123) |@| None).tupled             // None
```

> ✅ The `|@|` operator is syntactic sugar for `product`, making code more readable.

***

### 📌 Why Use These?

| Concept                | Use Case                                                                    |
| ---------------------- | --------------------------------------------------------------------------- |
| **Monad Transformers** | Simplify nested monads (e.g., `OptionT[List, A]`) — avoid manual unwrapping |
| **Semigroupal**        | Combine independent values in context (e.g., validate two fields together)  |

***

### 🧪 Try It Yourself

```scala
import cats.Semigroupal
import cats.instances.option._
import cats.syntax.semigroupal._

// Combine two options
val user = Some("Alice") |@| Some(30) |@| Some(true)
val userInfo = user.tupled  // Some(("Alice", 30, true))

// Monad Transformer: OptionT over List
import cats.data.OptionT
val listOpt = OptionT(List(Some(1), Some(2), None))
val doubled = listOpt.map(_ * 2).value  // List(Some(2), Some(4), None)
```
