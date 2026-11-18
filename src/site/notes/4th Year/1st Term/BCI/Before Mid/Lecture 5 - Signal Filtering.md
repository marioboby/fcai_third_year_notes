---
{"dg-publish":true,"permalink":"/4th-year/1st-term/bci/before-mid/lecture-5-signal-filtering/","dg-note-properties":{}}
---

# EEG Signal Processing & Decoding in Brain-Computer Interfaces: Filtering, ERPs, and Time-Frequency Analysis

> [!Abstract]
> Welcome to this masterclass on decoding the human brain! At its core, **Electroencephalography (EEG)**—the recording of electrical activity along the scalp—is a messy symphony of slow and fast waves all nested together. The fundamental thesis of neural signal processing is that meaningful cognitive and motor intentions are hidden within specific temporal windows, frequency bands, or phase-locked waveforms; by systematically applying digital filters, trial averaging, and time-frequency decompositions, we can strip away background noise and translate raw brainwaves into actionable commands for a **Brain-Computer Interface (BCI)**.

  

## 1. Spectral Filtering & Filter Design

When we look at a raw EEG trace in the **time domain** (how voltage changes moment by moment), we are seeing a composite of countless brain processes, muscle twitches, and environmental electrical noise all happening at once. To make sense of it, we transform the signal into the **frequency domain** to inspect its **spectrum**—a breakdown showing how much energy (or power) lives at each speed of oscillation, measured in **Hertz (Hz)**, or cycles per second.

  

### The Four Core Filter Archetypes

Every digital filter examines the frequency spectrum of a signal and applies a specific **gain**—a multiplier that keeps desired frequencies (gain $\approx 1$) and **attenuates** (shrinks or silences, gain $\approx 0$) the rest:

  

- **Low-Pass Filter:** Keeps the slow, low frequencies below a chosen cutoff point and attenuates all faster waves above it, resulting in a smooth, slow-rolling wave in the time domain.
    
      
    - _Example:_ If we apply a low-pass filter around $5\text{ Hz}$ to a jagged EEG recording from electrode C3, the rapid spikes vanish, leaving only the slow underlying drift of the brainwave.
        
          
        
- **High-Pass Filter:** Keeps the fast, high frequencies above a cutoff point and eliminates the slow, underlying drifts.
    
      
    - _Example:_ Applying a high-pass filter above $15\text{ Hz}$ strips away the big, slow waves and isolates the rapid, low-amplitude ripples riding on top of them.
        
          
        
- **Band-Pass Filter:** Keeps only a specific "window" or band of frequencies while silencing everything slower and faster than that range.
    
      
    - _Example:_ Isolating the $8\text{--}12\text{ Hz}$ range gives us a clean, rhythmic oscillation that waxes and wanes in amplitude over time.
        
          
        
- **Band-Stop (Notch) Filter:** The exact opposite of a band-pass filter; it carves out and removes one specific frequency range while leaving everything else untouched.
    
      
    - _Analogy:_ Think of a band-stop filter like a pair of acoustic earplugs designed specifically to block the hum of an airplane engine ($50\text{ Hz}$ or $60\text{ Hz}$ power-line noise in EEG labs) while still letting you hear both the deep bass of announcements and the high pitch of a conversation.
        
          
        

### How Filtering Actually Works: From Frequency to Convolution

How do we actually apply a filter to our data? We can work in either the frequency domain or the time domain, and **The Convolution Theorem:** multiplying two spectra together once in the frequency domain produces the exact same mathematical result as sliding a filter kernel across the signal in the time domain:

  

- **Step 1: Design in the Frequency Domain:** We first sketch the ideal shape of our filter's gain—for instance, a bell-shaped curve centered at $10\text{ Hz}$ that stays high between $8\text{--}12\text{ Hz}$ and drops to zero everywhere else.
    
      
    
- **Step 2: Bring the Filter to the Time Domain:** Using an inverse Fourier transform, we convert that frequency shape into a short time-domain wave called a **kernel** (or impulse response).
    
      
    - A filter of **order** $N$ produces a kernel with $N + 1$ sample points (e.g., an order of $120$ gives a kernel that is $121$ points long, looking like a little $10\text{ Hz}$ wave that fades out at both ends).
        
          
        
- **Step 3: Time-Domain Convolution (The Sliding Dot Product):** We place our $121$-point kernel over the first $121$ points of our EEG recording, multiply the overlapping points element-by-element, and sum all those products together into a single output number.
    
      
    - We then shift the kernel forward by one time point and repeat the process across the entire recording.
        
          
        
    - This operation is a sliding **dot product**, which mathematically measures **similarity**: wherever the raw EEG contains a strong $8\text{--}12\text{ Hz}$ rhythm that matches the shape of our kernel, the products add up to a large number; where the EEG doesn't match, they cancel out to near zero.
        
          
        
    - _Analogy:_ This is the exact same mechanism used by a **Convolutional Neural Network (CNN)** in computer vision, where a small feature detector (kernel) slides pixel-by-pixel over an image to highlight edges or textures.
        
          
        

