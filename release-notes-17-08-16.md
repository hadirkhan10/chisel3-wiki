## Release Notes 08/16/17

### Chisel3

- Make .dir give correct direction for Module io in compatibility [Chisel PR (#673)]:(https://github.com/freechipsproject/chisel3/pull/673)
- Rename userDir->specifiedDir [Chisel PR (#671)]:(https://github.com/freechipsproject/chisel3/pull/671)
- README: Java package [Chisel PR (#670)]:(https://github.com/freechipsproject/chisel3/pull/670)
- Give default direction to children of Vecs in compatibility code [Chisel PR (#667)]:(https://github.com/freechipsproject/chisel3/pull/667)
- Don't assign default direction to Analog in Chisel._ [Chisel PR (#664)]:(https://github.com/freechipsproject/chisel3/pull/664)
- Address scalastyle issues, out of date comments, extraneous imports. [Chisel PR (#658)]:(https://github.com/freechipsproject/chisel3/pull/658)
- Black box top-level IO fix [Chisel PR (#655)]:(https://github.com/freechipsproject/chisel3/pull/655)
- Add rebinding test [Chisel PR (#654)]:(https://github.com/freechipsproject/chisel3/pull/654)
- Fix style of literal creators [Chisel PR (#637)]:(https://github.com/freechipsproject/chisel3/pull/637)
- Simplify macOS install instructions [Chisel PR (#599)]:(https://github.com/freechipsproject/chisel3/pull/599)
- Fixed point width inference was wrong when binary points didn't align. [Chisel PR (#590)]:(https://github.com/freechipsproject/chisel3/pull/590)

### Firrtl

- Constant propagation across module boundaries [Firrtl PR (#633)]:(https://github.com/freechipsproject/firrtl/pull/633)
- bug fix for cases when we want to flatten a module in which a module is instantiated multiple times [Firrtl PR (#634)]:(https://github.com/freechipsproject/firrtl/pull/634)
- DCE for IsInvalid [Firrtl PR (#629)]:(https://github.com/freechipsproject/firrtl/pull/629)
- Flatten transformation [Firrtl PR (#631)]:(https://github.com/freechipsproject/firrtl/pull/631)

### Chisel-testers

- pass testerBackend argument, not hardcoded firrtl backend to function call [Chisel-testers PR (#146)]:(https://github.com/freechipsproject/chisel-testers/pull/146)
- add ability to apply ad hoc edits to vcs command line [Chisel-testers PR (#163)]:(https://github.com/freechipsproject/chisel-testers/pull/163)
- Augment vcs command [Chisel-testers PR (#161)]:(https://github.com/freechipsproject/chisel-testers/pull/161)

### Interpreter

- ReadWrite memory was not processing input signals correctly setValue for ReadWriteMemory did not have proper match list fieldDependencies for ReadWriteMemory had wrong label for wmask and was missing wdata Added test for read write memory [Firrtl-interpreter PR (#88)]:(https://github.com/freechipsproject/firrtl-interpreter/pull/88)
