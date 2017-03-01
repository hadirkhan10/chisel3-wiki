Welcome to the Chisel cookbook. This cookbook is still in early stages. If you have any requests or examples to share, please [file an issue](https://github.com/ucb-bar/chisel3/issues/new) and let us know!

Please note that these examples make use of [Chisel's scala-style printing](Printing in Chisel#scala-style).

* [How do I create a UInt from an instance of a Bundle?](#how-do-i-create-a-uint-from-an-instance-of-a-bundle)
* [How do I create a Bundle from a UInt?](#how-do-i-create-a-bundle-from-a-uint)
* [How do I create a Vec of Bools from a UInt?](#how-do-i-create-a-vec-of-bools-from-a-uint)
* [How do I create a UInt from a Vec of Bool?](#how-do-i-create-a-uint-from-a-vec-of-bool)
* [How do I create a Vector of Registers?](#how-do-i-create-a-vector-of-registers)
* [How do I create a Reg of type Vec?](#how-do-i-create-a-reg-of-type-vec)
* [How do I create a finite state machine?](#how-do-i-create-a-finite-state-machine)
* [How do I do a reverse concatenation like in Verilog?](#how-do-i-do-a-reverse-concatenation-like-in-verilog)

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

### Vectors and Registers

## How do I create a Vector of Registers?

You create a [Reg of type Vec](#how-do-i-create-a-reg-of-type-vec). Because Vecs are a *type* rather than a *value*, we must bind the Vec to some concrete *value*.

## How do I create a Reg of type Vec?

For information, please see the API documentation
(https://chisel.eecs.berkeley.edu/api/index.html#chisel3.core.Vec)

```scala
  // Reg of Vec of 32-bit UInts without initialization
  val regOfVec = Reg(Vec(4, UInt(32.W)))
  regOfVec(0) := 123.U // a couple of assignments
  regOfVec(2) := regOfVec(0)

  // Reg of Vec of 32-bit UInts initialized to zero
  //   Note that Seq.fill constructs 4 32-bit UInt literals with the value 0
  //   Vec(...) then constructs a Wire of these literals
  //   The Reg is then initialized to the value of the Wire (which gives it the same type)
  val initRegOfVec = Reg(init = Vec(Seq.fill(4)(0.asUInt(32.W))))

  // Simple test (cycle comes from superclass)
  when (cycle === 2.U) { assert(regOfVec(2) === 123.U) }
  for (elt <- initRegOfVec) { assert(elt === 0.U) }
```

### How do I create a finite state machine?

Use Chisel Enum to construct the states and switch & is to construct the FSM
control logic.

```scala
class DetectTwoOnes extends Module {
  val io = IO(new Bundle {
    val in = Input(Bool())
    val out = Output(Bool())
  })

  val sNone :: sOne1 :: sTwo1s :: Nil = Enum(3)
  val state = Reg(init = sNone)

  io.out := (state === sTwo1s)

  switch (state) {
    is (sNone) {
      when (io.in) {
        state := sOne1
      }
    }
    is (sOne1) {
      when (io.in) {
        state := sTwo1s
      } .otherwise {
        state := sNone
      }
    }
    is (sTwo1s) {
      when (!io.in) {
        state := sNone
      }
    }
  }
}
```

### How do I do a reverse concatenation like in Verilog?

```verilog
wire [1:0] a;
wire [3:0] b;
wire [2:0] c;
wire [8:0] z = [...];
assign {a,b,c} = z;
```

The easiest way to accomplish the above in Chisel would be:

```scala
class MyBundle extends Bundle {
  val a = UInt(2.W)
  val b = UInt(4.W)
  val c = UInt(3.W)
}

val z = Wire(UInt(width=9))
// z := ...
val unpacked = (new MyBundle).fromBits(z)
unpacked.a
unpacked.b
unpacked.c
```

If you **really** need to do this for a once-time case (think thrice! likely you can better structure the code using bundles), then rocket-chip has a [Split utility](https://github.com/ucb-bar/rocket-chip/blob/master/src/main/scala/util/Misc.scala#L118) which can accomplish this.