Chisel has a number of new features that are worth checking out.  This page is an informal list of these features and projects.

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
