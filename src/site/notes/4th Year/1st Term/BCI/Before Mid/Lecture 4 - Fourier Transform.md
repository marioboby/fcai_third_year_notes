---
{"dg-publish":true,"permalink":"/4th-year/1st-term/bci/before-mid/lecture-4-fourier-transform/","dg-note-properties":{}}
---


# The Fourier Transform: From Spinning Vectors to Spectral Decomposition in Brain-Computer Interfacing

> [!Abstract] 
> The **Fourier Transform** is a foundational mathematical paradigm that decomposes any arbitrary, complex time-series signal into a linear combination of elementary sinusoidal waves, each defined by a specific frequency, magnitude, and initial phase. Grounded in **Euler's formula** and complex vector geometry, it operates as an inner-product similarity mechanism—projecting a time-domain signal onto counter-rotating complex exponentials to isolate its constituent spectral frequencies without loss of information. In domains such as **Brain-Computer Interfacing (BCI)** and digital signal processing, this bidirectional mapping between the time and frequency domains enables dimensionality reduction, latent feature extraction, and targeted spectral filtering.

  

## 1. Theoretical Foundations & The Philosophy of Decomposition

Welcome to one of the most elegant ideas in all of mathematics and engineering. When we record a biological or physical signal—such as an electroencephalogram (EEG) in **Brain-Computer Interfacing (BCI)**—we are confronted with a highly intricate waveform evolving over time. Looking at raw voltage fluctuations over time often obscures the underlying physiological or physical generators. The core philosophy of the **Fourier Transform** is to transform an intractable, composite object into simple, well-understood building blocks paired with a single reconstruction rule.

  

### 1.1 Historical Genesis & The Superposition Principle

- **Fourier's Thermal Conduction Problem**: In 1807, the French mathematician **Joseph Fourier** (1768–1830) investigated how heat diffuses through a solid metal bar.
    
      
    - To solve the heat equation, he demonstrated a radical proposition: any arbitrary temperature profile—and by extension, any complicated signal—can be expressed as a sum of simple sine and cosine waves that add up to the original waveform.
        
          
        
    - **Decomposition and Reconstruction**: Rather than slicing a signal into short segments of time, Fourier's method pulls the signal apart into continuous **frequencies** (such as $0\text{ Hz}$, $1\text{ Hz}$, $2\text{ Hz}$, and $3\text{ Hz}$ components), each spanning the entire duration of the observation window. By adding these constituent waves back together one by one at every time point, the exact original waveform is recovered.
        
          
        
    - **Analogy:** If thinking about the number $9$ as a monolithic entity is difficult, we can decompose it into simpler additive parts, such as $9 = 6 + 3$. Just as **Singular Value Decomposition (SVD)** factors a complex matrix into a sum of smaller, simpler rank-one matrices, the **Fourier Transform** breaks a complicated time-series signal into elementary sinusoidal waves (the "easy pieces") governed by linear addition (the "one rule that puts them back together").
        
          
        

### 1.2 Why Leave the Time Domain?

- **Time Domain vs. Frequency Domain**: The **time domain** plots signal **amplitude** (vertical axis) against **time** in seconds (horizontal axis), showing how a system's state fluctuates moment by moment. Conversely, the **frequency domain** plots **magnitude** or **power** (vertical axis) against **frequency** in **Hertz ($\text{Hz}$)**—defined as cycles per second—revealing the rate of oscillation.
    
      
    - **Dimensionality Compression**: A long, densely sampled time-domain signal containing hundreds or thousands of time points can often be summarized by just a few numbers (its dominant frequency spikes) in the frequency domain.
        
          
        
    - **Unmasking Latent Features**: The time domain routinely hides overlapping processes that become immediately separable in the frequency domain, such as isolating two simultaneous human speakers in an audio recording or distinguishing between two distinct **EEG states** in a brain-computer interface.
        
          
        
    - **Spectral Filtering**: By transforming a signal into the frequency domain, we can selectively retain desired waves, discard unwanted noise or artifacts, and invert the transform back to the time domain to produce a clean, filtered signal.
        
          
        
    - **Case Study:** Consider a $1\text{-second}$ recording of an oscillating wave that swings between an amplitude of $-4$ and $+4$ while completing $2$ full cycles. In the time domain, you must track a continuously curving trajectory across the entire $1\text{-second}$ interval. In the frequency domain, this entire waveform collapses into a single, unmistakable spike of **magnitude** $4.0$ (or **power** $= \text{magnitude}^2 = 16$) located precisely at $f = 2.0\text{ Hz}$. If you change the oscillation rate $f$ or the wave's amplitude, that single spectral spike simply shifts horizontally or scales vertically to follow it.
        
          
        

