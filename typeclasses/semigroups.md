# Semigroups

## 📘 Typeclasses: Semigroups

### 🔹 Definition

A **Semigroup** is a type class that defines how to combine two values of the same type. It provides an associative binary operation (often denoted as `combine` or `+`).

> _“A semigroup is a set equipped with an associative binary operation.”_

***

### 🧩 Basic Usage in Scala (with Cats)

#### 1. Import Required Components

```scala
import cats.Semigroup
import cats.instances.int._      // Provides Semigroup instance for Int
```

#### 2. Explicit Use of Semigroup

```scala
val intSemigroup = Semigroup[Int]
val result = intSemigroup.combine(2, 45)  // Result: 47
```

#### 3. Syntax Sugar (Using Operator `|+|`)

```scala
import cats.syntax.semigroup._   // Imports the |+| operator

val anResult = 2 |+| 45         // Result: 47
```

> ✅ The `|+|` operator is syntactic sugar for `combine`, making code more readable and concise.

***

### 🛠️ General Utility: Generic Reduction Function

You can write generic functions that work with any type `T` that has a `Semigroup` instance:

```scala
def reduceList[T: Semigroup](list: List[T]): T =
  list.reduce(_ |+| _)
```

#### Example Usage:

```scala
val numbers = List(1, 2, 3, 4)
val sum = reduceList(numbers)  // Result: 10

val strings = List("Hello", ", ", "World")
val concatenated = reduceList(strings)  // Result: "Hello, World"
```

> 💡 This function works for any type `T` that implements `Semigroup` — including `Int`, `String`, `List`, `Option`, etc. — as long as there’s an implicit `Semigroup[T]` available.

***

### 📌 Why Use Semigroups?

* **Abstraction**: Write generic code that works across many types.
* **Composability**: Combine values in a consistent, predictable way.
* **Extensibility**: Add `Semigroup` instances for your own custom types.
* **API Design**: Useful for building flexible, reusable libraries (e.g., logging, aggregations, monoids).

***

### ⚠️ Note: Semigroup vs Monoid

* A **Semigroup** only requires `combine(a, b): T`.
* A **Monoid** extends Semigroup and adds an identity element (`empty: T`) such that `a |+| empty == a`.

> You’ll often see `Monoid` used in practice because it allows reducing empty lists (via `fold`), but `Semigroup` is sufficient when you know the collection is non-empty.

***

### 🧪 Try It Yourself

```scala
import cats.Semigroup
import cats.instances.string._
import cats.syntax.semigroup._

val greeting = "Hello" |+| " World"     // "Hello World"
val total = List(10, 20, 30).reduce(_ |+| _)  // 60
```
