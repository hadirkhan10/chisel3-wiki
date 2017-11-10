## Release Notes 11/09/17

### Chisel3

- Default to unidoc for scaladoc generation. [#715](https://github.com/freechipsproject/chisel3/pull/715)
- Update InvalidateAPISpec tests. [#714](https://github.com/freechipsproject/chisel3/pull/714)

### Firrtl

- Add InfoSpec for checking Info propagation; add FirrtlCheckers and scalatest helpers for testing; emit source locators as comments in emitted Verilog [#682](https://github.com/freechipsproject/firrtl/pull/682)
- Fix bug emitting and reparsing ExtModule String parameters [#675](https://github.com/freechipsproject/firrtl/pull/675)

### Chisel-testers

No change from 10/27/17

### Interpreter

- Printf %b format not working Was printing true, now prints binary value in sign magnitude format fix for [Chisel 3 Issue 716](https://github.com/freechipsproject/chisel3/issues/716) [#103](https://github.com/freechipsproject/firrtl-interpreter/pull/103)

### DSPTools

Initial 1.0.0 release of the DSP generator library for both Scala 2.11 and 2.12.
Please visit the [GitHub repository](https://github.com/ucb-bar/dsptools) for instructions and examples.
- Cross compile for Scala 2.12. [#118](https://github.com/ucb-bar/dsptools/pull/118)
- Remove dependency on plotly. [#117](https://github.com/ucb-bar/dsptools/pull/117)