## 2. Complex Geometry: Euler's Formula, Conjugates, and Helices

To understand _how_ the Fourier Transform actually works under the hood, we must move beyond flat 2D sine waves on paper and embrace the geometry of the complex plane. In Fourier analysis, waves are not merely up-and-down squiggles; they are the shadows cast by spinning vectors.

  

### 2.1 Euler's Formula and Rotating Phasors

- **Euler's Formula**: The bridge between trigonometry and complex exponentiation is governed by **Euler's identity**, which states that for any angle $\theta$ and magnitude $m$:
    
      
    
    $$m e^{i\theta} = m(\cos\theta + i\sin\theta)$$
    
    where $i = \sqrt{-1}$ defines the imaginary axis.
    
      
    - **Geometric Interpretation**: In the complex plane—where the horizontal axis represents the **real part ($\text{Re} \cdot \cos$)** and the vertical axis represents the **imaginary part ($\text{Im} \cdot \sin$)**—the quantity $e^{i\theta}$ represents a unit vector rotated by angle $\theta$.
        
          
        
    - **Orthogonal Shadows (Projections)**: The real component $\cos\theta$ is the vector's horizontal shadow on the real axis, while the imaginary component $\sin\theta$ is its vertical shadow on the imaginary axis.
        
          
        
    - **Pythagorean Magnitude**: If you observe a stationary complex vector in the plane with horizontal projection $2$ and vertical projection $4$, its **magnitude** (the radius of its circle, or the hypotenuse of the right triangle) is given by the Euclidean norm: $\text{magnitude} = \sqrt{2^2 + 4^2} \approx 4.47$.
        
          
        
    - **Time Evolution and Helices**: When we let the angle grow linearly with time according to $\theta = 2\pi f t$, the vector rotates **counter-clockwise** at a speed of $f$ turns per second. In a 3D space formed by the real axis, imaginary axis, and **time progression**, this spinning vector traces out a three-dimensional corkscrew or **helix**.
        
          
        
    - **Example:** At a static angle of $\theta = 35^\circ$, the vector $e^{i\theta}$ sits in the first quadrant of the complex plane. If we instead drive a vector of length $3$ at frequency $f = 2\text{ Hz}$ over time—written in 3D as $3\cos(2\pi \cdot 2t) + i\,3\sin(2\pi \cdot 2t)$—its real shadow traces a cosine wave $3\cos(2\pi \cdot 2t)$ while its imaginary shadow traces a sine wave $3\sin(2\pi \cdot 2t)$.
        
          
        

### 2.2 Complex Conjugates and the Physical Meaning of Negative Frequencies

Students often ask: _"Professor, if physical signals like EEG voltages or sound waves are strictly real numbers, why do we build them out of imaginary numbers?"_ The answer lies in the symmetry of the **complex conjugate**.

  

