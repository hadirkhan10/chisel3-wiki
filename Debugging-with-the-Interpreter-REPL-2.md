### Debugging using the Interpreter REPL Page 2

If you are here you have completed [Debugging with the Interpreter-REPL Page 1](Debugging-with-the-Interpreter-REPL-1)
You should hopefully be looking at the Repl's prompt of ```>>```

### What can you do?

Type: help 
``` 
load fileName                                    load/replace the current firrtl file
script fileName                                  load a script from a text file
run [linesToRun|all|list|reset]                  run loaded script
vcd [load|run|list|test|help]                    control vcd input file
record-vcd [<fileName>]|[done]                   firrtl_interpreter.vcd loaded script
type regex                                       show the current type of things matching the regex
poke inputPortName value                         set an input port to the given integer value
mempoke memory-instance-name index value         set memory at index to value
rpoke regex value                                poke value into ports that match regex
eval componentName                               show the computation of the component
peek componentName                               show the current value of the named circuit component
mempeek memory-instance-name index               peek memory at index
rpeek regex                                      show the current value of things matching the regex
randomize                                        randomize all inputs except reset)
poison                                           poison everything)
reset [numberOfSteps]                            assert reset (if present) for numberOfSteps (default 1)
step [numberOfSteps]                             cycle the clock numberOfSteps (default 1) times, and show state
waitfor componentName value [maxNumberOfSteps]   wait for particular value (default 1) on component, up to maxNumberOfSteps (default 100)
show [state|input|lofirrtl]                      show useful things
info                                             show information about the circuit
timing [clear|bin]                               show the current timing state
verbose [true|false|toggle]                      set evaluator verbose mode (default toggle) during dependency evaluation
eval-all [true|false|toggle]                     set evaluator to execute un-needed branches (default toggle) during dependency evaluation
allow-cycles [true|false|toggle]                 set evaluator allow combinational loops (could cause correctness problems
ordered-exec [true|false|toggle]                 set evaluator execute circuit in dependency order, now recursive component evaluation
help                                             show available commands
quit                                             exit the interpreter
```

### Show me the circuit I'm working on

Type: show lofirrtl 
``` 
circuit GCD :
  module GCD :
    input clock : Clock
    input reset : UInt<1>
    input io_value1 : UInt<16>
    input io_value2 : UInt<16>
    input io_loadingValues : UInt<1>
    output io_outputGCD : UInt<16>
    output io_outputValid : UInt<1>

    reg x : UInt<4>, clock with :
      reset => (UInt<1>("h0"), x) @[GCD.scala 21:15]
    reg y : UInt<4>, clock with :
      reset => (UInt<1>("h0"), y) @[GCD.scala 22:15]
    node _T_9 = gt(x, y) @[GCD.scala 24:10]
    node _T_10 = sub(x, y) @[GCD.scala 24:24]
    node _T_11 = asUInt(_T_10) @[GCD.scala 24:24]
    node _T_12 = tail(_T_11, 1) @[GCD.scala 24:24]
    node _T_13 = sub(y, x) @[GCD.scala 25:25]
    node _T_14 = asUInt(_T_13) @[GCD.scala 25:25]
    node _T_15 = tail(_T_14, 1) @[GCD.scala 25:25]
    node _GEN_0 = mux(_T_9, _T_12, x) @[GCD.scala 24:15]
    node _GEN_1 = mux(_T_9, y, _T_15) @[GCD.scala 24:15]
    node _GEN_2 = mux(io_loadingValues, io_value1, _GEN_0) @[GCD.scala 27:26]
    node _GEN_3 = mux(io_loadingValues, io_value2, _GEN_1) @[GCD.scala 27:26]
    node _T_17 = eq(y, UInt<1>("h0")) @[GCD.scala 33:23]
    io_outputGCD <= x
    io_outputValid <= _T_17
    x <= bits(_GEN_2, 3, 0)
    y <= bits(_GEN_3, 3, 0)
```
This is the low Firrtl representation of your circuit.
It's a beyond the scope of this tutorial to go into it, but for the most part you can infer what's going on by reading.
The names reflect the names used in the GCD.scala file, and you can even see references to the original source along
the right side of the text.
For example: ``` @[GCD.scala 21:15]```