### Engineering Trade-Offs: Ideal vs. Real Filters

Why don't we just build a "brick-wall" filter that jumps instantaneously from $0$ to $1$ at $8\text{ Hz}$ and drops instantaneously back to $0$ at $12\text{ Hz}$?

  

- **The Danger of the Ideal Filter:** An **ideal filter** shifts from $0 \to 1$ in a single vertical step in the frequency domain.
    
      
    - **Critical Warning — Ringing Artifacts:** Sharp, sudden edges in the frequency domain create artificial oscillations—called **ringing**—in the time domain. If you filter a clean $0.5\text{ s}$ burst of brain activity with an ideal brick-wall filter, fake ripples appear _before_ the burst even starts and continue ringing _after_ it ends!
        
          
        
- **Real Filters & The Transition Zone:** To prevent ringing, every practical filter uses a smooth **transition zone**—a gradual slope between the frequencies that are fully attenuated ($0$) and those that are fully kept ($1$).
    
      
    
- **Filter Order & Sharpness:** The steepness of this transition slope is controlled by the **filter order**.
    
      
    - A low order (e.g., order $3$) has a wide, gentle transition zone that lets in some neighboring frequencies outside $8\text{--}12\text{ Hz}$.
        
          
        
    - A higher order (e.g., order $10$) creates a much sharper, steeper transition zone that hugs the $8\text{--}12\text{ Hz}$ box tightly while still keeping time-domain ringing to a minimum.
        
          
        
    - _Analogy:_ Stopping a car instantaneously from $100\text{ km/h}$ to $0$ (an ideal filter) causes a violent whiplash crash (ringing artifacts); applying the brakes smoothly over a short braking distance (a transition zone) brings you to a clean stop without distortion.
        
          
        

### FIR vs. IIR Filters: Offline Precision vs. Online Speed

Depending on whether we are analyzing a pre-recorded dataset in a lab or running a live BCI wheelchair in real time, we choose between two filter families:

  

- **FIR (Finite Impulse Response):**
    
      
    - Uses a relatively long kernel (e.g., $121$ sample points) that slides over the signal using only the input samples.
        
          
        
    - **Pros & Cons:** Highly accurate and stable (no phase distortion when applied forward and backward), but computationally slower and requires a buffer of data.
        
          
        
    - **Best Use Case:** **Offline analysis**, where the entire EEG recording has already been collected and stored on disk.
        
          
        
- **IIR (Infinite Impulse Response):**
    
      
    - Uses very few input points (e.g., just $5$ recent input samples) because it is recursive—it feeds back and reuses its own previous output values (e.g., $4$ of its own past outputs) to calculate the current point ("now").
        
          
        
    - **Pros & Cons:** Extremely fast and requires almost no memory or delay, though slightly less accurate and prone to subtle phase shifts.
        
          
        
    - **Best Use Case:** **Online (real-time) analysis**, where we must filter the data on the fly at the exact millisecond new samples arrive from the headset.
        
          
        
    - _Case Study:_ In a clinical neuro-rehabilitation trial, you would use an **IIR filter** during the live session so the robotic exoskeleton moves the patient's arm immediately with zero lag, and later use an **FIR filter** offline when writing your research paper to get the cleanest possible signal views.
        
          
        

## 2. Motor Imagery & Oscillatory Power Decoding

Can we tell purely from an EEG recording which hand a person is _imagining_ moving, without them twitching a single muscle? This paradigm is known as **Motor Imagery (MI)**, and it is one of the foundational pillars of Brain-Computer Interfaces.

  

### Neuroscience of the Mu Rhythm

To understand how the brain imagines movement, we look at the **motor cortex**—the strip of brain tissue running from ear to ear across the top of the head that controls voluntary movement:

  

- **Electrode Placement & Contralateral Organization:** In the standard 10-20 EEG system, electrode **C3** sits over the left hemisphere's motor cortex, **Cz** sits dead-center at the vertex, and **C4** sits over the right hemisphere's motor cortex.
    
      
    - Because the human nervous system is wired **contralaterally** (crossed), the left motor cortex (C3) controls the **right hand**, while the right motor cortex (C4) controls the **left hand**.
        
          
        
- **The Mu Rhythm ($8\text{--}12\text{ Hz}$):** When your hands are completely at rest, millions of neurons in your motor cortex fire in a synchronized, idling standby loop at about $8\text{--}12\text{ Hz}$, known as the **mu rhythm** (and a harmonic beta peak around $20\text{ Hz}$).
    
      
    
