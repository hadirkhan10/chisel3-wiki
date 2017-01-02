Chisel *BlackBoxes* are used to instantiate externally defined modules. This construct is useful
for hardware constructs that cannot be described in Chisel and for connecting to FPGA or other IP not defined in Chisel.

Modules defined as a `BlackBox` will be instantiated in the generated Verilog, but no code
will be generated to define the behavior of module.

Unlike Module, BlackBox has no implicit clock and reset. Ports declared
in the IO Bundle will be generated with the requested name (ie. no preceding `io_`).

### Parameterization

**This is an experimental feature and is subject to API change**

Verilog parameters can be passed as an argument to the BlackBox constructor.

For example, consider instantiating a Xilinx differential clock buffer (IBUFDS) in a Chisel design:

```scala
import chisel3._
import chisel3.experimental._ // To enable experimental features

class IBUFDS extends BlackBox(Map("DIFF_TERM" -> "TRUE",
                                  "IOSTANDARD" -> "DEFAULT")) {
  val io = IO(new Bundle {
    val O = Output(Clock())
    val I = Input(Clock())
    val IB = Input(Clock())
  })
}
```

In the Chisel-generated Verilog code, `IBUFDS` will be instantiated as:

```verilog
IBUFDS #(.DIFF_TERM("TRUE"), .IOSTANDARD("DEFAULT")) ibufds (
  .IB(ibufds_IB),
  .I(ibufds_I),
  .O(ibufds_O)
);
```

[Prev(Modules)](Modules) [Next(State Elements)](State Elements)