### Show me the state of the circuit
Type: show or show stateType 
```
CircuitState 0 (FRESH)
Inputs: clock= 0, io_loadingValues=☠ 1☠, io_value1=☠ 59401☠, io_value2=☠ 23169☠, reset=☠ 1☠
Outputs: io_outputGCD=☠ 5☠, io_outputValid= 0
Registers      : x=☠ 5☠, y=☠ 11☠
FutureRegisters: x=☠ 9☠, y=☠ 1☠
Ephemera: _GEN_0=☠ 5☠, _GEN_1=☠ 6☠, _GEN_2=☠ 59401☠, _GEN_3=☠ 23169☠, _T_10=☠ -6☠, _T_11=☠ 26☠, _T_12=☠ 10☠, _T_13=☠ 6☠, _T_14=☠ 6☠, _T_15=☠ 6☠, _T_17= 0, _T_9= 0
Memories
```
This describes the state of the circuit.  Following *CircuitState* is the clock cycle.
Inputs, Outputs, Registers are pretty obvious.  The FutureRegister reflect what the value of the register will be on
the next cycle.  The ephemera are other intermediate signals.  Low Firrtl is quite verbose, and current practice
is to create an intermediate wire for every operation of the circuit.

>You have probably noticed that all the values are bracketed by small skulls ``` _GEN_3=☠ 23169☠``` this is the Repl's
whimsical way of showing these values have not been assigned to from an initialized value, they are *poisoned*.

### Try Reset
>You can combine multiple commands on one line using the ; (semi-colon), this will make things a little more succinct
as we move forward.
Type: reset 3 ; show
```
CircuitState 3 (STALE)
Inputs: clock= 0, io_loadingValues=☠ 1☠, io_value1=☠ 59401☠, io_value2=☠ 23169☠, reset= 0
Outputs: io_outputGCD=☠ 9☠, io_outputValid= 0
Registers      : x=☠ 9☠, y=☠ 1☠
FutureRegisters: x=☠ 9☠, y=☠ 1☠
Ephemera: _GEN_2=☠ 59401☠, _GEN_3=☠ 23169☠, _T_17= 0
``` 
Reset 3 will set the reset to 1 for 3 cycles (default is 1) and then set it back to 0.
Nothing much changed here.  In GCD nothing depended on the reset value.
Often registers will be given reset values, but out simple circuit does not do it.

### Repl Command line Interface
Before we go to far, we want to note that the Repl uses **jline**.
It's a relatively complete interface that provides command line history and command (and often command argument) completion.
There's a few glitches and a few omissions but using your arrow keys can save you a lot of typing.
Let us know if you find any annoying ones.

### Poking
Ok, it's time to get serious about running our circuit.
We need to get some values pushed into the inputs and figure out what is going on.
Looking at our test file, we see more or less what we need to do.  The test harness does the following:
```scala
      poke(gcd.io.value1, i)
      poke(gcd.io.value2, j)
      poke(gcd.io.loadingValues, 1)
      step(1)
      poke(gcd.io.loadingValues, 0)
```
We can do this in the Repl like this:
Let's do the first three instruction using 4 as both values to compute GCD on (because we know that answer):
```
poke io_value1 4
poke io_value2 4
poke io_loadingValues 1
show
```
We see
``` 
CircuitState 4 (STALE)
Inputs: clock= 0, io_loadingValues= 1, io_value1= 4, io_value2= 4, reset= 0
Outputs: io_outputGCD=☠ 9☠, io_outputValid= 0
Registers      : x=☠ 9☠, y=☠ 1☠
FutureRegisters: x=☠ 9☠, y=☠ 1☠
Ephemera: _GEN_2=☠ 59401☠, _GEN_3=☠ 23169☠, _T_17= 0
```
We can see that what have poked is visible in the input line, so that's good.
>Note: the **(Stale)** after CircuitState 4.
This means the Repl does not immediately propagate values through the circuit in a ```poke```.
Once the values have been propagate, by a ```step``` or ```peek``` the state will be indicated as **(FRESH)**