- **The Complex Conjugate**: For a complex vector $m(\cos\theta + i\sin\theta)$, its **complex conjugate** is formed by flipping the sign of the imaginary part only:
    
      
    
    $$m e^{-i\theta} = m\cos(-\theta) + i\,m\sin(-\theta) = m(\cos\theta - i\sin\theta)$$
    
    because cosine is an even function ($\cos(-\theta) = \cos\theta$, casting the exact same shadow on the real axis) and sine is an odd function ($\sin(-\theta) = -\sin\theta$, flipping the vertical shadow across the real axis).
    
      
    - **Negative Frequency**: Geometrically, $e^{-i\theta}$ represents a vector at angle $-\theta$ that rotates in the opposite direction—**clockwise**—as time progresses. In the Fourier domain, a "negative frequency" is simply a vector spinning clockwise rather than counter-clockwise.
        
          
        
    - **Synthesizing Real Waves from Complex Pairs**: When you add a counter-clockwise complex wave $e^{i\theta}$ and its clockwise conjugate twin $e^{-i\theta}$, their vertical imaginary parts ($+i\sin\theta$ and $-i\sin\theta$) perfectly cancel out at every instant in time, leaving a purely real value along the horizontal axis:
        
          
        
        $$e^{i\theta} + e^{-i\theta} = 2\cos\theta \implies \cos\theta = \frac{1}{2}\left(e^{i\theta} + e^{-i\theta}\right)$$
        
        Thus, two complex helices spinning in opposite directions combine to form a single real-valued wave whose average lies strictly on the real axis.
        
          
        
    - **Example:** At $\theta = 35^\circ$, adding $e^{i 35^\circ}$ and $e^{-i 35^\circ}$ cancels their opposite vertical projections and yields a purely real vector of length $2\cos(35^\circ) = 1.64$ along the horizontal real axis. Consequently, any real-valued cosine wave of amplitude $A$ at frequency $f$ splits its amplitude equally between two conjugate twins: half ($A/2$) at the positive frequency $+f$ and half ($A/2$) at the negative frequency $-f$.
        
          
        

### 2.3 The Phase Trap & Initial Configuration

- **The Sine-Wave Trap**: A naive intuition suggests we can build any signal solely out of standard sine waves ($\sin(2\pi f t)$). However, every pure sine wave starts at $0$ at time $t = 0$.
    
      
    - If all our constituent vectors started at angle $0$ (or if all waves started at amplitude $0$), their sum at the very first time point $t = 0$ would always be forced to equal $0$—yet real-world signals can start at any arbitrary value!
        
          
        
    - **Initial Configuration**: To escape this trap, every frequency component must be endowed with a two-part **initial configuration** at $t = 0$:
        
          
        1. **Magnitude ($\vert{}c\vert{}$)**: The size (radius) of the circle traced by the spinning vector, which dictates "how much" of that frequency is present.
            
              
            
        2. **Initial Phase ($\angle c$)**: The starting angle of the vector at $t = 0$, which dictates "where" in its cycle each wave begins.
            
              
            
    - **Analogy:** Think of the Fourier synthesis of a signal like a synchronized clockwork mechanism made of multiple spinning arms linked tip-to-tail. Before you release the gears to spin at their respective speeds ($0\text{ Hz}, 1\text{ Hz}, 2\text{ Hz}, 3\text{ Hz}$), you must set both the _length_ of each arm (**magnitude**) and the _starting clock-hand angle_ of each arm (**initial phase**) so that their combined tip starts at the exact initial height of your signal at $t=0$.
        
          
        

## 3. Mathematical Mechanics of the Discrete Fourier Transform (DFT)

Now we arrive at the central mathematical question of the lecture: Given a raw time-series signal $x(t)$, _how do we mathematically isolate each vector's initial configuration (magnitude and phase)?_

  

### 3.1 The "Stop the Vector" Mechanism (Forward Transform)

