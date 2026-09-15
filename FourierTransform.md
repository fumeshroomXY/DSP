# Fourier Transform
Fourier Transform is the mathematical tool that connects the **time domain** to the **frequency domain**.

Suppose you have a signal: $x(t)$ and you want to know:

- What frequencies does it contain?
- How much of each frequency is present?
- What phase does each frequency have?

The Fourier Transform answers these questions.

"Take a complicated waveform and express it as a **combination of pure sine waves**."


## Mathematical Definition
Continuous-time Fourier Transform: $X(f)=\int_{-\infty}^{\infty} x(t)e^{-j2\pi ft}dt$

- $x(t)$ = signal in time domain
- $X(f)$ = signal in frequency domain
- $f$ = frequency

Suppose you have a signal: $x(t)$ and you want to know:

- "How much 100 Hz is inside?"

- "How much 200 Hz is inside?"

- "How much 1000 Hz is inside?"

The Fourier Transform checks **one frequency at a time**.

### Step 1: Pick a frequency
Think of $e^{-j2\pi ft}$ as a "frequency detector."

- To detect 100 Hz, use $e^{-j2\pi100t}$

- To detect 200 Hz, use $e^{-j2\pi200t}$

- To detect 1000 Hz, use $e^{-j2\pi1000t}$

The Fourier Transform just repeats this test for **every possible frequency**.

Now let's test 100 Hz so we create a pure frequency: $e^{-j2\pi 100 t}$

### Step 2: Multiply it with the signal

Suppose the signal is exactly 100 Hz: $x(t)=e^{j2\pi 100 t}$ 

Time domain: $x(t)=\cos(2\pi 100t)+j\sin(2\pi 100t)$

Multiply with the detector: $x(t)e^{-j2\pi 100 t}=e^{j2\pi 100 t} \cdot e^{-j2\pi 100 t}$

Using the exponent rule: $e^a e^b = e^{a+b}$

we get $e^{j2\pi 100 t-j2\pi 100 t} = e^0 = 1$

The oscillation disappears completely! The signal contains a **strong** 100 Hz component.

### Step 3: Sum over all time

The integral $X(f) = \int x(t)e^{-j2\pi ft}dt$ answers how well $x(t)$ matches frequency $f$.

#### Why not look at one instant?
Imagine the signal $x(t)=\cos(2\pi 100t)$

At one particular time: $t=0.001$

the value might be $0.81$

**That single value tells you almost nothing about whether the signal contains 100 Hz**.

You must observe the signal **over many cycles**.

The integral is just the **continuous-time version of the sum**.

#### What happens when frequencies match?

Suppose $x(t)=e^{j2\pi100t}$

and we test for 100 Hz: $x(t)e^{-j2\pi100t}=1$

The product is always 1.
```
1 1 1 1 1 1 1 ...
```
Integrating means adding lots of 1's: $1+1+1+1+\cdots$

The result becomes large.

Therefore: $X(100)$ is large.

#### What happens when frequencies don't match?
Suppose we test 200 Hz instead: $e^{j2\pi100t}e^{-j2\pi200t} = e^{-j2\pi100t}$

Now the product oscillates:
```
+1
+0.7
 0
-0.7
-1
-0.7
 0
+0.7
+1
...
```
When you integrate, positives and negatives cancel: $(+1)+(-1)+(+1)+(-1)\approx 0$

The result is small.

Therefore: $X(200)\approx 0$


### Why use the exponential?
Using Euler's formula: $e^{j\theta}=\cos\theta+j\sin\theta$

A complex exponential is just a **rotating point on the unit circle**.

A sinusoid can be written as: $x(t)=e^{j\omega t}$

and this is extremely convenient because: $e^{j\omega(n+1)} = e^{j\omega n}e^{j\omega}$ which makes filter analysis **simple**.

### Why the minus sign in $e^{-j2\pi ft}$

Suppose the signal is exactly 100 Hz: $x(t)=e^{j2\pi 100 t}$

The Fourier transform uses: $e^{-j2\pi 100 t}$

Multiply them: $e^{j2\pi 100 t-j2\pi 100 t} = e^0 = 1$

Now the integral becomes $\int 1dt$ which is large.

This tells us: "Yes, 100 Hz is present."

#### What if we used a plus sign?

Suppose we used $e^{+j2\pi 100 t}$ instead.

Then: $e^{j2\pi 100 t} \cdot e^{j2\pi 100 t} = e^{j2\pi 200 t}$

Now we get a 200 Hz oscillation.

The signal does not become a constant, so the frequency detection is **much less natural**.

#### Another way to see it

You can think of $e^{-j2\pi ft}$ as "rotating backward."

If the signal contains $e^{j2\pi ft}$ which rotates forward at the same speed, then

- forward rotation
- backward rotation

cancel each other.

## $X(f)$

$X(f)$ is usually **complex-valued**, not just a strength.

$X(f)=|X(f)|e^{j\phi(f)}$

It contains:

- Magnitude $|X(f)|$. How strong the frequency is.

- Phase $\phi(f)$. Where that sinusoid is shifted in time.

When people look at a spectrum, they are often looking at: $|X(f)|$ which shows only the strength.


## Connect $x(t)$ with detector
Suppose the signal is $x(t)=\cos(2\pi100t)$

and the "detector" is $e^{-j2\pi100t} = \cos(2\pi100t)-j\sin(2\pi100t)$.

Multiply the signal and detector: $x(t)e^{-j2\pi100t} = \cos(2\pi100t)e^{-j2\pi100t}$

Replace the cosine with exponentials: $\cos(2\pi100t) = \frac{ e^{j2\pi100t} + e^{-j2\pi100t} }{2}$​

Therefore $x(t)e^{-j2\pi100t} = \frac12 \left( e^{j2\pi100t} + e^{-j2\pi100t} \right) e^{-j2\pi100t} = \frac12 \left( 1+e^{-j2\pi200t} \right)$

Notice what happened:

- One term became constant (DC): $\frac12$​
- The other became a 200 Hz oscillation: $\frac12 e^{-j2\pi200t}$

If you now integrate (or average) over a long time: $\int e^{-j2\pi200t}dt \approx 0$

because positive and negative rotations cancel out.

Only the DC term survives: $\int \frac12dt$

This produces a large nonzero result.

This is why the detector says: "Yes, I found a 100 Hz component!"
