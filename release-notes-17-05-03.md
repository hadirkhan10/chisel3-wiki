## Release Notes 05/03/17

### Chisel3

- Update instructions for installing firrtl. [Chisel PR #597](https://github.com/ucb-bar/chisel3/pull/597)
- Deprecate fromBits and clock/reset constructors [Chisel PR #583](https://github.com/ucb-bar/chisel3/pull/583)
- Dropimportnotstrict492 - More updates to get things through rocket-chip. [Chisel PR #592](https://github.com/ucb-bar/chisel3/pull/592)
- Remove explicit import of NotStrict - fixes #492 [Chisel PR #494](https://github.com/ucb-bar/chisel3/pull/494)
- Remove VecLike/IndexedSeq from Mem type [Chisel PR #589](https://github.com/ucb-bar/chisel3/pull/589)
- 	Fix assignment from 0-entry Vec: add test [Chisel PR #580](https://github.com/ucb-bar/chisel3/pull/580)
- Fix macOS install instructions for homebrew package manager [Chisel PR #575](https://github.com/ucb-bar/chisel3/pull/575)
- Module Hierarchy Refactor [Chisel PR #469](https://github.com/ucb-bar/chisel3/pull/469)
- Fix one hot mux [Chisel PR #573](https://github.com/ucb-bar/chisel3/pull/573)
- Change Enum to emit minimum widths of 1 [Chisel PR #574](https://github.com/ucb-bar/chisel3/pull/574)

### Firrtl

- Add info on reset block lines to ANTLR grammar [Firrtl PR #468](https://github.com/ucb-bar/firrtl/pull/468)
- move circuit dumping to trace so debug gives annos only [Firrtl PR #524](https://github.com/ucb-bar/firrtl/pull/524)
- "Scope" test resource (top.cpp). [Firrtl PR #398](https://github.com/ucb-bar/firrtl/pull/398)
- Speed up CSE by doing CSE on node expression before recording the node [Firrtl PR #543](https://github.com/ucb-bar/firrtl/pull/543)

### Chisel-testers

- Check for irrevocable IO in AdvTester sink; warn otherwise [Chisel-testers PR #121](https://github.com/ucb-bar/chisel3/pull/121)
- Remove references to obsolete Mem fields. [Chisel-testers PR #140](https://github.com/ucb-bar/chisel3/pull/140)
- Update import in order to get the correct Module definition. [Chisel-testers PR #139](https://github.com/ucb-bar/chisel3/pull/139)

### Interpreter

- Some small enhancements to the firrtl-repl `peek` and `rpeek` [Firrtl-interpreter PR #79](https://github.com/ucb-bar/firrtl-interpreter/pull/79).
These now only evaluate the portion of the graph specifically necessary to resolve the peeked value.
If you want to evalute the whole circuit, use `step` or `rpeek .*`.
This makes it much easier to use verbose to see the computations being done to figure out the peek.
Added arguments to `show`:
  - `show state` - shows the state of the circuit, this is the default,
  - `show input` - the original input supplied,
  - `show lofirrtl` - shows the lowered input
