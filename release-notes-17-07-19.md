## Release Notes 07/19/17

### Chisel3

- Update deprecated code in build.sbt [Chisel PR (#648)](https://github.com/freechipsproject/chisel3/pull/648)
- Ensure IO is non-null before attempting to autoWrapPorts. [Chisel PR (#643)](https://github.com/freechipsproject/chisel3/pull/643)
- Fix syntax of README.md [Chisel PR (#641)](https://github.com/freechipsproject/chisel3/pull/641)
- Directions internals mega-refactor [Chisel PR (#617)](https://github.com/freechipsproject/chisel3/pull/617)

### Firrtl

- do not swap wire names with node names [Firrtl PR (#630)](https://github.com/freechipsproject/firrtl/pull/630)
- Fix ConstProp bug where multiple names would swap with one [Firrtl PR (#626)](https://github.com/freechipsproject/firrtl/pull/626)
- Fix bug in DiGraph.reverse on an graph with one vertex, no edges [Firrtl PR (#628)](https://github.com/freechipsproject/firrtl/pull/628)
- Fixed inability to disable combo loop check [Firrtl PR (#619)](https://github.com/freechipsproject/firrtl/pull/619)
- ConstProp registers that are only connected to or reset to a consant [Firrtl PR (#621)](https://github.com/freechipsproject/firrtl/pull/621)
- Connect registers with no connections to zero [Firrtl PR (#621)](https://github.com/freechipsproject/firrtl/pull/621)
- Add test for padding constant connections to wires in ConstProp [Firrtl PR (#621)](https://github.com/freechipsproject/firrtl/pull/621)
- Preserve "better" names in Constant Propagation [Firrtl PR (#620)](https://github.com/freechipsproject/firrtl/pull/620)
- [Travis] Explicitly limit chisel tests parallelism to 2 [Firrtl PR (#616)](https://github.com/freechipsproject/firrtl/pull/616)
- [Travis] Use Build Stages [Firrtl PR (#616)](https://github.com/freechipsproject/firrtl/pull/616)
- Make Constant Propagation respect dontTouch [Firrtl PR (#617)](https://github.com/freechipsproject/firrtl/pull/617)
- Promote ConstProp to a transform [Firrtl PR (#617)](https://github.com/freechipsproject/firrtl/pull/617)
- [Testing] Clean up SimpleTransformSpec execute methods [Firrtl PR (#617)](https://github.com/freechipsproject/firrtl/pull/617)
- [Testing] Have SimpleTransformSpec mix in FirrtlMatchers [Firrtl PR (#617)](https://github.com/freechipsproject/firrtl/pull/617)
- Add RemoveReset transform to replace register reset with a Mux [Firrtl PR (#615)](https://github.com/freechipsproject/firrtl/pull/615)
- Emitting reg update mux tree, only walk netlist for wires and nodes [Firrtl PR (#615)](https://github.com/freechipsproject/firrtl/pull/615)
- Add support for wires in ConstProp [Firrtl PR (#614)](https://github.com/freechipsproject/firrtl/pull/614)
- Speed up ConstProp by doing ConstProp before recording node [Firrtl PR (#614)](https://github.com/freechipsproject/firrtl/pull/614)

### Chisel-testers

- Update deprecated code in build.sbt [Chisel-testers PR (#160)](https://github.com/freechipsproject/chisel-testers/pull/160)
- Cycle test fix 2 [Chisel-testers PR (#158)](https://github.com/freechipsproject/chisel-testers/pull/158)
- Fix cycle test problem [Chisel-testers PR (#157)](https://github.com/freechipsproject/chisel-testers/pull/157)
- Updating to keep up with Direction mega PR [Chisel-testers PR (#156)](https://github.com/freechipsproject/chisel-testers/pull/156)

### Interpreter

- Problem disabling Combinational Loop Detection The interpreter was not picking up the disable combinational loop detection command line flag. The default initial lowering pass now picks up this flag. This required a lot of manipulation of the optionsManager traits Which affected many files in main and test. Affected many imports as well. Fixed flag which disabled default initial lowering pass [Firrtl-interpreter PR (#87)](https://github.com/freechipsproject/firrtl-interpreter/pull/87)
