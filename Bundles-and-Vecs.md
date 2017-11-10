`Bundle` and `Vec` are classes that allow the user to expand
the set of Chisel datatypes with aggregates of other types.

Bundles group together several named fields of potentially different
types into a coherent unit, much like a `struct` in C. Users
define their own bundles by defining a class as a subclass of `Bundle`
```scala
class MyFloat extends Bundle {
  val sign        = Bool()
  val exponent    = UInt(8.W)
  val significand = UInt(23.W)
}

val x  = Wire(new MyFloat)
val xs = x.sign
```

> Currently, there is no way to create a bundle literal like ```8.U``` for ```UInt```s. Therefore, in order to create literals for bundles, we must declare a [[wire|Combinational-Circuits#wires]] of that bundle type, and then assign values to it. We are working on a way to declare bundle literals without requiring the creation of a Wire node and assigning to it.

```scala
// Floating point constant.
val floatConst = Wire(new MyFloat)
floatConst.sign := true.B
floatConst.exponent := 10.U
floatConst.significand := 128.U
```

A Scala convention is to capitalize the name of new classes and we
suggest you follow that convention in Chisel too.  The ```width```
named parameter to the `UInt` constructor specifies the number
of bits in the type.

Vecs create an indexable vector of elements, and are constructed as
follows:
```scala
// Vector of 5 23-bit signed integers.
val myVec = Wire(Vec(5, SInt(23.W)))

// Connect to one element of vector. 
val reg3 = myVec(3) 
```

(Note that we specify the number followed by the type of the `Vec` elements. We also specifiy the width of the `SInt`)

The set of primitive classes
(`SInt`, `UInt`, and `Bool`) plus the aggregate
classes (`Bundles` and `Vec`s) all inherit from a common
superclass, `Data`.  Every object that ultimately inherits from
`Data` can be represented as a bit vector in a hardware design.

Bundles and Vecs can be arbitrarily nested to build complex data
structures:
```scala
class BigBundle extends Bundle {
 // Vector of 5 23-bit signed integers.
 val myVec = Vec(5, SInt(23.W))
 val flag  = Bool()
 // Previously defined bundle.
 val f     = new MyFloat
}
```

Note that the builtin Chisel primitive and aggregate classes do not
require the `new` when creating an instance, whereas new user
datatypes will.  A Scala `apply` constructor can be defined so
that a user datatype also does not require `new`, as described in
[Function Constructor](Functional-Module-Creation)

[Prev (Functional Abstraction)](Functional-Abstraction) [[Next (Ports)|Ports]]