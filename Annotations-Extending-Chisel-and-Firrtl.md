Annotations are used to mark modules and their elements in a way that can be accessed by Firrtl transformation passes.  Custom passes and the annotations that guide their behavior extend the circuit generation capabilities of Chisel/Firrtl.  This article focuses on the approach to building a library that contains Annotations and Transforms.  The [src/test/scala/chiselTests/AnnotatingDiamondSpec.scala](src/test/scala/chiselTests/AnnotatingDiamondSpec.scala) 

### Write a transform
```
class IdentityTransform extends Transform {
  override def inputForm: CircuitForm = LowForm

  override def outputForm: CircuitForm = LowForm

  override def execute(state: CircuitState): CircuitState = {
    getMyAnnotations(state) match {
      case Nil => state
      case myAnnotations =>
        state
    }
  }
}
```