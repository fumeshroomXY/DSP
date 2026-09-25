# IFFT (Inverse Fast Fourier Transform)
It's the algorithm that converts a signal from the **frequency domain** back into the **time domain**.

Suppose an FFT tells you that a signal contains:

- A 1 kHz sine wave
- Amplitude = 1

The FFT output is just frequency information.

If you feed those frequency bins into an IFFT, the IFFT **reconstructs** the original time-domain waveform:
```
Frequency bins
      │
      ▼
     IFFT
      │
      ▼
sin(2π·1000·t)
```

## Mathematical Definition
For an N-point IFFT: $x[n] = \frac{1}{N}\sum_{k=0}^{N-1} X[k] e^{j2\pi kn/N}$

where:
- $X[k]$ = frequency-domain bins
- $x[n]$ = reconstructed time-domain samples

Compare with FFT: $X[k] = \sum_{n=0}^{N-1} x[n] e^{-j2\pi kn/N}$

For a **conjugate-symmetric IFFT**, an important property is:

If the frequency bins satisfy $X[k] = X^*[N-k]$ (conjugate symmetry)

then the IFFT output is **purely real-valued**. 

# Examples
## Example 1: A Single Frequency Bin
Suppose the frequency-domain data is $X=[0, 4, 0, 0]$

This means:

- Bin 0 (DC) = 0
- Bin 1 = 4
- Bin 2 = 0
- Bin 3 = 0

The 4-point IFFT is $x[n]=\frac{1}{4}\sum_{k=0}^{3}X[k]e^{j2\pi kn/4}$

Since only $X[1]$ is nonzero: $x[n] = \frac{1}{4}\cdot4\cdot e^{j2\pi n/4} = e^{j2\pi n/4}$

Now evaluate each sample.

- n = 0, $x[0]=e^{j0}=1$
- n = 1, $x[1]=e^{j\pi/2}=j$
- n = 2, $x[2]=e^{j\pi}=-1$
- n = 3, $x[3]=e^{j3\pi/2}=-j$

Therefore $x=[1, j, −1, −j]$

After 4 samples we're back to the starting point:
```
1 → j → -1 → -j → 1
```
That's exactly **one cycle in 4 samples**, which is what Bin 1 represents.
```
Bin 0 = 0 cycles / frame
Bin 1 = 1 cycle / frame
Bin 2 = 2 cycles / frame
Bin 3 = 3 cycles / frame
```

## Example 2: Conjugate-Symmetric IFFT (Real Output)
Suppose $X=[0, 2, 0, 2]$

Notice: $X[1]=X^*[3]$, so the spectrum is conjugate-symmetric.

Compute: $x[n] = \frac14 \Big( 2e^{j2\pi n/4} + 2e^{-j2\pi n/4} \Big)$

Using $e^{j\theta}+e^{-j\theta}=2\cos\theta$ gives $x[n]=\cos(2\pi n/4)$

Now evaluate:
- n = 0, x[0]=1
- n = 1, x[1]=0
- n = 2, x[2]=-1
- n = 3, x[3]=0

Result: $x=[1, 0, −1, 0]$, which is purely real.

# Intuition
Think of the IFFT as a signal synthesizer:

- Bin 0 adds a DC level.
- Bin 1 adds a sinusoid with 1 cycle per frame.
- Bin 2 adds a sinusoid with 2 cycles per frame.
- Bin 100 adds a sinusoid with 100 cycles per frame.

The IFFT simply **adds all these basis sinusoids together** to produce the final time-domain samples.

A very useful way to think about it is:
```
Frequency bins
   *│
    ├─ Bin 1 oscillator
    ├─ Bin 2 oscillator
    ├─ Bin 3 oscillator
    └─ ...
         │
        *▼
       SUM
         │
         ▼*   Time-domain samples
```
The FFT does the opposite: it measures how much of each oscillator is present in a signal.
