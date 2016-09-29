How to build and test in simulation an interesting chisel circuit.
### The Cast
There are three kinds of players here
 - The Chisel developers: People who invented and built the toolchain
 - The Chisel Library Developers: People who have added functionality and domain specific utilities, e.g. DSP tools for chisel
 - The Circuit Developer: Will assume for the moment this means you. You have a circuit you want to build.

### The Tools
 - Chisel: A front-end circuit generator that implements an embedded Scala circuit DSL which compiles into a Firrtl file
 - Firrtl: An extensible multi-pass compiler that lowers high level firrtl into lower firrtl or to verilog
 - Firrtl Interpreter: A Scala level simulator that executes low firrtl.
 - chisel-

### The Circuit and Its tests

### Execution