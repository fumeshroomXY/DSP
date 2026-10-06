# Fixed-point
A processing unit that supports **integer arithmetic** has most of the hardware needed for fixed-point arithmetic, but that does **not automatically mean** it fully supports fixed-point operations **efficiently**.

## Why fixed-point is "almost" integer
A fixed-point number is just an integer with an implied binary point.

For example, in Q15 format:
```
Real value    Stored integer

0.5           16384
1.0           32768
1.5           49152

Q15(0.5) + Q15(1.0)
16384 + 32768 = 49152
```
This is exactly the same operation as an integer add.

So if a unit can do:
```
ADD
SUB
MUL
SHIFT
```
it can already perform basic fixed-point arithmetic.

## What may be missing
Efficient fixed-point DSP often needs extra features:

### Saturation arithmetic
Normal integer:
```
127 + 1 = -128   (8-bit overflow)
```
Saturating arithmetic:
```
127 + 1 = 127
```
DSP algorithms often require saturation.

### Scaling shifts
For Q15 multiplication:
```
Q15 × Q15 → Q30
```
After multiplication:
```
result = (a * b) >> 15;
```
Efficient fixed-point hardware may provide this automatically.

### Multiply-Accumulate (MAC)
DSP code frequently does:
```
sum += a[i] * b[i];
```
Dedicated DSP units can perform MAC **in a single cycle**.

An integer ALU might require **MUL + ADD as separate instructions**.

## Examples
### Simple MCU ALU
```
Integer Add     ✓
Integer Sub     ✓
Integer Mul     ✓
Shift           ✓
Saturation      ✗
MAC             ✗
```
Can do fixed-point? ✅ Yes

Efficient? ❌ Not particularly.

### DSP Engine
```
Integer Add     ✓
Integer Mul     ✓
Shift           ✓
Saturation      ✓
MAC             ✓
Rounding        ✓
```
Can do fixed-point? ✅ Yes

Efficient? ✅ Very.

So the key idea is: 
- The **CPU** supplies the **integer operations**.
- The **DSP library** supplies the scaling, rounding, and fixed-point rules.



# Float-point
## FPU (Floating-Point Unit)
An FPU is **specialized for floating-point arithmetic**.

If the CPU has an FPU:
```
ALU       ✓
FPU       ✓
```
then:
```
float c = a + b;
```
may become something like:
```
FADD
```
executed directly by hardware.
```
Application
    ↓
FPU Instruction
    ↓
FPU Hardware
```
**Much faster and usually lower CPU load.**


## CPU can also support float-point calculations without FPU
Suppose you write:
```
float a = 1.5f;
float b = 2.5f;
float c = a + b;
```
If the CPU has no FPU:

1. The compiler generates calls to a **software floating-point library**.
2. The library manipulates the float bit patterns using integer instructions.
3. The CPU executes **integer operations only**.

Conceptually:
```
Application
    ↓
Software Float Library
    ↓
Integer Instructions
    ↓
CPU
```
The result is still correct, but **much slower**.

## Performance Difference
```
Integer add,          ~1 cycle
Float add with FPU,   ~1-5 cycles
Float add in software,  dozens to hundreds of cycles
Float divide in software,  hundreds to thousands of cycles
```
Exact numbers depend on the CPU, but software floating-point is often **10× to 100× slower** than hardware floating-point.

## Why Embedded DSP Often Uses Fixed-Point
On MCUs without an FPU:
```
Integer      Fast
Fixed-point  Fast
Float        Slow
```
Therefore DSP libraries often provide: Q15, Q31 fixed-point versions of FFT, FIR, IIR, etc.

Because they run using integer arithmetic and can be **much faster than software floating-point**.