- **Primary Law of Motor Imagery (Event-Related Desynchronization):** When you move—or merely _imagine_ moving—a hand, the corresponding contralateral motor cortex wakes up from its idling standby loop to process the motor command, causing the $8\text{--}12\text{ Hz}$ mu rhythm power to **drop** significantly.
    
      
    - Imagining **right-hand** movement lowers mu power over **C3** (left hemisphere).
        
          
        
    - Imagining **left-hand** movement lowers mu power over **C4** (right hemisphere).
        
          
        
    - _Analogy:_ Think of the mu rhythm like a choir humming a single, loud monotone note while waiting for the conductor. The moment the conductor cues the left side of the choir to start singing complex individual melodies (active motor processing), the loud unison hum on that side drops away.
        
          
        

### Case Study: BCI Competition II (Dataset III, Graz University of Technology)

Let's examine how a real motor imagery experiment is structured and sampled:

  

- **Dataset Parameters:** Recorded from $1$ subject performing $140$ trials ($70$ left-hand imagery, $70$ right-hand imagery) across $3$ motor cortex channels (C3, Cz, C4).
    
      
    
- **Sampling & Nyquist Limit:** The EEG is digitized at a **sampling rate** of $128\text{ samples per second}$ ($128\text{ Hz}$) and pre-filtered between $0.5\text{--}30\text{ Hz}$.
    
      
    - By the **Nyquist Theorem**, a sampling rate of $128\text{ Hz}$ allows us to accurately measure frequencies up to half that rate—$64\text{ Hz}$ (the **Nyquist frequency**).
        
          
        
- **Anatomy of a Single $9\text{-Second}$ Trial:**
    
      
    - $0\text{--}2\text{ s}$: **Rest period** (baseline idling).
        
          
        
    - At $2\text{ s}$: An acoustic **beep** sounds and a visual **fixation cross** appears on screen to tell the user to get ready and stop blinking.
        
          
        
    - $3\text{--}9\text{ s}$: An **arrow cue** ($\leftarrow$ or $\rightarrow$) appears on screen, instructing the subject to continuously imagine moving either their left or right hand for the remainder of the trial (with the purest imagery window typically analyzed between $4\text{--}7\text{ s}$).
        
          
        

### The End-to-End Classification Pipeline

Can we just feed the entire raw $0.5\text{--}30\text{ Hz}$ EEG signal into a machine learning classifier and ask it to guess left vs. right? **No!** Outside of $8\text{--}12\text{ Hz}$ (and a small echo around $20\text{ Hz}$), the frequency spectra for left-hand and right-hand trials overlap almost completely—keeping all frequencies simply drowns the classifier in irrelevant noise. Instead, we follow a targeted feature-extraction workflow:

  

- **Step 1: Raw Data Tensor:** Start with our 3D block of data: $\text{trials } (140) \times \text{channels } (3) \times \text{time } (9\text{ s} \times 128\text{ Hz})$.
    
      
    
- **Step 2: Band-Pass Filtering:** Pass every trial and channel through an $8\text{--}12\text{ Hz}$ band-pass filter and bring the filtered waves back to the time domain.
    
      
    - If you inspect a single trial at electrode C3 after $8\text{--}12\text{ Hz}$ filtering, the difference is striking: during a left-hand trial, the C3 wave stays tall and vigorous from $3\text{--}8\text{ s}$, whereas during a right-hand trial, the C3 wave flattens out after the $3\text{ s}$ cue!
        
          
        
- **Step 3: Feature Extraction (Log Power):** Collapse the active imagery time window ($4\text{--}7\text{ s}$) into a single summary number per channel—its **log power** (achieved either by squaring and averaging the filtered time-domain amplitude, or by integrating the **Power Spectral Density (PSD)** directly in the frequency domain between $8\text{--}12\text{ Hz}$).
    
      
    - Now, every trial is represented by just two coordinates: its $8\text{--}12\text{ Hz}$ log power at **C3** (x-axis) and at **C4** (y-axis).
        
          
        
- **Step 4: Linear Classification:** When we plot all $140$ trials on a 2D scatter plot (C3 power vs. C4 power), the trials naturally separate into two distinct clouds:
    
      
    - **Left-hand trials** cluster in the bottom-right (high C3 power, low C4 power).
        
          
        
    - **Right-hand trials** cluster in the top-left (low C3 power, high C4 power).
        
          
        
    - A simple straight **linear decision boundary** (such as Linear Discriminant Analysis or a Linear SVM) drawn diagonally down the middle cleanly separates left from right!
        
          
        

## 3. Event-Related Potentials (ERPs) & Time-Domain Averaging

While motor imagery looks at continuous _waves_ that rise and fall over several seconds, another major branch of neuroscience and BCI looks at rapid, split-second electrical spikes triggered by specific sensory events.

  

### What is an ERP? Time-Locking vs. Phase-Locking

An **Event-Related Potential (ERP)** is a stereotyped voltage deflection in the EEG that occurs after a specific sensory, cognitive, or motor **stimulus** (such as a flash of light, a sound beep, or a visual cue) after the exact same time delay in every trial:

  

