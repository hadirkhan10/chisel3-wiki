## Release Notes 10/06/17

### Chisel3

- Remove warning in Queue for compatibility code [#702](https://github.com/freechipsproject/chisel3/pull/702)
- cloneType and chiselCloneType API change [#653](https://github.com/freechipsproject/chisel3/pull/653)
  - cloneType is now marked (through comments only) as an internal API.
  - chiselCloneType is deprecated (and changed to cloneTypeFull internally, analogous to cloneTypeWidth).
  - chiselTypeOf(data) introduced as the external API to get a chisel type from a hardware object
  - #### Intended usage:
       Cloning is an implementation detail, and chisel types and hardware objects both should act as immutable types,
       with operations like Input(...), Reg(...), etc returning a copy and leaving the original unchanged.
       Hence, the clone operations are all deprecated.
       
       `Input(...)`, `Output(...)`, `Flipped(...)` require the object to be unbound

### Firrtl

- Add Yosys formal equivalence checking to Travis regressions [#670](https://github.com/freechipsproject/firrtl/pull/670)
- Make ReplaceAccesses optimize multi-dimensional accesses [#665](https://github.com/freechipsproject/firrtl/pull/665)
- Update README.md to link tech report [#550](https://github.com/freechipsproject/firrtl/pull/550)
- Fixed zero width cat but [#651](https://github.com/freechipsproject/firrtl/pull/651)
- StringLit.verilogEscape should support all printable ASCII chars [#668](https://github.com/freechipsproject/firrtl/pull/668)
- Namespace - only save suffix for temp names [#667](https://github.com/freechipsproject/firrtl/pull/667)

### Chisel-testers

- Pass repl args [#173](https://github.com/freechipsproject/chisel-testers/pull/173)

### Interpreter

- Fix VCD command completion a bit [#99](https://github.com/freechipsproject/firrtl-interpreter/pull/99)
  - Effectively deprecate fr-use-vcd-script, inconsistent generated naming of targetDir's has doomed it.
  - Place some guards of setting values from VCD file.  particularly as circuit is initialized, bad values can occur.

### DSPTools

Initial SNAPSHOT release of the DSP generator library.
Please visit the [GitHub repository](https://github.com/ucb-bar/dsptools) for instructions and examples.
*Note:* Currently, we only support Scala 2.11 versions of this library - we don't have 2.12 versions of all of its dependencies.
We continue to release Scala 2.11 versions of the BIG4 (required by DSPTools).
If you wish to experiment with the DSPTools library, you will need to use Scala 2.11.
- Fix DspReal lit tests - pass lits over interface. [#109](https://github.com/ucb-bar/dsptools/pull/109)
- Clone fix - don't use literals where pure types are expected. [#108](https://github.com/ucb-bar/dsptools/pull/108)
- Enable DspReal constants and initialize test hardware in preparation for the InvalidateAPI. [#107](https://github.com/ucb-bar/dsptools/pull/107)
