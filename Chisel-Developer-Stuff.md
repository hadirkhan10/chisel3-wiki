## Code Coverage
### How to get coverage information for chisel
We currently use [scoverage](https://github.com/scoverage) as our coverage tool.
By default full coverage is not enabled for sub-projects.
In addition to excluding a bunch of un-helpful stuff, it also excludes useful stuff like [chiselFrontEnd](https://github.com/freechipsproject/chisel3/tree/master/chiselFrontend/src/main/scala/chisel3).
To locally build coverage information that includes **chiselFrontend** edit your local `build.sbt` file. and change this line.

`aggregate := false,` to be `aggregate := true,`

To build the coverage information run `make coverage` from the command line.
This will spit out a lot of information, near the end of which will be the location of the coverage html files.
That line will look like 
```
[info] Written HTML coverage report [/Volumes/UCB-BAR/coverage-chisel3/chiselFrontend/target/scala-2.11/scoverage-report/index.html]
```
Open the index.html file in your browser and you will have moderately easy to understand report.
#### What can go wrong.
Currently running tests is quite slow. This is being worked on.

If an error occurs during testing coverage information will not be generated. This is not usually a problem but
if you are experimenting it might be. The easiest thing to do is run the command manually in sbt as follows
```
bash> sbt
sbt> coverage
sbt> test
sbt> coverageReport
sbt> coverageAggregate
```
