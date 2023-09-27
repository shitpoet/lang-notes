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


## Literals

    true
    false
    12
    012      # 12 (no octal literals)
    .45      # TOCHECK
    12.45
    12.
    0b1010
    0o777    # octal prefix
    0xff00ee
    123_456.789_012
    1.0e3
    nil      # for reference types incl. Optional
    1..5     # closed range
    1..<5    # half-open range

### String interpolation

All string literals support interpolation with `\()`:

    "x is equal to \(x)"
    "Six by nine is \(6 * 9), not 42!"

### Raw strings

    #"x is equal to "\(x)""#
    ##"""##
    ###"""###

### MUlti-line strings

    """
      some
      long
      string
    """

    #"""
      text
    """#

#### Arrays, dictionaries

Empty collections' literals require context:

    []      // note: full type is non-deducable
    [:]     // note: full type is non-deducable

    let a: [Int] = []              // ok
    let a: Array<Int> = []         // ok
    let d: [String: Double] = [:]  // ok


## Type conversions

    Int(s)
    String(i)
    Character(s)  //TOCHECK: correct?


## Mutable and immutable variables

    let x = 5
    var y = 6
    let `let` = 7

## Labeled parameters

TODO

## Console IO

    readline()

