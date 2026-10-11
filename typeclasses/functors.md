# Functors

## 📘 Typeclasses: Functors

> _“A Functor is a higher-kinded type class that provides a `map` method to transform values inside a context.”_

Functors allow you to apply a function to the contents of a container (like `List`, `Option`, `Tree`, etc.) without changing the structure itself. This enables **uniform transformation** across different data types using the same high-level API.

***

### 🧩 Basic Usage in Scala (with Cats)

#### 1. Import Required Components

```scala
import cats.Functor
import cats.instances.list._     // Provides Functor instance for List
```

#### 2. Explicit Use of Functor

```scala
val listFunctor = Functor[List]
val incrNum = listFunctor.map(List(1, 2, 3))(_ + 1)  // Result: List(2, 3, 4)
```

#### 3. Syntax Sugar (Using `.map`)

```scala
import cats.syntax.functor._   // Imports .map syntax for any Functor

val tree: Tree[Int] = Branch(40, Leaf(10), Leaf(20))
val processedTree = tree.map(_ + 1)  // Applies +1 to all nodes
```

> ✅ The `.map` syntax works on any type that has a `Functor` instance — including custom types like `Tree`.

***

### 🛠️ General Utility: Generic Transformation Function

You can write generic functions that work with any type `F[_]` that implements `Functor`:

```scala
def do10x[F[_]: Functor](container: F[Int]): F[Int] =
  container.map(_ * 10)
```

#### Example Usage:

```scala
val numbers = List(1, 2, 3)
val times10 = do10x(numbers)  // Result: List(10, 20, 30)

val maybeNumber = Some(5)
val maybeTimes10 = do10x(maybeNumber)  // Result: Some(50)

val tree: Tree[Int] = Branch(1, Leaf(2), Leaf(3))
val treeTimes10 = do10x(tree)  // Applies *10 to all nodes
```

> 💡 This function works for any `F[Int]` where `F` has a `Functor` instance — e.g., `List`, `Option`, `Either`, `Tree`, etc.

***

### 📌 Why Use Functors?

* **Abstraction**: Write code that works uniformly across containers.
* **Composability**: Chain transformations cleanly (`map(...).map(...)`).
* **Extensibility**: Define `Functor` instances for your own data types.
* **API Design**: Essential for functional pipelines, data processing, and domain modeling.

***

### 🧪 Try It Yourself

```scala
import cats.Functor
import cats.instances.option._
import cats.syntax.functor._

val opt = Some(7)
val doubled = opt.map(_ * 2)  // Some(14)

val emptyOpt = None
val stillEmpty = emptyOpt.map(_ * 2)  // None (structure preserved)

val list = List(1, 2, 3)
val incremented = list.map(_ + 1)  // List(2, 3, 4)
```