### Step
Type: step ; show
Now things are looking a lot cleaner, as all the poisons have been cleared out as our inputs have propagated through
the circuit.
``` 
CircuitState 5 (FRESH)
Inputs: clock= 0, io_loadingValues= 1, io_value1= 4, io_value2= 4, reset= 0
Outputs: io_outputGCD= 4, io_outputValid= 0
Registers      : x= 4, y= 4
FutureRegisters: x= 4, y= 4
Ephemera: _GEN_2= 4, _GEN_3= 4, _T_17= 0
```
The ```x``` and ```y``` registers have been initialized to our inputs.
We know of course that our circuit won't start calculating the GCD until we clear the ```io_loadingValues```, so let's
do that.

### Getting our first result
Type: poke io_loadingValues 0 ; step ; show
```
CircuitState 6 (FRESH)
Inputs: clock= 0, io_loadingValues= 0, io_value1= 4, io_value2= 4, reset= 0
Outputs: io_outputGCD= 4, io_outputValid= 1
Registers      : x= 4, y= 0
FutureRegisters: x= 4, y= 0
Ephemera: _GEN_0= 4, _GEN_1= 0, _GEN_2= 4, _GEN_3= 0, _T_10= 4, _T_11= 4, _T_12= 4, _T_17= 1, _T_9= 1
```
Sweet, ```io_outputValid``` is one indicating the GCD has been calculated and ```io_outputGCD``` is 4.
Which is correct.
So now we have to figure out what's wrong with the test of our *munged* GCD.

### What broke?
Let's go back and repeat our failed test with verbose output
>In the interest of brevity, I'm doing this in a separate terminal, so I can keep my Repl running, but I could just as easily stopped and
started it again afterwards. 

In *another* sbt, I run
``` 
testOnly gcd.GCDTester -- -z "running with --is-verbose"
```
and searching a bit through the output, we find
```
[info] [0.090] EXPECT AT 316   io_outputValid got 1 expected 1 PASS
[info] [0.090]   POKE io_value1 <- 13
[info] [0.091]   POKE io_value2 <- 29
[info] [0.091]   POKE io_loadingValues <- 1
[info] [0.091] STEP 316 -> 317
[info] [0.091]   POKE io_loadingValues <- 0
[info] [0.091] STEP 317 -> 326
[info] [0.092] EXPECT AT 326   io_outputGCD got 13 expected 1 FAIL
[info] [0.092] EXPECT AT 326   io_outputValid got 1 expected 1 PASS
```
So let's try the inputs.  
Back in our Repl window.
Type:
``` 
poke io_value1 13
poke io_value2 13
poke io_loadingValues 1
step 
show
```
and we see:
``` 
Inputs: clock= 0, io_loadingValues= 1, io_value1= 13, io_value2= 29, reset= 0
Outputs: io_outputGCD= 13, io_outputValid= 0
Registers      : x= 13, y= 13
```
So we see something weird here.
```y``` should be assigned the value 29 from ```io_value2``` but is 13.  
This particular case might be a bit confusing since ```y``` seems to have the value for ```x```, but it's a coincidence.
The short story is we have found the problem, and we would probably pretty quickly figure it out from here.
One way to probe it would be to stick the 29 directly into ```y```.
``` 
poke y 29 ; show
```
and what comes back is:
``` 
Error: exception error: ConcreteUInt(29, 4) bad width 4 needs 5 firrtl_interpreter.InterpreterException: error: ConcreteUInt(29, 4) bad width 4 needs 5
```
The repl checks all pokes against the width's specified in the lo firrtl file.
It's telling us the register ```y``` is not big enough to hold the value 29.

Another way to confirm this is to type: ```type y```
```
type y 13.U<4>
```
which indicates that y currently has the value 13 and is an unsigned integer of width 4 (a 4 bit number)

#       The specific problem has been identified.

## A couple more useful Repl commands

