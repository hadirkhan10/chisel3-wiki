# Intervals
**Intervals** are a new experimental numeric type that comprises UInt, SInt and FixedPoint numbers.
It augments these types with range information, i.e. upper and lower numeric bounds.
This information can be used to exercise tighter programmatic control over the ultimate widths of
signals in the final circuit.  The **Firrtl** compiler can infer this range information based on 
operations and earlier values in the circuit. Intervals support all the ordinary bit and arithmetic operations
associated with UInt, SInt, and FixedPoint and adds the following methods for manipulating the range of
a **source** Interval with the IntervalRange of **target** Interval 

### Clip -- Fit the value **source** into the IntervalRange of **target**, saturate if out of bounds
The clip method applied to an interval creates a new interval based on the argument to clip,
and constructs the necessary hardware so that the source Interval's value will be mapped into the new Interval.
Values that are outside the result range will be pegged to either maximum or minimum of result range as appropriate.

### Wrap -- Fit the value **source** into the IntervalRange of **target**, wrapping around if out of bounds
The wrap method applied to an interval creates a new interval based on the argument to wrap,
and constructs the necessary
hardware so that the source Interval's value will be mapped into the new Interval.
Values that are outside the result range will be wrapped until they fall within the result range.

### Squeeze -- Fit the value **source** into the smallest IntervalRange based on source and target, clipping the result
The clip method applied to an interval creates a new interval based on the argument to clip,
and constructs the necessary hardware so that the source Interval's value will be mapped into the new Interval.
Values that are outside the result range will be pegged to either maximum or minimum of result range as appropriate.

### Applying binary point operators to an Interval

Consider a Interval with a binary point of 3: aaa.bbb 

| operation | after operation | binary point | lower | upper | meaning | 
| --------- | --------------- | ------------ | ----- | ----- | ------- |
| setBinaryPoint(2) | aaa.bb |  2 | X | X  | set the precision |
| shiftLeftBinaryPoint(2) | a.aabbb |  5 | X | X  | increase the precision |
| shiftRighBinaryPoint(2) | aaaa.b |  1 | X | X  | reduce the precision |

