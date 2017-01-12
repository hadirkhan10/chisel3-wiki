### The Execution Pattern
It is a bit difficult to describe what it means to run chisel.  Chisel is not a monolithic application. It is more analogous to a collections of tools in the vein of a compiler family like gcc.  At first, when developing a new chisel circuit it is not that important what is going on under the hood.  A circuit is instantiated in the context of a test harness, usually in the form of the PeekPokeTester family and invoked via boilerplate like the following
```
   "here we assign to a F8.1 from a F8.3" in {
      chisel3.iotesters.Driver(() => new FixedPrecisionChanger(8, 3, 8, 1)) { c=>
        new FixedPointTruncatorTester(c, 6.875, 6.5)
      } should be (true)
    }
```
This article is about the chisel3.iotesters.Driver above does and how it does it, and what other members of the execution pattern do and how they do it.

### Executing chisel code written in scala.
Because Scala is based on Java and thus runs in a [JVM](https://www.google.com/search?q=jvm&oq=jvm) environment, it's a little more complicated to run than say, ```ls```.  To make this easier we use ```sbt``` which helps manage starting a JVM, managing library dependencies and running things that can be run.  *Can be run* in scala means that there is a main method defined in a companion object, though it can also be accomplished via the test harness shown above and objects that extend App.  The goal of the *Execution Pattern* is to describe a standard way of invoking various parts of the chisel toolchain particularly with respect to providing command line or internal control of those toolchain elements

### Execution Pattern Project Hierarchy 
The hierarchy currently looks like

| Level        | Project/Tool |
| ---------        | --------- |
| highest | chisel-testers |
| - | firrtl-interpreter, firrtl-repl |
| - | chisel3 |
| lowest | firrtl |

where higher levels depend on and, typically, call the layers below.  For example when running a test, the tester is 
invoked, which then invokes chisel3 to generate the circuit, which invokes firrtl to compile the circuit into 
low firrtl. After low firrtl has been generated, the firrtl-interpreter is invoked in order to execute 
the test on the device under test.  

Invocation here means that a execute method of a Driver companion object has been called.  Those methods can in as 
necessary or as controlled by the options can invoke the execute methods of lower level Driver objects.

### Execution Pattern Code
The execute methods of a Driver companion object (typically documented as Driver#execute) is built with a set of
scala base classes currently housed in firrtl.  

> The base classes live in firrtl because, as the lowest member of the hierarchy, all higher projects depend and
> thus have access to its members

firrtl
### Drivers and execute methods.
Each 
