* [Why DecoupledIO instead of ReadyValidIO?](#why-decoupledio-instead-of-readyvalidio)
* [Why do I have to wrap module instantiations in `Module(...)`?](#why-do-i-have-to-wrap-module-instantiations-in-module)
* [Why Chisel?](#why-chisel)
* [I just want some verilog, What do I do?](#get-me-verilog)

### Why DecoupledIO instead of ReadyValidIO?

There are multiple kinds of Ready/Valid interfaces that impose varying restrictions on the producers and consumers. Chisel currently provides the following:

* [DecoupledIO](https://chisel.eecs.berkeley.edu/api/index.html#chisel3.util.DecoupledIO) - No guarantees
* [IrrevocableIO](https://chisel.eecs.berkeley.edu/api/index.html#chisel3.util.IrrevocableIO) - Producer promises to not change the value of 'bits' after a cycle where 'valid' is high and 'ready' is low. Additionally, once 'valid' is raised it will never be lowered until after 'ready' has also been raised.

### Why do I have to wrap module instantiations in `Module(...)`?

In short: Limitations of Scala

Chisel Modules are written by defining a [Scala class](http://docs.scala-lang.org/tutorials/tour/classes.html) and implementing its constructor. As elaboration runs, Chisel constructs a hardware AST from these Modules. The compiler needs hooks to run before and after the actual construction of the Module object. In Scala, superclasses are fully initialized before subclasses, so by extending Module, Chisel has the ability to run some initialization code before the user's Module is constructed. However, there is no such hook to run after the Module object is initialized. By wrapping Module instantiations in the Module object's apply method (ie. `Module(...)`), Chisel is able to perform post-initialization actions. There is a [proposed solution](https://issues.scala-lang.org/browse/SI-4330), so eventually this requirement will be lifted, but for now, wrap those Modules!

### Why Chisel?

Borrowed from [Chisel Introduction](Chisel Introduction)

>We were motivated to develop a new hardware language by years of
struggle with existing hardware description languages in our research
projects and hardware design courses.  _Verilog_ and _VHDL_ were developed
as hardware _simulation_ languages, and only later did they become
a basis for hardware _synthesis_.  Much of the semantics of these
languages are not appropriate for hardware synthesis and, in fact,
many constructs are simply not synthesizable.  Other constructs are
non-intuitive in how they map to hardware implementations, or their
use can accidently lead to highly inefficient hardware structures.
While it is possible to use a subset of these languages and still get
acceptable results, they nonetheless present a cluttered and confusing
specification model, particularly in an instructional setting.

>However, our strongest motivation for developing a new hardware
language is our desire to change the way that electronic system design
takes place.  We believe that it is important to not only teach
students how to design circuits, but also to teach them how to design
*circuit generators* ---programs that automatically generate
designs from a high-level set of design parameters and constraints.
Through circuit generators, we hope to leverage the hard work of
design experts and raise the level of design abstraction for everyone.
To express flexible and scalable circuit construction, circuit
generators must employ sophisticated programming techniques to make
decisions concerning how to best customize their output circuits
according to high-level parameter values and constraints.  While
Verilog and VHDL include some primitive constructs for programmatic
circuit generation, they lack the powerful facilities present in
modern programming languages, such as object-oriented programming,
type inference, support for functional programming, and reflection.

>Instead of building a new hardware design language from scratch, we
chose to embed hardware construction primitives within an existing
language.  We picked Scala not only because it includes the
programming features we feel are important for building circuit
generators, but because it was specifically developed as a base for
domain-specific languages.

### Get me verilog
I wrote a module, I want to see the verilog, what do I do?
Here's a simple hello world module in a file HelloWorld.scala
```
package intro
import chisel3._
class HelloWorld extends Module {
  val io = IO(new Bundle{})
  printf("hello world\n")
}
```
Add the following
```
object HelloWorld extends App {
  chisel3.Driver.execute(args, () => new HelloWorld)
}
```
Now you can get some verilog, start sbt
```
bash> sbt
> run-main intro.HelloWorld
[info] Running examples.HelloWorld
[info] [0.004] Elaborating design...
[info] [0.100] Done elaborating.
[success] Total time: 1 s, completed Jan 12, 2017 6:24:03 PM
```
or as a single command line
```
bash> sbt 'run-main intro.HelloWorld'
```
After either of the above there will be a HelloWorld.v file in the current directory.  
You can see additional options with
```
bash> sbt 'run-main intro.HelloWorld --help'
```
This will return a comprehensive usage line with available options.