- **The Unwinding Trick**: Suppose at a specific frequency $f$, a signal component is a spinning vector represented by $c \cdot e^{i 2\pi f t}$, where $c$ is the unknown complex **Fourier coefficient** encoding the vector's initial configuration.
    
      
    - Because the vector is spinning counter-clockwise via $e^{+i 2\pi f t}$, we can freeze it in place by multiplying it by the **negative rotation** (a clockwise-spinning unit vector at the exact same test frequency $f$):
        
          
        
        $$c \cdot e^{i 2\pi f t} \cdot e^{-i 2\pi f t} = c$$
        
    - The positive and negative rotations cancel each other out, stopping the vector dead in its tracks at its initial position $c$!
        
          
        
- **The Discrete Fourier Transform (DFT) Analysis Equation**: Across a discrete whole signal $x_n$ consisting of $N$ samples (where normalized time is $t_n = n/N$, running from $0$ to almost $1$, and discrete frequency index is $k = 0, 1, \dots, N-1$), we compute the $k$-th **Fourier coefficient** $c_k$ as:
    
      
    
    $$c_k = \frac{1}{N} \sum_{n=0}^{N-1} x_n e^{-2\pi i k n / N}$$
    
    - **Dot Product as Similarity**: Multiplying every signal sample $x_n$ by the complex sine wave sample $e^{-2\pi i k n / N}$ and summing them together ($\text{row} \cdot \text{column}$) is a **dot product**—a mathematical measure of similarity between the signal and that test wave.
        
          
        
    - **Why Divide by $N$? (Normalization)**: Whether we record a $2\text{ Hz}$ wave for $2\text{ seconds}$ or $4\text{ seconds}$, the physical strength of the $2\text{ Hz}$ component has not changed. Dividing the sum by the number of samples $N$ computes the **average** (center of mass), ensuring that the Fourier coefficient $c_k$ does not artificially grow with signal duration.
        
          
        
    - **Resonance vs. Cancellation in the Complex Plane**:
        
          
        - **When Test Frequency $f$ IS in the Signal**: The stopped sample points all lean heavily to one side of the complex plane, so their average produces a non-zero complex vector $c_k$ pointing to the initial configuration.
            
              
            
        - **When Test Frequency $f$ IS NOT in the Signal**: The sample products wrap symmetrically around the origin in closed loops and cancel each other out, averaging to $0$.
            
              
            
    - **Extracting the Spectra**: Because each coefficient $c_k$ is a complex number ($c_k = \text{real} + i\,\text{imag}$), we extract two distinct spectra across all frequencies $c_0, c_1, c_2, \dots$:
        
          
        1. **Magnitude Spectrum (The Spectrum)**: Plotting $\vert{}c_k\vert{} = \sqrt{\text{real}^2 + \text{imag}^2}$ (computed via `abs(c)`) against frequency in $\text{Hz}$.
            
              
            
        2. **Phase Spectrum**: Plotting $\angle c_k = \text{angle}(c_k)$ against frequency in $\text{Hz}$.
            
              
            
    - **Example:** Suppose we test a composite signal at $f = 1.00\text{ Hz}$ and obtain the complex Fourier coefficient $c_1 = 0.50 + 1.00i$. Its magnitude is $\vert{}c_1\vert{} = \sqrt{0.5^2 + 1^2} = 1.12$, and its initial phase is $\angle c_1 = 63^\circ$. Because its conjugate twin at $-1\text{ Hz}$ carries the other half of the wave's energy, the true time-domain amplitude of the $1\text{ Hz}$ wave is $2\vert{}c_1\vert{} = 2.24$. Similarly, testing the same signal at $f = 2.00\text{ Hz}$ yields $c_2 = 2.00 + 0.00i$ ($\vert{}c_2\vert{} = 2.00$, $\text{phase} = 0^\circ$), which corresponds to a $4\cos(2\pi \cdot 2t)$ wave split between $+2\text{ Hz}$ and $-2\text{ Hz}$, whereas testing at $f = 3\text{ Hz}$ (which is absent from the signal) causes the points to loop around the origin and cancel to $0$.
        
          
        

### 3.2 Step-by-Step Hand Calculation ($N = 4$)

To strip away any mystique from the summation formula, let us trace a complete discrete calculation by hand for a $4$-sample cosine wave:

  

