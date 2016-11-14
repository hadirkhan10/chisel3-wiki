* [Why Decoupled instead of ReadyValid?](#why-decoupled-instead-of-readyvalid)
* [Why do I have to wrap `Data` in `Wire(...)`?](#why-do-i-have-to-wrap-data-in-wire)
* [Why do I have to wrap module instantiations in `Module(...)`?](#why-do-i-have-to-wrap-module-instantiations-in-module)
* [Why Chisel?](#why-chisel)

### Why Decoupled instead of ReadyValid?

There are multiple kinds of Ready/Valid interfaces that impose varying restrictions on the producers and consumers. Gives static type safety between these interfaces.

### Why do I have to wrap [`Data`](https://chisel.eecs.berkeley.edu/api/index.html#chisel3.core.Data) in `Wire(...)`?
### Why do I have to wrap module instantiations in `Module(...)`?
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