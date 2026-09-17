# Summary
- First, given a input waveform, we want to know the output waveform after it passes a filter.
- Since any waveform can be decomposed into **a sum of sinusoidal frequency components**, we only need to know how the filter affects each frequency.
- To represent all frequencies, we need to ensure the sampling frequency must be **twice greater** than the highest frequency.
- The half of sample frequency is Nyquist frequency. It represents the highest unique frequency that can be seen in a sampled signal.
- Then we correspond [0, sampling frequency] to $[0,2\pi]$ to represent different frequencies.
- The interval $[\pi,2\pi]$ does not contain new frequencies. Therefore, for real signals, all unique positive frequencies are already contained in $[0,\pi]$
- Every frequency corresponds to **a point on the unit circle**: $z=e^{j\omega}$


# What is $n$ in $x[n]$?

In discrete-time systems, we don't have continuous time $t$.

Instead, we have samples:
```
Time:     0    1     2    3    4    5 ...
Sample:  x[0] x[1] x[2] x[3] x[4] x[5]
```

So: $x[n]$ means the value of the signal at sample number $n$.

# What is $\omega$?
In DSP we often use digital frequency $\omega$ instead of physical frequency $f$.

The conversion is

$\omega = 2\pi \frac{f}{f_s}$

​Therefore:

- DC: $f=0$ becomes $\omega=0$
- Nyquist: $f=f_s/2$ becomes $\omega = 2\pi \frac{f_s/2}{f_s} = \pi$

So: $[0,f_s/2]$ maps to $[0,\pi]$

The digital frequency axis actually goes from $0 \rightarrow 2\pi$ because $e^{j\omega}$  is periodic: $e^{j\omega} = e^{j(\omega+2\pi)}$

But the interval $[\pi,2\pi]$ does not contain new frequencies. It is just the negative-frequency side.

Therefore, for real signals, all unique positive frequencies are already contained in $[0,\pi]$ which corresponds to $[0,f_s/2]$

# Connect with the unit circle
In DSP and digital filters, $z$ is generally **a complex variable**: $z = re^{j\omega}$ where

- $r$ = magnitude (distance from the origin)
- $\omega$ = angle (frequency)
- $j=\sqrt{-1}$​

Because we care more about the frequency, so we suppose $r=1$. 

Then using Euler's formula:

$e^{j\theta} = \cos\theta + j\sin\theta$

(We use $e^{j\theta}$ because it makes the mathematics and calculation much simpler)

Every frequency corresponds to a point on the unit circle:

$z = e^{j\omega}$

- When: $\omega=0$ → $z=1$ (DC)
- When: $\omega=\pi$ → $z=-1$ (Nyquist)

As $\omega$ runs from 0 to $\pi$, you move along the upper half of the unit circle.

For $x[n] = e^{j\omega n} = \left(e^{j\omega}\right)^n = z^n$, the angle is $\theta = \omega n$

At each new sample:
```
n=0  -> angle = 0
n=1  -> angle = ω
n=2  -> angle = 2ω
n=3  -> angle = 3ω
```
So every sample rotates by $\omega$ radians.

## What does ω do?
$\omega$ tells us **how fast the signal changes from sample to sample**.

### DC 

$\omega=0$ → $z = e^{j0} = 1$ → $x[n] = 1$

Constant signal.

### Nyquist

$\omega=\pi$ → $z = e^{j\pi} = -1$ → $x[n] = (-1)^n$

```
1 -1 1 -1 1 -1
```
Highest frequency.


# Why DSP loves exponentials
For imaginary numbers: $e^{j\theta}$ describes rotation.

As $\theta$ increases:
```
1 -> j -> -1 -> -j -> 1
```
The point rotates around the unit circle.

So exponentials are not only for growth. They also **naturally describe rotation**.

Suppose $x[n] = e^{j\omega n}$

Now advance one sample: $x[n+1] = e^{j\omega(n+1)} = e^{j\omega n}e^{j\omega}$

Notice something interesting:

- Moving from one sample to the next simply multiplies by $e^{j\omega}$ which is a fixed rotation.

This matches exactly how digital filters work, because delays become powers of $z^{-1}$.

If the input is $x[n]=e^{j\omega n}$

then the output is $y[n] = H(e^{j\omega})e^{j\omega n}$

The filter only changes:

- amplitude
- phase

It does **not** change the frequency.

This property makes filter analysis incredibly simple.

Sines and cosines have this property too, but complex exponentials make the mathematics **much cleaner**.

# Why do poles often come in pairs?
#### Example

Consider $H(z)=\frac{1}{1-1.5z^{-1}+0.81z^{-2}}$

The poles satisfy $1-1.5z^{-1}+0.81z^{-2}=0$

Multiplying by $z^2$: $z^2-1.5z+0.81=0$

Solving: $z = 0.75 \pm j0.487$

These are **complex poles**.

The pole locations can also be written as $z = 0.894\, e^{\pm j0.576}$

which tells us:

- radius $r=0.894$
- angle $=\pm0.576$ rad

For filters with real coefficients ($a_i,b_i$​ are real numbers), any complex pole $p=a+jb$

must be accompanied by its complex conjugate $p^*=a-jb$

So a biquad can contain:

- 2 real poles
- 2 real zeros
- or 1 complex-conjugate pole pair
- or 1 complex-conjugate zero pair
