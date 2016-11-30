Ports are used as interfaces to hardware components.  A port is simply
any `Data` object that has directions assigned to its members.

Chisel provides port constructors to allow a direction to be added
(input or output) to an object at construction time. Primitive port
constructors wrap the type of the port in `Input` or `Output`.

An example port declaration is as follows:
```scala
class Decoupled extends Bundle {
  val ready = Output(Bool())
  val data  = Input(UInt(32.W))
  val valid = Input(Bool())
}
```

After defining ```Decoupled```, it becomes a new type that can be
used as needed for module interfaces or for named collections of
wires.

The direction of an object can also be assigned at instantiation time:
```scala
class ScaleIO extends Bundle {
  val in    = (new MyFloat).asInput
  val scale = (new MyFloat).asInput
  val out   = (new MyFloat).asOutput
}
```

The methods ```asInput``` and ```asOutput``` force all modules of
the data object to the requested direction.

By folding directions into the object declarations, Chisel is able to
provide powerful wiring constructs described later.

[Prev (Bundles and Vecs)](Bundles and Vecs) [Next (Modules)](Modules)