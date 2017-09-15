## Release Notes 09/14/17

This is the first Chisel3 BIG4 SNAPSHOT release to support Scala 2.12.
Unfortunately, behavior we've come to rely on when generating anonymous classes structurally has been deemed a [bug](https://github.com/scala/bug/issues/10047) in [Scala 2.12 ("Inferred types for fields")](http://scala-lang.org/news/2.12.0/).
This is Chisel3 issue [606](https://github.com/freechipsproject/chisel3/issues/606).

The following code:
```scala
import Chisel._

class MyModule extends Module {
  val io = new Bundle {
    val in = UInt(INPUT, width = 8)
    val out = UInt(OUTPUT, width = 8)
  }
  io.out := io.in
}
```

Results in:
```
Error:(12, 6) value out is not a member of Chisel.Bundle
  io.out := io.in
Error:(12, 16) value in is not a member of Chisel.Bundle
  io.out := io.in
```
*NOTE:* This seems to be constrained to compatibility mode.
The normal chisel3 wrappers `IO()`, `Wire()` cause the inferred value to be the structural type of their argument (the behavior we desire).
As stated in Chisel3 issue [606](https://github.com/freechipsproject/chisel3/issues/606), the work around is to add:
```scala
scalacOptions ++= Seq("-Xsource:2.11")
```
to your build.sbt file if you're using Chisel3 in compatibility mode with Scala 2.12.

### Chisel3

- Update sbt to 0.13.16; add Scala 2.12 support. [Chisel PR #675](https://github.com/freechipsproject/chisel3/pull/675)
- Added API to get Verilog from Chisel [Chisel PR #676](https://github.com/freechipsproject/chisel3/pull/676)
- Update README [Chisel PR #683](https://github.com/freechipsproject/chisel3/pull/683)
- Use firrtl elses in elsewhen/otherwise case emission [Chisel PR #510](https://github.com/freechipsproject/chisel3/pull/510)
- More of the bindings refactor [Chisel PR #635](https://github.com/freechipsproject/chisel3/pull/635)
- Make Reset a trait [Chisel PR #672](https://github.com/freechipsproject/chisel3/pull/672)

### Firrtl

- Update sbt to 0.13.16; add Scala 2.12 support. [Firrtl PR #639](https://github.com/freechipsproject/firrtl/pull/639)
- Make pathsInDAG walk all possible paths [Firrtl PR #655](https://github.com/freechipsproject/firrtl/pull/655)
- Write tests on multi-rooted circuits for ConstProp [Firrtl PR #645](https://github.com/freechipsproject/firrtl/pull/645)
- Add InstanceGraph tests [Firrtl PR #646](https://github.com/freechipsproject/firrtl/pull/646)
- Added option to emit final annotations [Firrtl PR #649](https://github.com/freechipsproject/firrtl/pull/649)
- Reorder port and wire assignments in Verilog [Firrtl PR #641](https://github.com/freechipsproject/firrtl/pull/641)

### Chisel-testers

- Simple fixed point support [Chisel-testers PR #167](https://github.com/freechipsproject/chisel-testers/pull/167)
- Update sbt to 0.13.16; add Scala 2.12 support. [Chisel-testers PR #165](https://github.com/freechipsproject/chisel-testers/pull/165)
- Don't add 0-width wires to verilated c++ [Chisel-testers PR #162](https://github.com/freechipsproject/chisel-testers/pull/162)
- Explicitly cast reset to Bool [Chisel-testers PR #164](https://github.com/freechipsproject/chisel-testers/pull/164)

### Interpreter

- Update sbt to 0.13.16; add Scala 2.12 support. [Firrtl-interpreter PR #89](https://github.com/freechipsproject/firrtl-interpreter/pull/89)

### DSPTools

Initial SNAPSHOT release of the DSP generator library.
Please visit the [GitHub repository](https://github.com/ucb-bar/dsptools) for instructions and examples.
*Note:* Currently, we only support Scala 2.11 versions of this library - we don't have 2.12 versions of all of its dependencies.
We continue to release Scala 2.11 versions of the BIG4 (required by DSPTools).
If you wish to experiment with the DSPTools library, you will need to use Scala 2.11.
