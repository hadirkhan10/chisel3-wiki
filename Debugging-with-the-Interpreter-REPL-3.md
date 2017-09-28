### Debugging using the Interpreter REPL VCD Page 3
## VCD Files and the Repl

If you are here you have completed 
[Debugging with the Interpreter-REPL Page 1](Debugging-with-the-Interpreter-REPL-1) and 
[Debugging with the Interpreter-REPL Page 2](Debugging-with-the-Interpreter-REPL-2).

This part of the tutorial describes using VCD files with Repl.  
The Repl can produce a VCD file during execution or it can consume one,
i.e. use a pre-existing VCD file to set the inputs and control stepping.  
We assume that we are still in the process of debugging a deliberately broken GCD file as we 
were in the earlier pages of this tutorial.

## Creating a VCD
### Using command line flags
The Repl can create a vcd file using the capabilities of the underlying firrtl-interpreter, simply through the
command line flags we saw when running help in page 1 of this section.
The flags
``` 
  -fiwv, --fint-write-vcd  writes vcd execution log, filename will be base on top
  -fivsuv, --fint-vcd-show-underscored-vars
                           vcd output by default does not show var that start with underscore, this overrides that
```
can be used as part of the command line and will automatically create a VCD file in the target directory.
But the Repl has additional commands that allow you to turn on and off the creation of a VCD file
during execuction.

### Using Repl commands
At any time while running the Repl a VCD dump can be started via the
```
record-vcd gcd-run-1.vcd
```
After this all circuit changes that occur, from poke, step etc will be recorded.
>The suffix .vcd will **NOT** be automatically appended so, you should probably do it yourself, as was done above.
 
In order to flush the log to disk the command
```$xslt
record-vcd done
```
must be run.  The file, in this case *gcd-run-1.vcd* will be placed in the local directory.

## Using a VCD file as an input script.
### Loading a vcd file.
There are two ways to make a VCD file available as a driving input to the REPL.  

#### From command line
There are two ways to execute the VCD file.  The first can be used if you used the command line options
```--fint-write-vcd``` or its barely shorter abbreviation ```-fiwv```.
If you used these then the VCD file should be in the default target directory with a name derived from the
Module under test with a VCD suffix.

Just specify ```--fr-use-vcd-script``` when starting the REPL.
Alternatively you can use the ```fr-vcd-script-override <file-name>``` to specify the exact name of the VCD file you want.

Back to our example.  Let's go back and re-run the GCD tests and have it generate a VCD file for us.
```
test:runMain gcd.GCDRepl --fr-use-vcd-script
```
From another shell in the root of the repo, I can see the files created by the above execution in a sub-directory of
test_run_dir with a very ugly name (this name may well be different on your system, though it should be similar)
``` 
ls test_run_dir/gcd.GCDMain2061991994/
GCD.anno	GCD.fir		GCD.lo.fir	GCD.vcd
```
We can see that the VCD file was created.  So let's fire up the the Repl again using 
```
test:runMain gcd.GCDRepl --vcdScriptOverride test_run_dir/gcd.GCDMain2061991994/GCD.vcd
```
which displays
```
Total FIRRTL Compile Time: 273.0 ms
End of dependency graph
Circuit state created
Flags: allow-cycles: false ordered-exec: false
dependency graph 21 elements 36 statements 48 nodes
loaded vcd script file ./GCD.vcd
vcd:             0.2
timescale:       1ps
date:            2017-09-26T20:37+0000
unique wires:    9
events:          2214
```
the ordinary startup junk has more information now about the contents of the VCD file.  

#### from the Repl prompt
Alternatively we could load it from the prompt with the following command.
``` 
vcd load test_run_dir/gcd.GCDMain2061991994/GCD.vcd
```
It's basically the same with the same feedback but we have to know the exact name of the file.

### Running the VCD
#### The VCD command submenu
You can see the available commands with
```
vcd help
```
which gives us
```
vcd run                    run one event
vcd run to step            run event until a step occurs
vcd run to <event-number>  run up to given event-number
vcd run <number-of-events> run this many events
vcd run set <event>        set next event to run
vcd run test               test outputs after each run command
vcd run notest             do not test outputs after each run command
vcd run verbose            run in verbose mode (the default)
vcd run noverbose          do not run in verbose mode
vcd list
vcd list all
vcd list <event-number>
vcd list <event-number> <window-size>
```
#### vcd list
``` 
vcd list
```
Will show you the next 10 events in the vcd file.
Running it again will show you the next 10 and so forth.
>There is not a nice relationship between vcd events and interpreter cycles unfortunately,
although they are roughly 2 events per cycle.
```
vcd list 23
```
Will list starting at event 23.  
>Don't use ```list all```.
It will run a lot, and you can't stop it without killing your session in *sbt*

#### vcd run
``` 
vcd run ; show
```
yields
``` 
CircuitState 0 (FRESH)
Inputs: clock= 0, io_loadingValues=☠ 0☠, io_value1=☠ 14440☠, io_value2=☠ 23499☠, reset= 1
Outputs: io_outputGCD=☠ 2☠, io_outputValid= 0
Registers      : x=☠ 2☠, y=☠ 10☠
FutureRegisters: x= 2, y= 8
Ephemera: _GEN_0=☠ 2☠, _GEN_1=☠ 8☠, _GEN_2= 2, _GEN_3= 8, _T_13=☠ 8☠, _T_14=☠ 8☠, _T_15=☠ 8☠, _T_17= 0, _T_9=
```
Looks, like about all that's happened is reset has been set to 1.  
I happen to know that the interpreter test harness effectively runs reset 10 at spin up so lets run up to that point using
``` 
vcd run to 32 ; show
```
will take you to the a finished GCD calculation.
```
recording: io_outputValid to 1
poke io_value2 15
poke io_loadingValues 1
poke clock 0
Testing outputs Event: 33 Time: 33 ====================
output io_outputGCD is 1 expected 1
output io_outputValid is 1 expected 1
firrtl>> show

CircuitState 31 (FRESH)
Inputs: clock= 0, io_loadingValues= 1, io_value1= 1, io_value2= 15, reset= 0
Outputs: io_outputGCD= 1, io_outputValid= 1
Registers      : x= 1, y= 0
FutureRegisters: x= 1, y= 15
Ephemera: _GEN_2= 1, _GEN_3= 15, _T_17= 1
```
Note: As the VCD script is executed it sets the inputs and compares the registers and outputs of the circuit.
Sometimes it can take a while to get things in sync, and you will get non-fatal warning messages.
In our stilted case, we are interested in getting the inputs to a certain values using the VCD script.
You may want to turn of the comparison with ```vcd run notest```



 

