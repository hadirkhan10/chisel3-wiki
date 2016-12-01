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
    val foo = UInt(4.W)
    val bar = UInt(4.W)
  }
  val bundle = Wire(new MyBundle)
  bundle.foo := 0xc.U
  bundle.bar := 0x3.U
  val uint = bundle.asUInt
  printf(p"$uint") // 195

  // Test
  assert(uint === 0xc3.U)
```

### How do I create a Bundle from a UInt?

On an instance of the Bundle, call the method fromBits with the UInt as the argument

```scala
  // Example
  class MyBundle extends Bundle {
    val foo = UInt(4.W)
    val bar = UInt(4.W)
  }
  val uint = 0xb4.U
  val bundle = (new MyBundle).fromBits(uint)
  printf(p"$bundle") // Bundle(foo -> 11, bar -> 4)

  // Test
  assert(bundle.foo === 0xb.U)
  assert(bundle.bar === 0x4.U)
```

### How do I create a Vec of Bools from a UInt?

Use the builtin function chisel3.core.Bits.toBools to create a Scala Seq of Bool,
then wrap the resulting Seq in Vec(...)

```scala
  // Example
  val uint = 0xc.U
  val vec = Vec(uint.toBools)
  printf(p"$vec") // Vec(0, 0, 1, 1)

  // Test
  assert(vec(0) === false.B)
  assert(vec(1) === false.B)
  assert(vec(2) === true.B)
  assert(vec(3) === true.B)
```

### How do I create a UInt from a Vec of Bool?

Use the builtin function asUInt

```scala
  // Example
  val vec = Vec(true.B, false.B, true.B, true.B)
  val uint = vec.asUInt
  printf(p"$uint") // 13
  
  /* Test
   *
   * (remember leftmost Bool in Vec is low order bit)
   */
  assert(0xd.U === uint)
```
