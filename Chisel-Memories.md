## Loading Memories in simulation

***
THIS INFORMATION ON THIS PAGE NOT YET SUPPORTED ON MASTER
***
Chisel now supports a method for annotating memories to be loaded from a text file containing
hex or binary numbers. When using verilog simulation it uses the `$readmemh` or `$readmemb`
verilog extension. The treadle simulator can also load memories using the same annotation.

### How to annotate
Assuming you have a memory in a Module
```scala
  val memory = Mem(memoryDepth, memoryType)
```
At the top of your file just add the import 
```scala
import chisel3.util.loadMemoryFromFile
```
Now just add the memory annotation using
```scala
  loadMemoryFromFile(memory, "/workspace/workdir/mem1.txt")
```
The default is to use `$readmemh` (which assumes all numbers in the file are in ascii hex),
bu to use ascii binary there is an optional third argument. You will need to add an additional import.
```scala
import chisel3.util.MemoryLoadFileType
```
```scala
  loadMemoryFromFile(memory, "/workspace/workdir/mem1.txt", MemoryLoadFileType.Binary)
```
See: `/freechipsproject/chisel-testers/src/test/scala/examples/ComplexMemoryLoadingSpec.scala` and
`/Volumes/UCB-BAR/chisel-testers/src/test/scala/examples/LoadMemoryFromFileSpec.scala`
for working examples.

### Notes on files
There is no simple answer to where to put this file. It's probably best to create a resource directory somewhere and reference that through a full path. Because these files may be large, we did not want to copy them.
> Don't forget there is no decimal option, so a 10 in an input file will be 16 decimal

### Aggregate memories
Aggregate memories are supported but in bit of a clunky way. Since they will be split up into a memory per field, the following convention was adopted.  When specifying the file for such a memory the file name should be regarded as a template. If the memory is a Bundle e.g.
```scala
Bundle {
 val a = UInt(16.W)
 val b = UInt(32.W)
 val c = Bool()
}
```
The memory will be split into `memory_a`, `memory_b`, and `memory_c`. Similarly if a load file is specified as `"memory-load.txt"` the simulation will expect that there will be three files, `"memory-load_a.txt"`, `"memory-load_b.txt"`, `"memory-load_c.txt"`
> Note: The use of `_` and that the memory field name is added before any file suffix. The suffix is optional but if present is considered to be the text after the last `.` in the file name.


