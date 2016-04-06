This page is a starting point for recording common and not so common problems in developing with Chisel3.  In particular, those situations where there is a work around that will keep you going.

### Bundles may not contain function defs with no arguments.
The following example will produce a firrtl error
```scala
class MyModule extends Module {
  val x = UInt(INPUT)
  def nonZero: Bool = {
    x > UInt(0)
  }
}
```
```
firrtl.passes.CheckHighForm$NotUniqueException: Example.fir@10.4: [module MyModule] Reference nonZero does not have a unique name.
```
The work-around: Give the function a dummy argument with a default value
```scala
class MyModule extends Module {
  val x = UInt(INPUT)
  def nonZero(dummy: Int = 0): Bool = {
    x > UInt(0)
  }
}
```

