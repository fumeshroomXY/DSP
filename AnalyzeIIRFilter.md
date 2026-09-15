Example Filter
$H(z)=\frac{0.0675+0.1349z^{-1}+0.0675z^{-2}} {1-1.1430z^{-1}+0.4128z^{-2}}$

This is a typical Butterworth low-pass biquad.

# Difference Equation
Starting from $H(z)=\frac{Y(z)}{X(z)}$

we have $Y(z)(1-1.1430z^{-1}+0.4128z^{-2}) = X(z)(0.0675+0.1349z^{-1}+0.0675z^{-2})$

Taking the inverse Z-transform:

$y[n] = 0.0675x[n] +0.1349x[n-1] +0.0675x[n-2] +1.1430y[n-1] -0.4128y[n-2]$

This uses:

- $x[n],x[n−1],x[n−2]$
- $y[n−1],y[n−2]$

Therefore it is a 2nd-order IIR filter.

# Zeros

The numerator is $B(z)=0.0675+0.1349z^{-1}+0.0675z^{-2}$

Multiply by $z^2$: $0.0675z^2+0.1349z+0.0675=0$

Divide by 0.0675: $z^2+2z+1=(z+1)^2=0$

Therefore the filter has two zeros: $\boxed{z=-1}$​ (two coincident zeros)

What does that mean?

Remember: $z=e^{j\omega}$

At Nyquist frequency: $\omega=\pi$ $z=e^{j\pi}=-1$

Since a zero exists exactly at $z=-1$, $H(e^{j\pi})=0$

So **high-frequency components are strongly suppressed**.

# Poles

Denominator: $A(z)=1-1.1430z^{-1}+0.4128z^{-2}$

Multiply by $z^2$: $z^2-1.1430z+0.4128=0$

Solve: $z = \frac{1.1430 \pm \sqrt{1.1430^2-4(0.4128)} }{2}$ 

$z = 0.5715 \pm 0.296j$

Therefore poles are $0.5715+0.296j$, $0.5715-0.296j$
​
# Pole Radius

Calculate: $r=\sqrt{0.5715^2+0.296^2}\approx 0.642$ 

Since $r<1$, both poles lie inside the unit circle.

Therefore **Stable**


# Pole Angle
$\theta=\tan^{-1} \left( \frac{0.296}{0.5715} \right)\approx 0.48 \text{ rad}$

Pole location: $0.642 e^{\pm j0.48}$

The pole angle roughly determines where the filter transition occurs.

# DC Gain

Evaluate at $z=1$

Numerator: $0.0675+0.1349+0.0675 = 0.2699$

Denominator: $1-1.1430+0.4128 = 0.2698$

Thus H(1) = $\frac{0.2699}{0.2698} \approx 1$

$\text{DC gain } \approx 1$​

Low frequencies pass through unchanged.

# Nyquist Gain

Evaluate at $z=-1$

Numerator: $0.0675-0.1349+0.0675\approx 0$

Denominator: $1+1.1430+0.4128 = 2.5558$

Therefore H(-1)=0, $\text{Nyquist gain}=0$​

High frequencies are completely removed.

# Impulse Response

If $x[n]=\delta[n]$

then $h[n]=0.0675, 0.212, 0.281, 0.234, 0.151, ...$

The response gradually **decays toward zero**.

Because there are complex poles, you will often see a slight damped oscillation:

$h[n] \approx Cr^n\cos(\theta n+\phi)$

with $r=0.642$

and $\theta=0.48$

# Frequency Response Interpretation

The poles are near the positive real axis: $0.5715 \pm 0.296j$

and the zeros are at $-1$

This combination creates:

- Gain ≈ 1 at DC
- Gain gradually decreases with frequency
- Gain = 0 at Nyquist