- **The Challenge of Single-Trial Noise:** If you look at $6$ individual raw EEG trials at electrode Cz right after the user sees a flash of light at $t = 0\text{ s}$, you cannot see the ERP at all. The ongoing background EEG activity is tens of microvolts ($\mu\text{V}$) tall, completely burying the tiny $5\text{--}10\text{ }\mu\text{V}$ stimulus response.
    
      
    
- **The Solution — Trial Averaging:** Because the ERP always happens at the exact same millisecond delay after the flash, whereas background brain waves wander randomly, averaging hundreds of trials together (e.g., $N = 288$ attended flashes) causes the random background waves to cancel out to zero while the hidden ERP waveform emerges crystal clear!
    
      
    - Mathematically, averaging collapses our 3D data tensor across the trials dimension: $\text{trials} \times \text{channels} \times \text{time} \to \text{channels} \times \text{time}$.
        
          
        
    - For multi-subject studies, researchers also compute a **group-level grand average** by averaging the individual ERPs of all participants together.
        
          
        
- **Essential Taxonomy — Time-Locked vs. Phase-Locked Signals:** Trial averaging is ruthless: it keeps _only_ what is identical at the exact same millisecond in every single trial. To understand what survives an average and what gets destroyed, we must distinguish two properties:
    
      
    - **Time-Locked AND Phase-Locked (Evoked Activity / ERPs):** The response happens at the same delay after the stimulus (_time-locked_) AND always starts with the exact same wave direction—e.g., always peaking upward at $+300\text{ ms}$ (_phase-locked_). **Result:** Survives the average! This is the classic ERP.
        
          
        
    - **Time-Locked, NOT Phase-Locked (Induced Activity / Oscillations):** An oscillation (like the $8\text{--}12\text{ Hz}$ mu rhythm) reliably changes its amplitude after the cue (_time-locked_), but on Trial 1 it happens to be at a wave peak ($+10\text{ }\mu\text{V}$) at $t = 4.0\text{ s}$, while on Trial 2 it happens to be in a wave trough ($-10\text{ }\mu\text{V}$) at $t = 4.0\text{ s}$ (_not phase-locked_). **Result:** The peaks and troughs cancel each other out, and the average is completely flat!
        
          
        
    - **NOT Time-Locked, NOT Phase-Locked (Background Noise):** Random ongoing brain activity and resting noise that has no relationship to the stimulus timing. **Result:** Cancels out to a flat line.
        
          
        
    - **NOT Time-Locked, BUT Phase-Locked:** Physically impossible! As a logical rule, having the same phase at the same moment requires having the same reference moment (time-locking) in the first place.
        
          
        
    - _Analogy:_ Imagine recording $100$ videos of an audience at a concert. If the singer yells "Jump!" at $t = 0$ and everyone jumps at $t = 0.3\text{ s}$ (time-locked and phase-locked), averaging the videos shows a massive synchronized jump. If the singer says "Shake your heads!" at $t = 0$ and everyone starts shaking left-and-right at their own random starting angles (time-locked, not phase-locked), averaging the videos blurs all the lefts and rights into a motionless, flat average.
        
          
        

### The P300 Component

The most famous ERP component in cognitive neuroscience and BCI is the **P300**:

  

- **Morphology & Timing:** Named because it is a **Positive** voltage peak occurring approximately **$300\text{ ms}$** ($0.3\text{--}0.4\text{ s}$) after a stimulus (often preceded by a smaller sensory **P2** peak around $150\text{--}200\text{ ms}$).
    
      
    
- **Cognitive Trigger (The "Oddball" Effect):** The P300 is linked directly to **selective attention and surprise**. If you watch a stream of frequent, boring flashes (non-targets) while waiting for a rare, meaningful flash (your target), your brain fires a massive P300 wave _only_ when the attended target flashes; unattended flashes produce no P300.
    
      
    
- **Relativity of Polarity:** Note that whether a peak points "up" (positive) or "down" (negative) in an EEG plot is always relative to the chosen **reference electrode** (e.g., earlobe or mastoid).
    
      
    

### Baseline Correction in the Time Domain

Before you can average trials together to extract an ERP, you must solve a major practical problem: slow voltage drift.

  

- **The Drift Problem:** A slight head movement, an eye blink, sweat, or talking can shift the baseline voltage of an entire trial up or down by hundreds of microvolts.
    
      
    - Suppose Trial 1 sits normally between $0\text{--}10\text{ }\mu\text{V}$, Trial 2 shifts up due to a blink and sits at $1000\text{--}1010\text{ }\mu\text{V}$, and Trial 3 sits at $20\text{--}30\text{ }\mu\text{V}$. Even though all three trials contain the exact same $+10\text{ }\mu\text{V}$ P300 peak, averaging them raw will be overwhelmingly pulled and distorted by Trial 2's massive $1000\text{ }\mu\text{V}$ offset!
        
          
        
