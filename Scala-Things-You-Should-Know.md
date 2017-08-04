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
// or even still more exotically periods and parens can be dropped entirely. 
intList map intToString
```  
The goal again here is simply to help you recognize the different notational types when you encounter them.  As you use Scala these will seem more comfortable and familiar.  Authors tend to gravitate to particular styles and there are also individual syntactical situations in which one notation will seem more natural. One liners tend to use the more concise forms, complex blocks usually have a more narrative appearance.

### Scala Match and Matching
The scala *matching* concept is used throughout chisel and needs to be part of any chisel programmers basic understanding. Scala provides the match operator which supports:
- Simple testing for alternatives, something like a *C* switch statement
- More complex testing of ad-hoc combinations of values
- Taking actions based on the type of a variable when it's type is unknown or underspecified, for example when
  - variable is taken from a heterogeneous list ```val mixedList = List(1, "string", false)```
  - or variable is known to be a member of a super-class but not which specific sub-class it is.
- Extraction of sub strings of a string that are specified with a *regular expression*

#### Simple match statement
```scala
// y is an integer variable defined somewhere else in the code
val x = y match {
  case 0 => "zero"
  case 1 => "one"
  case 2 => "two"
  case _ => "many"
}
println("y is " + x)
```
The match operator checks possible values and for each case returns a string.  A couple of things to note:
- A match is searched in the order of the case statements, once a case statement has been matched, no other
checks against other case statements are made.
- The use of underscore as a wildcard, to handle any value not found. 

#### Testing adhoc combinations with match
Here's a simple example of a truth table implemented with a match statement and tuple of values
```scala
def animalType(biggerThanBreadBox: Boolean, meanAsCanBe: Boolean): String = {
  (biggerThanBreadBox, meanAsCanBe) match {
    case (true, true) => "wolverine"
    case (true, false) => "elephant"
    case (false, true) => "shrew"
    case (false, false) => "puppy"
  }
}
```
In this example a tuple is created from the method parameters ```(biggerThanBreadBox, meanAsCanBe)``` is created and tested with the match statement.  A case is listed for all the possible combinations. 
The call ```animalType(true, false)``` would return the string "elephant"

#### Processing a heterogenous list with match
```scala
val mixedList = List(1, "string", false, 1.57)
val stringList = mixedList.map { 
  case i: Int => i.toString
  case s: String => s
  case b: Boolean => 
    if(b) {
      "T"
    } else {
      "F"
    }
  case _ =>
    "unsupported type"
}
```
```stringList``` will be a list of strings ```List("1", "string", "F", "unsupported type")```
What's going on?  We have created a list that contains three different types of elements.  We are going to map this string into another string.  The ```map``` method on string implicitly *match*es each element against the cases.  Other things to note:
- Each code block that follows a the ```=>``` operator continues until it reaches either the ending brace of the match or the next case statement.
- The map statement is sometimes written out more explicitly as follows, See: [Parameterized Code Blocks](Scala-Things-You-Should-Know#Parameterized-Code-Blocks)
```scala
val stringList = mixedList.map { element => 
  element match {
    case i: Int => i.toString
    ...
  }
}
```
This works as it did before, but uses the parameterized code block convention to pass each member of list to the ```map``` code block as the variable ```element```.  In this code block each element is processed by the ```match``` statement.

#### Unapply methods and scala match
Scala unapply methods are another form of syntactic sugar giving match statements the ability to both match on types and extract values from those types during the matching.  Scala unapply methods can be coded explicitly by developers or, more commonly, can be generated by the use of case classes.  See [Case Classes Section](Scala-Things-You-Should-Know#Case-Classes).  There are reams of web resources on this but we'll try to capture a number of important features in hopefully not too confusing example.
First let's define a number of classes
```scala
trait Component { def name: String }
case class Monitor(name: String, diagonal: Double, scanRate: Int) extends Component
case class Keyboard(name: String, numberOfKeys: Int) extends Component
case class Mouse(name: String) extends Component
```
We have defined a trait, a lightweight super-class, and we have created a number of case classes that extend Component.  In each case they provide a name, required when extending the trait and other parameters based on their type.  We will now create a collection of components called System which will provide a displayComponents method that uses match to identify them.
```scala
class System(val components: List[Component]) {
  def displayComponents(): Unit = {
    components.foreach {
      case Monitor("BrandX", size, _) =>
        // brandx is square, lists horizontal size, all are 60Hz
        val diag = size * 1.414
        println(s"monitor BrandX of diagonal size $diag scan rate is 60")
      case Monitor(name, size, scan) =>
        println(s"monitor $name of diagonal size $size scan rate $scan")
      case Keyboard(name, keys) =>
        println(s"keyboard $name with $keys keys")
      case _: Mouse =>
        println("There is a mouse")
      case x: Component =>
        println(s"unknown component ${x.name}")
    }
  }
}
```
The next case statement ```case Monitor(name, size, scan) =>``` will *match* any Monitor instance (except for any with BrandX because the prior case statement caught all those).  In this case when the component is a Monitor we grab the the parameters of the Monitor class as name, size and scan, and we use those directly to print out this type of monitor.


We can begin to see the power of the match capability.  ```def displayComponents(): Unit = {``` declares the displayComponents method that returns nothing.  ```components.foreach {``` uses the foreach method to iterate over each element in the list of components. The code block of the foreach matches on a number of possibilities.

```case Monitor("BrandX", size, _) =>``` This matches a component whose type is Monitor and whose first parameter has the value. Now here is the cool part, the size symbol takes *diagonal* (the second parameter of the Monitor class) to a local variable size.  The size variable is only defined until the next case statement or to the end of the block.  Note also the use of underscore, the Monitor class has third parameter which must be dealt with, so we use the underscore to stand in for the third parameter but to indicate we do not need to use it's value.  This case statement then has a small code block
```scala
        // brandx is square, lists horizontal size, all are 60Hz
        val diag = size * 1.414
        println(s"monitor BrandX of diagonal size $diag scan rate is 60")
```
that does a little math on the size variable and prints out information on the monitor.

Next we match on Keyboard class instances using ```case Keyboard(name, keys) =>``` as before using match's ability to instantiate local variables with values from the class constructor parameters.  

The next case statement ```case _: Mouse =>``` uses a different match idiom.  The underscore says we have match here but we don't need to reference what was matched, but we do insist that the match be on a instance of class Mouse. See [Underscore the Scala Wildcard](Scala-Things-You-Should-Know#Underscore-the-Scala-Wildcard)

The final case statement ```case x: Component =>``` illustrates matching an instance of Component and assigning that instance to a local variable x.  Since x is a Component the only thing know about it, is that it will have a method name. So we use that method, wrapped in braces because it contains more complex scala code, to print this.

Because we declared components to be a ```List[Component]``` the compiler guarantees we don't have any other classes instances in there.  Otherwise a thorough developer should add an additional match to avoid errors.  That code would look like
```scala
    case _ =>
      // do some error handling here
```
### Case Classes
Chisel often uses case classes, a declaration looks something like 
```scala
case class Drill(variableSpeed: Boolean, amps: Int, rpm: Int)
```
The case class provides a bunch of useful features, the ones most commonly taken advantage of are:
- Allows external access to the class parameters.  These are not accessible by default a class declared with ```class X(y: Int)``` would not allow the programmer to reference ```x.y``` when x is an instance of X created via ```val x = new X(88)```
- Eliminates the need to use new when instantiating the class, i.e. one can write ```val d = Drill(true, 10, 3000)```
- Automatically creates an unapply method that supplies access to all of the class Parameters.  This allows a programmer to us match on the class as we discussed in [Scala Match and Matching](Scala-Things-You-Should-Know#scala-match-and-matching). So assuming a match is being done on an instance of Drill, once can write.
```scala
  someVarThatMightHaveADrill match {
    case Drill(hasVarSpeed, amps, rpm) =>
       // here we have access to local variables hasVarSpeed, amps, and rpm, that come
       // someVarThatMightHaveADrill's parameters.
```

### The Underscore (_) Scala's Wildcard
### Named parameters
When a method is defined in scala, for example
```scala
def myMethod(count: Int, wrap: Boolean, wrapValue: Int = 24): Unit = { ... }
```
When calling the method, you will often see the parameter names along with the passed in values
```scala
myMethod(count = 10, wrap = false, wrapValue = 23)
```
For frequently called methods, the parameter ordering may be obvious but for less common methods and, in particular, boolean arguments, including the names with calls can make your code a lot more readable.  Using named parameters can allow you to re-arrange arguments, and in combination with parameters that have a default value, can make it so the caller only has to pass (by name) the specific arguments that do not use the default value.  Parameters to class definitions also used this named argument scheme (they are actually just the parameters to the constructor method for the class).

### String interpolation
There are lot of ways of building strings with dynamic content.  The simplest is simply to use the add operator.
```
println("my dog " + dogsName + " went to the " + destination)
```
But there are a lot of more alternative (possibly more elegant ways) to do the same thing.  The first is the **s** string interpolator.  When a string begins with **s"** any occurrences of a dollar sign followed by a scala variable, or a dollar sign followed by a code block, will have that replaced by the string value of the variable or block.  For our previous example we could make this 
```
println(s"my dog $dogsName went to the $destination")
```
As an example of using a code block, let's assume we have a list ```val list = List("dog", "cat", "fox")``` and we'd like to print this out with each animal capitalized. 
```
println(s"Animal list ${list.map(s => s.capitalize).mkString(" -- ")}")
```
would produce the line 
    Animal list Dog -- Cat -- Fox
We used the ${...} to execute some scala code on that list.  map converts the list to a new list in which each word has been capitalized, then that new list is changed into a string by taking each element and joining it to the others with the string " -- " between each element.

Another less commonly used interpolator is the **f** which allows printf style formatting specifiers to be included at each interpolation point.
```
println(f"$lineNumber%6d ${x.name}%40s ${x.age}%3d")
```
Can be used to make some columns that line up nicely.  Scala does have printf too if you really need it.  But be a little careful with that Chisel provides it's own version of printf specifically for debug printing inside an executing simulation.

### Syntactic Sugar

