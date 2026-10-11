# Monads

## 📘 Typeclasses: Monads

> _“A Monad is a higher-kinded type class that provides two core operations: `pure` (to wrap values) and `flatMap` (to sequence computations).”_

Monads allow you to chain operations that may fail, produce side effects, or carry context — while keeping your code flat, readable, and composable. They’re the backbone of functional pipelines in Scala (e.g., `Option`, `List`, `Future`, `Either`).

***

### 🧩 Core Operations

#### 1. `pure` — Wrap a value into a monadic context

```scala
val wrapped = Monad[Option].pure(2)  // Some(2)
```

#### 2. `flatMap` — Transform and flatten nested contexts

```scala
val result = Monad[Option].flatMap(Some(2)) { x =>
  if (x % 2 == 0) Some(x + 1) else None
}  // Some(3)
```

> 💡 `flatMap` lets you sequence operations where each step may return a new monadic value — and automatically “flattens” the result.

***

### 🛠️ Basic Usage in Scala (with Cats)

#### 1. Import Required Components

```scala
import cats.Monad
import cats.instances.option._   // Provides Monad instance for Option
```

#### 2. Explicit Use of Monad

```scala
val optMonad = Monad[Option]
val anOption = optMonad.pure(2)  // Some(2)

val result = optMonad.flatMap(anOption) { x =>
  if (x % 2 == 0) Some(x + 1) else None
}  // Some(3)
```

***

### 🔄 Monads Extend Functors

Every `Monad` is also a `Functor`, so you can always use `.map`:

```scala
val doubled = anOption.map(_ * 2)  // Some(4)
```

You can even implement `map` in terms of `pure` and `flatMap`:

```scala
def map[F[_], A, B](fa: F[A])(f: A => B)(implicit M: Monad[F]): F[B] =
  M.flatMap(fa)(a => M.pure(f(a)))
```

***

### 🧪 Syntax Sugar (Using `for` comprehensions or `.flatMap`)

```scala
import cats.syntax.flatMap._   // Imports .flatMap
import cats.syntax.functor._   // Imports .map
import cats.syntax.applicative._ // Imports .pure

val result = for {
  x <- Some(2)
  y <- if (x % 2 == 0) Some(x + 1) else None
} yield y  // Some(3)
```

Or using `.flatMap` directly:

```scala
val result = Some(2).flatMap { x =>
  if (x % 2 == 0) Some(x + 1) else None
}
```
