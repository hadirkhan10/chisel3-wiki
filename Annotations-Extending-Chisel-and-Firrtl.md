Annotations are used to mark modules and their elements in a way that can be accessed by Firrtl transformation passes.  Custom passes and the annotations that guide their behavior extend the circuit generation capabilities of Chisel/Firrtl.  This article focuses on the approach to building a library that contains Annotations and Transforms.  We will walk through  [src/test/scala/chiselTests/AnnotatingDiamondSpec.scala](https://github.com/ucb-bar/chisel3/blob/master/src/test/scala/chiselTests/AnnotatingDiamondSpec.scala) to see the basic concepts.

### Imports
We need a few basic imports to reference the components we need.  The chisel3 is a standard 
```scala
import chisel3._
import chisel3.internal.InstanceId
import firrtl.{CircuitForm, CircuitState, LowForm, Transform}
import firrtl.annotations.{Annotation, ModuleName, Named}
```
### Write a transform
This is an identity transform that returns whatever it is passed without alteration.  See [Writing Firrtl Transforms](/ucb-bar/firrtl/wiki) for the gory details on writing transforms that actually do something.
```scala
class IdentityTransform extends Transform {
  override def inputForm: CircuitForm = LowForm
  override def outputForm: CircuitForm = LowForm

  override def execute(state: CircuitState): CircuitState = {
    getMyAnnotations(state) match {
      case Nil => state
      case myAnnotations =>
        // Use annotations contained in the myAnnotations list to modify state
        // and return that modified state.
        state
    }
  }
}
```
This creates a transform that operates on low Firrtl (LowForm) and returns low Firrtl.  ```getMyAnnotations``` returns a list of annotations for your pass.  This example does nothing with those annotations.
### Create an Annotation Factory
The following creates an annotation that is connected to your transform, note the ```classOf[IdentityTransform]```.  The unapply is a convenience method for extracting information from your annotation by using the Scala ```match``` operator
```scala
object IdentityAnnotation {
  def apply(target: Named, value: String): Annotation = Annotation(target, classOf[IdentityTransform], value)

  def unapply(a: Annotation): Option[(Named, String)] = a match {
    case Annotation(named, t, value) if t == classOf[IdentityTransform] => Some((named, value))
    case _ => None
  }
}
```