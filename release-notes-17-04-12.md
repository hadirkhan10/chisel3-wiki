## Release Notes 04/12/17

### Chisel3

- Use input element to decide if Vec of values has direction. [Chisel PR #570](https://github.com/ucb-bar/chisel3/pull/570)
- Define CompileOptions case class to support CompileOptions manipulation. [Chisel PR #572](https://github.com/ucb-bar/chisel3/pull/572)
- Make Module instantiations draw clock from Builder instead of parent [Chisel PR #568](https://github.com/ucb-bar/chisel3/pull/568)
- Creating FixedPoint literals was throwing away width when specifically provided. This caused one hot muxing problems in dsptools FixedPoint spec fixed based on error uncovered by this change. [Chisel PR #564](https://github.com/ucb-bar/chisel3/pull/564)
- Support Vec(0) fields in Bundles, just like Option[Data]; add test [Chisel PR #562](https://github.com/ucb-bar/chisel3/pull/562)
- Fix getWidth on empty Vecs; add test [Chisel PR #561](https://github.com/ucb-bar/chisel3/pull/561)
- Fixed fix, allow Mux of different binary points and widths [Chisel PR #559](https://github.com/ucb-bar/chisel3/pull/559)

### Firrtl

- DecorateMems should not delete annoations [Firrtl PR #523](https://github.com/ucb-bar/firrtl/pull/523)
- Change implicit value classes to prefix with _ [Firrtl PR #522](https://github.com/ucb-bar/firrtl/pull/522)
- Find a single cycle from potentially many in a combinational SCC [Firrtl PR #518](https://github.com/ucb-bar/firrtl/pull/518)
- Make force-append-anno-file work. Fixes #515 [Firrtl PR #516](https://github.com/ucb-bar/firrtl/pull/516)
- Change findSCCs to iterative implementation [Firrtl PR #513](https://github.com/ucb-bar/firrtl/pull/513)
- Fix bug where zero width expressions in nodes wouldn't get zeroed [Firrtl PR #514](https://github.com/ucb-bar/firrtl/pull/514)
- Add pass to detect combinational loops [Firrtl PR #480](https://github.com/ucb-bar/firrtl/pull/480)
- Pass now subclasses Transform [Firrtl PR #477](https://github.com/ucb-bar/firrtl/pull/477)
- Add TargetDirAnnotation to give transforms access [Firrtl PR #503](https://github.com/ucb-bar/firrtl/pull/503)

### Chisel-testers

- Finish even on test failure
- Update deprecated usage (Reg(init=...), Reg(next=...), log2Up()).
- Add example of one way to initialize bundles. [Chisel-testers PR #136](https://github.com/ucb-bar/chisel-testers/pull/136)

### Interpreter

- Expect a PassException on a circuit with a combinational loop. [Firrtl-interpreter PR #77](https://github.com/ucb-bar/firrtl-interpreter/pull/77)
