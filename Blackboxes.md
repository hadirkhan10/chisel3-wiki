Chisel *blackboxes* are used to instanciate module that defined externally. It's
usefull for hardware template like differential buffer, clock pll, dcm, etc. defined by constructor.

Modules defined as a ```BlackBox``` will be declared in verilog, but no code
will be generated to define the behaviour of module.

Unlike Module, BlackBox has no implicit clocks and reset. Input/Output port name declared
in bundle will be generated with same name (without ```io_``` before).

Verilog parameters can be added in constructor with a Map of String to String.

As an example, consider defining a Xilinx differential clock buffer (IBUFDS):

```scala
import chisel3._
import chisel3.experimental._

class IBUFDS extends BlackBox(Map("DIFF_TERM" -> "TRUE",
                                  "IOSTANDARD" -> "DEFAULT")) {
  val io = IO(new Bundle {
    val O = Output(Clock())
    val I = Input(Clock())
    val IB = Input(Clock())
  })
}
```

The Verilog code generated will be :

```verilog
IBUFDS #(.DIFF_TERM("TRUE"), .IOSTANDARD("DEFAULT")) ibufds (
  .IB(ibufds_IB),
  .I(ibufds_I),
  .O(ibufds_O)
);
```
