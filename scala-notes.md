# Scala notes for non-Scala programmers

Scala is a multi-paradigm programming language targeting mostly JVM, but also having backend for JavaScript and LLVM. On JVM Scala has interop with Java.

Scala is heavely influenced by Java, but at the same time Scala encourages functional programming style with immutables values versus Java-style OOP.

Scala is statically typed, with type inference.

Scala has generic types, but it inhertis type erasure from Java.

Good resources:

* official comparison between Scala and Java - https://docs.scala-lang.org/scala3/book/scala-for-java-devs.html
* official comparison between Scala and JavaScript - https://docs.scala-lang.org/scala3/book/scala-for-javascript-devs.html
* official comparison between Scala and Python - https://docs.scala-lang.org/scala3/book/scala-for-python-devs.html

This document considers Scala 3, not Scala 2.

## Hello world

Minimal hello world in Scala in *worksheet mode*:

    println("hello world")

In worksheet mode, file must have `sc` instead of `scala` extension and be run with `scala-cli run hello.sc`.

In normal, non-worksheet mode:

    @main
    def main() = println("hello world")

File should have `scala` extension, could be run by both `scala hello.scala` and `scala-cli run hello.scala`.


## Comments and indents

    // comment
    /* comment */

Indents are 2 spaces.

## Basic types

All types are divided into two groups:

            Any
          /    \
    AnyVal    AnyRef

Value types (`AnyVal`):

    Unit    // void/NoneType-like type aka empty tuple

    Boolean
    Byte
    Short
    Int
    Long
    Float
    Double
    Char

Integer types are signed only, just like in Java.

Boxing/unboxing is transparent, like in C#, and unlike Java.

Reference types (`AnyRef`):

    String
    Array

There is also `Nothing`, a subtype of all types (without a value). Rarely needed.

## Type aliases

    type Meter = Int    // just an alias
    val x: Meter = 11
    val y: Int = 22
    println(x + y)


## Literals, special values, and some constructors

    true
    false
    12_300
    12.5f
    12.5d
    1e6
    Double.PositiveInfinity
    Double.NegativeInfinity
    Double.NaN
    'c'
    ()          // the single value of `Unit` type aka empty tuple
    "hello"                     // comparable
    s"some var is $var"         // simple interpolation
    f"some var is $var%.2f"     // printf-like interpolation
    """multi-line string"""
    List(1, 2, 3)               // immutable linked list
    Vector(1, 2, 3)             // immutable dynamic array
    Array(1, 2, 3)
    Set(1, 2, 3)
    Map(1 -> "one")
    Nil
    ArrayBuffer(1, 2, 3)    // import scala.collection.mutable.ArrayBuffer
    ArrayBuffer.empty[T]
    ListBuffer(1, 2, 3)     // import scala.collection.mutable._
    mutable.LinkedList      // dont know what it's named LinkedList and not List
    null                    // should not be used, for Java interop
    List.empty
    List.empty[T]

> A “buffer” in Scala is a sequence that can grow and shrink.

Opinionated note: In my point of view this naming isn't perfect, because in low-level programming a buffer is usually of a fixed size.


## Variables

    val n = 100     // type inference
    var i = 0       // mutable

    val b: Boolean = true
    val c: Char = 'x'
    val by: Byte = 127
    val sh: Short = 234
    val i: Int = 123_00
    val l: Long = 17179869184
    val f: Float = 1.7e6f
    val d: Double = Double.PositiveInfinity
    val d: Double = Double.NaN
    val s: String = "hello"
    val a: Array[Int] = Array(11, 22, 33)
    val a2: Array[Int] = Array(11, 22, 33)

No definite assignment analysis, but because every consturction is an expression it's not that important.

## Conversions

Widening number conversions are allowed.
`Char` -> `Int` is allowed.
`Long` to `Float` is deprecated. (It was allowed in older versions of Scala without a warning.)

    Byte -> Short -> Int -> Long    Float -> Double
                      ^
                      |
                    Char

`Char` is not converable to `Short` because `Short` is signed, and `Char` is unsigned. Just like in Java.

    x.toInt      // can throw

    x.toFloat

    x.toString   // works for primitives and for
                 // objects with user-defined toString

    s.toInt
    s.toIntOption
    s.toIntOption.getOrElse(-1)


## Operators