- **The Baseline Correction Workflow:**
    
      
    - **Step 1: Define the Baseline Window:** Choose a brief quiet period _just before_ the stimulus occurs (e.g., $-0.2\text{ to }0\text{ s}$, a $200\text{ ms}$ window prior to the flash). It must be close enough to the stimulus to share the same drift offset, share the same scaling, and be completely free of the ERP component itself.
        
          
        
    - **Step 2: Compute the Trial Baseline Mean ($\mu$):** For each trial independently, calculate the average voltage $\mu$ inside that pre-stimulus baseline window (e.g., $\mu_1 \approx 0\text{ }\mu\text{V}$, $\mu_2 \approx 1000\text{ }\mu\text{V}$, $\mu_3 \approx 20\text{ }\mu\text{V}$).
        
          
        
    - **Step 3: Subtract the Baseline ($x - \mu$):** Subtract each trial's baseline mean $\mu$ from every single time point $x$ in that trial. Now, all three trials sit identically aligned at $0\text{--}10\text{ }\mu\text{V}$!
        
          
        
    - **Step 4: Average Across Trials & Verify:** Now compute the trial average. As a sanity check, the pre-stimulus baseline of your final averaged ERP should hover flat around $0\text{ }\mu\text{V}$, making the true divergence between attended and unattended trials stand out sharply.
        
          
        
- **Edge Case — Fast Flashes:** What if your stimuli flash so rapidly that the previous flash's ERP is still happening during the current flash's $-200\text{ ms}$ pre-stimulus window (leaving no clean room for a baseline)? In that case, use the **mean of the whole epoch** as $\mu$ to center the trial.
    
      
    - _Analogy:_ Baseline correction is like pressing the **"Tare" (Zero) button** on a digital kitchen scale after placing a heavy mixing bowl on it. Whether you use a light plastic bowl ($20\text{ g}$) or a heavy ceramic bowl ($1000\text{ g}$), pressing Tare zeroes out the container so you measure only the $10\text{ g}$ of flour (the ERP) you pour in afterward.
        
          
        

### Case Study: The P300 Speller (BNCI 2014-009, Aricò et al., 2014)

How do we turn the P300 wave into a communication device for patients with locked-in syndrome (such as late-stage ALS) who cannot move or speak? Enter the classic **P300 Speller**:

  

- **The $6 \times 6$ Character Matrix:** The user looks at a grid of $36$ alphanumeric characters ($A\text{--}Z$, $0\text{--}9$) arranged in $6$ rows and $6$ columns:
    
      
$$
\begin{bmatrix}
A & B & C & D & E & F \\
G & H & I & J & K & L \\
M & N & O & P & Q & R \\
S & T & U & V & W & X \\
Y & Z & 1 & 2 & 3 & 4 \\
5 & 6 & 7 & 8 & 9 & 0
\end{bmatrix}
$$
        
          
        
- **The Stimulation Protocol:** Instead of flashing one letter at a time (which would take forever), the system flashes **whole rows and whole columns** in random order—making $12$ flashing groups ($6\text{ rows} + 6\text{ columns}$).
    
      
    - A flash occurs every **$250\text{ ms}$** ($4\text{ flashes per second}$).
        
          
        
    - To type a single character, every row and every column flashes **$8\text{ times}$** ($8\text{ repetitions} \times 12\text{ groups} = 96\text{ total flashes}$, taking $24\text{ seconds}$ per character).
        
          
        
    - Recording Setup: $16\text{ EEG channels}$ sampled at $256\text{ samples per second}$, testing $18\text{ characters}$ per subject.
        
          
        
- **Target vs. Non-Target Geometry:** To type a letter (say, the letter **'B'**, which sits at the intersection of **Row 1** and **Column 2**), the user simply focuses their attention on 'B' and silently counts whenever it lights up.
    
      
    - Out of the $96$ flashes for that character, exactly **$16\text{ flashes}$** contain the attended letter ($8\text{ flashes}$ of Row 1 + $8\text{ flashes}$ of Column 2). These $16$ are the **targets** and elicit a P300 wave $\sim 300\text{ ms}$ later.
        
          
        
    - The other **$80\text{ flashes}$** light up the remaining $5$ rows and $5$ columns that do _not_ contain 'B'. These $80$ are **non-targets** and do not elicit a P300.
        
          
        
- **Decoding & Evidence Accumulation:** Because the computer controls the screen, we know the exact millisecond each row and column flashed.
    
      
    - After each flash, a classifier scores the EEG window $\sim 300\text{ ms}$ later for P300 evidence and adds that score to a running tally for that specific row or column.
        
          
        
    - At the end of the flashes, the system simply picks the **strongest row $\times$ the strongest column**—their intersection on the $6 \times 6$ grid reveals the spelled character (e.g., spelling out **B - C - I**)!
        
          
        
