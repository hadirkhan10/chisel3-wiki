## Release Notes 04/23/17

### Chisel3
- The register creation API has been changed to offer clearer functional choices. [Chisel PR #553](#553)
  - ```RegInit``` should be used to create a register with a specified initializer.
  - ```RegNext``` should be used to create a register with a specified next value.
- ```log2Up``` and ```log2Down``` have been deprecated, users should now use ```log2Ceil``` and ```log2Floor```.  Internal references to these functions have been fixed.. [Chisel PR #528](#528)
- ```QueueIO``` and ```ArbiterIO``` work proper when given zero entries. [Chisel PR #X](#X)
- Support has been added for field names that begin with a number in the new aggregate type ```Record```. [Chisel PR #531](#531)
- New warnings on inappropriate parameters for a number of chisel constructs. [Chisel PR #455](#455)
  - ```Vec``` creation warns when type given is a literal. [Chisel PR #530](#530)
- If ```OHToUInt``` is given only one argument it always returns that. [Chisel PR #546](#546)
- Custom transforms are executed in the order they are specified  in the options manager. [Chisel PR #532](#532)
- ```ShiftRegister``` applies the enable to all registers created. [Chisel PR #370](#370)
- Fixed a bug where compile errors were caused by assertions containing %. [Chisel PR #500](#500)
- Fixed an an inconsistency in Chisel, a ```UInt``` -& ```UInt``` now returns an ```SInt```. [Chisel PR #502](#502)
- There is a new low level API for creating elements of a given type ```asTypeOf```. [Chisel PR #450](#450)
  - fromBits currently untouched but will be chisel3-deprecated if possible as a future PR.
  - Code that needs to create a super-type of several types into `cloneSupertype` See. #446
    - This should be the only thing that changes externally visible API, in that the checks are now more consistent.
    - oneHotMux now checks against inconsistent input types.
  - Simplify Vec construction using above.
  - Move `cloneTypeWidth` from `Data` to `Bits`. It used to silently drop the `width` parameter when called on any non-Bits, this makes uses of it explicit and always correct, minus the Bool case.
  - Eventually want to get rid of cloneTypeWidth. The only stragglers are `Bits.pad` (which can instead create a new UInt/etc from scratch) and Reg (which clears widths behind the scenes for you). Not sure how to address the Reg case.
    - Deprecate `flatten`, hopefully will remove it eventually using more local operations.

### Firrtl

- Custom transforms that do not supply a proper return value get a much better error message  [Firrtl #497](/ucb-bar/firrtl/pull/497)
- A performance problem with zero width wires has been fixed  [Firrtl #502](/ucb-bar/firrtl/pull/502)
- There are new tutorials for how to write firrtl transformations  [Firrtl #492](/ucb-bar/firrtl/pull/492)
- There are new tools in analyses package for digraphs and netlist operations  [Firrtl #479](/ucb-bar/firrtl/pull/479)
- A bug was fixed that failed to infer as single-port RAM  [Firrtl #473](/ucb-bar/firrtl/pull/473)
- A bug was fixed where inline instances were not correctly prefixed  [Firrtl #473](/ucb-bar/firrtl/pull/473)
- The types and names of ports in emitted Verilog are now aligned  [Firrtl #463](/ucb-bar/firrtl/pull/463)
- Support has been added to allow the ReplSeqMem transform to not deduplicate some memories, based on their name [Firrtl #447](/ucb-bar/firrtl/pull/447)
- A bug was fixed.  Expanding whens is now always in the correct order [Firrtl #303](/ucb-bar/firrtl/pull/303)

### Chisel-testers

- Custom transforms are now being run before verilator was called. [Chisel-testers #127](/ucb-bar/chisel-testers/pull/127)
- Several new examples have been added to ```src/test/scala/examples``. 
  - A parallel version of GCD using the Advanced Tester. [Chisel-testers #120](/ucb-bar/chisel-testers/pull/120)
  - Example of re-running a simulation with out rebuilding using verilator. [Chisel-testers #119](/ucb-bar/chisel-testers/pull/119)
- A number of verilator internal signal errors were fixed. [Chisel-testers #112](/ucb-bar/chisel-testers/pull/112)

### Interpreter

- targetDirName is now set to *test_run_dir* for test output. [Firrtl-interpreter #76](/ucb-bar/firrtl-interpreter/pull/76)
- A bug was fixed.  Right shift now properly returns 0 when shift amount is greater than width of the target [Firrtl-interpreter #75](/ucb-bar/firrtl-interpreter/pull/75)
