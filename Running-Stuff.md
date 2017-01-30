### The Execution Pattern
It is a bit difficult to describe what it means to run chisel.  Chisel is not a monolithic application. It is more analogous to a collections of tools in the vein of a compiler family like gcc.  At first, when developing a new chisel circuit it is not that important what is going on under the hood.  A circuit is instantiated in the context of a test harness, usually in the form of the PeekPokeTester family and invoked via boilerplate like the following
```
   "here we assign to a F8.1 from a F8.3" in {
      chisel3.iotesters.Driver(() => new FixedPrecisionChanger(8, 3, 8, 1)) { c=>
        new FixedPointTruncatorTester(c, 6.875, 6.5)
      } should be (true)
    }
```
This article is about the chisel3.iotesters.Driver above does and how it does it, and what other members of the execution pattern do and how they do it.

### Executing chisel code written in scala.
Because Scala is based on Java and thus runs in a [JVM](https://www.google.com/search?q=jvm&oq=jvm) environment, it's a little more complicated to run than say, ```ls```.  To make this easier we use ```sbt``` which helps manage starting a JVM, managing library dependencies and running things that can be run.  *Can be run* in scala means that there is a main method defined in a companion object, though it can also be accomplished via the test harness shown above and objects that extend App.  The goal of the *Execution Pattern* is to describe a standard way of invoking various parts of the chisel toolchain particularly with respect to providing command line or internal control of those toolchain elements

### Execution Pattern Project Hierarchy 
The hierarchy currently looks like

| Level        | Project/Tool | Options |
| ---------        | --------- | ------- |
| highest | chisel-testers | See: [TesterOptions](/ucb-bar/chisel-testers/blob/master/src/main/scala/chisel3/iotesters/TesterOptions.scala) |
| - | firrtl-interpreter, firrtl-repl | See: [Interpreter Options](/ucb-bar/firrtl-interpreter/blob/master/src/main/scala/firrtl_interpreter/Driver.scala |
| - | chisel3 | See: [ChiselExecutionOptions](/ucb-bar/chisel3/blob/master/src/main/scala/chisel3/ChiselExecutionOptions.scala) |
| lowest | firrtl | See: [CommonOptions, FirrtlOptions](/ucb-bar/firrtl/blob/master/src/main/scala/firrtl/ExecutionOptionsManager.scala) |

where higher levels depend on and, typically, call the layers below.  For example when running a test, the tester is 
invoked, which then invokes chisel3 to generate the circuit, which invokes firrtl to compile the circuit into 
low firrtl. After low firrtl has been generated, the firrtl-interpreter is invoked in order to execute 
the test on the device under test.  

Invocation here means that a execute method of a Driver companion object has been called.  Those methods can in as 
necessary or as controlled by the options can invoke the execute methods of lower level Driver objects.

### Execution Pattern Code
The execute methods of a Driver companion object (typically documented as Driver#execute) is built with a set of
scala base classes currently housed in firrtl. A driver will typically look have the following basic components

#### A Driver companion object with two execution methods
```scala
object Driver {
  def execute(optionsManager: ..., ): ExecutionResult {
    ...  
  }
  def execute(args: Array[String]: ..., ): ExecutionResult {
    ...  
  }
}
```
The args based version of the execute method is amenable to command line arguments.  The optionsManager version has additional capabilities for passing along data between toolchain components so it is used when calling a Driver#execute of a lower level.  Generally for convenience, the string based method parses the args, generates an options manager from them and the calls the options manager version.

#### An options manager
The optionManager is a container class that can hold one more configuration classes.
```scala
def execute(
      optionsManager: ExecutionOptionsManager with HasChiselExecutionOptions with HasFirrtlOptions,
      dut: () => Module): ChiselExecutionResult = {
```  

#### Has*ComposableOptions*
A given declaration of a options manager extends one more traits that implement ```ComposableOptions```. In the example above from chisel3 it extends ```HasChiselExecutionOptions``` and ```HasFirrtlOptions`. Each composable options trait includes the code to add the configuration associated with it.  The ```HasChiselExecutionOptions``` is very simple and looks like
```scala
trait HasChiselExecutionOptions {
  self: ExecutionOptionsManager =>

  var chiselOptions = ChiselExecutionOptions()

  parser.note("chisel3 options")

  parser.opt[Unit]("no-run-firrtl")
    .abbr("chnrf")
    .foreach { _ =>
      chiselOptions = chiselOptions.copy(runFirrtlCompiler = false)
    }
    .text("Stop after chisel emits chirrtl file")
}
```
- The ```self: ExecutionOptionsManager =>``` says this can only extends subclasses of ExecutionOptionsManger.
- The ```var chiselOptions = ChiselExecutionOptions()``` adds a chiselOptions entry to the ExecutionOptionsManager.
- The remaining code connects command line parsing to the values stored in the ChiselOptions. The parser used is a library provided by [scopt](https://github.com/scopt/scopt) 

#### Composable Options Instances
The ChiselOptions are also quite simple and look like
```scala
case class ChiselExecutionOptions(
                                   runFirrtlCompiler: Boolean = true
                                   // var runFirrtlAsProcess: Boolean = false
                                 ) extends ComposableOptions

```
They extend ComposableOptions and are usually implemented as a [case class](http://docs.scala-lang.org/tutorials/tour/case-classes.html) which cannot be altered and so must be copied with new values set.  This is the reason for the construct 
``` 
    .foreach { _ =>
      chiselOptions = chiselOptions.copy(runFirrtlCompiler = false)
    }
```
in the parsing code.
> The base classes live in firrtl because, as the lowest member of the hierarchy, all higher projects depend and
> thus have access to its members

#### Execution results
And finally the execute method should return either a SuccessResult and associated data or a FailureResult and associated data.  Both Results should extend a common trait appropriate to the tool.  Chisel3's looks like
```scala
/**
  * This family provides return values from the chisel3 and possibly firrtl compile steps
  */
trait ChiselExecutionResult

/**
  *
  * @param circuitOption  Optional circuit, has information like circuit name
  * @param emitted            The emitted Chirrrl text
  * @param firrtlResultOption Optional Firrtl result, @see ucb-bar/firrtl for details
  */
case class ChiselExecutionSucccess(
                                  circuitOption: Option[Circuit],
                                  emitted: String,
                                  firrtlResultOption: Option[FirrtlExecutionResult]
                                  ) extends ChiselExecutionResult

/**
  * Getting one of these indicates failure of some sort
 *
  * @param message  a clue perhaps will be provided in the here
  */
case class ChiselExecutionFailure(message: String) extends ChiselExecutionResult
```
In Chisel3 case it uses the return result to get the circuit, any chisel firrtl (chirrtl) and possibly the result of the firrtl compilation

### What are all the options.
The complete list of options is in flux. New features and options to control them are continuously added to the toolchain.  The options available at any one time are depend on which tool is being called, and consists of that tools plus any lower level tools it might interact with.  The easiest way of getting a current list is to call A driver's execute methods passing it the command line option ```--help``` and reading the output.  Key in the following to see all the options from chisel-testers down to firrtl.
```scala
package xyz
import chisel3.iotesters.PeekPokeTester
import chisel3._

class Dummy extends Module { val io = IO(new Bundle {}) }
class DummyTester(c: Dummy) extends PeekPokeTester(c) {}
object Dummy extends App { iotesters.Driver.execute(args, () => new Dummy){ c => new DummyTester(c) }}
```
Then from the command line
```
bash> sbt 'run-main xyz.Dummy --help'
```
This will return a reasonably long list of information.  
```
Usage: chisel-testers [options]

common options
  -tn <top-level-circuit-name> | --top-name <top-level-circuit-name>
        This options defines the top level circuit, defaults to dut when possible
  -td <target-directory> | --target-dir <target-directory>
        This options defines a work directory for intermediate files, default is .
  -ll <Error|Warn|Info|Debug|Trace> | --log-level <Error|Warn|Info|Debug|Trace>
        This options defines a work directory for intermediate files, default is .
  -cll <FullClassName:[Error|Warn|Info|Debug|Trace]>[,...] | --class-log-level <FullClassName:[Error|Warn|Info|Debug|Trace]>[,...]
        This options defines a work directory for intermediate files, default is .
  -ltf | --log-to-file
        default logs to stdout, this flags writes to topName.log or firrtl.log if no topName
  -lcn | --log-class-names
        shows class names and log level in logging output, useful for target --class-log-level
  --help
        prints this usage text
tester options
  -tbn <firrtl|verilator|vcs> | --backend-name <firrtl|verilator|vcs>
        backend to use with tester, default is firrtl
  -tigv | --is-gen-verilog
        has verilog already been generated
  -tigh | --is-gen-harness
        has harness already been generated
  -tic | --is-compiling
        has harness already been generated
  -tiv | --is-verbose
        set verbose flag on PeekPokeTesters, default is false
  -tdb <value> | --display-base <value>
        provides a seed for random number generator, default is 10
  -ttc <value> | --test-command <value>
        run this as test command
  -tlfn <value> | --log-file-name <value>
        write log file
  -twffn <value> | --wave-form-file-name <value>
        wave form file name
  -tts <value> | --test-seed <value>
        provides a seed for random number generator
firrtl-interpreter-options
  -fiwv | --fint-write-vcd
        writes vcd execution log, filename will be base on top
  -fivsuv | --fint-vcd-show-underscored-vars
        vcd output by default does not show var that start with underscore, this overrides that
  -fiv | --fint-verbose
        makes interpreter very verbose
  -fioe | --fint-ordered-exec
        operates on dependencies optimally, can increase overhead, makes verbose mode easier to read
  -fiac | --fr-allow-cycles
        allow combinational loops to be processed, though unreliable, default is false
  -firs <long-value> | --fint-random-seed <long-value>
        seed used for random numbers generated for tests and poison values, default is current time in ms
  -fimed <long-value> | --fint-max-execution-depth <long-value>
        depth of stack used to evaluate expressions
  -fisfas | --show-firrtl-at-load
        compiled low firrtl at firrtl load time
  -filcol | --run-lower-compiler-on-load
        run lowering compuler when firrtl file is loaded
chisel3 options
  -chnrf | --no-run-firrtl
        Stop after chisel emits chirrtl file
firrtl options
  -i <firrtl-source> | --input-file <firrtl-source>
        use this to override the default input file name , default is empty
  -o <output> | --output-file <output>
        use this to override the default output file name, default is empty
  -faf <output> | --annotation-file <output>
        use this to override the default annotation file name, default is empty
  -ffaaf | --force-append-anno-file
        use this to force appending annotation file to annotations being passed in through optionsManager
  -X <high|low|verilog> | --compiler <high|low|verilog>
        compiler to use, default is verilog
  --info-mode <ignore|use|gen|append>
        specifies the source info handling, default is append
  -fil <circuit>[.<module>[.<instance>]][,..], | --inline <circuit>[.<module>[.<instance>]][,..],
        Inline one or more module (comma separated, no spaces) module looks like "MyModule" or "MyModule.myinstance
  -firw <circuit> | --infer-rw <circuit>
        Enable readwrite port inference for the target circuit
  -frsq -c:<circuit>:-i:<filename>:-o:<filename> | --repl-seq-mem -c:<circuit>:-i:<filename>:-o:<filename>
        Replace sequential memories with blackboxes + configuration file
  -clks -c:<circuit>:-m:<module>:-o:<filename> | --list-clocks -c:<circuit>:-m:<module>:-o:<filename>
        List which signal drives each clock of every descendent of specified module

```