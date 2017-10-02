## Release Notes 09/27/17

### Chisel3

- Disallow assignment to op results [Chisel PR #698](https://github.com/freechipsproject/chisel3/pull/698)
- Generate aggregate coverage but publish a single Jar [Chisel PR #695](https://github.com/freechipsproject/chisel3/pull/695)
- Bump testing coverage for Scala 2.12 support [Chisel PR #693](https://github.com/freechipsproject/chisel3/pull/693)

### Firrtl

- Fix string lit [Firrtl PR #663](https://github.com/freechipsproject/firrtl/pull/663)
- Some ScalaDoc warning fixes [Firrtl PR #662](https://github.com/freechipsproject/firrtl/pull/662)
- Fix problem where wrong verilog file is used. [Firrtl PR #661](https://github.com/freechipsproject/firrtl/pull/661)
- Provide mechanism so that programs can optionally [Firrtl PR #660](https://github.com/freechipsproject/firrtl/pull/660)
- Create way of collecting program arguments in Driver [Firrtl PR #659](https://github.com/freechipsproject/firrtl/pull/659)
- Bump testing coverage for Scala 2.12 support. [Firrtl PR #656](https://github.com/freechipsproject/firrtl/pull/656)

### Chisel-testers

- executeFirrtlRepl did not pass along args [Chisel-testers PR #172](https://github.com/freechipsproject/chisel-testers/pull/172)
- Add support for peek and poking memories to the interpreter backend. [Chisel-testers PR #169](https://github.com/freechipsproject/chisel-testers/pull/169)
- Enable testing coverage. [Chisel-testers PR #168](https://github.com/freechipsproject/chisel-testers/pull/168)

### Interpreter

- A small package of changes that make the repl more useful. - Give some feedback when a firrtl file is loaded. - Give some feedback when a vcd script is loaded - Now can load vcd in repl, it used to require command line - New eval command shows how wire got it's current value - Repl command can be separated with ; colon, very useful with history [Firrtl-interpreter PR #98](https://github.com/freechipsproject/firrtl-interpreter/pull/98)
- Will now write VCD log file even if errors or exceptions occur If write VCD is enabled and interpreter gets and exception or a stop occurs in the circuit. Write out the VCD file if at all possible. [Firrtl-interpreter PR #97](https://github.com/freechipsproject/firrtl-interpreter/pull/97)
- Make backward compatibility with black box fix better. DspTools won't need to be changed right away. Fix printstatement bug introduced by firrtl change [Firrtl-interpreter PR #96](https://github.com/freechipsproject/firrtl-interpreter/pull/96)
- BlackBoxImpls instance must be distinct due to internal state This fix More generally adds instance name as prefix to BB inputs; Allows each black box to be a single instance; Deprecates the fullName function in BlackBoxImplementation [Firrtl-interpreter PR #95](https://github.com/freechipsproject/firrtl-interpreter/pull/95)
- Convert to new firrtl StringLit. [Firrtl-interpreter PR #94](https://github.com/freechipsproject/firrtl-interpreter/pull/94)
- Add repl commands mempeek and mempoke Would be nice to have block versions of these but at least these give access. One could construct script files to load blocks of memories using mempoke [Firrtl-interpreter PR #93](https://github.com/freechipsproject/firrtl-interpreter/pull/93)
- Add pokeMemory and peekMemory to allow testers to directly set values in memories.  Added Memory#forceWrite to make this easier to do. Add test for memory poking and peeking, [Firrtl-interpreter PR #92](https://github.com/freechipsproject/firrtl-interpreter/pull/92)
- Enable testing coverage. [Firrtl-interpreter PR #91](https://github.com/freechipsproject/firrtl-interpreter/pull/91)

### DSPTools

Initial SNAPSHOT release of the DSP generator library.
Please visit the [GitHub repository](https://github.com/ucb-bar/dsptools) for instructions and examples.
*Note:* Currently, we only support Scala 2.11 versions of this library - we don't have 2.12 versions of all of its dependencies.
We continue to release Scala 2.11 versions of the BIG4 (required by DSPTools).
If you wish to experiment with the DSPTools library, you will need to use Scala 2.11.
- Update sbt version used for build. [Dsptools PR #106](https://github.com/ucb-bar/dsptools/pull/106)
- Fix up Sign, add some tests [Dsptools PR #104](https://github.com/ucb-bar/dsptools/pull/104)
- Enable testing coverage. [Dsptools PR #103](https://github.com/ucb-bar/dsptools/pull/103)
- Importing dsptools.numbers._ now includes implicits by default [Dsptools PR #102](https://github.com/ucb-bar/dsptools/pull/102)
- Split BlackBoxFloat.v into a file for each module [Dsptools PR #101](https://github.com/ucb-bar/dsptools/pull/101)