### eval show how a given wire gets it's value
```eval``` can show the complete calculation of how the current value of a wire is computed.  
To try it on our circuit.
Type: ```eval y```
The result:
``` 
resolve dependencies
    evaluate     UInt<1>("h0")
    evaluated     0.U<1>
    evaluate     y <= bits(_GEN_3, 3, 0)  @[GCD.scala 22:15]
      evaluate     _GEN_3
          evaluate     _GEN_3 <= mux(io_loadingValues, io_value2, _GEN_1)  @[GCD.scala 27:26]
            evaluate     io_loadingValues
            evaluated     1.U<1>
            evaluate     io_value2
            evaluated     29.U<16>
          evaluated    _GEN_3 <= 29.U<16>
      evaluated     29.U<16>
    evaluated    y <= 13.U<4>
```
Is another way of seeing the failure to propagate ```io_value2``` to ```y```
>The computation of ```y``` is dependent on the state of other variables so, for example, we will see a different
calculation if we change the value of ```io_loadingValues``` and ```eval``` again
```
poke io_loadingValues 0 ; eval y
```
gives back:
``` 
resolve dependencies
    evaluate     UInt<1>("h0")
    evaluated     0.U<1>
    evaluate     y <= bits(_GEN_3, 3, 0)  @[GCD.scala 22:15]
      evaluate     _GEN_3
          evaluate     _GEN_3 <= mux(io_loadingValues, io_value2, _GEN_1)  @[GCD.scala 27:26]
            evaluate     io_loadingValues
            evaluated     0.U<1>
            evaluate     _GEN_1
                evaluate     _GEN_1 <= mux(_T_9, y, _T_15)  @[GCD.scala 24:15]
                  evaluate     _T_9
                      evaluate     _T_9 <= gt(x, y)  @[GCD.scala 24:10]
                        evaluate     x
                        evaluated     13.U<4>
                        evaluate     y
                        evaluated     13.U<4>
                      evaluated    _T_9 <= 0.U<1>
                  evaluated     0.U<1>
                  evaluate     _T_15
                      evaluate     _T_15 <= tail(_T_14, 1)  @[GCD.scala 25:25]
                        evaluate     _T_14
                            evaluate     _T_14 <= asUInt(_T_13)  @[GCD.scala 25:25]
                              evaluate     _T_13
                                  evaluate     _T_13 <= sub(y, x)  @[GCD.scala 25:25]
                                    evaluate     y
                                    evaluated     13.U<4>
                                    evaluate     x
                                    evaluated     13.U<4>
                                  evaluated    _T_13 <= 0.S<5>
                              evaluated     0.S<5>
                            evaluated    _T_14 <= 0.U<5>
                        evaluated     0.U<5>
                      evaluated    _T_15 <= 0.U<4>
                  evaluated     0.U<4>
                evaluated    _GEN_1 <= 0.U<4>
            evaluated     0.U<4>
          evaluated    _GEN_3 <= 0.U<16>
      evaluated     0.U<16>
    evaluated    y <= 0.U<4>
```
Which shows how the circuit subtracts the smaller of ```x``` and ```y``` from the larger.
>Note: the ```eval``` calculation does not go deeper when it hits a register.
Except, of course, if you eval a register.

### waitfor, run until something happens
```waitfor``` lets a circuit run until a simple condition is met.
If we were running an unbroken GCD we could wait on the ```io_outputVaid``` with out having to manually step through watch it visually.
As long as we keep our values small enough we can run it in the same session we have going now.
Consider the following commands and responses:
``` 
firrtl>> poke io_loadingValues 1 ; poke io_value1 10 ; poke io_value2 15 ; step
step 1 in 0.001061492
firrtl>> poke io_loadingValues 0
firrtl>> waitfor io_outputValid 1 100
io_outputValid == value 1 in 3 cycles
firrtl>> peek io_outpugGCD
peek io_outputGCD  5
```
We poke 10 and 15 and wait for output valid, and when it is after 3 cycles the correct value 5 is on the output.

### What's next
Check out [Debugging with the Interpreter-REPL VCD Page 3](Debugging-with-the-Interpreter-REPL-3)