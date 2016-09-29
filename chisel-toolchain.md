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
 - Device Under Test or the DUT:  written using Chisel in Scala
 - Device Tester: an subclass of PeekPokeTester that allows setting DUT's inputs, testing its outputs and stepping the clock
 - Device Launcher:  Most commonly done in the form of a class that extends a scalatest

### Scene 1.  Preparing to test
1. ChiselerX checkouts the [ucb-bar/chisel-template](ucb-bar/chisel-template) and follows the instructions, renaming it and changing it to be a new git project
1. They decide to use the [ucb-bar/dsptools](ucb-bar/dsptools) library, which simply requires that they add a new dependency line in their ```build.sbt``` file in the root directory
1. They create a DUT.scala and in it create a DUT Chisel Module, using constructs from the dsp library, including:
  1. annotations
  1. instances of black boxes
1. They create a device tester which takes a DUT and pokes its input and outputs and steps it
1. They create a launcher for the tester based on the scalatest library
1. They execute the launcher which kicks off the following flurry of activity

### Scene 2. Execution
1. Options for the execution are parsed and assembled into individual configurations for each part of the execution toolchain
1. The circuit is elaborated.
 1. The circuit is parsed
  1. An internal graph of the circuit is generated
  1. Annotations of circuit components are collected
  1. References to black boxes are checked.
  1. The library registers any custom passes it requires with a firrtl configuration object
1. The circuit is emitted, i.e. serialized as text.  That text may be saved to disk
1. The firrtl compiler is launched
 1.
