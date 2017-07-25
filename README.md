Gridstone Kotlin Style Guide
============================

A style guide for developers at Gridstone who write in Kotlin.

Naming
------

- Variable names are `camelCase`
- Type names are `CaptialCamelCase`
- Function names are `camelCase`
- Constants are `CAPITAL_SNAKE_CASE`
- Enum entries are `CAPITAL_SNAKE_CASE`

```kotlin
const val GREETING = "Hello, world!"

class Foo {
  private val secret = "shhh"

  fun greetTheWorld() {
    println(GREETING)
  }
}

enum Bar { FIRST_BAR, SECOND_BAR }
```

Formatting
----------

### Line length

Lines may be no more than 100 characters in length.
- Preserves readability for individual lines
- Ensures that two files can be displayed side-by-side on most displays

### Indentation

Use 2 space characters for indentation. Use 4 spaces for continuation indents.

```kotlin
fun foo() {
  val bar = Observable.just(1)
      .map { it.toString() }
}
```

- Using spaces instead of tabs means code must look good regardless of tab-length settings
- Using 2 spaces helps maximise the use of our limited horizontal real estate

### Aligning parameters

When parameters of functions or classes extends into multiple lines those parameters must be
aligned.

```kotlin
data class Foo(val bar1: Int,
               val bar2: String,
               val bar3: Float)

fun bar(baz1: Int,
        baz2: String,
        baz3: Float) {
  // ...
}

val foo = bar(baz1,
              baz2,
              baz3)
```
It's also acceptable to align parameters as
```kotlin
fun foo(bar1, bar2
        bar3, bar4)
```

- Alignment is especially useful for data classes which may have many properties

### Aligning invocations

When methods are invoked in a chain they must be aligned.
```kotlin
// OK
val foo: List<String> = list
    .filter {
      it > 10
    }
    .map {
      it.toString()
    }

// Not OK
val bar: List<String> = list.filter {
    it > 10
}.map {
    it.toString()
}
```

### Short things

Short methods and classes may be declared on a single line.

```kotlin
data class Foo(val foo1: Int, val foo2: String)

fun bar(): String = "What would you like to drink?"
```

### Colons

Use a space either side of a colon when declaring inheritance. Use only a space after a colon
when declaring a variable/method type.
```kotlin
interface Foo<out T : Any> : Bar {
  fun foo(a: Int): T
}
```

### Blank lines
Lines that are intentionally blank for formatting purposes should contain no characters at all.

Type Inference
--------------

Type inference is powerful but can be easily abused. As a general rule, type inference should
only be used when it's explicitly clear what the resulting type will be.

```kotlin
// OK
fun foo() = "Hello."
val bar = Bar()

// Not OK
val baz = generateBaz()
```

Under any other the circumstances the type must be explicitly declared, even if it's not
necessary for the compiler.

Lambdas
-------

### Avoiding parenthesis

Whenever possible lambdas should be placed outside of parentheses.
```kotlin
// OK
list.filter { it > 10 }
// Not OK
list.filter({ it > 10 })
```

### Using `it`

The implicit `it` parameter given to lambdas should be used judiciously. Unless a lambda is very
short, consider overriding `it` with a more meaningful name.
```kotlin
// Specifying the lambda parameter of `user` rather than `it` helps this code.
webServices.getUsers()
    .flatMapIterable { it }
    .map { user ->
      when {
        user.isAdmin() -> Admin(user)
        user.isManager() -> Manager(user)
      }
    }
```

### Unused parameters

Unused lambda parameters should be replaced with an underscore.
```kotlin
foo { _, _, bar -> println(bar) }
```

Control Flow
------------

### Prefer an early exit

Whenever possible return from a method rather than creating additional levels of indentation with
conditions.
```kotlin
// OK
fun foo(bar: Int) {
  if (bar < 3) return

  // Do other stuff.
}

// Not OK
fun foo(bar :Int) {
  if (bar > ) {
    // Do other stuff.
  }
}
```

