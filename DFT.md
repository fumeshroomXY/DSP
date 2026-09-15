# DFT(Discrete Fourier Transform)

## Definition

For an $N$-point signal: $X[k] = \sum_{n=0}^{N-1} x[n] e^{-j2\pi kn/N}$

where:
- $N$ = number of samples
- $n$ = time/sample index
- $k$ = frequency-bin index
- $X[k]$ = strength of frequency $k$

## What does $k$ mean?

If $N=8$, then the DFT checks these frequencies: $k=0,1,2,3,4,5,6,7$

Each corresponds to $f_k = \frac{k}{N}f_s$​ where $f_s$​ is the **sampling frequency**.

Example:

- $k=0$: DC (constant signal)
- $k=1$: one cycle in the observation window
- $k=2$: two cycles
- ...
- $k=4$: Nyquist frequency


## How the DFT works

Recall the **"frequency detector"** in the Fourier Transform.

To measure frequency $k$, DFT multiplies the signal by $e^{-j2\pi kn/N}$

and sums: $X[k] = \sum_{n=0}^{N-1} x[n] e^{-j2\pi kn/N}$

If the signal contains that frequency:

- products add constructively
- result becomes large

If not:
- positive and negative parts cancel
- result becomes small

## Example

Let $x[n]=1$ for $n=0...7$.

The signal is constant.

For $k=0$: $X[0] = \sum_{n=0}^{7} 1 = 8$

For $k=1$: $X[1] = 1+e^{-j2\pi/8} +e^{-j4\pi/8} +\cdots +e^{-j14\pi/8} = 0$

All rotating vectors cancel out.

Result: $X = [8,0,0,0,0,0,0,0]$

Meaning: The signal contains **only DC**.

## Connection to $z=e^{j\omega}$

In the Fourier Transform, we discussed the frequency response: $H(e^{j\omega})$

There: $z=e^{j\omega}$ lies on the unit circle.

The DFT evaluates only specific points on that unit circle: $\omega_k=\frac{2\pi k}{N}$​

so $z_k=e^{j2\pi k/N}$

For $N=8$:

$z_0=e^{j0}$

$z_1=e^{j\pi/4}$

$z_2=e^{j\pi/2}$
...

These are 8 equally spaced points around the unit circle.

The DFT measures the signal **at exactly these frequencies**.
