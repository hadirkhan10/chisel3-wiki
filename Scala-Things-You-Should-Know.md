### Packages
Scala packages are a way to hierarchically organize code.  Scala source files should have a package name at the beginning something like 
```scala
package mytools
class Tool1 { ... }
```
This name can be used when referencing code defined in this file.  
```scala
import mytools.Tool1
```
Note: The package name  **should** match the directory hierarchy, this is not mandatory but failing to abide by this guideline can produce some unusual and difficult to diagnose problems. Package names by convention are lower case and do not contain separators like underscore.  This sometimes makes good descriptive names difficult.  One approach is too add a layer of hierachy ```package good.tools```.  Do your best.  Chisel itself plays some games with the package names that do not conform to these rules.