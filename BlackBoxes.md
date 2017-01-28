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

### Providing Implementations for Blackboxes
The Chisel Execution Harness see [Running Stuff](Running-Stuff) provides the following ways of delivering the code underlying the blackbox.  Consider the following blackbox that adds two real numbers together.  The numbers are represented in chisel3 as 64-bit unsigned integers.
```scala
class BlackboxRealAdd extends BlackBox {
  val io = IO(new Bundle() {
    val in1 = Input(UInt(64.W))
    val in2 = Input(UInt(64.W))
    val out = Output(UInt(64.W))
  })
}
```
The implementation is described by the following verilog
```
module BBFAdd(
    input  [63:0] in1,
    input  [63:0] in2,
    output reg [63:0] out
);
  always @* begin
  out <= $realtobits($bitstoreal(in1) + $bitstoreal(in2));
  end
endmodule
```
In order to deliver the verilog snippet above to the backend simulator, chisel3 provides the following tools based on the chisel/firrtl [annotation system](Annotations-Extending-Chisel-and-Firrtl).  Add the trait ```HasBlackBoxResource``` to the declaration, and then call a function in the body to say where the system can find the verilog.  The Module now looks like
```scala
class BlackboxRealAdd extends BlackBox with HasBlackBoxResource {
  val io = IO(new Bundle() {
    val in1 = Input(UInt(64.W))
    val in2 = Input(UInt(64.W))
    val out = Output(UInt(64.W))
  })
  setResource("/real_math.v")
}
```
The verilog snippet above gets put into a resource file names ```real_math.v```.  What is a resource file?  It comes from a java convention of keeping files in a project that are automatically included in library distributions.  In a typical chisel3 project [see chisel-template](https://github.com/ucb-bar/chisel-template) this would be a directory in the source hierarchy
```
src/main/resources/real_math.v
```



[Prev(Modules)](Modules) [Next(State Elements)](State Elements)