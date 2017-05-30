## Release Notes 05/30/17

### Chisel3

- Correct misleading example code
- Support updated scalatest/scalacheck; bump sbt and Scala versions. [Chisel PR (#605)](https://github.com/ucb-bar/chisel3/pull/605)
- Update comments describing Decoupled/ReadyValid - fix #437. [Chisel PR (#493)](https://github.com/ucb-bar/chisel3/pull/493)

### Firrtl

- Prep for Scala 2.12 [Firrtl PR (#557)](https://github.com/ucb-bar/firrtl/pull/557)
- Fix performance bug in DCE [Firrtl PR (#596)](https://github.com/ucb-bar/firrtl/pull/596)
- Fix performance bug in ZeroWidth [Firrtl PR (#594)](https://github.com/ucb-bar/firrtl/pull/594)
- Delete black_box_verilog_files.f if we aren't going to create it - fixes #504 [Firrtl PR (#551)](https://github.com/ucb-bar/firrtl/pull/551)
- Bump scopt for prettier option parsing [Firrtl PR (#546)](https://github.com/ucb-bar/firrtl/pull/546)
- Upgrade Logging facility [Firrtl PR (#488)](https://github.com/ucb-bar/firrtl/pull/488)
- Make sure not to DCE input-only extmodules unless specified [Firrtl PR (#590)](https://github.com/ucb-bar/firrtl/pull/590)

### Chisel-testers

- Use createTestDirectory to ensure a pristine enviroment - fixes #132. [Chisel-testers PR (#141)](https://github.com/ucb-bar/chisel-testers/pull/141)
- Add firrtl PR 488 support for setting logging options [Chisel-testers PR (#129)](https://github.com/ucb-bar/chisel-testers/pull/129)

### Interpreter

- Fix BlackBox problem when box has multiple outputs Issue 84 [Firrtl-interpreter PR (#85)](https://github.com/ucb-bar/firrtl-interpreter/pull/85)
- Fixed illegal call to set log level [Firrtl-interpreter PR (#83)](https://github.com/ucb-bar/firrtl-interpreter/pull/83)
- Cleanup of build.sbt - Added header - Removed scopt dependency, this should come from firrtl dependency [Firrtl-interpreter PR (#82)](https://github.com/ucb-bar/firrtl-interpreter/pull/82)
