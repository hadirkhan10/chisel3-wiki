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

To annotate it to be loaded from a file 
import chisel3.util.loadMemoryFromFile
