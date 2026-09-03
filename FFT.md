# FFT(Fast Fourier Transform)
FFT converts a signal from the time domain (時間領域) to the **frequency domain (周波数領域)**.
- Inputs: x0 to x7 (8 samples), taken at different times, separated by the sampling period(0, 1T, 2T, 3T...)
- Outputs: X0 to X7 (8 frequency bins, 0Hz, 125, 250, 375, 500, -375, -250, -125)
- Processing stages: 3 stages (log₂(8) = 3)
- FFT point count: The number of FFT points must be a power of 2.

## Frequency resolution
```
Frequency Resolution = Fs / N
```

​​- Fs​ = Sampling frequency
- N = FFT point count

Example:

Fs = 1000 Hz 

N = 1024

resolution = 1000 / 1024 = 0.97Hz

| Bin | Frequency |
| --- | --------- |
| X0  | 0 Hz      |
| X1  | 0.97 Hz   |
| X2  | 1.95 Hz   |
| X3  | 2.93 Hz   |
| X.  | ... Hz   |

## Nyquist-Shannon Sampling Theorem
It states: Fs > 2 * Fmax​
- Fs​ = sampling frequency
- Fmax​ = highest frequency component in the signal

To reconstruct or correctly analyze a signal, the sampling frequency must be **greater than twice** the highest frequency contained in the signal.

### What happens if sampling is too slow?
Suppose the actual signal is:
```
Frequency = 700 Hz
Sampling rate = 1000 Hz
```
The Nyquist frequency is: FN ​= 500Hz

Since: 700 > 500

the theorem is violated.

The FFT will show a false frequency called an **alias**.

The apparent frequency is:

Falias =∣Fs​−Fsignal​∣

so: ∣1000−700∣ = 300Hz

The FFT may show a strong peak at 300 Hz instead of 700 Hz.

This phenomenon is called **aliasing**.

### What if Fs​ = 2Fmax
In theory, a frequency equal to Fs​/2 can still be represented, but in practice it is a problematic edge case.
Example:
Suppose:

Fmax​=500 Hz and Fs​=1000 Hz

Then the signal period is: T ​= 2 ms and the sampling period is: Ts ​= 1 ms

You get exactly 2 samples per cycle.

Take a sine wave: x(t) = sin(2π⋅500t)

Sample it at 1000 Hz: x[n] = sin(2π⋅500⋅n/1000​) = sin(πn)

Since sin(πn)=0

for every integer n, the sampled sequence becomes:
```
0, 0, 0, 0, 0, ...
```
The 500 Hz signal has completely disappeared!


Suppose the same 500 Hz signal has a phase shift:

x(t) = cos(2π⋅500t)

Now: x[n] = cos(πn)

which becomes:
```
1, -1, 1, -1, 1, -1, ...
```
Here the 500 Hz signal is visible.

So at the Nyquist frequency, the sampled result **depends heavily on phase**.

This is why engineers prefer Fs > 2Fmax.​

### In practice

Engineers rarely sample at exactly 2Fmax​.

Instead they use a margin:

Fs = 2.5Fmax​ or 5Fmax​ or higher.

Reasons:
- Easier anti-aliasing filter design
- Better waveform representation
- More accurate FFT analysis


## Upper frequency limit
For real-valued input signals, the highest useful frequency is: Fs / 2 (Nyquist frequency)​. 

Example:
Fs = 1000 Hz

Fmax ​= 500Hz


## Why only half the FFT output is used?
For a real signal: x[n]
the FFT output is symmetric:
```
Positive frequencies: 0 Hz ----> Fs/2
Negative frequencies: -Fs/2 ----> 0 Hz
```
Therefore:

- X0 ~ X511 contain unique information
- X512 ~ X1023 are mirror images

So with a 1024-point FFT, we typically analyze only the **first 512 bins**.

