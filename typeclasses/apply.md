# Apply

## 📘 Typeclasses: Apply

> _“Apply is a weaker version of Applicative — it provides `ap` (apply) and `product`, but not `pure`. It sits between Functor and Applicative.”_

While `Applicative` gives you full power to lift values (`pure`) and apply functions inside context (`ap`), `Apply` only gives you `ap` and `product` — making it ideal for types that can combine values but don’t have a meaningful “empty” or “default” value.

***

### 🧩 Definition

An `Apply` type class extends `Functor` and `Semigroupal`, and adds:

* **`ap`** — Apply a function inside a context to a value inside a context.
* **`product`** — Combine two values inside contexts into a tuple (inherited from `Semigroupal`).

```scala
trait Apply[F[_]] extends Functor[F] with Semigroupal[F] {
  def ap[A, B](ff: F[A => B])(fa: F[A]): F[B]
}
```

> ✅ So:\
> **Apply = Functor + Semigroupal + ap**

Unlike `Applicative`, `Apply` does **not** require `pure`.

***

### 🛠️ Basic Usage in Scala (with Cats)

#### 1. Import Required Components

```scala
import cats.Apply
import cats.instances.option._   // Provides Apply instance for Option
```

#### 2. Explicit Use of Apply

```scala
val applyOption = Apply[Option]

val funcApp = applyOption.ap(Some((x: Int) => x + 1))(Some(2))  // Some(3)
```

#### 3. Syntax Sugar (Using `.ap` and `.mapN`)

```scala
import cats.syntax.apply._   // Imports |@| and .mapN

val tupleOfOpts = (Option(1), Option(2), Option(3))

val optionOfTuple = tupleOfOpts.tupled    // Some((1, 2, 3))
val sumOpt = tupleOfOpts.mapN(_ + _ + _)  // Some(6)
```

> 💡 The `.tupled` and `.mapN` methods come from `Semigroupal` (which `Apply` extends), and are very convenient for combining multiple optional or effectful values.

***

### ⚖️ Apply vs Applicative

| Feature   | Apply                            | Applicative                  |
| --------- | -------------------------------- | ---------------------------- |
| `pure`    | ❌ Not required                   | ✅ Required                   |
| `ap`      | ✅ Available                      | ✅ Available                  |
| `product` | ✅ Available                      | ✅ Available                  |
| Use Case  | Combining values without lifting | Lifting + applying functions |
| Example   | `Validated`, `NonEmptyList`      | `List`, `Option`, `Either`   |

> 📌 In Cats:
>
> * `Applicative` extends `Apply`.
> * Many types (like `Validated`) implement `Apply` but not `Applicative` — because they can’t define `pure` meaningfully (or don’t need to).
