Welcome to the Chisel cookbook. This cookbook is still in early stages. If you have any requests or examples to share, please [file an issue](https://github.com/ucb-bar/chisel3/issues/new) and let us know!

Please note that these examples make use of [Chisel's scala-style printing](Printing in Chisel#scala-style).

* [How do I create a UInt from an instance of a Bundle?](#how-do-i-create-a-uint-from-an-instance-of-a-bundle)
* [How do I create a Bundle from a UInt?](#how-do-i-create-a-bundle-from-a-uint)
* [How do I create a Vec of Bools from a UInt?](#how-do-i-create-a-vec-of-bools-from-a-uint)
* [How do I create a UInt from a Vec of Bool?](#how-do-i-create-a-uint-from-a-vec-of-bool)

### How do I create a UInt from an instance of a Bundle?

Call asUInt on the Bundle instance

```scala
  // Example
  class MyBundle extends Bundle {
    val foo = UInt(width = 4)
    val bar = UInt(width = 4)
  }
  val bundle = Wire(new MyBundle)
  bundle.foo := UInt(0xc)
  bundle.bar := UInt(0x3)
  val uint = bundle.asUInt
  printf(p"$uint") // 195

  // Test
  assert(uint === UInt(0xc3))
```

### How do I create a Bundle from a UInt?

On an instance of the Bundle, call the method fromBits with the UInt as the argument

```scala
  // Example
  class MyBundle extends Bundle {
    val foo = UInt(width = 4)
    val bar = UInt(width = 4)
  }
  val uint = UInt(0xb4)
  val bundle = (new MyBundle).fromBits(uint)
  printf(p"$bundle") // Bundle(foo -> 11, bar -> 4)
  
  // Test
  assert(bundle.foo === UInt(0xb))
  assert(bundle.bar === UInt(0x4))
```

### How do I create a Vec of Bools from a UInt?

Use the builtin function chisel3.core.Bits.toBools to create a Scala Seq of Bool,
then wrap the resulting Seq in Vec(...)

```scala
  // Example
  val uint = UInt(0xc) 
  val vec = Vec(uint.toBools)
  printf(p"$vec") // Vec(0, 0, 1, 1)
  
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
  printf(p"$uint") // 13
  
  // Test
  // (remember leftmost Bool in Vec is low order bit)
  assert(UInt(0xd) === uint)
```
