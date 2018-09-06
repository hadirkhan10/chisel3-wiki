# Chisel + Scala Primer

Developed by: Christopher Yarp, Jan 2016

### Chisel Software Architecture

```
Chisel
(Hardware Construction Language)
Domain Specific Language
```
```
Firrtl
(Hardware description intermediate Language)
```
```
Scala
(General Functional / Object Oriented Language)
```
```
Java / Java Runtime Environment (JRE)
(General Object Oriented Language and Runtime)
Cross Platform, Heavily Documented, Large Standard Library
```

### Scala build tool (SBT)

(Build Tool for Scala)
Build Tool,
Package Manager

# Scala

- Semicolons
- Accessing member functions -> dots are optional
- Infix functions
- Optional parens for #arg < 2
- Val vs Var -> why val is preferred (functional purity, immutability)
- Def -> Functions and Procedures
- Ranges
- Pattern matching
- Tuples
- Mapping, reducing, folding, scanning (related to fold), zip, unzip, zipwithindex
- Annon functions
- Apply function – and the underscore
- Companion Objects
- Alt to define constructors?
- Convenience functions -> Like static class methods in java
- Scala collections vs. Array (be careful -> Seq is often better)
- The _
- Val fctnToExe = add _
- Trailing underscore specifies that value should be the function and not the return value
- SBT

### Running Scala Interactively

Scala provides a REPL (Read-Evaluate-Print Loop) interface that can be accessed using sbt
```
> sbt console

[info] Compiling 4 Scala sources to /Volumes/UCB-BAR/chisel-template/target/scala-2.11/classes...
[warn] there were 48 feature warnings; re-run with -feature for details
[warn] one warning found
[info] Starting scala interpreter...
[info]
Welcome to Scala version 2.11.7 (Java HotSpot(TM) 64-Bit Server VM, Java 1.8.0_20).
Type in expressions to have them evaluated.
Type :help for more information.

scala> val a = 1
a: Int = 1

scala> val b = 7
b: Int = 7

scala> val c = a + b
c: Int = 8

scala> val z = (0 to 10 by 2).reverse
z: scala.collection.immutable.Range = Range(10, 8, 6, 4, 2, 0)

scala>
```
Can use this to try out scala snippets …
Or just as a cool calculator!

### Semicolons & Ending Statements

