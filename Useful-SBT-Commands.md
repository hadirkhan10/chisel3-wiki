| command | what it does (more or less) |
| ------- | --------------------------- |
| run     | run a main, if there is only one run that one, through up a menu asking which one |
| runMain example.Driver | run the main method in the example.Driver companion object | 
| compile                 | compile everything in the src/main hierarchy
| test:compile            | compile everything in the src/main and src/test hierarchy what's nice here is that once this is run then one can use tab command completion of package and class names |
| test                    | run all tests in src/test hierarchy |
| testOnly example.AdderSpec | run all tests in example.AdderSpec |
| console  | run an interactive shell session, also knows as REPL |
| help     | lists other exciting commands |

**NOTE** To run these commands directly from the command line (as opposed to the SBT console),
enclose the entire command (along with its arguments) in quotation marks - e.g. `sbt "testOnly example.AdderSpec"`
