# Foldable

## 📘 Typeclasses: Foldable

> _“Foldable is a higher-kinded type class that provides folding methods — allowing you to reduce any container (like List, Option, Tree) into a single value.”_

`Foldable` abstracts over data structures that can be “folded” — meaning you can traverse them and accumulate a result. It gives you powerful, generic functions like `foldLeft`, `foldRight`, `combineAll`, and `toList`.

***

### 🧩 Definition

A `Foldable[F[_]]` provides at least:

* **`foldLeft`** — Reduce from left to right.
* **`foldRight`** — Reduce from right to left (stack-safe via `Eval`).

```scala
trait Foldable[F[_]] {
  def foldLeft[A, B](fa: F[A], b: B)(f: (B, A) => B): B
  def foldRight[A, B](fa: F[A], lb: Eval[B])(f: (A, Eval[B]) => Eval[B]): Eval[B]
}
```

> ✅ In Cats, `Foldable` also provides convenience methods like:
>
> * `combineAll` — Combine all elements using a `Monoid`.
> * `toList` — Convert to `List`.
> * `find`, `exists`, `forall`, etc.

***

### ⚖️ foldLeft vs foldRight

| Method      | Direction    | Stack Safety        | Use Case                        |
| ----------- | ------------ | ------------------- | ------------------------------- |
| `foldLeft`  | Left → Right | ✅ Safe              | Most common reductions          |
| `foldRight` | Right → Left | ✅ Safe (via `Eval`) | For lazy or infinite structures |

> 💡 `foldRight` uses `cats.Eval` to ensure stack safety — even for large or infinite structures.

***

### 🛠️ Basic Usage in Scala (with Cats)

#### 1. Import Required Components

```scala
import cats.Foldable
import cats.instances.list._     // Provides Foldable instance for List
import cats.syntax.foldable._    // Imports .foldLeft, .foldRight, .combineAll, etc.
```

#### 2. Explicit Use of Foldable

```scala
val listFoldable = Foldable[List]

val sum = listFoldable.foldLeft(List(1, 2, 3), 0)(_ + _)  // 6

val product = listFoldable.foldRight(List(1, 2, 3), Eval.now(1)) { (a, acc) =>
  acc.map(a * _)
}.value  // 6
```

#### 3. Syntax Sugar (Using `.foldLeft`, `.foldRight`, `.combineAll`)

```scala
val numbers = List(1, 2, 3)

val sum = numbers.foldLeft(0)(_ + _)        // 6
val product = numbers.foldRight(Eval.now(1))((a, acc) => acc.map(a * _)).value  // 6

// Using combineAll (requires Monoid[Int])
val total = numbers.combineAll  // 6
```

***

### 🧪 Convenience Methods

#### `combineAll` — Summarize with Monoid

```scala
import cats.Monoid
import cats.instances.int._

val sumAll = List(1, 2, 3).combineAll  // 6
```

#### `map` + `fold` — Transform then reduce

```scala
val mappedConcat = List(1, 2, 3).map(_.toString).foldLeft("")(_ + _)  // "123"
// Or using syntax:
val mappedConcat2 = List(1, 2, 3).foldMap(_.toString)  // "123"
```

> ✅ `foldMap` is a shortcut: `foldMap(f) == foldLeft(...)(... map f ...)`.

***

### 📌 Why Use Foldable?

* **Abstraction**: Write code that works on any container — `List`, `Option`, `Tree`, `Vector`, etc.
* **Composability**: Chain folds with maps, filters, etc.
* **Extensibility**: Define `Foldable` instances for your own data types.
* **API Design**: Essential for aggregations, summaries, and data processing pipelines.