- Semicolons are used in C type languages to denote the end of a statement
- Scala uses them for the same purpose
- Scala also infers line ends as the end of a statement
- Except if the line ends in =, {, (, or an operator
- Semicolons can be used to separate statements in a single line

> Information from Programming Scala, 2nd Edition by Dean Wampler and Alex Payne

### Different Function Call Syntax

- Typical Java syntax is also typically valid scala syntax
- Methods with no arguments can omit ()
  - object.fun() is equivalent to object.fun
- Methods with  no arguments can be called with a postfix syntax (no longer recommended as semicolons are optional)
  - object.fun() is equivalent to object fun
- Methods with one argument can be called with infix notation
  - object.oneArg(5) is equivalent to object oneArg 5
  - Infix notation should only be used for purely functional method (no side effects) or methods that take a function as a parameter
  - Should be used for high-order functions (map, foreach, …)
  
> Information from Scala Style Guide: http://docs.scala-lang.org/style/method-invocation.html

### val vs var

- val defines a constant value
- var defines a variable
- vals are preferred in Scala
- Note that if val is a reference to an object, the underlying object may be modified but the reference cannot be changed

> Information from Scala for the Impatient by Cay S. Horstmann
> Scala Documentation: Concrete Mutable Collection Classes: http://docs.scala-lang.org/overviews/collections/concrete-mutable-collection-classes.html

### Why are vals preferred?    Functional Purity

- A pure function is one that has no side effects
- A function has a side effect when it relies on or changes some external state
- A pure functions will always yield the same return value with the same input data (arguments)
- Purely functional languages (Scala is not one) only allow pure functions
- You have been writing pure functions for a long time … in math class!
- f(x) = x*x + 2x + 1 is a pure function!
- vars are mutable and represent state, vals are immutable
- Ideally, one would have some input and apply several functions to it

> Information from Scala for the Impatient by Cay S. Horstmann

### Defining Functions

- Functions:
  - def fctnName(a: Type, b: AnotherType): ReturnType = {
  - 	function body …}
  - def addInt(c: Int, d: Int) = c + d 
  - The return type is optional, the argument types are mandatory
  - If recursion is used, return type is mandatory
- Procedures: Functions without a return value
  - def procedureName(a: Type, b: AnotherType) {procedure 	body …}
  - def printMe(a: Int) {println("Val="+a)}

### class vs object

- The object keyword is used to describe singleton objects
  - A singleton object only has only one instance
  - You can think of it as defining a class and instantiating it only once

### class vs. object   Companion Objects

- Why is there an object keyword?
- Java classes allow static methods and variables
  - Functions and fields that can be called without an object
    - Class.Function, Class.Feild
  - They can serve many uses including helper functions, factories, and more
- Scala does not allow static methods to be declared in classes
- Instead, scala defines companion objects with the same name as the class.
- Static methods are placed in the companion object
- Because the companion object has the name name as the class, a call to Class.method is actually a call to the method in the companion object

### The apply function

- The apply function is syntactic sugar to overload the () operator.
- You can define an apply method in classes that allow you to use object(arg) in your code
- You can define an apply function in the companion object typically to act as a factory (returns an object of the companion class).

```scala
class MyClass (name: String, id: Int) {
    var myName = name
    var myId = id
    def printMe() {println(myName + " ID: " + myId)}
    def apply() = myId
    def apply(a: Int) {myId = a}}

object MyClass {
    def apply(s: String) = new MyClass(s, 0)
}
val a = MyClass("bob")
a.printMe()
a(10)          // output bob ID: 0
a.printMe()    // output bob ID: 10
```

### When to use new?

- new will call the constructor
- ```val a = MyClass("bob")``` calls the apply function in the companion object
- ```val b = new MyClass("bob", 10)``` calls the constructor

### Control Statements and Loops

#### If/else -- Like Java

```scala
if(n < len) {
  //do something}
} else if(n == len) {
  //do something
} else {
  //do something
}
```
#### While -- Also like Java

```scala
while(n < len) {
  //do something
}
```

#### For -- not like Java

```scala
for (i <- 0 until len){
	//do something
}
```
With special syntax for nested loops
```scala
for {
  i <- 0 to len1
  j <- 0 until len2
} {
  //do something
}

```

- Well … actually like Java but the for each variant
- Can loop over elements in collection (using an iterator)
- In the previous for loops, 0 until len creates a Range that can be iterated over.
```scala
val a = Seq(1, 2, 3, 4, 5)
for (i <- a){
	//i takes the value of each
	//element in the sequence a	
	println(i)
}
```
> Information from Learning Scala by Jason Swartz

### Ranges

- Ranges are used in several parts of scala
  - Especially in for loops
- Ranges are sequences of numbers that
  - Start at one number (inclusive)
  - End at another number (inclusive or exclusive)
  - By some increment

```scala
val a = 0 to 5            // a = (0, 1, 2, 3, 4, 5)
val b = 0 until 5         // b = (0, 1, 2, 3, 4)
val c = 0 to 4 by 2       // c = (0, 2, 4)
val d = 1.5 to 3.0 by 0.5 // d = (1.5, 2.0, 2.5, 3.0)
```

> Information from Programming Scala, 2nd Edition by Dean Wampler and Alex Payne

### Matching

- Like a C switch statement but much more powerful
- Can match on values, types, Boolean expressions, and more!
- A bit advanced for this course but good to know if you come across it
- Related to partial functions

```scala
val a = Seq(8, 6, 7, 5, 3, 0, 9)

for( i <- a ){
	val rtn = i match {
		case _ if i > 5 => "over"
		case 5          => "five"
		case _         => "under"
	}
	println(rtn)
}
```

### Tuples

- Tuples are simply ordered collections of data
- The values cannot be iterated over
- Data in tuples do not need to have the same type
- Data from tuples can be extracted using a special syntax where the index starts from 1
- The relation operator -> is syntactic sugar for making 2-tuples and is targeted at key-value pairs

In the console
```scala
val stuff = (3, 5.6, "hello")        
stuff: (Int, Double, String) = (3,5.6,hello)
println(stuff._1)
3
println(stuff._2)
5.6
println(stuff._3)
hello
val keyValPair = "name" -> "Oski"

```
> Information from Programming Scala, 2nd Edition by Dean Wampler and Alex Payne

### Functions as “First Class Citizens”

- You have seen that you can assign a number to a val or var
    - ```val a = 5```
- You can also assign a function to a val or var!
```
val a = scala.math.abs _
a(-5)
5
```
- The space, underscore specifies that you want to assign the function to a and not the value returned
- Functions can actually be passed around, just like numeric values!

> Information from Scala Cookbook by Alvin Alexander

### Higher Order Functions

- High Order Functions either:
  - take a function as an argument
  - return a function
- Why functions can be passed!
```scala
def highOrder(a: Int, b: Int, c:Int, fun:(Int,Int) => Int) = {
	val tmp1 = fun(a, b)
	val tmp2 = fun(b, c)
	fun(tmp1, tmp2)
}

highOrder: (a: Int, b: Int, c: Int, fun: (Int, Int) => Int)Int

def addInt(a:Int, b:Int) = a + b
addInt: (a: Int, b: Int)Int

val result = highOrder(2, 5, 7, addInt)
result: Int = 19
```

> Information from Becoming Functional by Joshua Backfield
> Scala Cookbook by Alvin Alexander

### Anonymous Functions

- It seems silly to define a function like addInt if we only pass it to high order functions
- What we would like is define the function we want inside the function call
- We can do this with anonymous functions!
- Anonymous functions are sometimes called lambda functions, closures, or function literals
- There is a subtle difference between lambda functions and closures
```scala
def highOrder(a: Int, b: Int, c:Int, fun:(Int,Int) => Int) = {
	val tmp1 = fun(a, b)
	val tmp2 = fun(b, c)
	fun(tmp1, tmp2)
}

val result = highOrder(2, 5, 7, (a, b) => a+b)
result: Int = 19
```
Types are not needed since the inference engine infers the types of a and b from highOrder

- There is even more syntactic sugar for anonymous functions!
- Underscores can be used “as positionally matched arguments”
```scala
def highOrder(a: Int, b: Int, c:Int, fun:(Int,Int) => Int) = {
	val tmp1 = fun(a, b)
	val tmp2 = fun(b, c)
	fun(tmp1, tmp2)
}

val result = highOrder(2, 5, 7, _+_)
result: Int = 19
```

> Information from Becoming Functional by Joshua Backfield. 
> Scala Cookbook by Alvin Alexander. 
> Information from Scala Style Guide: http://docs.scala-lang.org/style/method-invocation.html

### The underscore

- The underscore _ is the wildcard character in Scala
    - It is used in several contexts
- Import statements to import all sub packages
    - In match statements to act as a placeholder
    - In assigning a function to a val
    - Anonymous functions to act as a placeholder 

### Useful High Order Functions

These functions take a Sequence or List and perform some operation
- Map
    - Applies a function to each element in a list and returns the resulting transform in a list
- Filter 
    - Generates a new list with elements of the original list that fit some filter condition
- foldLeft, foldRight
    - A reduce operation where a list is traversed either from the left or from the right.  In each step, a function is preformed on the list element and the result of the last fold operation, returning a single value.
    - Results in a single value at the end of evaluation

> Information from Programming Scala, 2nd Edition by Dean Wampler and Alex Payne

#### zip - zipWithIndex and unzip

- Sometimes, you want to pair up values in two lists.  You can do this with zip
```scala
val list1 = Seq(1, 2, 3, 4)
val list2 = Seq(5, 6, 7, 8)
val zipped = list1 zip list2   // zipped: Seq[(Int, Int)] = List((1,5), (2,6), (3,7), (4,8))
```
- Unzip splits a list of tuples into a tuple of lists
```scala
val unzipped = zipped.unzip   // unzipped: (Seq[Int], Seq[Int]) = (List(1, 2, 3, 4),List(5, 6, 7, 8))
```
- zipWithIndex zips each value with its index in the list
```scala
val list1WInd = list1.zipWithIndex  // list1WInd: Seq[(Int, Int)] = List((1,0), (2,1), (3,2), (4,3))
```

> Scala Cookbook by Alvin Alexander. 

# Chisel

- Why val and not var?
- Why := (it is not =)
    - Connect operator
- Bool vs. Bits(1)
- Scala vs. Chisel Types
- Different Errors (Java vs. Scala vs. Chisel)
- Reg of Vec not Vec of Reg?

### Why val and not var?

- Like scala, chisel prefers using vals when possible
- Given that chisel is constructing a circuit, chisel constructs often represent physical things like nets, registers, …
- It can be easier to reason about the circuit when constructs are assigned a unique, constant name (within a scope).

### Why := (it is not =)  or  What is going on with :=?

- Since we like to use vals when declaring chisel constructs, how do we make circuit assignments?
- Scala won’t let you use =
- := is a special operator defined by chisel to represent circuit connections (similar to assignment in Verilog)
    - It defines which chisel constructs should be connected and in which cases
    - The when block will put in multiplexers as appropriate
- Data about connections is used by chisel to build a graph representation of the circuit

### Scala vs. Chisel Types

- Scala and Chisel have different types
- This is because chisel needs information that Scala does not
- Bit width information
- Unfortunately, the type inference system that Scala uses does not work as well with Chisel types.
- Type promotion is not necessarily automatic (an artifact of Chisel’s implementation
- You may need to cast between Scala and Chisel types
- You may need to cast between Chisel types
```scala
val a = 1.U(8.W)
a.toBits //converts to bits
val b = 5
val c = b.U(8.W) //cast from Int to UInt 									//(actually construct obj)
```

### Errors and Chisel

- Because Chisel is built on Scala and Scala is Built on Java, you can get 4 kinds of error messages
    - Scala compiler errors: Type mismatches, syntax, …
    - Firrtl errors: errors found during low level transforms and verilog emission
    - Chisel checks: Illegal chisel but legal scala
    - Java Stack Trace: Underlying implementation crashed

### A simple debugging example

- Problem: Types
    - Chisel Type Expected
    - “Compile Time” Error
- Solution
    - Explicitly Cast to Chisel Type

```scala
  when(io.en) {
    index := 0.U
  }
  .otherwise {
    index := index + 1
  }
```
Results in the error below note the line number at *GCD.scala:35*
```
[info] Compiling 1 Scala source to /Volumes/UCB-BAR/chisel-template/target/scala-2.11/classes...
[error] /Volumes/UCB-BAR/chisel-template/src/main/scala/example/GCD.scala:35: type mismatch;
[error]  found   : Int(1)
[error]  required: chisel3.core.UInt
[error]       index := index + 1
[error]                        ^
[error] one error found
[error] (compile:compileIncremental) Compilation failed
[error] Total time: 2 s, completed Mar 7, 2017 8:02:15 PM
```
*index* is a chisel UInt type and so the 1 must be explicitly made a chisel type as well.
```scala
  when(io.en) {
    index := 0.U
  }
  .otherwise {
    index := index + 1.U
  }
```

### Decoupled 

- The Decoupled interface is a ready/valid interface
- The producer drives the bits (data) and valid lines
- The receiver drives the ready line
- The producer raises the valid line when data is on the bits line
- The receiver raises the ready line when they are ready to receive data
- If ready and valid are both high, a transaction occurred (sometime called fire)
    - The receiver has to read in bits during the cycle that both ready and valid or high
    - The producer is allowed to put a new value on the bits line in the next cycle
    - If valid is still high in the next cycle, it is assumed that there is a new value

```scala
class DecoupledIO[+T <: Data](gen: T) extends Bundle {
    val ready = Input(Bool())
    val valid = Output(Bool())
    val bits  = Output(gen) 
//…
}
```

### HCL vs HDL

| HCL | HDL |
| --- |:---:|
|Code describes how the hardware design is constructed | Code *describes* the behavior of hardware and the connections between hardware components
 HCL code is a program. Support for functional programming and object oriented paradigms | Generally static description (except for generate blocks) |
 HDL is generated by running the HCL Code |  |
 