- **Setup**: Let the discrete signal be a single cosine turn sampled at $N = 4$ points:
    
      
    
    $$x = [1, 0, -1, 0]$$
    
    with sample indices $n = [0, 1, 2, 3]$.
    
      
    
- **Evaluating Frequency Bin $k = 1$**:
    
      
    - First, evaluate the unwinding complex exponential $e^{-2\pi i (1) n / 4}$ at each sample $n \in \{0, 1, 2, 3\}$:
        
          
        
        $$e^{-2\pi i n / 4} = [1, -i, -1, i]$$
        
    - Next, multiply each signal sample $x_n$ by its corresponding unwinding vector and divide the sum by $N = 4$:
        
          
        
        $$c_1 = \frac{1\cdot(1) + 0\cdot(-i) + (-1)\cdot(-1) + 0\cdot(i)}{4} = \frac{1 + 0 + 1 + 0}{4} = \frac{2}{4} = \frac{1}{2}$$
        
    - **Geometric Result**: When placed head-to-tail in the complex plane, the non-zero products line up along the positive real axis, yielding $c_1 = 0.50$—the wave is found!
        
          
        
- **Evaluating the Remaining Bins ($k = 0, 2, 3$)**:
    
      
    - For $k = 0$ ($0\text{ Hz}$ DC component) and $k = 2$ (the Nyquist bin), the sample products cancel out completely: $c_0 = 0$ and $c_2 = 0$.
        
          
        
    - For $k = 3$ (the negative-frequency mirror of $k = 1$), the products line up identically to yield $c_3 = 1/2 = 0.50$.
        
          
        
    - **One-Sided Amplitude Recovery**: Doubling the non-mirror bin ($2 \times 1/2 = 1$) recovers the exact peak amplitude ($1$) of our original cosine wave $x = [1, 0, -1, 0]$.
        
          
        

### 3.3 The Inverse Discrete Fourier Transform (IDFT) & Spectral Filtering

- **The Synthesis Equation (Inverse Transform)**: Once we have the $N$ complex coefficients $c_k$, how do we return from the frequency domain to the time domain? We simply reverse the operation:
    
      
    
    $$x_n = \sum_{k=0}^{N-1} c_k e^{+2\pi i k n / N}$$
    
    - **Forward vs. Inverse Symmetry**: Whereas the forward transform uses a **minus sign ($-$)** in the exponent to _stop_ the vectors and divides by $N$ to take an average, the inverse transform uses a **plus sign ($+$)** to _spin the vectors forward again_ from their initial configurations ($c_k$) and simply adds them up at every time point $n$ without dividing by $N$.
        
          
        
    - **Lossless Bijection**: The mapping $\text{signal }(N,) \iff \text{coeff }(N,)$ is a direct, one-to-one bijection where nothing is lost. In numerical practice (using double-precision floating-point arithmetic), reconstructing a signal like $4\cos(2\pi \cdot 2t)$ via the inverse transform reproduces the original time series to within machine precision (a largest error on the order of $3 \times 10^{-13}$).
        
          
        
    - **3D Complex Reconstruction**: When calling `ifft` on a complex helical signal such as $3\cos(2\pi \cdot 2t) + i\,3\sin(2\pi \cdot 2t)$ (which has a single spike $c \approx 3$ at $2\text{ Hz}$), both orthogonal parts come back simultaneously: the real part reconstructs the cosine wave, and the imaginary part reconstructs the sine wave.
        
          
        