C-like operators:

    + - * /
    %  // applicable to integer and real numbers
    =
    += -= *= /= %=      // no ++/-- incs/decs

    < <= == => > !=
    eq (identity check)

    && || !

    & | ~ ^       // bitwise ops

    myStr1 + myStr2     // `++` also works

    a ++ b     // combine two collections
    // even `Array`'s, also `String`'s

    c :+ x     // append to collection
    x +: c     // prepend to collection

### Equality operators

Equality operator `==` first check for nulls, then call `equals` method on the boxed operand. Notice that it's different from Java's `equals` method, which is always called on the left "operand".

Prefer `==` over `equals`.

`eq` is an identiity comparision operator. It's for `AnyRef` objects. A call on `AnyVal` objects will box them.

    a eq b  // true if a and b point to the same `AnyRef` object
            // or if both are nulls
    Nil eq Nil      // true
    null eq null    // true (but recommendation is to not use `null`)

    1 eq 1                      // true, boxing, ints interning?
    1_000_000 eq 1_000_000      // false, boxing

The name can be misleading a little bit. This operator is similar to Python's `is`, but... I forgot what the difference is, sorry.

### User-defined operators

    class ...:
      def +(T other): ... = ...

    // note: `val` makes fields public in `class`
    class Color(val red: Int, val green: Int, val blue: Int):
      def *(k: Double): Color =
        Color((red * k).toInt, (green * k).toInt, (blue * k).toInt)


## Control flow

Scala has C-like `if`, `while`, `for`.

But note that the Control flow statements are expressions.

Loops are converted to `foreach/flatMap/map/filter` expressions internally.

    if x == 1 then
        ...
    else if x == 2 then
        ...
    else
        ...

    val numAsString = i match
      case 1 | 3 | 5 | 7 | 9 => "odd"
      case 2 | 4 | 6 | 8 | 10 => "even"
      case _ => "too big"

    // `match`'s `case` can have `if`.

    // rhs of a `case` can be empty - equal to `()`
    x match
        case 1 => "one"
        case 2 => "two"
        case _ =>

        // the last case can aslo be written as:
        case _ => ()

    coll match
        case h :: t => println(h)    // head / tail
        case _ => println("empty collection")

Loops:

    while ... do
        ...

    while
        ...
        ...
    do
        ...

    for el <- array do println(el)

    for i <- 0 to array.length do ...

    for i <- array.indices do ...

    for i <= 1 to 5 do println(i)

    for
      i <- 1 to 5
      j <- 'a' to 'b'
      k <- 1 to 10 by 5
      n = 100           // introduce a constant
      if i % 2 == 0     // guard (checked every time)
      if j < 7          // guard (checked every time)
    do
      println(i)

Note that guard `if` in `for` doesn't stop the loop if its condition is `false`.

    // maps are iterable as key-value pairs
    for (k, v) <- myMap do
      println(k)
      println(v)

There's also old, more C/Java-like syntax `if () ...`, `while () ...`, `for () ...`, and `for () yield ....`. Still supported without a warning in Scala 3.4.

No `break` and `continue` keywords. Although there's standrard `breakable` and `break` functions:

    import util.control.Breaks._
    breakable {
      var i = 0
      while i < 10 do
        println(i)
        i += 1
        if i == 5 then break
    }

`return`'s in loops are deprecated in Scala 3.

There's no `do ... while` loop.

`while` and `for-do` loops have type of `Unit`.

`for-yield` loop aka for-comprehension returns a collection:

    println( for i <- 5 to 1 by -1 yield i )
    // creates Vector(5, 4, 3, 2, 1) - not Array or List

    val x = for
      i <- 1 to 3
    yield
      val j = i * i
      j
    // Vector(1, 4, 9)

Any object is iterable by `for-do` or `for-yield` if it implements `map`, `flatMap`, `filter`, `withFilter`, `foreach`. A subset of the methods is enough for specific cases. F.ex. `if` guard requires `withFilter` or `filter`.

`while`, `for-do` and `for-yield` are all converted to `map` et al function calls with lambdas:

This

    for (x <- c1; y <- c2; z <-c3) {...}

is translated into

    c1.foreach(x => c2.foreach(y => c3.foreach(z => {...})))

This

    for (x <- c1; y <- c2; z <- c3) yield {...}

is translated into

    c1.flatMap(x => c2.flatMap(y => c3.map(z => {...})))

