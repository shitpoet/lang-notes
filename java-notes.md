# Java notes for non-Java programmers

Java (1995) has C-like syntax with curly braces. It's an imperative, garbage collecting language with developed C++-like OOP. New versions support lambdas. Very similar to C#.

As a whole Java syntax is very stable, and many features are implemented as classes instead of syntax, for example, `Optional`.

Java has extensive standard library.

Related languages: Kotlin, C#, JavaScript, C++.


## Fragmented type system

There're two category of types:

* primitive types
* reference types

Primitive types have value semantics.

Primitive types are:

`boolean`, `byte`, `short`, `int`, `long`, `float`, `double`, `char`

A programmer can't create new primitive types.

All numeric types in Java are signed! (`char` is unsigned.)

`char` is a single 16-bit Unicode character, as in C# and JavaScript. So Java has good support of Unicode out of the box.

TOADD: records

All other types are reference types. For example, `Object`, `String`, `ArrayList`.

There's also distinct type category for C-like arrays of fixed-size. Arrays have reference semantics also.

## Literals

Literals for primitive types:

    true false
    123_456 123_456L (long) (there's no `byte` or `short` suffix)
    0xfe, 0b1010
    +123.0e10 (double), 123.0f, 123.0d (exists, but not needed)
    'a', 'b', 'c'

Literals for reference types:

    "hello"
    null

Special characters:

    \b (backspace)
    \t (tab)
    \n (line feed)
    \f (form feed)
    \r (carriage return)
    \" (double quote)
    \' (single quote)
    \\ (backslash)

There're also weird special class literals:

    Object.class
    Integer.class
    String.class
    ArrayList.class


## Fixed-size arrays

    int[] a = { 11, 22, 33 };
    a.length
    a[0], a[1], a[2]

Note that array `length` is a special attribute, so no `()`.

No slice syntax. No negative indices.

TOCHECK: No methods.


## Strings

Strings are immutable.

Strings has no indexed access, but there is `charAt` method.

Strings can be `null`.

    s.length()

Operator `==` compares strings' identities, not values (but there's `equals` method).

    s.equals(t)


## Definite initialization analysis

Everything but local variables is zero-initialized (default-initialized) by default.

    `false`,  `0`, `+0.0`,`'\0'`, `null`

Java does definite initialization (DI) analysis.

`final` variables can obtain value after introduction, or in different code paths thanks to DI analysis.

Java requires all code path to have `return`-like statement in a function.

For uninitialized variables Java has definite initialization analys, used also for `final` local variables.


## Generics and type erasure

Generics in Java are checked on compile time and then normalized to use `Object` internally. It's called type erasure. So there is no reification of templated code.

TOCHEK: Generics on primitive types are impossible, due to type erasure, because primitive types are not convertable to `Object` easily. But one can write generic code on boxed primitives, like `Boolean`, `Character`, `Integer`.


## `static` methods aren't generic by default in a generic class

Static methods do not inherit genericness of a class. To make a static method generic, one can use the following syntax:

    public static <T> Optional<T> of(T value) {
    }

