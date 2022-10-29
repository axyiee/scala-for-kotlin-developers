# Scala for Kotlin developers

This article is a pretty basic introduction to Scala for Kotlin developers, addressing the most frequently used Scala **3**
features, alongside a comparison with their Kotlin counterparts. This will assume some familiarity with Kotlin, but not with
Scala and this will be also updated over time to cover more features and use cases.

I highly recommend reading the [official documentation](https://docs.scala-lang.org/scala3/book/introduction.html) for a
more in-depth explanation of the language.

## Table of contents

1. [Why Scala?](#why-scala)
1. [Feature comparison](#feature-comparison)
1. [Scala basics](#scala-basics)
1. [Scala's type system](#scalas-type-system)
1. [Solid concepts](#solid-concepts)

## Why Scala?

Scala is a modern, general-purpose multi-paradigm programming language designed to be concise, expressive, and type-safe.
The principles of modern Scala have been highly influenced by the functional programming language paradigm. Just like Kotlin,
Scala compiles to many platforms, including the JVM, JavaScript, and native code.

The reason why Scala is a better choice than Kotlin for most use cases is that Scala has a much more powerful type system,
a better step into functional programming than Kotlin, a powerful macro system, and has better LSP support, so you don't need
a JetBrains IDE to use it properly!

## Feature comparison

### Standard library (stdlib)

While using Scala, you will find out that most of the time you don't need Java's stdlib directly, even though Scala has complete
Java interoperability. In contrast, Kotlin's stdlib is very small, and you will need to use Java's stdlib directly most of the time,
and almost everything is syntax sugar for Java's stdlib. That's not a big deal though, since the most important thing is providing
code flexibility and safety, and Kotlin does that very well.

### Functional programming

Both of these languages are multi-paradigm, but Kotlin is more object-oriented than Scala, and Scala is more functional than Kotlin.
Scala has a much more powerful type system, has built-in curried function syntax, and it's much easier to work with functional
programming concepts in Scala than in Kotlin.

Some good examples to show this are:

- [Collections](#collections) - shown later in this article. You can reimplement an algorithm that replicates built-in bevahiour using
  functional programming concepts in a concise way.
- Curried functions - Scala has a lot of curried functions built into the core standard library, such as `foldLeft` and `foldRight`.
  Kotlin can do this too, but its syntax is much more verbose and is going to be a lot more difficult to read since it is a callback
  hell.
- Ecosystem - Scala has a lot of libraries that are built around functional programming concepts, which integrate better with the language
  itself than Kotlin does.

### Type system

Scala is pretty similar to Kotlin in terms of type system, but it has the advantages of having intersection types (merging multiple
types into one, requiring all of them to be satisfied), union types (a type that can be one of the multiple types), algebraic data types
(represented as enumerations similar to Rust's), a powerful type parameter (often referred as generics as well) variance system,
opaque types, structural types, and more. Kotlin has a very powerful type system as well, but it's not as powerful as Scala's.

### Collections

Scala has a very powerful collection library, some of which you can also find in Kotlin. Scala has more collection types than
Kotlin and has the advantage of being able to "explain" the collection structure itself in more easily and concisely way using simple
built-in features. For example, to explain a List we could say a List is a:

```scala
enum List[+A] {
    case Nil
    case Cons(head: A, tail: List[A])
}
```

### Lightweight threads (coroutines)

Kotlin has a very built-in powerful coroutine system, often referred to as lightweight threads. You can control the code execution using
continuations. In other words, you can suspend the execution of a coroutine, and resume it later, a powerful asynchronous and concurrency
tool.

Scala itself has Futures: a high-level and idiomatic approach to asynchronous programming and concurrency. It is also a powerful tool, but
not as powerful and straightforward as Kotlin's coroutines. Looking further, Scala has many open-source solutions created by the community,
most of them focusing on a functional approach. For example, [Cats Effect](https://typelevel.org/cats-effect/) which similarly to Kotlin has
a concept of fibers: a lightweight thread that can be suspended and resumed.

### Value classes and inline functions

Inline functions, parameters, and classes are powerful solutions to reduce the overhead of the JVM. Kotlin has a very powerful inline system,
which is also available in Scala just by using the `inline` keyword behind a function or function parameter. However, Kotlin's value classes
system is more straightforward to use than Scala's, since Kotlin's value classes are just a wrapper around a primitive type.

You can do the same thing in Scala by using an opaque type and creating extensions around it. You can make this process as simple as the Kotlin
approach by using a helper like [newtypes](https://github.com/monix/newtypes).

## Scala basics

### Variables

The variable definition syntax in Scala is identical to Kotlin. Immutable variables are defined using the `val` keyword, and mutable variables
are defined using the `var` keyword, followed by the variable name and the type preceded by a colon. The type can be omitted if
the compiler can infer it. A value can be assigned to a variable using the `=` operator succeeding a value of the same type.

```scala
val a: Int = 1
a = 5 // Error: reassignment to val
var b: Int = 2
b = 3 // OK
b = "" // Error: type mismatch
val a = 5 // Type is inferred
```

Kotlin has lazy-loading variables, which are variables that are initialized only when they are used for the first time. It is not
a built-in feature though (it is a delegated property), but it is not a big deal. Scala has a built-in lazy-loading variable
feature, which is defined using the `lazy` keyword before the `var` or `val` keyword.

```scala
lazy val a: Int = 5
```

In a way, variables in Scala can also be declared in a way similar to Rust code blocks. For example, the following code is valid in
Scala:

```scala
val a = {
    val b = 5
    b + 1
}
```

### Functions (methods)

Scala's functions can be named or anonymous. Named functions are defined using the `def` keyword, followed by the function name,
the parameters, the return type preceded by a colon, and the function body preceded by an equal sign. The return type can be omitted
if the compiler can infer it. The parameters can be omitted if the function has no parameters. The function body can be omitted if
You can wrap the function body in curly braces if you want to write multiple lines of code, but it is not necessary if you only
have one line of code. You can also omit the `return` keyword if you are returning a value as the last line of code.

Also, it is worth mentioning that by omitting function parameters instead of letting it as `()`, the function call cannot have any parameters
either, nor have parentheses after the call, similarly to Kotlin's getter variable (`val x: Int get() = 5`).

```scala
def add(a: Int, b: Int): Int = a + b

def add(a: Int, b: Int): Int = {
    a + b
}

def x(): Int = 5 // can be called as x()

def z: Int = 5 // can be called as z, not as z()
```

To declare the main function; the entry point of a Scala program, you can use the `@main` annotation, which is a built-in annotation
that is available in Scala 3. The function name can be the bytecode entry point class name.

```scala
@main def App(): Unit =
    println("Hello world!")

// you can also do
@main def App(args: Array[String]): Unit =
    println("Hello world!")
```

Scala's function and variables also don't need to be defined in a specific order, as long as they are defined before they are used, and
they can be defined as top-level as well, meaning that they can be defined outside a class, or object, or trait.

String interpolation can be done in Scala by using the `s` keyword before the string, and then using the `$` symbol before the variable
name. You can also use the `${}` syntax to execute code inside the string.

```scala
val name = "John"
val x = s"Hello, $name!"
val y = s"Hello, ${name.toUpperCase}!"
```

Functions or methods in Scala can also be named symbolic, which means that they can be defined using symbols instead of letters:

```scala
extension [A](rcv: A)
    def |>[B](f: A => B): B = f(rcv)

// List(10, 20, 40) |> sum |> println
```

Anonymous functions, or lambdas, are defined using the `=>` operator, which is similar to Kotlin's `->` operator. You can think of
JavaScript's arrow functions as well:

```scala
val add = (a: Int, b: Int) => a + b
val sub: (Int, Int) => Int = (a, b) => a - b
```

Anonymous functions passed as a parameter can also be shortened using the `_` symbol, which is similar to Kotlin's `it` keyword:

```scala
List(1, 2, 3).map(_ + 1)
// kotlin counterpart: List(1, 2, 3).map { it + 1 }
```

### Classes and abstractions

Abstractions in Scala don't need to be wrapped in curly braces! You can simply use a colon (`:`) after the class name or constructor,
just like in an indent-based language like Python, it is up to you to decide which code style you prefer. Instancing a class in Scala
is done just like in Kotlin.

Scala's classes are very similar to Kotlin's classes. They are defined using the `class` keyword, followed by the class name, the
constructor parameters wrapped in parentheses, and the class body. The constructor parameters can be omitted if the class has no
constructor parameters. To extend another class or trait, you can use the `extends` keyword before the class body, followed by the
class or trait name. If you want to extend multiple classes or traits, you can separate them using commas.

Scala also has the concept of data classes, which are called case classes in Scala. They are classes that are used to store data,
and they are defined using the `case class` keyword, and all of their parameters are automatically defined as `val` variables by
default.

Companion objects in Scala can be declared by creating an `object` with the same name as the class. Companion objects are objects
that are associated with a class, and they can be used to define static methods and variables, as well as to define factory methods
for the class. Companion objects can also be used to define extension methods for the class.

Traits in Scala are similar to interfaces in Kotlin. They are defined using the `trait` keyword instead of `class`, and can also
have a constructor if you want!

```scala
trait Human {
    val name: String

    def sayHello(): Unit = {
        println(s"Hello, my name is $name")
    }
}

class Person(val name: String) extends Human {
    override def sayHello(): Unit = {
        println(s"Hello, my name is $name")
    }
}
```

### Algebraic Data Types (ADT or enumerations)

You can think of Scala ADTs as a mix of Kotlin and Rust concepts. They are defined using the `enum` keyword, followed by the
enum name, and the enum body. The enum body can be empty, or it can contain a list of enum values, separated by commas and preceded
by the `case` keyword. The enum values can also have parameters, and they can be defined as `val` or `var` variables. The enum
values can also have an individual constructor.

```scala
enum Color {
    case Red, Green, Blue
}

// can also be defined as
// enum Color:
//    case Red, Green, Blue

enum Color(val hex: String) {
    case Red("#FF0000"), Green("#00FF00"), Blue("#0000FF")
}

// or
enum Color(val hex: String) {
    case Red extends Color("#FF0000")
    case Green extends Color("#00FF00")
    case Blue extends Color("#0000FF")
}
//

enum IpAddress {
    case Ipv4(a: Int, b: Int, c: Int, d: Int)
    case Ipv6(a: Int, b: Int, c: Int, d: Int, e: Int, f: Int, g: Int, h: Int)
}

// IpAddress.Ipv4(127, 0, 0, 1)
```

### Extension functions

Extension functions in Scala are defined inside an extension scope. An extension scope is defined using the `extension` keyword,
followed by a parameter wrapped in parentheses containing the type that extension function will be applied, and the extension
scope body. For example:

```scala
extension (x: Int)
    def isEven: Boolean = x % 2 == 0

// you can do multiple as well
extension (x: Int) {
    def isOdd: Boolean = x % 2 != 0
    def isEven: Boolean = x % 2 == 0
}
```

### Pattern matching

You may also notice that Scala has a very similar pattern-matching syntax to Rust. Pattern matching is a way to match a value
against a pattern, and it can be used to destructure a value or to match a value against a list of patterns. Pattern matching
can be done by inserting the value reference (variable or expression) followed by the `match` keyword, then the pattern-matching
body. The pattern-matching body can be defined using the `case` keyword, followed by the pattern, and the pattern body. The pattern
body can be defined using the `=>` operator, followed by the pattern body. The pattern body can be a single expression, or a block
of code. The pattern-matching body can also contain a default case, which is defined using the `case _` syntax.

```scala
val x = 10
x match {
    case 1 => println("x is 1")
    case 2 => println("x is 2")
    case _ => println("x is not 1 or 2")
}

// or
val x = 10
val y = x match {
    case 1 => "x is 1"
    case 2 => "x is 2"
    case _ => "x is not 1 or 2"
}

// you can match it on case classes as well
case class Person(name: String, age: Int)

val person = Person("John", 20)
person match {
    case Person("John", 20) => println("John is 20 years old")
    case Person("John", 30) => println("John is 30 years old")
    case Person(name, age) => println(s"$name is $age years old")
}
```

You can also use a pattern guard to filter the pattern matching. A pattern guard is defined using the `if` keyword, followed by
the pattern guard check, similar to an `if` statement:

```scala
val x = 10
x match {
    case 1 => println("x is 1")
    case 2 => println("x is 2")
    case _ if x > 9 => println("x is greater than 9")
    case _ => println("x is not 1 or 2")
}

// you can also match types
val y: Any = 10
y match {
    case y: Int => println("y is an Int")
    case y: String => println("y is a String")
    case _ => println("y is neither an Int nor a String")
}
```

An explanation of the destructuring pattern matching syntax can be found in the
[extractor objects documentation](https://docs.scala-lang.org/tour/extractor-objects.html).

## Scala's type system

### Intersection types

Intersection types in Scala are defined using the `&` symbol, and they are used to combine multiple types into a single type. For
example:

```scala
type C = A & B
```

It means that type `C` can refer to any type that is a subtype of both `A` and `B`. You can also use intersection types to define
generic types:

```scala
type C[A, B] = A & B
```

### Union types

Union types in Scala are defined using the `|` symbol, and they are used to define a type that can be any of the types defined in
the union. For example:

```scala
type C = A | B
```

It means the type `C` can refer to any type that is a subtype of either `A` or `B`. You can also use union types to define
generic types:

```scala
type C[A, B] = A | B
```

### Type aliases

Type aliases in Scala are defined using the `type` keyword, followed by the alias name, and the type that the alias will be
associated with. Type aliases in Scala can be intersected or united.

```scala
type IntOrString = Int | String // union
type ClassAandB = ClassA & ClassB // intersection - a type must implement both
type MyNumber = Int

// a trait may also require a type
trait IWantANumber {
    type NumType
}
```

### Variance

Variance in Scala is defined using the `+` and `-` symbols, and they are used to define the variance of a type parameter. When
using the positive variance symbol (`+`), it means that the type parameter is covariant, and when using the negative variance
symbol (`-`), it means that the type parameter is contravariant.

A covariant type parameter means that for example, if you have a `List[+A]`, and A is a subtype of B, then List[+A] is also
a subtype of List[B]. A contravariant type parameter means that for example, if you have a `List[-A]`, and A is a subtype of B,
then List[B] is also a subtype of List[-A].

We could talk more about Scala's type systems, but I think that's enough for now. You can learn more about Scala's type systems
in the [Scala 3 documentation](https://docs.scala-lang.org/scala3/reference/new-types/type-parameters.html).

## Solid concepts

A really good thing about Scala is that it has a lot of great concepts and philosophies that you couldn't find in the majority
of other programming languages. You can also think of implicit conversions, extractor objects, contextual abstractions and its
advanced type system as a few of the concepts that Scala has.

## That's it!

That's it for this article. I hope you enjoyed it. If you have any questions, feel free to ask me or any other experienced
Scala developer on the [Scala Discord server](https://discord.gg/scala).

I'm not a Scala expert, so if you see any mistakes, feel free to contact me! I'll be happy to fix them and give you credit for it.

There are many more things that I did not cover in this article, but I think that's enough to learn the basics. I recommend learning the
following if you want to get better at Scala:

- Case classes behind the scenes: copy and unapply implementations
- Apply and custom operator functions
- Opaque types
- Structural types
- Dependent types
- Implicit conversions
- Contextual abstractions
- Generic inner type parameters

I want to thank the following people for helping me with Scala knowledge on the Scala Discord server:

- BalmungSan
- Volodymyr Lahush
- SethTisue
- armanbilge
- zygfryd
- Lesser Spotted Bambi
