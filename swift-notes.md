# Swift notes for non-Swift programmers

Swift (2014) is a language from Apple, replacing their Objective C language.

Swift is reference-counting, value oriented (copy-on-write) imperative and OOP programming language without a virtual machine.

## Primitive types

    Bool
    Int / UInt
    Float
    Double
    Character
    String

`Int` and `UInt` are 64-bit on 64-bit platforms and 32-bit on 32-bit platforms.

So `Int` is 64-bit on most modern systems Swift is used for. This is in contrast to the most mainstream languages where default `int` type gravitates towards 32-bit (C, C++, Java).

There're fixed width signed and unsigned integer types:

    Int8, Int16, Int32, Int64
    UInt8, UInt16, UInt32, UInt64


## Mutable and immutable variables

    let x = 5
    var y = 6

## Labeled parameters

TODO
