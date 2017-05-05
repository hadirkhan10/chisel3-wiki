# Overview
Chisel is an embedded domain specific language or DSL written in Scala. Embedded means that the constructs of the Chisel language used within programs written in Scala. This allows developers to take advantage the very powerful Scala language in their circuit design. The goal of a Chisel program is to produce a circuit, and as a Chisel program is executed the circuit is assembled as chisel parts of the program are invoked under control of the Scala parts. A common shared experienced of developers is a recognition of mental distinction between Scala-land and Chisel-land. This document attempts to describe some lessons learned about this aspect of the Chisel experience.

## Code generation
At the simplest level Scala is used to create circuit components and to connect these components together. But the real power of Scala is provide a framework for expressing parameterized models of circuit generators. 

## Suggestions
### Delay instantiation of Chisel components 
A common pattern is creations of arrays of elements, perhaps a parallel set of adders or a 2 dimensional lookup table.  A good practice is to generated Scala structures that represent this and once fully modeled then create the Chisel elements.  The [collection api](http://docs.scala-lang.org/overviews/collections/overview.html) is one of Scala's strongest features, learn it, use it to build the conceptual module, and finally use it to generate the target circuitry.
### Avoid scala code that considers the literal values of a Chisel component
It's tempting when one has created some Chisel literal to try and use the value of that literal to control some
 subsequent Scala code, perhaps as a parameter or iteration control.  Although in
  some cases this is possible, typically it just leads to confusion and unpredictable behavior.  Don't do this.
```scala
for(i < 0 until 10) {
  val enable := (i %2 == 0).B
  if(enable.litValue()) {
    ...
  }
```

### Keep a clear distinction between types and values
Many of the Chisel constructs require generators, that is, functions or values that are used specify widths and other information. For example: The first argument to the Register constructor is used as the type of the register. It is legal to specify this with a UInt with a specified value as below.
```scala
  val accumulator = Reg(UInt(42.W))
```
This does create an accumulator with sufficient width to accommodate the number 42, but it does not initialize the register to that value.  Better to use
```scala
  val accumulator = Reg(UInt(log2Up(42).W))
```
if you just want a register of that size or 
```scala
  val accumulator = RegInit(42.U)
```
if the register should be initialized with 42.
```scala
  val accumulator = RegInit(42.U(64.W))
```
if the register should be initialized with 42 but you want to make sure registers is of a particular size.

Another example of this is needing a register that holds a bunch of initialized values.
```scala
  val found = Reg(Vec(100, true.B))
```
This creates a vector of Bool's but they are not initialized, the Bool(true) is used only for it's type value.
Instead use
```scala
  val found = Reg(Vec.fill(100) { true.B })
```

### Circuits are not imperative, don't let Scala lead you to believe that they are.
Consider the case where a vector of values is being sent to a module, incrementing the vector index on some condition.
```scala
  val index = RegInit(0.U(log2Up(max_index).W))
  when(condition) {
    module.io.in.a := vector(index)
    module.io.in.valid := true.B
  } elsewhen {
    module.io.in.valid := false.B
  }
  when(module.io.in.ready) {
    index := index + 1.U
  }
```
In general the following is preferred
```scala
  val index = RegInit(0.U(log2Up(max_index).W))
  module.io.in.a := vector(index)
  module.io.in.valid := condition
  when(module.io.in.ready) {
    index := index + 1.U
  }
```
The data is always being offered to the module, only the valid signal is wired to the condition.  The index is updated when the condition.  Placing the io assignments in the when clauses is an example of inappropriate Scala-land thinking