- **The Speed vs. Accuracy Trade-Off:** Why flash each row and column $8$ times instead of just once? Because a single EEG trial is noisy! Looking at the empirical decoding accuracy across the $18$ test characters in Subject 1 shows the power of trial averaging in action:
    
      
    - **$1\text{ repetition}$** ($12\text{ flashes}$): Only **$9\text{ / }18$** characters spelled correctly ($50\%$).
        
          
        
    - **$2\text{ repetitions}$** ($24\text{ flashes}$): **$14\text{ / }18$** characters correct ($78\%$).
        
          
        
    - **$4\text{ repetitions}$** ($48\text{ flashes}$): **$18\text{ / }18$** characters correct ($100\%$).
        
          
        
    - **$8\text{ repetitions}$** ($96\text{ flashes}$): **$18\text{ / }18$** characters correct ($100\%$, with an even wider safety margin).
        
          
        

## 4. Time-Frequency Analysis & Spectral Dynamics

We now hit a fascinating paradox when studying brain rhythms like the $8\text{--}12\text{ Hz}$ mu wave. Why can't we just use our earlier tools to see _when_ a brain rhythm turns on and off across multiple trials?

  

- **Why the Classical Power Spectrum Fails:** Taking a standard Fourier transform across a whole $9\text{-second}$ trial compresses all of time into a single static graph—it tells you _what_ frequencies were present overall, but gives zero information about _when_ they happened (e.g., before vs. after the arrow cue).
    
      
    
- **Why ERP Trial Averaging Fails:** If we average our raw motor-imagery trials together in the time domain like an ERP, the $8\text{--}12\text{ Hz}$ mu rhythm disappears completely! Because the mu rhythm is **not phase-locked** to the cue, the peaks and troughs across different trials cancel each other out into a flat line.
    
      
    
- **The Solution — Time-Frequency (TF) Analysis:** Instead of averaging the raw voltage waves first, we chop each trial into small time slices, compute the frequency power spectrum for _each slice_ on every trial, and **then average the power envelopes across trials**. Now, we see a rich 2D map showing how power rises and falls over time for every frequency—revealing that $8\text{--}12\text{ Hz}$ power at C3 drops sharply right after the $3\text{ s}$ right-hand cue!
    
      
    
- **The Fundamental Law of Time-Frequency (The Gabor / Heisenberg Uncertainty Trade-Off):** You cannot be infinitely sharp in time and infinitely sharp in frequency simultaneously ($\text{sharp in time} \leftrightarrow \text{blurry in frequency}$, and vice versa). To know _precisely when_ an event occurred, you must look at a tiny sliver of time, which doesn't contain enough wave cycles to measure frequency accurately; to measure frequency with laser precision, you must observe many wave cycles over a long window, which blurs out the exact moment the power changed.
    
      
    

Let's unpack the three classic algorithms used to build a Time-Frequency representation.

  

### Method 1: Short-Time Fourier Transform (STFT)

The **Short-Time Fourier Transform (STFT)** is the most intuitive way to track frequency changes over time:

  

- **Step-by-Step Operational Workflow:**
    
      
    1. **Take a Window:** Isolate a short, fixed-length time window of the raw EEG signal (for example, a $0.5\text{ s}$ segment).
        
          
        
    2. **Fourier Transform:** Compute the Fourier transform of just that short segment to get its local power spectrum.
        
          
        
    3. **Assign to Center:** Assign that entire spectrum to the time point at the exact **center** of the window.
        
          
        
    4. **Flip & Color-Code:** Turn the spectrum graph vertically (so frequency from $0\text{--}30\text{ Hz}$ runs along the y-axis) and represent the power height using color (**blue = low power, red = high power**).
        
          
        
    5. **Slide & Stack:** Slide the window forward by one time point (for a smooth plot), repeat the Fourier transform, and stack the vertical color columns side-by-side along the x-axis (time) to build the complete 2D **Time-Frequency (TF) plot**.
        
          
        
- **The Fixed-Window Trade-Off in STFT:** Because STFT uses the _exact same window duration_ for all frequencies, choosing your window length forces a direct compromise:
    
      
    - **Short Window (e.g., $0.25\text{ s}$):** Very **sharp in time** (you can pinpoint the exact tenth of a second a burst happens), but **blurry in frequency** (the $10\text{ Hz}$ mu band smears vertically into a thick, fuzzy band because $0.25\text{ s}$ only holds $2.5\text{ cycles}$ of a $10\text{ Hz}$ wave).
        
          
        
    - **Long Window (e.g., $1.0\text{ s}$):** Very **sharp in frequency** (draws razor-thin horizontal lines at $10\text{ Hz}$ and $20\text{ Hz}$), but **blurry in time** (smears rapid power drops horizontally across a full second).
        
          
        
    - _Analogy:_ Choosing an STFT window is like choosing a camera shutter speed for a sports match: a fast shutter ($0.25\text{ s}$) freezes the athlete's exact position in time without motion blur, while a long exposure ($1.0\text{ s}$) captures the full rhythmic trajectory of the ball's spin but blurs the athlete across the frame.
        
          
        

