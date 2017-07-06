## Release Notes 06/22/17

### Chisel3

- Fix a small typo in the README [Chisel PR (#627)](https://github.com/freechipsproject/chisel3/pull/627)
- Add dontTouch for annotating Data to not be removed [Chisel PR (#620)](https://github.com/freechipsproject/chisel3/pull/620)
- Dont try to instantiate firrtl.Transform from Annotation [Chisel PR (#620)](https://github.com/freechipsproject/chisel3/pull/620)


### Firrtl

- Add --no-dce command-line option to skip DCE [Firrtl PR (#610)](https://github.com/freechipsproject/firrtl/pull/610)
- Replace IsInvalids on LowForm with connection to zero [Firrtl PR (#605)](https://github.com/freechipsproject/firrtl/pull/605)
- Canonicalize spacing in RemoveValidIf [Firrtl PR (#605)](https://github.com/freechipsproject/firrtl/pull/605)
- Make ExpandWhens delete 'is invalid' for attached Analog components [Firrtl PR (#606)](https://github.com/freechipsproject/firrtl/pull/606)
- Style changes to ExpandWhensSpec [Firrtl PR (#607)](https://github.com/freechipsproject/firrtl/pull/607)
- Add option to disable combinational loop detection [Firrtl PR (#604)](https://github.com/freechipsproject/firrtl/pull/604)
- Move CheckCombLoops from passes/ to transforms/ [Firrtl PR (#604)](https://github.com/freechipsproject/firrtl/pull/604)
- Change CheckCombLoops to a Transform [Firrtl PR (#604)](https://github.com/freechipsproject/firrtl/pull/604)
- Fixes a typo in the verilog `elsif` code generation [Firrtl PR (#603)](https://github.com/freechipsproject/firrtl/pull/603)
- Update travis button for new organization [Firrtl PR (#602)](https://github.com/freechipsproject/firrtl/pull/602)
- Display the total time firrtl took to compile [Firrtl PR (#599)](https://github.com/freechipsproject/firrtl/pull/599)
- Change base of randomization values to \_RAND instead of \_GEN [Firrtl PR (#595)](https://github.com/freechipsproject/firrtl/pull/595)
- Add some comments to `endif` [Firrtl PR (#595)](https://github.com/freechipsproject/firrtl/pull/595)

### Chisel-testers

- bump versions; replace PropertyCheckConfig with PropertyCheckConfiguration [Chisel-testers PR (#144)](https://github.com/freechipsproject/chisel-testers/pull/144)
- Fix double elaboration of DUT when using the interpreter [Chisel-testers PR (#154)](https://github.com/freechipsproject/chisel-testers/pull/154)
- Update deprecated usage. [Chisel-testers PR (#149)](https://github.com/freechipsproject/chisel-testers/pull/149)
- cancel AccumBlackBox_PeekPokeTest_VCS test if no vcs [Chisel-testers PR (#152)](https://github.com/freechipsproject/chisel-testers/pull/152)
- This adds two examples of creating black box implementations [Chisel-testers PR (#151)](https://github.com/freechipsproject/chisel-testers/pull/151)

### Interpreter

- Prepare for Scala 2.12 - bump versions and replace deprecated usage. [Firrtl-interpreter PR (#80)](https://github.com/freechipsproject/firrtl-interpreter/pull/80)

