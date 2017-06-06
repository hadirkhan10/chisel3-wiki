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

### A Simple Class Example
An example of creating a Scala class might be
```scala
class MyCounter(counterBits: Int) {
  val max = 1 << (counterBits + 1) - 1
  var counter = 0
  def inc(): Int = {
    counter = counter + 1
    counter
  }
  println(s"counter created with max value $max")
}
```
What is here:
* class MyCounter: This is the definition of MyCounter
* (counterBits: Int): Creating MyCounter requires an integer argument, nicely named to suggest it is the bitwidth of the counter
* braces delimit a block of code. Most classes use a code block to define variables and constants and methods (functions)
* the class contains a member variable max which is computed as the class is created to be the maximum value that can be contained in counterBits bits
* 
The code block delimited by the braces is where member variables, method definitions and initialization code goes.  In _Instance_ of a class is created by the new operator thus ```val x = new MyClass``` which creates a new instance .  Often you will see the following.  ```val x = MyClass```