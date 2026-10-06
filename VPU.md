# VPU (Vector Processing Unit)
A **VPU** shines when the same arithmetic operation is applied to **many data elements in parallel**.

For example, instead of:
```
for(i = 0; i < 8; i++)
{
    y[i] = a[i] + b[i];
}
```
a VPU can process 8 elements simultaneously:
```
[a0 a1 a2 a3 a4 a5 a6 a7]
+
[b0 b1 b2 b3 b4 b5 b6 b7]
=
[y0 y1 y2 y3 y4 y5 y6 y7]
```
So the main advantage is **data-level parallelism**.

## Operations that are ideal for a VPU
### 1. Vector Add/Subtract
```
y[i] = a[i] + b[i];
```
Every element is independent. Nearly perfect VPU utilization.

### 2. Multiply-Accumulate (MAC)
```
sum += a[i] * b[i];
```
This is the foundation of:

- FIR filters
- FFT
- Matrix multiplication
- Neural networks

Most DSP VPUs have dedicated MAC instructions, so this is where acceleration is huge.

### 3. FIR Filter
```
y[n] = Σ(h[k] * x[n-k])
```
Example:
```
h0*x0
h1*x1
h2*x2
h3*x3
```
can be multiplied simultaneously.

FIR is one of the most VPU-friendly DSP algorithms.

### 4. FFT
FFT consists of many:
```
Add
Subtract
Multiply
```
operations performed repeatedly on large arrays.

The **butterfly structure** maps well onto vector registers.

### 5. Matrix Operations
```
C = A × B
```
Many parallel multiplications and additions.

High arithmetic density.

Excellent VPU utilization.

## Operations that are bad for a VPU
### 1. Conditional operation is required
For signed data, absolute value is:
```
if (x < 0)
    x = -x;

or abs(x)
```
### 2. Dependencies
```
sum += x[i]
```
Each iteration depends on the previous result.

This is called a **reduction**.

### 3. Random memory access
```
x[index[i]]
```
Random access hurts vector performance.

### 4. Low arithmetic intensity
```
y = abs(x)
```
Only one simple operation per load.

## Many VPUs can process float-point data
Modern VPUs often support **float** and sometimes **double**.

Operations:
```
y[i] = a[i] + b[i];
y[i] = a[i] * b[i];
```
executed on multiple floating-point values simultaneously.