See more examples of these transformations in this SO-answer https://stackoverflow.com/a/1059501/3167374.

`flatMap` (map + flat) is used to return a single collection and not a collection of collections in case of several `<-` generators:

    for i <- List(11, 22); j <- List(33, 44) yield i * 10 + j

Note as we can still write one-liner here using `;` separator inside the header of the `for`.


## Strings

    s + t
    s ++ t
    s.charAt
    s.toUpperCase

## Arrays

Immutable `Vector` and `List` are the prefered sequential collections.

But Scala also supports Java/C/C#-like mutable fixed-size arrays `Array` and mutable and resizable `ArrayBuffer`

* `Array` is like C/C++/Java `T[]` fixed-size arrays.
* `ArrayBuffer` is like Java's `ArrayList` or C#'s `List`. It's resizable.

`Array`:

    val a = Array(11, 22, 33)
    a(0) *= 2
    println(a.mkString(" '))
    println(a.toList)

`ArrayBuffer`:

    import scala.collection.mutable.ArrayBuffer
    val a = ArrayBuffer[String]()
    a += "AA"
    a += "BB"
    println(a(1))  // BB

`Array` `==` compares the references:

    val a = Array(11, 22)
    val b = Array(11, 22)
    println(a == b)  // false

But `ArrayBuffer`' `==` compares by content:

    import scala.collection.mutable.ArrayBuffer
    val a = ArrayBuffer(11, 22)
    val b = ArrayBuffer(11, 22)
    println(a == b)  // true
    println(a eq b)  // false

So `Array` is the very low-level data structure. `ArrayBufer` is at a bit higher level.


## Tuples

Python-like, but immutable tuples:

    val a = ("eleven")
    val b = ("eleven", 11)
    val c = ("eleven", 11, 11.0)
    val d = ("eleven", 11, 11.0, Person("Eleven"))
    c(0)
    c(1)
    c(2)
    c.size      // 3

Deconstruction:

    val (name, quantity) = ingredient

Type of a tuple is written as `(T1, T2, ...., Tn)`.


## Functions

Some features:

* standalone functions, methods, anonymous functions
* overloading
* default values
    * overloads with default values are forbidden
* named arguments: f(y = 1), any argument can be passed with a name
* implicit arguments
* multiple parameters lists (incl. the case of no parameters list at all)

    def add(a: Int, b: Int): Int = a + b

    // return type inference
    def add(a: Int, b: Int) = a + b

    // named lambda, not idiomatic
    val add = (a: Int, b: Int) => a + b

    // short form labmda
    _ * 10
    // eq. to: (note type inference for the arg.)
    (x) => x * 10
    // eq. to:
    x => x * 10

    _ > _
    // eq. to
    (a, b) => a > b

Type of a function:

    Int => Int
    (Int, Int) => Int

Example with named parameters (from Tour of Scala):

    def printName(first: String, last: String): Unit =
        println(s"$first $last")

    printName("John", "Public")  // Prints "John Public"
    printName(first = "John", last = "Public")  // Prints "John Public"
    printName(last = "Public", first = "John")  // Prints "John Public"
    printName("Elton", last = "John")  // Prints "Elton John"

    printName(last = "Public", "John")  // error: positional after named argument

Scala has weak/local type inference, so sometimes one would need to provide types even if they're deductable from the context.


## Classes and objects

> In Scala, all types inherit from a top-level class `Any`
>  whose immediate children are AnyVal (value types, such as Int and Boolean)
> and AnyRef (reference types, as in Java).

           Any
        /      \
    AnyVal    AnyRef
      \         |
       \       Null
        \       /
         Nothing

`toString` is used like in Java/C#. Requires `override def` (?).

Implementation of abstract trait method requires an `override` (?).


## Collection methods

Scala has a lot of collection methods.

    /* creation */

    // `Range` constructors: to, until
    (1 to 5).toList                   // List(1, 2, 3, 4, 5)
    (1 until 5).toList                // List(1, 2, 3, 4)

    (1 to 10 by 2).toList             // List(1, 3, 5, 7, 9)
    (1 until 10 by 2).toList          // List(1, 3, 5, 7, 9)
    (1 to 10).by(2).toList            // List(1, 3, 5, 7, 9)

    ('d' to 'h').toList               // List(d, e, f, g, h)
    ('d' until 'h').toList            // List(d, e, f, g)
    ('a' to 'f').by(2).toList         // List(a, c, e)

    // range method
    List.range(1, 3)                  // List(1, 2)
    List.range(1, 6, 2)               // List(1, 3, 5)

    List.fill(3)("foo")               // List(foo, foo, foo)
    List.tabulate(3)(n => n * n)      // List(0, 1, 4)
    List.tabulate(4)(n => n * n)      // List(0, 1, 4, 9)

    /* methods */

    // these examples use a List, but they’re the same with Vector

    val a = List(10, 20, 30, 40, 10)      // List(10, 20, 30, 40, 10)

    a(0)
    a(1)
    a(2)

    a.length

    foreach/forEach

    a.head                                // 10
    a.headOption                          // Some(10)
    a.init                                // List(10, 20, 30, 40)
    a.last                                // 10
    a.lastOption                          // Some(10)
    a.tail                                // List(20, 30, 40, 10)

    a.slice(2, 4)                         // List(30, 40)

    a.take(3)                             // List(10, 20, 30)
    a.takeRight(2)                        // List(40, 10)
    a.takeWhile(_ < 30)                   // List(10, 20)

    a.drop(2)                             // List(30, 40, 10)
    a.dropRight(2)                        // List(10, 20, 30)
    a.dropWhile(_ < 25)                   // List(30, 40, 10)

    a.contains(20)                        // true
    a.exists(_ > 20)                       // true
    a.find(_ > 20)                        // Some(30)

    a.filter(_ < 25)                      // List(10, 20, 10)
    a.filter(_ > 100)                     // List()

    findFirst/find
    reduce
    reduceLeft
    fold
    foldLeft
    foldRight

    flatten
    // f.ex.: List(List(11), List(22, 33), List(44)).flatten

    a.distinct                            // List(10, 20, 30, 40)

    // map, flatMap
    val fruits = List("apple", "pear")
    fruits.map(_.toUpperCase)             // List(APPLE, PEAR)
    fruits.flatMap(_.toUpperCase)         // List(A, P, P, L, E, P, E, A, R)

    val nums = List(10, 5, 8, 1, 7)
    nums.sorted                           // List(1, 5, 7, 8, 10)
    nums.sortWith(_ < _)                  // List(1, 5, 7, 8, 10)
    nums.sortWith(_ > _)                  // List(10, 8, 7, 5, 1)

    List(1, 2, 3).updated(0, 10)            // List(10, 2, 3)
    List(2, 4).union(List(1, 3))            // List(2, 4, 1, 3)

    c1.diff(c2)	    // Returns the difference of the elements in c1 and c2.

    // zip
    val women = List("Wilma", "Betty")    // List(Wilma, Betty)
    val men = List("Fred", "Barney")      // List(Fred, Barney)
    val couples = women.zip(men)          // List((Wilma, Fred), (Betty, Barney))

    c.groupBy(f)    // Partitions the collection into a Map of collections according to f.
    c.partition(p)	// Returns two collections according to the predicate p.
                    // (predicate is individually checked per element)
                    // c.t. `span`
    c.span(p)	    // Returns a collection of two collections, the first created by c.takeWhile(p), and the second created by c.dropWhile(p).
    c.splitAt(n)	Returns a collection of two collections by splitting the collection c at element n.

    c slice(from, to)	Returns the interval of elements beginning at element from, and ending at element to.
    c1.containsSlice(c2)	// Returns true if c1 contains the sequence c2.

    c.exists(p)	// Returns true if p is true for any element in the collection.
    c.count(p)	// Counts the number of elements in c where p is true.

    // sum, min, max
    c.sum	// Returns the sum of all elements in the collection. (Requires an Ordering be defined for the elements in the collection. (what??))
    c.min	// Returns the smallest element from the collection. (Can throw java.lang.UnsupportedOperationException.)
    c.max	// Returns the largest element from the collection. (Can throw java.lang.UnsupportedOperationException.)
    c.product  // ... * ... * ...

## Option

Usage of `null` and exceptions is discouraged, instead `Option[T]` is used:

    def f(...): Option[Int] = ...

However there's `try/catch`, it can be used with `Option`'s `Some/None` constructions:

    def f(...): Option[Int] =
        try
            Some(...)
        catch
            case e: NumberFormatException => None
