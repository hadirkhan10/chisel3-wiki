Chisel has a number of new features that are worth checking out.  This page is an informal list of these features and projects.

### Interval Type
See [Experimental - Interval Types](Experimental---Interval-Type)

### FixedPoint
FixedPoint numbers are basic *Data* type along side of UInt, SInt, etc.  Most common math and logic operations
are supported. Chisel allows both the width and binary point to be inferred by the Firrtl compiler which can simplify 
circuit descriptions.  See: [FixedPoint](FixedPoint)
 and [FixedPointSpec]blob/master/src/test/scala/chiselTests/FixedPointSpec.scala)

### Module Variants
The standard Chisel *Module* requires a ```val io = IO(...)```, the experimental package introduces several
new ways of defining Modules
- BaseModule: no contents, instantiable
- BlackBox extends BaseModule
- UserDefinedModule extends BaseModule: this module can contain Chisel RTL. No default clock or reset lines. No default IO. - User should be able to specify non-io ports, ideally multiple of them.
- ImplicitModule extends UserModule: has clock, reset, and io, essentially current Chisel Module.
- RawModule: will be the user-facing version of UserDefinedModule
- Module: type-aliases to ImplicitModule, the user-facing version of ImplicitModule.

### Bundle Literals

Chisel 3.2 introduces an experimental mechanism for Bundle literals in #820, but this feature is largely incomplete and not ready for user code yet. The following is provided as documentation for library writers who want to take a stab at using this mechanism for their library's bundles.

```scala
class MyBundle extends Bundle {
  val a = UInt(8.W)
  val b = Bool()

  // Bundle literal constructor code, which will be auto-generated using macro annotations in
  // the future.
  import chisel3.core.BundleLitBinding
  import chisel3.internal.firrtl.{ULit, Width}

  // Full bundle literal constructor
  def Lit(aVal: UInt, bVal: Bool): MyBundle = {
    val clone = cloneType
    clone.selfBind(BundleLitBinding(Map(
      clone.a -> litArgOfBits(aVal),
      clone.b -> litArgOfBits(bVal)
    )))
    clone
  }

  // Partial bundle literal constructor
  def Lit(aVal: UInt): MyBundle = {
    val clone = cloneType
    clone.selfBind(BundleLitBinding(Map(
      clone.a -> litArgOfBits(aVal)
    )))
    clone
  }
}
```

Example usage:

```scala
val outsideBundleLit = (new MyBundle).Lit(42.U, true.B)
```