## Release Notes 10/27/17

### Chisel3

- Invalidate API [#645](https://github.com/freechipsproject/chisel3/pull/645)

  Prior to this pull request, chisel automatically generated a `DefInvalid` for `Module IO()`,
  and each `Wire()` definition.
  This made it difficult to detect cases where output signals were never driven.
  Chisel now supports a `DontCare` element, which may be connected to an output signal,
  indicating that that signal is intentionally not driven.
  Firrtl will complain with a "not fully initialized" error if a signal is not driven
  by hardware or connected to a `DontCare`.
  
  This feature is controlled by `CompileOptions.explicitInvalidate` and is set to `false`
  in `NotStrict` (Chisel2 compatibility mode), and `true` in `Strict` mode.
  
  Please see the corresponding [API tests](https://github.com/freechipsproject/chisel3/blob/master/src/test/scala/chiselTests/InvalidateAPISpec.scala)
  for examples.

### Firrtl

No change from 10/06/17

### Chisel-testers

- Running verilator broken if port name contains dutname as substring [#171](https://github.com/freechipsproject/chisel-testers/pull/171)
- Ensure all output signals are driven - prep for InvalidateAPI. [#175](https://github.com/freechipsproject/chisel-testers/pull/175)

### Interpreter

- Support execution of the interactive Firrtl Repl on windows [#100](https://github.com/freechipsproject/firrtl-interpreter/pull/100).

### DSPTools

Initial 1.0.0 release of the DSP generator library.
Please visit the [GitHub repository](https://github.com/ucb-bar/dsptools) for instructions and examples.
*Note:* Currently, we only support Scala 2.11 versions of this library - we don't have 2.12 versions of all of its dependencies.
We continue to release Scala 2.11 versions of the BIG4 (required by DSPTools).

If you wish to experiment with the DSPTools library, you will need to use Scala 2.11.
- Update DspTesterUtilities.scala and add DspTesterSpec.scala [#113](https://github.com/ucb-bar/dsptools/pull/113)
- InvalidateAPI - connect more outputs. Avoid "incompletely initialized" warning from FIRRTL when InvalidateAPI goes in. [#110](https://github.com/ucb-bar/dsptools/pull/110)
