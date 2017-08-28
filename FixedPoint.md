Quick guide to using *FixedPoint*
FixedPoint is subclass of Bits
```
val io = IO(new Bundle {
  val fixedInput = FixedPoint(32.W, 16.BP)  // create a io port FixedPoint number with 32 
                                             // total bits 16 bits of which are mantissa
  val fixedOutput = FixedPoint(24.W, 8.BP)  // create a io port FixedPoint number with 24 
                                             // total bits 16 bits of which are mantissa
}

val fixedReg1 = FixedPoint(Width(), BP())  // create a register whose width and binary point will be inferred 

when(cond) {
  fixedReg1 = 6.77.F                     // set register to a FixedPoint constant
}


```