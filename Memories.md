Chisel provides facilities for creating both read only and read/write memories.  

## ROM

Users can define read only memories with a `Vec`:

``` scala
    Vec(inits: Seq[T])
    Vec(elt0: T, elts: T*)
```

where `inits` is a sequence of initial `Data` literals that initialize the ROM. For example,  users cancreate a small ROM initialized to 1, 2, 4, 8 and loop through all values using a counter as an address generator as follows:

``` scala
    val m = Vec(Array(1.U, 2.U, 4.U, 8.U))
    val r = m(counter(m.length.U))
```

We can create an *n* value sine lookup table using a ROM initialized as follows:

``` scala
    def sinTable(amp: Double, n: Int) = {
      val times = 
        (0 until n).map(i => (i*2*Pi)/(n.toDouble-1) - Pi)
      val inits = 
        times.map(t => round(amp * sin(t)).asSInt(32.W))
      Vec(inits)
    }
    def sinWave(amp: Double, n: Int) = 
      sinTable(amp, n)(counter(n.U))
```

where `amp` is used to scale the fixpoint values stored in the ROM.

## Mem

Memories are given special treatment in Chisel since hardware implementations of memory have many variations, e.g., FPGA memories are instantiated quite differently from ASIC memories.  Chisel defines a memory abstraction that can map to either simple Verilog behavioral descriptions, or to instances of memory modules that are available from external memory generators provided by foundry or IP vendors.

Chisel supports random-access memories via the `Mem` construct. Writes to `Mem`s are **combinational/asynchronous-read, sequential/synchronous-write**. These `Mem`s will likely be synthesized to register banks.

Chisel also has a construct called `SyncReadMem` for **sequential/synchronous-read, sequential/synchronous-write** memories. Most SRAMs in modern technologies (FPGA, ASIC) tend to no longer support combinational (asynchronous) reads, and `SyncReadMem`s will likely be synthesized to technology SRAMs (as opposed to register banks).

Ports into Mems are created by applying a `UInt` index.  A 1024-entry register file with one write port and one sequential/synchronous read port might be expressed as follows:

```scala
val width:Int = 32
val addr = Wire(UInt(width.W))
val dataIn = Wire(UInt(width.W))
val dataOut = Wire(UInt(width.W))
val enable = Wire(Bool())

// assign data...

// Create a synchronous-read, synchronous-write memory (like in FPGAs).
val mem = SyncReadMem(1024, UInt(width.W))
// Create one write port and one read port.
mem.write(addr, dataIn)
dataOut := mem.read(addr, enable)
```
Creating an asynchronous-read version of the above simply involves replacing `SyncReadMem` with just `Mem`.

Chisel can also infer other features such as single ports and masks directly with Mem.

Single-ported SRAMs can be inferred when the read and write conditions are
mutually exclusive in the same `when` chain:

``` scala
val mem = SyncReadMem(2048, UInt(32.W))
when (write) { mem.write(addr, dataIn) }
.otherwise { dataOut := mem.read(addr, read) }
```

If the same `Mem` address is both written and sequentially read on the same clock
edge, or if a sequential read enable is cleared, then the read data is
undefined.

`Mem` and `SyncReadMem` also support write masks for subword writes.  A given bit is written if
the corresponding mask bit is set.

``` scala
val ram = Mem(256, UInt(32.W))
when (wen) { ram.write(waddr, wdata, wmask) }
```

[Prev(State Elements)](State-Elements) [Next(Interfaces \& Bulk Connections)](Interfaces-Bulk-Connections)
