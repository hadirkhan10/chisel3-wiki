This page is intended to give a brief overview of a few key Scala concepts for Chisel users. We recommend reading this before starting [A Short Users Guide to Chisel](Short Users Guide to Chisel) as this cheatsheet will help explain some non-obvious Scala features and patterns.

For people completely new to Scala, we recommend working through an online tutorial like [this one](https://www.tutorialspoint.com/scala/index.htm) or [this one](http://docs.scala-lang.org/tutorials/); or better yet, taking the [Functional Programming in Scala Coursera Course](https://www.coursera.org/learn/progfun1)

### Classes

### Singleton Objects

A singleton object is an 

### Companion Objects

A companion object is a singleton object that is associated with a class of the same name. For example:

```scala
class Foo(value: Int)
object Foo {
  def makeFoo(num: Int): Foo = {
    val foo = new Foo(num)
    println("Made a Foo with value " + foo.value)
    foo
  }
}
```

### Apply Methods

`apply` is the method invoked whenever parentheses are used on an object. For example:

```scala
val xs = List("a", "b", "c")
println(xs(1))
```

This prints `b`. So where is the `apply` method in the above example? Actually there are two:

1. `List("a", "b", "c")` is calling [`apply` on the List companion object](http://www.scala-lang.org/api/current/scala/collection/immutable/List$.html#apply[A](xs:A*):List[A]). This method returns a list which was then assigned to `xs`
1. `xs(1)` is calling [`apply` on the instance of class List itself](http://www.scala-lang.org/api/current/scala/collection/immutable/List.html#apply(n:Int):A), which is used to index into the List and return the n-th element (`b` in this example).

