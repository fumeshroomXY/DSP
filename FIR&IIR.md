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
