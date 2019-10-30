# Intervals
**Intervals** are a new experimental numeric type that comprises UInt, SInt and FixedPoint numbers.
It augments these types with **range** information, i.e. upper and lower numeric bounds.
This information can be used to exercise tighter programmatic control over the ultimate widths of
signals in the final circuit.  The **Firrtl** compiler can infer this range information based on 
operations and earlier values in the circuit. Intervals support all the ordinary bit and arithmetic operations
associated with UInt, SInt, and FixedPoint and adds the following methods for manipulating the range of
a **source** Interval with the IntervalRange of **target** Interval 

# Creating Intervals
To take as much advantage as you can from Intervals it is best to not specify their range or width and let the Firrtl compiler figure it out for you.
But there are many cases, such as IO ports and certain register use cases where you must specify the **range**.
The **range** is specified in a chisel3 class IntervalRange, which has 3 parameters. A lower bound, an upper bound and a binary point. Creating an IntervalRange class is quite verbose so chisel3 provides a short hand way to specify them. It looks like
```scala
range"[0,500).2"
```
The notation may look familiar, it is based on the mathematical way of specifying range on the number line. 

- `[0` means this range starts at zero.
- `500)` means that this range goes up to but does not include 500, i.e. The largest number in this range is 499.75. The `.75` is because the binary point for this range is 2
- `.2` specifies the binary point to be used for this range. The binary point is optional.

`[` and `]` specified closed endpoints in the range where the value associated for each is include in the range. Likewise `(` and `)` indicated open endpoints that are not include in the range.
The endpoints must be either a decimal (with or without a fractional component) value or a `?` which indicates inferred end point.

Here are some example of creating Interval hardware types
```scala
val i1 = Interval()                 // No range specified.
val i2 = Interval(range"[?,?]")     // Another way of specifying no range.
val i3 = Interval(range"[?,?].4")   // Also inferred range, but with a binary point of 2
val i4 = Interval(range"[-8,6).3")  // Range goes from -8 to 5.875, binary point is 3
```

Currently binary points must be positive, though there is interest in supporting negative binary points as a way of specifying a form of exponentiation. 

### Clip -- Fit the value **source** into the IntervalRange of **target**, saturate if out of bounds
The clip method applied to an interval creates a new interval based on the argument to clip,
and constructs the necessary hardware so that the source Interval's value will be mapped into the new Interval.
Values that are outside the result range will be pegged to either maximum or minimum of result range as appropriate.

> Generates necessary hardware to clip values, values greater than range are set to range.high, values lower than range are set to range min.

### Wrap -- Fit the value **source** into the IntervalRange of **target**, wrapping around if out of bounds
The wrap method applied to an interval creates a new interval based on the argument to wrap,
and constructs the necessary
hardware so that the source Interval's value will be mapped into the new Interval.
Values that are outside the result range will be wrapped until they fall within the result range.

> Generates necessary hardware to wrap values, values greater than range are set to range.high, values lower than range are set to range min.

> Does not handle out of range values that are less than half the minimum or greater than twice maximum

### Squeeze -- Fit the value **source** into the smallest IntervalRange based on source and target.
The squeeze method applied to an interval creates a new interval based on the argument to clip, the two ranges must overlap
behavior of squeeze with inputs outside of the produced range is undefined.

> Generates no hardware, strictly a sizing operation

#### Range combinations

| Condition | A.clip(B) | A.wrap(B) | A.squeeze(B) |
| --------- | --------------- | --------------- | --------------- |
| A === B   | max(Alo, Blo), min(Ahi, Bhi)  | max(Alo, Blo), min(Ahi, Bhi)  | max(Alo, Blo), min(Ahi, Bhi)  |
| A contains B   | max(Alo, Blo), min(Ahi, Bhi)  | max(Alo, Blo), min(Ahi, Bhi)  | max(Alo, Blo), min(Ahi, Bhi)  |
| B contains A   | max(Alo, Blo), min(Ahi, Bhi)  | max(Alo, Blo), min(Ahi, Bhi)  | max(Alo, Blo), min(Ahi, Bhi)  |
| A min < B min, A max in B  | max(Alo, Blo), min(Ahi, Bhi)  | max(Alo, Blo), min(Ahi, Bhi)  | max(Alo, Blo), min(Ahi, Bhi)  |
| A min in B, A max > B max  | max(Alo, Blo), min(Ahi, Bhi)  | max(Alo, Blo), min(Ahi, Bhi)  | max(Alo, Blo), min(Ahi, Bhi)  |
| A strictly less than B   | error               | error               | error               |
| A strictly greater than B   | error               | error               | error               |


### Applying binary point operators to an Interval

Consider a Interval with a binary point of 3: aaa.bbb 

| operation | after operation | binary point | lower | upper | meaning | 
| --------- | --------------- | ------------ | ----- | ----- | ------- |
| setPrecision(2) | aaa.bb |  2 | X | X  | set the precision |
| increasePrecision(2) | a.aabbb |  5 | X | X  | increase the precision |
| decreasePrecision(2) | aaaa.b |  1 | X | X  | reduce the precision |