- **Spectral Filtering Mechanism**:
    
      
    - If you intentionally modify or zero out specific coefficients in the spectrum before running the inverse transform, the reconstructed time series becomes a modified version of the original—this is the essence of **filtering**.
        
          
        
    - **Low-Pass Filtering**: Keep only the slow-spinning vectors (low frequencies) and zero out the fast ones to smooth the signal.
        
          
        
    - **Band-Pass Filtering**: Keep only a specific frequency band of vectors (e.g., isolating an EEG rhythm) and zero out everything outside that band.
        
          
        
    - **Example:** Suppose a composite waveform is built from $4$ non-zero rotating vectors ($0, 1, 2, 3\text{ Hz}$). If we alter the spectrum to keep only $2$ slow coefficients and set the higher-frequency coefficients to zero, spinning the remaining vectors via the inverse transform constructs a smoothed, low-pass filtered version of the original signal.
        
          
        

## 4. Sampling Theory, The Nyquist Limit, and Aliasing

When we work with digital signals on a computer, we do not have access to continuous time; we only observe discrete snapshots taken at a fixed **sampling rate ($f_s$)**, measured in samples per second. This discretization imposes strict physical boundaries on what frequencies we can observe.

  

### 4.1 The Extremes of Observation: $0\text{ Hz}$ to the Nyquist Limit

- **The Slowest Extreme ($0\text{ Hz}$ / DC Offset)**: A $0\text{ Hz}$ wave is a completely flat line where nothing changes over time. Technically, just a single sample is enough to characterize a flat signal.
    
      
    
- **The Fastest Extreme (The Nyquist Frequency)**: Any true oscillation requires **at least 2 samples per cycle**—one sample to capture the peak and one sample to capture the trough.
    
      
    - **The Nyquist-Shannon Criterion**: Because **sampling rate ($f_s$)** is measured in samples per second and **frequency ($f$)** is measured in cycles per second, to unambiguously observe a frequency $f$, the sampling rate must satisfy $f_s > 2f$ (or equivalently, the maximum observable frequency is $\text{Nyquist} = f_s / 2$).
        
          
        
    - **The Phase-Zero Vanishing Edge Case**: At the exact boundary $f_s = 2f$, a cosine wave sampled at its peaks and troughs is visible, _but a sine wave sampled precisely at its zero-crossings vanishes completely into a flat line of zeros_!
        
          
        
    - **Example:** If we record $0.1\text{ s}$ of a signal at a sampling rate of $f_s = 100\text{ Hz}$, the slowest frequency we can see is $0\text{ Hz}$ (flat), and the fastest frequency we can faithfully resolve is the **Nyquist limit**: $f_s / 2 = 50\text{ Hz}$. Both a slow $2\text{ Hz}$ wave and a boundary $50\text{ Hz}$ cosine wave are accurately reflected by their samples as $2\text{ Hz}$ and $50\text{ Hz}$, respectively.
        
          
        

### 4.2 Aliasing and Spectral Mirroring

What happens if the physical signal oscillates _faster_ than the Nyquist limit ($f > f_s / 2$)? The discrete samples can no longer keep up with every peak and trough, triggering a deceptive phenomenon known as **aliasing**.

  

- **The Aliasing Mechanism**: When a signal's frequency exceeds $f_s / 2$, the discrete sampling grid skips oscillating points in the middle of each cycle. As a result, the captured sample points fall on the exact trajectory of a slower wave, masquerading as a false lower frequency given by $f_{\text{alias}} = f_s - f$.
    
      
    
