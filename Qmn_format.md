# Fixed-point number formats
Q1.31 and Q2.62 are fixed-point number formats. The notation is: $Qm.n$

where:

- $m$ = number of integer bits (including the sign bit in many DSP conventions)
- $n$ = number of fractional bits
- Total bits = $m+n$

## Q1.31

- 1 sign/integer bit
- 31 fractional bits
- Total = 32 bits

### Range

The smallest step is: $2^{-31}$

The range is: $-1 \le x \le 1-2^{-31}$

Approximately: $-1 \le x \le 0.9999999995$

### Examples
| Decimal | Q1.31 value                   |
| ------- | ----------------------------- |
| 0.5     | 0x40000000                    |
| 0.25    | 0x20000000                    |
| -0.5    | 0xC0000000                    |
| 1.0     | cannot be represented exactly |

For example: $0.5 = 2^{-1}$

so its binary form is
```
0.1000000000000000000000000000000
```
which becomes:
```
01000000000000000000000000000000
```

### Why DSP engineers like Q1.31
Most filter coefficients satisfy $|a|<1$

so Q1.31 gives:

- excellent precision (31 fractional bits)
- convenient 32-bit storage
- efficient implementation on CPUs/DSPs

## Q2.62
### Bit growth
```
Q1.31 × Q1.31 = Q2.62
```
because:
- integer bits: $1+1=2$
- fractional bits: $31+31=62$

### Example

Take: $0.5$ in Q1.31:
```
0x40000000
```
Multiply by itself: $0.25$

The processor actually computes:
```
0x40000000 × 0x40000000 = 0x1000000000000000
```
That 64-bit result is interpreted as a Q2.62 number.

- 1 sign/integer bit + 1 integer bit
- 62 fractional bits
- Total = 64 bits

### Why convert back to Q1.31?
The delay element stores: $y(n-1)$

If we kept Q2.62 everywhere:

- memory doubles
- computation cost increases

Therefore DSP implementations usually:

- Compute in Q2.62
- **Round/truncate** to Q1.31
- **Saturate** if necessary
- Store as Q1.31

# Saturation
Saturation prevents the result from exceeding the range that can be represented by the output format.

A signed Q1.31 number can represent approximately: $-1.0 \le x < 1.0$

More precisely: $-1 \le x \le 0.9999999995$

Suppose the accumulator produces: $1.25$

This cannot be represented in Q1.31.

## Without saturation
If you simply truncate the higher bits, the value may wrap around due to 2's complement arithmetic:
```
1.25 --> overflow --> negative number
```
That incorrect value may be fed back into the filter and affects all future outputs.

This creates large **distortion** and can even make an IIR filter **unstable**.

## With saturation
Instead of wrapping around, the value is clipped to the maximum representable value:

$\text{saturate}(1.25)=0.9999999995$

Similarly, $\text{saturate}(-1.4)=-1.0$

Graphically:
```
Input       Output
1.4  ------> 1.0
1.0  ------> 1.0
0.5  ------> 0.5
0    ------> 0
-0.5 ------> -0.5
-1.0 ------> -1.0
-1.6 ------> -1.0
```

## In DSP hardware
The saturation block typically performs:

<img src="images/saturate.png" width="70%">

So any value outside the Q1.31 range is **clipped to the nearest representable limit** rather than allowed to overflow and wrap around.
