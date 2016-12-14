### List of operators
Chisel defines a set of hardware operators

| Operation        | Explanation |
| ---------        | ---------           |
| **Bitwise operators**                       | **Valid on:** SInt, UInt, Bool    |
| `val invertedX = ~x`                        | Bitwise NOT |
| `val hiBits = x & UInt("h_ffff_0000")`      | Bitwise AND                     |     
| `val flagsOut = flagsIn | overflow`         | Bitwise OR                      |   
| `val flagsOut = flagsIn ^ toggle`           | Bitwise XOR                     |  
| **Bitwise reductions.**                     | **Valid on:** SInt and UInt. Returns Bool. |
| `val allSet = x.andR`                       | AND reduction                     |   
| `val anySet = x.orR`                        | OR reduction                      |  
| `val parity = x.xorR`                       | XOR reduction                     |   
| **Equality comparison.**                    | **Valid on:** SInt, UInt, and Bool. Returns Bool. |
| `val equ = x === y`                         | Equality                          |
| `val neq = x =/= y`                         | Inequality                        |
| **Shifts**                                  | **Valid on:** SInt and UInt       |
| `val twoToTheX = 1.S << x`                  | Logical shift left                |
| `val hiBits = x >> 16.U`                    | Right shift (logical on UInt and arithmetic on SInt). |  
| **Bitfield manipulation**                   | **Valid on:** SInt, UInt, and Bool. |
| `val xLSB = x(0)`                           | Extract single bit, LSB has index 0.     |  
| `val xTopNibble = x(15, 12)`                | Extract bit field from end to start bit position.     |            
| `val usDebt = Fill(3, "hA".U)`              | Replicate a bit string multiple times.     |               
| `val float = Cat(sign, exponent, mantissa)` | Concatenates bit fields, with first argument on left.     |
| **Logical Operations**                      | **Valid on:** Bool                                                                             
| `val sleep = !busy`                         | Logical NOT                       |       
| `val hit = tagMatch && valid`               | Logical AND                       |                 
| `val stall = src1busy || src2busy`          | Logical OR                        |                     
| `val out = Mux(sel, inTrue, inFalse)`       | Two-input mux where sel is a Bool |                                               
| **Arithmetic operations**                   | **Valid on Nums:** SInt and UInt.  |
| `val sum = a + b` *or* `val sum = a +% b`   | Addition (without width expansion) |
| `val sum = a +& b`                          | Addition (with width expansion)    |         
| `val diff = a - b` *or* `val diff = a -% b` | Subtraction (without width expansion) |
| `val diff = a -& b`                         | Subtraction (with width expansion) |            
| `val prod = a * b`                          | Multiplication                     |            
| `val div = a / b`                           | Division                           |           
| `val mod = a % b`                           | Modulus                            |           
| **Arithmetic comparisons**                  | **Valid on Nums:** SInt and UInt. Returns Bool. |
| `val gt = a > b`                            | Greater than                       |      
| `val gte = a >= b`                          | Greater than or equal              |                 
| `val lt = a < b`                            | Less than                          |   
| `val lte = a <= b`                          | Less than or equal                 |              

>Our choice of operator names was constrained by the Scala language.
We have to use triple equals```===``` for equality and ```=/=```
for inequality to allow the
native Scala equals operator to remain usable.

### Width Inference

Chisel provides bit width inference to reduce design effort. Users are encouraged to manually specify widths of ports and registers to prevent any surprises, but otherwise unspecified widths will be inferred by the compiler. All unspecified widths will be inferred to be the width of the maximum incoming connection. Hardware operators have output widths as defined by the following set of rules:

| operation        | bit width           |
| ---------        | ---------           |
| `z = x + y` *or* `z = x +% y`        | `w(z) = max(w(x), w(y))`   |
| `z = x +& y`       | `w(z) = max(w(x), w(y)) + 1`   |
| `z = x - y` *or* `z = x -% y`       | `w(z) = max(w(x), w(y))`    |
| `z = x -& y`       | `w(z) = max(w(x), w(y)) + 1`   |
| `z = x & y`        | `w(z) = min(w(x), w(y))`    |
| `z = Mux(c, x, y)` | `w(z) = max(w(x), w(y))`    |
| `z = w * y`        | `w(z) = w(x) + w(y)`        |
| `z = x << n`       | `w(z) = w(x) + maxNum(n)` |
| `z = x >> n`       | `w(z) = w(x) - minNum(n)` |
| `z = Cat(x, y)`    | `w(z) = w(x) + w(y)`      |
| `z = Fill(n, x)`   | `w(z) = w(x) * maxNum(n)` |
>where for instance `w(z)` is the bit width of wire `z`, and the `&`
rule applies to all bitwise logical operations.

Given a path of connections that begins with an unspecified width element (most commonly a top-level input), then the compiler will throw an exception indicating a certain width was uninferrable. 

A common "gotcha" comes from truncating addition and subtraction with the operators `+` and `-`. Users who want the result to maintain the full, expanded precision of the addition or subtraction should use the expanding operators `+&` and `-&`.

> The default truncating operation comes from Chisel's history as a microprocessor design language.


[Prev (Combinational Circuits)](Combinational Circuits)  [Next (Functional Abstraction)](Functional Abstraction)