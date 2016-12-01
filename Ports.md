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

By folding directions into the object declarations, Chisel is able to
provide powerful wiring constructs described later.

[Prev (Bundles and Vecs)](Bundles and Vecs) [Next (Modules)](Modules)