### Intervals
Intervals are a new experimental numeric type that comprises UInt, SInt and FixedPoint numbers.
It augments these types with range information, i.e. upper and lower numeric bounds.
This information can be used to exercise tighter programmatic control over the ultimate widths of
signals in the final circuit.  The **Firrtl** compiler can infer this range information based on 
operations and earlier values in the circuit.

Applying binary point operators to an Interval

For a number of the form
aaa.bbb is Interval 

| operation | after operation | binary point | lower | upper | meaning | 
| --------- | --------------- | ------------ | ----- | ----- | ------- |
| setBinaryPoint(2) | aaa.bb |  2 | X | X  | set the precision |