### Method 2: Wavelet Transform (Morlet Wavelets)

Instead of chopping the EEG with a rigid, fixed-duration box like STFT, the **Wavelet Transform** uses a flexible, scale-adaptive probe that naturally adapts its length to each frequency:

  

- **Anatomy of a Wavelet:** A **Morlet wavelet** is a short sine wave at one specific target frequency multiplied by a smooth, **bell-shaped (Gaussian) window** so that it tapers gracefully to zero at both ends.
    
      
    
- **The Role of Cycles:** We define the width of a wavelet not in fixed seconds, but by the number of **cycles** (full wave oscillations) that fit inside its bell-shaped envelope.
    
      
    - For a **$10\text{ Hz}$ wavelet** (where $1\text{ cycle} = 0.1\text{ s}$):
        
          
        - **$3\text{ cycles}$** $\approx 0.3\text{ s}$ long $\to$ short in time (sharp temporal precision), but sees a wide, blurry frequency spread of **$10 \pm 3.3\text{ Hz}$**.
            
              
            
        - **$6\text{ cycles}$** $\approx 0.6\text{ s}$ long $\to$ balanced length in time, tightening the frequency spread to **$10 \pm 1.7\text{ Hz}$**.
            
              
            
        - **$12\text{ cycles}$** $\approx 1.2\text{ s}$ long $\to$ very sharp in frequency, but blurs events horizontally across more than a second of time.
            
              
            
- **Convolution Across Frequencies:** How do we build the TF plot with wavelets?
    
      
    - Just like we slid our FIR filter kernel along the EEG earlier, we take our wavelet for a given frequency (say, a $6\text{-cycle}$ wavelet at $10\text{ Hz}$) and **convolve** (slide) it along the raw EEG signal. The dot-product similarity at each time point gives us the exact power at that moment!
        
          
        
    - We repeat this sliding convolution **once for every frequency**, generating one horizontal row of the Time-Frequency plot per wavelet frequency.
        
          
        
    - _Example:_ Comparing a $3\text{-cycle}$, $6\text{-cycle}$, and $12\text{-cycle}$ wavelet decomposition of the same C3 motor imagery trial clearly shows the uncertainty trade-off: the $3\text{-cycle}$ plot looks like vertical stripes (precise moments, smeared frequencies), the $12\text{-cycle}$ plot looks like horizontal ribbons (precise frequencies, smeared moments), and the $6\text{-cycle}$ plot hits the "Goldilocks" sweet spot in between.
        
          
        

### Method 3: The Filter-Hilbert Transform

What if we already know the exact frequency band we care about (like $8\text{--}12\text{ Hz}$) and want to track its instantaneous amplitude (power) and phase at _every single sample point_ in time? We use the **Hilbert Transform**:

  

- **Step-by-Step Operational Workflow:**
    
      
    
    1. **Band-Pass Filter the EEG:** First, isolate your target frequency band (e.g., filter the raw EEG between $8\text{--}12\text{ Hz}$). This gives you a clean, oscillating real wave $x(t)$.
        
          
        
    2. **Build the Partner Wave (Hilbert Transform):** Apply the **Hilbert transform** to your filtered wave. Mathematically, this builds an imaginary "partner wave" $\tilde{x}(t)$ that is shifted exactly **a quarter of a cycle ($90^\circ$ or $\frac{\pi}{2}\text{ radians}$) behind** the original wave.
        
          
        
    3. **Form the Analytic Signal (A 3D Corkscrew Vector):** When you plot the original wave on the horizontal axis and its quarter-cycle-delayed partner wave on the vertical axis over time, together they form a 2D vector spinning around a circle—tracing out a 3D corkscrew (helix) through time known as the **analytic signal**:
        
          
        
        $$z(t) = x(t) + i\tilde{x}(t) = A(t)e^{i\phi(t)}$$
        
    4. **Extract Envelope, Power, and Phase:** Looking at this turning vector "from the front" (in the 2D complex plane) gives us three instantaneous measurements at every single time sample:
        
          
        - **The Vector's Length = The Envelope (Instantaneous Amplitude, $A(t)$):** By the Pythagorean theorem ($A(t) = \sqrt{x(t)^2 + \tilde{x}(t)^2}$), even as the original wave crosses zero, its quarter-cycle partner is at a peak, so the vector's length smoothly traces the outer boundary (**envelope**) of the wave!
            
              
            
        - **The Vector's Angle = The Instantaneous Phase ($\phi(t)$):** Tells us where we are in the wave cycle ($0^\circ \to 360^\circ$) at any millisecond.
            
              
            
        - **$\text{Length}^2$ = Instantaneous Power ($A(t)^2$):** Squaring the envelope gives the band power at every time point.
            
              
            
    
    - _Analogy:_ Imagine watching a bicycle pedal from the side: it just appears to move up and down (a 1D wave), making it hard to tell how wide the crank is when the pedal passes through the middle. Viewing the **analytic signal** via the Hilbert transform is like looking at the bicycle crank from the side _and_ from above simultaneously—you see the pedal moving in a full circle, and the radius of that circle (the envelope) remains constant and obvious at every single angle.
        
          
        

