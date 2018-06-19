Chisel datatypes are used to specify the type of values held in state
elements or flowing on wires.  While hardware designs ultimately
operate on vectors of binary digits, other more abstract
representations for values allow clearer specifications and help the
tools generate more optimal circuits.  In Chisel, a raw collection of
bits is represented by the ```Bits``` type.  Signed and unsigned integers
are considered subsets of fixed-point numbers and are represented by
types ```SInt``` and ```UInt``` respectively. Signed fixed-point
numbers, including integers, are represented using two's-complement
format.  Boolean values are represented as type ```Bool```.  Note
that these types are distinct from Scala's builtin types such as
```Int``` or ```Boolean```.  Additionally, Chisel defines `Bundles` for making
collections of values with named fields (similar to ```structs``` in
other languages), and ```Vecs``` for indexable collections of
values.  Bundles and Vecs will be covered later.

Constant or literal values are expressed using Scala integers or
strings passed to constructors for the types:
```scala
1.U       // decimal 1-bit lit from Scala Int.
"ha".U    // hexadecimal 4-bit lit from string.
"o12".U   // octal 4-bit lit from string.
"b1010".U // binary 4-bit lit from string.

5.S    // signed decimal 4-bit lit from Scala Int.
-8.S   // negative decimal 4-bit lit from Scala Int.
5.U    // unsigned decimal 3-bit lit from Scala Int.

8.U(4.W) // 4-bit unsigned decimal, value 8.
-152.S(32.W) // 32-bit signed decimal, value -152.

true.B // Bool lits from Scala lits.
false.B
```
Underscores can be used as separators in long string literals to aid
readability, but are ignored when creating the value, e.g.:
```scala
"h_dead_beef".U   // 32-bit lit of type UInt
```

By default, the Chisel compiler will size each constant to the minimum
number of bits required to hold the constant, including a sign bit for
signed types. Bit widths can also be specified explicitly on
literals, as shown below. Note that (`.W` is used to cast a Scala Int
to a Chisel width)
```scala
"ha".asUInt(8.W)     // hexadecimal 8-bit lit of type UInt
"o12".asUInt(6.W)    // octal 6-bit lit of type UInt
"b1010".asUInt(12.W) // binary 12-bit lit of type UInt

5.asSInt(7.W) // signed decimal 7-bit lit of type SInt
5.asUInt(8.W) // unsigned decimal 8-bit lit of type UInt
```

For literals of type ```UInt```, the value is
zero-extended to the desired bit width.  For literals of type
```SInt```, the value is sign-extended to fill the desired bit width.
If the given bit width is too small to hold the argument value, then a
Chisel error is generated.

>We are working on a more concise literal syntax for Chisel using
symbolic prefix operators, but are stymied by the limitations of Scala
operator overloading and have not yet settled on a syntax that is
actually more readable than constructors taking strings.

>We have also considered allowing Scala literals to be automatically
converted to Chisel types, but this can cause type ambiguity and
requires an additional import.

>The SInt and UInt types will also later support an optional exponent
field to allow Chisel to automatically produce optimized fixed-point
arithmetic circuits.

## Casting

We can also cast types in Chisel:

```scala
val sint = 3.S(4.W)             // 4-bit SInt

val uint = sint.asUInt          // cast SInt to UInt
uint.asSInt                     // cast UInt to SInt

val uint = sint.asUInt(8.W)     // cast SInt to UInt (explicit width)
uint.asSInt(8.W)                // cast UInt to SInt (explicit width)
```

We can also perform casts on clocks, though you should be careful about this, since clocking (especially in ASIC) requires special attention:

```scala
val bool: Bool = false.B        // always-low wire
val clock = bool.asClock        // always-low clock

clock.asUInt                    // convert clock to UInt (width 1)
clock.asUInt.toBool             // convert clock to Bool 
```

[Prev (Hardware Expressible in Chisel)](Hardware-Expressible-in-Chisel)  [Next (Combinational Circuits)](Combinational-Circuits)

