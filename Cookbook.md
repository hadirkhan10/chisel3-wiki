Welcome to the Chisel cookbook. Please note that these examples make use of [Chisel's scala-style printing](Printing in Chisel#Scala-style).

### How do I create a UInt from an instance of a Bundle?

Call asUInt on the Bundle instance

```scala
  // Example
  class MyBundle extends Bundle {
    val foo = UInt(width = 2)
    val bar = UInt(width = 2)
  }
  val bundle = Wire(new MyBundle)
  bundle.foo := UInt("b10")
  bundle.bar := UInt("b01")
  val uint = bundle.asUInt
  printf(p"$uint")

  // Test
  assert(uint === UInt("b1001"))
```

### How do I create a Bundle from a UInt?

On an instance of the Bundle, call the method fromBits with the UInt as the argument

```scala
  // Example
  class MyBundle extends Bundle {
    val foo = UInt(width = 2)
    val bar = UInt(width = 2)
  }
  val uint = UInt("b1100")
  val bundle = (new MyBundle).fromBits(uint)
  printf(p"$bundle") 
  
  // Test
  assert(bundle.foo === UInt("b11"))
  assert(bundle.bar === UInt("b00"))
```

### How do I create a Vec of Bools from a UInt?

Use the builtin function chisel3.core.Bits.toBools to create a Scala Seq of Bool,
then wrap the resulting Seq in Vec(...)

```scala
  // Example
  val uint = UInt("b1100") 
  val vec = Vec(uint.toBools)
  printf(p"$vec")
  
  // Test
  assert(vec(0) === Bool(false))
  assert(vec(1) === Bool(false))
  assert(vec(2) === Bool(true))
  assert(vec(3) === Bool(true))
```

### How do I create a UInt from a Vec of Bool?

Use the builtin function asUInt

```scala
  // Example
  val vec = Vec(Bool(true), Bool(false), Bool(true), Bool(true))
  val uint = vec.asUInt
  printf(p"$uint")
  
  // Test
  // (remember leftmost Bool in Vec is low order bit)
  assert(UInt(13) === uint)
```
