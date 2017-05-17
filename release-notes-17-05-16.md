## Release Notes 05/16/17

### Chisel3

- Changed multiplication of SInt and UInt [Chisel PR (#611)](https://github.com/ucb-bar/chisel3/pull/611)
- Scope resources - move them down into chisel3 directory - fixes #549 [Chisel PR (#610)](https://github.com/ucb-bar/chisel3/pull/610)
- Add implicit CompileOptions to Record and Bundle [Chisel PR (#595)](https://github.com/ucb-bar/chisel3/pull/595)
- Connecting basic types wrong should error in chisel [Chisel PR (#497)](https://github.com/ucb-bar/chisel3/pull/497)
- Clear clock and reset scope for RawModule [Chisel PR (#607)](https://github.com/ucb-bar/chisel3/pull/607)

### Firrtl

- Bugfix: renaming instance ports was broken. [Firrtl PR (#588)](https://github.com/ucb-bar/firrtl/pull/588)
- Fix pad, second try [Firrtl PR (#465)](https://github.com/ucb-bar/firrtl/pull/465)
- Improved Global Dead Code Elimination [Firrtl PR (#549)](https://github.com/ucb-bar/firrtl/pull/549)
- Refactor WIR WSub{Field,Index,Access} - rename exp -> expr #521 [Firrtl PR (#586)](https://github.com/ucb-bar/firrtl/pull/586)
- Fix typo in ExecutionOptionsManager comment [Firrtl PR (#520)](https://github.com/ucb-bar/firrtl/pull/520)
- Update rename2 [Firrtl PR (#478)](https://github.com/ucb-bar/firrtl/pull/478)
- Add checks on register clock and reset types - addresses issue [#33](https://github.com/ucb-bar/firrtl/issues/33) [Firrtl PR (#553)](https://github.com/ucb-bar/firrtl/pull/553)
- Add test for source locators on multi-line reset registers [Firrtl PR (#554)](https://github.com/ucb-bar/firrtl/pull/554)

### Chisel-testers

No changes from 05/03/17

### Interpreter

- Fix discrepancies between interpreter's and verilator's vcd output. Make a constant for "clock" in VCD. Move control over vcd output clock to VCD. Fixed apparent flush problem with VCD writing. Some style cleanup.