- **Spectral Symmetry Around Nyquist**: Because of conjugate symmetry in the Discrete Fourier Transform of a real signal, the computed spectrum from $0$ to $f_s$ mirrors itself around the Nyquist frequency $f_s / 2$:
    
      
    - **$0\text{ to } f_s/2$ ($0\text{ to } 50\text{ Hz}$)**: Represents the true **positive frequencies**.
        
          
        
    - **$f_s/2\text{ to } f_s$ ($50\text{ to } 100\text{ Hz}$)**: Represents the **negative frequencies (the mirror image)**.
        
          
        
    - **One-Sided Spectrum Scaling Rule**: When plotting a standard one-sided amplitude spectrum from $0\text{ Hz}$ to $\text{Nyquist}$ ($N//2 + 1$ points), we must multiply the magnitudes of all interior bins by $2$ to account for their negative-frequency mirror twins—**except** for the $0\text{ Hz}$ bin and the Nyquist bin ($f_s / 2$), which have no separate mirror twin and must never be doubled.
        
          
        
    - **Case Study:** Suppose our ADC (Analog-to-Digital Converter) samples at $f_s = 100\text{ Hz}$ ($\text{Nyquist} = 50\text{ Hz}$):
        
          
        - If a **$51\text{ Hz}$** wave enters the sensor, its discrete samples are identical to those of a **$49\text{ Hz}$** wave ($100 - 51 = 49$), so the spectral spike lands falsely at $49\text{ Hz}$.
            
              
            
        - If a **$60\text{ Hz}$** wave (such as power-line interference) enters the sensor, it is seen as a **$40\text{ Hz}$** wave ($100 - 60 = 40$).
            
              
            
        - If a rapid **$98\text{ Hz}$** wave enters the sensor, the samples alias all the way down to masquerade as a slow **$2\text{ Hz}$** wave ($100 - 98 = 2$)!
            
              
            

## 5. Computational Implementation: Fourier From Scratch

Let us bridge our theoretical equations with practical scientific computing in **MATLAB** and **Python (NumPy)**, examining how the forward transform, one-sided spectral scaling, and inverse reconstruction are implemented line by line.

  

### 5.1 Vectorized & Loop-Based Implementation (Python / NumPy)

Consider a $2\text{-second}$ recording of a pure cosine wave $x(t) = 4\cos(2\pi \cdot 2t)$ sampled at $f_s = 100\text{ Hz}$:

  

Python

```
import numpy as np

# 1. Signal Generation (Time Domain)
fs = 100                                      # Sampling rate: 100 samples/sec
t = np.arange(0, 2, 1/fs)                     # Time vector (200,): 0 to 1.99 s
signal = 4 * np.cos(2 * np.pi * 2 * t)        # 200 samples of 4*cos(2*pi*2*t)

# 2. DFT Setup (Normalized Time & Frequency Bins)
N = len(signal)                               # N = 200 samples
complex_sine_wave_t = np.arange(N) / N        # Normalized time (200,): 0 ... 0.995
fr = np.arange(N)                             # Frequency bin indices (200,): 0 ... 199

# 3. Forward Fourier Transform (Time -> Frequency)
coeff = np.zeros(N, dtype=complex)
for i in range(N):
    complex_sine_wave = np.exp(-2j * np.pi * fr[i] * complex_sine_wave_t)
    coeff[i] = complex_sine_wave @ signal     # Dot product per frequency

coeff = coeff / N                             # Normalize by N (average)
m = np.abs(coeff)                             # Magnitude spectrum (200,)
phase = np.angle(coeff)                       # Phase spectrum (200,)

# 4. One-Sided Spectrum Construction (0 Hz to Nyquist)
nyquist = fs / 2                              # Nyquist = 50 Hz
pts_hz = np.linspace(0, nyquist, N // 2 + 1)  # 101 points: 0, 0.5, ..., 50 Hz
m[1:N//2] = m[1:N//2] * 2                     # Double interior bins (exclude 0 Hz & 50 Hz)

# 5. Inverse Fourier Transform (Frequency -> Time)
reconstructed = np.zeros(N, dtype=complex)
for i in range(N):
    reconstructed += coeff[i] * np.exp(2j * np.pi * fr[i] * complex_sine_wave_t)
```

  

- **Unpacking the Operational Workflow**:
    
      
    - **Frequency Resolution ($\Delta f$)**: Because the signal is $2\text{ seconds}$ long ($N = 200$ at $f_s = 100\text{ Hz}$), the $101$ frequency points spanning $0\text{ to } 50\text{ Hz}$ (`pts_hz`) are spaced by $0.5\text{ Hz}$ ($0, 0.5, 1.0, 1.5, 2.0, \dots, 50\text{ Hz}$).
        
          
        
    - **Why Index 4?**: Our $2\text{ Hz}$ signal completes $4$ full cycles over the $2\text{-second}$ window, so its positive frequency spike lands at array index $k = 4$ ($4 \times 0.5\text{ Hz} = 2.0\text{ Hz}$).
        
          
        
    - **Numerical Verification**: Out of the $200$ complex coefficients in `coeff`, $198$ evaluate to $0$, while two land on the real axis (`coeff[4] = 400 / 200 = 2` with `angle = 0` because a cosine starts at its peak, and its mirror twin at `coeff[196] = 2`). After doubling the interior bins (`m[1:N//2] *= 2`), the one-sided spectrum displays a single spike of magnitude $4$ at $2\text{ Hz}$.
        
          
        

### 5.2 Equivalent MATLAB Formulation (`wave_fourier.m`)

- In MATLAB, the exact same forward-and-inverse loop architecture is expressed concisely using matrix-vector multiplication:
    
      
    
    - **Forward Loop ($\text{time} \to \text{frequency}$)**:
        
          
        
        Matlab
        
        ```
        complex_sine_wave = exp(-2*pi*1i*fr(i)*complex_sine_wave_t);
        coeff(i) = complex_sine_wave * signal';
        coeff = coeff ./ N;
        ```
        
    
      
    
    - **Inverse Loop ($\text{frequency} \to \text{time}$)**:
        
          
        
        Matlab
        
        ```
        reconstructed = reconstructed + coeff(i) * exp(2*pi*1i*fr(i)*complex_sine_wave_t);
        ```
        
    
      
    - **Example:** Comparing the MATLAB and Python implementations side-by-side demonstrates that `complex_sine_wave * signal'` in MATLAB and `complex_sine_wave @ signal` in NumPy perform the exact same inner-product projection, mapping $N$ real time samples to $N$ complex frequency coefficients and back with zero information loss.
        
          
        

## Professor's Synthesis

If there is one overarching takeaway I want you to carry from this lecture into your research in **Brain-Computer Interfacing** and signal processing, it is this: **The Fourier Transform is not a black-box formula—it is a geometric coordinate transformation.**

  

When you look at a raw time-domain recording, you are watching the tangled, composite shadow of dozens of spinning complex vectors all added together at each instant in time. By multiplying that signal by a counter-rotating vector $e^{-i 2\pi f t}$ and averaging over time, you are mathematically "stepping onto the merry-go-round" at frequency $f$: any component spinning at that exact speed appears stationary to you and accumulates into a non-zero complex number $c_f$ (encoding its amplitude and starting phase), while every other frequency spins around you in a blur and averages out to zero.

  

In practical engineering and neuroscience, this geometric insight carries three critical implications:

  

1. **Feature Extraction & Compression**: Complex physiological states—such as distinguishing mental imagery states in an EEG-based BCI or separating overlapping acoustic sources—often look like chaotic noise in the time domain but separate cleanly into distinct, quantifiable spectral peaks in the frequency domain.
    
      
    
2. **Zero-Phase & Targeted Filtering**: Because the forward and inverse transforms form a lossless bijection ($\text{time} \iff \text{frequency}$), you have surgical control over a signal's spectral content. By attenuating specific frequency bins and spinning the remaining vectors forward via the inverse transform, you can strip out artifacts or isolate narrowband rhythms effortlessly.
    
      
    
3. **Hardware & Experimental Design Discipline**: No amount of post-processing math can rescue a signal that was improperly sampled. Because any frequency above the **Nyquist limit ($f_s / 2$)** folds back into the lower spectrum via **aliasing** ($f_{\text{alias}} = f_s - f$), you must always ensure your sampling rate is strictly greater than twice the highest physical frequency present in your environment ($f_s > 2f_{\max}$). Master the geometry of these spinning vectors, respect the Nyquist boundary, and the frequency domain will become your most powerful analytical lens.