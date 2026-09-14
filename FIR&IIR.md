In digital signal processing (DSP), FIR and IIR are two major types of **digital filters**.

# FIR (Finite Impulse Response)

A FIR filter's impulse response becomes exactly zero after a finite number of samples.

Example
```
y[n] = 1/4 * ​(x[n] + x[n−1] + x[n−2] + x[n−3])
x[n] = 1, 0, 0, 0, ...
h[n] = 1/4, 1/4, 1/4, 1/4, 0, 0, ...
The response ends after 4 samples → Finite.
```
Notice that the output depends **only on current and past input samples**.

#### Advantages

- Always stable (if coefficients are finite)
- Can achieve exact linear phase
- Simpler to analyze

#### Disadvantages
- More coefficients are usually needed
- Higher computational cost for sharp frequency responses

# IIR (Infinite Impulse Response)
An IIR filter's impulse response theoretically continues forever.

Example
```
y[n] = x[n] + 0.5y[n−1]

x[n] = 1, 0, 0, 0, ...
y[0] = 1
y[1] = 0.5
y[2] = 0.25
y[3] = 0.125
...
The response never becomes exactly zero.
```
Notice the output depends on:
- current/past inputs
- **past outputs**

This feedback is the key difference.

#### Advantages
- Achieves sharp filtering with fewer coefficients
- Lower CPU and memory usage

#### Disadvantages
- Can become unstable
- Phase response is generally nonlinear
- Harder to design and analyze

# FIR vs IIR
| Feature            | FIR                   | IIR                    |
| ------------------ | --------------------- | ---------------------- |
| Impulse Response   | Finite                | Infinite               |
| Feedback           | No                    | Yes                    |
| Stability          | Always stable         | May be unstable        |
| Linear Phase       | Easy                  | Generally impossible   |
| Computational Cost | Higher                | Lower                  |
| Memory Needed      | More                  | Less                   |
| Typical Use        | Audio, communications | Real-time embedded DSP |


# Biquad filter
Biquad stands for **biquadratic filter**. Its transfer function contains two quadratic polynomials:

$$H(z)=\frac{b_0+b_1z^{-1}+b_2z^{-2}}{1+a_1z^{-1}+a_2z^{-2}}$$

- Numerator: quadratic in $z^{-1}$
- Denominator: quadratic in $z^{-1}$

Hence **"bi" + "quadratic" = biquad**.

## Why is a biquad important?
A biquad is a **2nd-order filter section**:

- Up to **2 poles** because $b_0 + b_1 z^{-1} + b_2 z^{-2} = 0$ has at most **2 roots**.
- Up to **2 zeros** because $1 + a_1 z^{-1} + a_2 z^{-2} = 0$ has at most **2 roots**.

Most practical IIR filters are implemented by **cascading multiple biquads**.

For example:
```
Single biquad(2nd-order filter): $H(z)=H_1(z)$

Two biquads(4th-order filter): $H(z)=H_1(z)H_2(z)$

Four biquads(8th-order filter): $H(z)=H_1(z)H_2(z)H_3(z)H_4(z)$
```
A biquad is often implemented as:

$y[n] = b_0x[n] + b_1x[n-1] + b_2x[n-2] - a_1y[n-1] - a_2y[n-2]$

Notice it only needs:

- Current and two previous inputs
- Two previous outputs

This makes it efficient for DSPs, FPGAs, and microcontrollers.


## Why not implement a high-order filter directly?
Suppose you want an **8th-order low-pass filter**.

You could write it as one huge equation:

$H(z)=\frac{b_0+b_1z^{-1}+\cdots+b_8z^{-8}}{1+a_1z^{-1}+\cdots+a_8z^{-8}}$

or as four biquads:

$H(z)=H_1(z)H_2(z)H_3(z)H_4(z)$

where each $H_i$ is a 2nd-order filter.

The second approach is usually preferred because:

### Better numerical stability
Computers cannot store coefficients with infinite precision.

```
Suppose the true coefficient is: 0.123456789
but hardware stores:0.123457
```
This is called **coefficient quantization**.

For a high-order filter, tiny coefficient errors can move poles significantly.

For example:
```
Ideal pole location:
0.98 + j0.10

After rounding:
1.01 + j0.10
```

Now the pole is outside the unit circle: $∣p∣>1$ and the filter may become unstable.

#### Why biquads help

Each biquad only has: $b_0,b_1,b_2,a_1,a_2$​

so the rounding errors stay local.

Instead of managing 8 poles at once, you manage one pole pair at a time.


### Simpler tuning
Consider a filter with 8 poles.

In one giant equation:
```
8 poles all mixed together
```
It is difficult to understand which coefficient affects which behavior.

With biquads:
```
Stage 1 -> pole pair A
Stage 2 -> pole pair B
Stage 3 -> pole pair C
Stage 4 -> pole pair D
```
Now you can tune one stage at a time.

For example:
```
Stage 1:
controls low-frequency shape

Stage 2:
controls cutoff region

Stage 3:
controls transition band

Stage 4:
controls high-frequency behavior
```
If something looks wrong, you inspect the responsible stage instead of the entire filter.

### Easier implementation
You can build one reusable block and and connect several together:
```
Input
 |
[Biquad]
 |
[Biquad]
 |
[Biquad]
 |
[Biquad]
 |
Output
```
Much easier than creating a different circuit for each filter order.

### Intuitive analogy
Imagine you're building a staircase to the 8th floor.

One giant step, hard to climb and dangerous.
```
Ground
   |
   |
   |
   |
8th floor
```


Eight small steps, much easier to build and maintain.
```
Ground
Step1
Step2
Step3
Step4
Step5
Step6
Step7
Step8
```

## Stage
A **stage** usually refers to one filter section in a cascaded implementation.

A high-order IIR filter is often broken into several lower-order filters connected in series:

$H(z)=H_1(z)H_2(z)H_3(z)\cdots$

Each $H_i(z)$ is called a stage.

For example, a 6th-order IIR filter might be implemented as:

- Stage 1: 2nd-order section (biquad)
- Stage 2: 2nd-order section (biquad)
- Stage 3: 2nd-order section (biquad)

rather than one single 6th-order equation.

## Order
The **order** of a filter is essentially the number of memory elements (delays) it contains.

For a discrete-time filter, look at the highest delay term:

$$H(z)=\frac{b_0+b_1z^{-1}+b_2z^{-2}}{1+a_1z^{-1}+a_2z^{-2}}$$

The highest power of $z^{-1}$ in the denominator is 2, so this is a **2nd-order filter**.

Examples:

- 1st-order filter → one delay in feedback
- 2nd-order filter → two delays in feedback
- 8th-order filter → eight delays in feedback

Higher order generally means:

- Sharper frequency response
- Better selectivity
- More computation

A cascade of 4 biquads gives: $4×2=8$

so it's an 8th-order filter.

## Zeros
A **zero** is a frequency where the filter output becomes zero.

Mathematically: $H(z) = 0$

when $z$ equals a zero location.

### What does a zero do?
A zero creates attenuation. At some frequency, the signal is completely cancelled.

For example:

- Zero near DC ($z=1$) → removes low frequencies
- Zero near Nyquist ($z=-1$) → removes high frequencies

## Poles
A pole is where the denominator becomes zero.

### What does a pole do?

Poles create amplification or resonance.

Think of a tuning fork:

- Hit it once
- It keeps ringing

That's similar to what a pole does. A pole causes energy to persist.

A pole near a frequency tends to:

- Boost that frequency
- Increase ringing
- Increase filter sharpness