### Baseline Correction in the Time-Frequency Domain ($1/f$ Scaling)

When you plot a raw Time-Frequency map of an EEG recording, you immediately run into a visual and statistical roadblock: **low frequencies naturally have vastly more raw power than high frequencies** (a universal property of brain signals called $1/f$ scaling, plus the dominant $10\text{ Hz}$ mu peak). Because the $8\text{--}12\text{ Hz}$ mu rhythm is so huge in raw microvolts-squared, a raw TF color scale is completely dominated by a bright red bar at $10\text{ Hz}$, making higher frequencies (like the $20\text{ Hz}$ beta band) look invisibly dark by comparison!

  

- **The Row-by-Row TF Baseline Correction Workflow:** To put every frequency on an equal, comparable playing field, we normalize each frequency row against its own pre-stimulus resting baseline:
    
      
    1. **Compute Full TF Power:** Calculate the raw time-frequency power map across the entire trial (baseline and active imagery period together).
        
          
        
    2. **Calculate Each Frequency Row's Baseline Mean ($\mu_f$):** For each horizontal frequency row $f$ independently, compute its average raw power $\mu_f$ during the resting baseline window ($0.5\text{--}2.5\text{ s}$, before the cue).
        
          
        
    3. **Normalize Each Frequency Row:** Transform every time-frequency pixel $x(f, t)$ using either:
        
          
        - **Absolute Difference:** $x - \mu_f$
            
              
            
        - **Percentage Change (Event-Related Spectral Perturbation):**
            
              
            
            $$\text{\% Change} = \frac{x - \mu_f}{\mu_f} \times 100$$
            
            This expresses power in intuitive units of _"how many baselines away"_ (e.g., $+40\%$ above baseline rest, or $-40\%$ below baseline rest).
            
              
            
- **The Payoff — Cross-Frequency & Cross-Hemisphere Comparability:**
    
      
    - Once converted to **% change**, every frequency from $1\text{ Hz}$ to $30\text{ Hz}$ becomes directly comparable on the exact same color scale ($-60\%$ blue to $+60\%$ red)!
        
          
        
    - When we plot the baseline-corrected TF maps for our Graz Motor Imagery dataset across C3 and C4, the complete neurophysiological picture leaps off the screen:
        
          
        - **Right-Hand Imagery:** **C3** (left hemisphere) turns deep **blue** ($-40\%$ to $-60\%$ power drop) starting right after the $3\text{ s}$ cue in _both_ the **$10\text{ Hz}$ mu band** AND the **$20\text{ Hz}$ beta band**, while C4 stays neutral/red.
            
              
            
        - **Left-Hand Imagery:** The exact mirror image occurs—**C4** (right hemisphere) turns deep **blue** across both the $10\text{ Hz}$ mu and $20\text{ Hz}$ beta bands!
            
              
            
        - Remember the core neuro-decoding rule: **Less mu and beta power = more cortical activity!**
            
              
            

## Professor's Synthesis

If there is one overarching takeaway to carry with you from this lecture, it is that **how you process an EEG signal depends entirely on the neurobiological code the brain is using for that task**:

  

- When the brain responds to an external event with a **time-locked and phase-locked** spike (like the **P300** in a visual speller), **time-domain baseline correction and trial averaging** are your best friends—they cancel out ongoing oscillations and isolate the exact waveform of attention, allowing a locked-in patient to type out words purely by looking at a flashing grid.
    
      
    
- Conversely, when the brain encodes internal intentions through **time-locked but non-phase-locked oscillations** (like the **$8\text{--}12\text{ Hz}$ mu rhythm desynchronization** during imagined hand movements), simple time-domain averaging destroys the very signal you are looking for! Instead, you must turn to **band-pass filtering ($8\text{--}12\text{ Hz}$), STFT, Morlet wavelets, or the Filter-Hilbert transform** to extract the signal's **power envelope** before averaging or classifying—and always apply **row-wise percentage baseline correction** so that subtle high-frequency dynamics (like $20\text{ Hz}$ beta) aren't overshadowed by dominant slow waves.
    
      
    

As you build your own BCI pipelines, always keep the fundamental engineering trade-offs in mind: balance **filter order** against time-domain **ringing**, choose **FIR** for offline precision and **IIR** for real-time speed, and tune your **STFT window length or wavelet cycle count** to strike the optimal balance between knowing _when_ the brain acted and knowing _what frequency_ it used to speak.
