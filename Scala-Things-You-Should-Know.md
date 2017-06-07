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
class WrapCounter(counterBits: Int) {
  val max: Long = (1 << counterBits) - 1
  var counter = 0L
  def inc(): Long = {
    counter = counter + 1
    if(counter > max) counter = 0
    counter
  }
  println(s"counter created with max value $max")
}
```
What is here:
* ```class WrapCounter```: This is the definition of **WrapCounter**
* ```(counterBits: Int)```: Creating MyCounter requires an integer argument, nicely named to suggest it is the bit width of the counter
* braces ({}) delimit a block of code. Most classes use a code block to define variables and constants and methods (functions)
* ```val max: Long =``` the class contains a member variable **max**, declared as ```Long``` and initialized as the class is created
* ```(1 << counterBits) - 1``` computes the maximum value that can be contained in **counterBits** bits.  Since **max** was created with _val_ it cannot be changed.
* a variable **counter** is created and initialized to **0L**, the **L** says that 0 is a long value and from this **counter** is inferred to be Long.
* **max** and **counter** are commonly called _member variables_ of the class
* a class method **inc** is defined which takes no arguments and returns a **Long** value
* the body of the method **inc** is a code block that:
  * ```counter = counter + 1``` increments **counter**
  * ```if(counter > max) counter = 0``` tests if it is greater than the **max** value and sets it back to zero if it is
  * ```counter``` the last line of the code block is important
    * any value expressed at the last line of a code block is considered to be the return value of that code block, that return value can be used or ignore by the programmer
    * this applies quite generally, for example since an if statement or and if then else statement defines its true and false clauses with code blocks the if itself can return a value
    * for example ```val result = if( 10 * 10 > 90) { "greater" } else { "lesser" }``` would created a variable with the value "greater"
  * so in this case the function **inc** returns the value of **counter**
* ```println(s"counter created with max value $max")``` this prints a string to the standard out.  Because the **println** is directly in the defining code block it is part of the classes initialization code and is run, i.e. prints out the string, every time an instance of this class is created.
* The string printed in this case is an _interpolated_ string
  * the leading **s** in front of the first double quote identifies this as an interpolated
  * an interpolated string is processed at run time  
  * the **$max** is replaced with the value of max
  * if the **$** is followed by a code block arbitrary scala can be in that code block
    * for example **${max + max}**
    * the return value of this code block will be inserted  in place of ${...}
    * if the return value is not a string it will be converted to one, virtually every class or type in scala has implicit conversion to a string)
  * it should be noted that classes that print something every time an instance is created is darned annoying and should avoided

### Creating an instance of a class
Let's use our example above to create a class.  Scala instances are created via the built-in magic keyword **new**
```
val x = new WrapCounter(4)
```
Now often in scala code one sees, instances being created without the keyword new, for example ```val y = WrapCounter(6)```
This occurs often enough to merit special attention.  I it described below under [Companion Objects](#Companion-Objects)

### Companion Objects
A companion object is similar to a class but can only occur once.  First let's assume we have the definition of some class X
```scala
class X(x: Int, y: Int) { val z = x + y }
```
We define an example companion object for X.
```scala
object X {
  val const = 77
  def apply(i: Int): X = { new X(i, const)  }
  def apply(i: Int, j: Int): X = { new X(i, j) }
  def show(x: X): Unit = { println(s"x contains value z = ${x.z}") }
}
```
Our companion object X does 3 things
1. It defines a constant, companion object are the canonical way to define constants related to a class
  * ```val const = 77``` 
1. It defines two **apply** methods.  In this case the **apply** methods are known as factory methods in that they return instances of the **class X**
  * ```def apply(i: Int): X = { new X(i, const)  }``` creates an instance of X using only one argument, it uses it's constant const to provide the other missing argument
  * ```def apply(i: Int, j: Int): X = { new X(i, j) }``` defines a basic factory that requires the same arguments as the constructor for **class X**
  * These factory methods can be called naively like this ```val x1 = X.apply(4)``` or ```val x2 = X.apply(33)``` which eliminates the need to use the new keyword
  * But the real magic is that the compiler assumes the apply method any time it sees parentheses applied to an instance or object.  By way of example 
    *  ```val x1 = X(4)``` is equivalent to ```val x1 = X.apply(4)```
    * ```val x2 = X(33)``` is equivalent to ```val x2 = X.apply(33)```
  * Factory methods, usually provided via companion objects, allows alternative ways to express instance creations, they can provide additional tests for constructor parameters, conversions, and eliminate the need to use the keyword new

> We will not here that there is another way of creating a default factory method for a class via the case modifier in class definition ```case class X(...) { ... }``` which implicitly creates a factory method that does not require new and does a number of useful functions.  There are restrictions on case classes, but they are a commonly used scala idiom.

### Code Blocks
Code blocks are delimited by braces.  A block can contain zero or more lines of scala code. The last line of scala code becomes the return value (which may be ignored) of the code block.  A code block with no lines would return and special null-like object called Unit. Code blocks are used throughout scala, they are the bodies of class definitions, they form function and method definitions, they are the clauses of if statements, they are the bodies of for and many other scala operators.

### Parameterized Code Blocks
Code blocks can take parameters.  In the case of class and method definitions these parameters look fairly like most conventional programming languages.  In the example below ```c``` and ```s``` are parameters of the code block.
```scala
def add1(c: Int): Int = {
  c + 1
}
class RepeatString(s: String) {
  val repeatedString = s + s
}
```
**IMPORTANT**: There is another way in which code blocks may be parameterized, it is visible all over the place in a scala program and I found it to be one of the scala constructs that took me a while to get used to.  Here are some examples
```scala
val intList = List(1, 2, 3)
val stringList = intList.map { i =>
  i.toString
}
```
The code block is begin passed to a method map of the class List.  The map method requires that it's code block have a single parameter.  The code block is called for each member of list, the code block returns that parameter converted to a String. Scala is almost excessively accepting of variations of this syntax.  You might see this written in many different ways. Assuming intList is defined as above, all the following will return a list of string versions of the original integer list
```scala
intList map ( s => s.toString )
intList.map( s => s.toString )
intList.map { s: Int => s.toString }
intList.map { (s: Int) => s.toString }
```
Even more exotically one can create a variable which points to a code block
```scala
val intToString = { c: Int =>
  c + 1
}
intList.map(intToString)
// or even more exotically periods and parens can be dropped entirely. 
intList map intToString
```  
The goal again here is simply to help you recognize the different notational types when you encounter them.  As you use Scala these will seem more comfortable and familiar.  Authors tend to gravitate to particular styles and there are also individual syntactical situations in which one notation will seem more natural. One liners tend to use the more concise forms, complex blocks usually have a more narrative appearance.

### Scala Match and Matching
### Syntactic Sugar

