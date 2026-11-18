---
{"dg-publish":true,"permalink":"/4th-year/1st-term/bci/before-mid/lecture-3-egg-input-formatting-code-example/","tags":["BCI","EEG","Machine-Learning","NumPy","Signal-Processing"],"dg-note-properties":{"tags":["BCI","EEG","Machine-Learning","NumPy","Signal-Processing"],"course":"Brain-Computer Interfacing","instructor":"Dr. Mahmoud Eid","topic":"EEG as a 3D Tensor on Real Data"}}
---


# EEG as a 3D Tensor on Real Data

## 1. Warm-up: Digital Signal Processing & The Cosine Wave

Before analyzing noisy, multi-channel biological EEG signals, we examine how a single, clean oscillation is mathematically defined and digitally stored by a computer.

### Mathematical Formulation
A continuous cosine wave is defined as:

$$x(t) = A \cos(2\pi f t + \phi)$$

* **$f$ (Frequency in $\text{Hz}$):** Number of full oscillation cycles per second.
* **$T$ (Period in seconds):** Duration of one single cycle ($T = \frac{1}{f}$).
* **$f_s$ (Sampling Rate in $\text{Hz}$):** Number of discrete measurements (samples) stored per second. **Core Intuition:** The computer does *not* store a continuous curve; it only stores discrete numbers (orange dots) sampled at step intervals of $\Delta t = \frac{1}{f_s}$.
* **Samples per Cycle:** The number of discrete points capturing a single wave cycle is $\frac{f_s}{f} = f_s \cdot T$.
* **$t$ (Time Vector):** A 1D array of timestamps for each sample: $0, \frac{1}{f_s}, \frac{2}{f_s}, \dots, \text{duration} - \frac{1}{f_s}$ (where sample $k$ occurs at $\frac{k}{f_s}$ seconds).
* **$A$ (Amplitude):** Peak height of the wave from the zero line.
* **$\phi$ (Phase in radians):** Shifts the wave horizontally in time without changing its shape or frequency. A positive $+\phi$ shifts the wave to the **left** (peaks happen earlier in time) by a time shift of:

$$\Delta t_{\text{shift}} = \frac{\phi}{2\pi f}$$

> [!warning] The Nyquist-Shannon Sampling Theorem & Aliasing
> The **Nyquist Frequency** is half the sampling rate ($\frac{f_s}{2}$). To accurately represent a wave without **aliasing**, the signal frequency $f$ must stay strictly below $\frac{f_s}{2}$ ($f < \frac{f_s}{2}$).
> * **Special Parameter Cases:**
>   * `phi = np.pi / 2`: Shifts cosine left by a quarter cycle ($90^\circ$), turning $\cos(2\pi f t + \frac{\pi}{2})$ into $-\sin(2\pi f t)$.
>   * `f = 10, fs = 10`: Only $1$ sample per cycle ($\frac{f_s}{f} = 1$). You sample the exact same phase point every cycle, resulting in a flat horizontal line!
>   * `f = 35, fs = 40`: Violates Nyquist ($35\text{ Hz} > 20\text{ Hz}$). The samples alias and masquerade as a false, slower $5\text{ Hz}$ wave ($|35 - 40| = 5\text{ Hz}$).

### Code & Line-by-Line Intuition

```python
import numpy as np
import matplotlib.pyplot as plt

f = 2              # 2 cycles/sec -> Period T = 1/2 = 0.5 s
fs = 100           # 100 samples/sec -> step size 1/fs = 0.01 s (10.0 ms)
duration = 2       # 2 seconds total -> 2 * 100 = 200 samples
A = 1              # Peak amplitude = 1
phi = np.pi / 4    # 45 degrees phase shift (0.785 rad)

# 1. Discrete time vector: 200 samples from 0.000s to 1.990s
t = np.arange(0, duration, 1 / fs)          

# 2. Vectorized cosine evaluation at every timestamp in 't'
x = A * np.cos(2 * np.pi * f * t + phi)     

# 3. Wave metrics
T = 1 / f                         # T = 0.500 s (samples per cycle = fs/f = 50.0)
shift = phi / (2 * np.pi * f)     # Left time shift = (pi/4)/(4*pi) = 0.0625 s (62.5 ms)

# 4. Dense time axis (5000 points) to visually draw the "continuous" wave behind the orange dots
t_fine = np.linspace(0, duration, 5000)
x_fine = A * np.cos(2 * np.pi * f * t_fine + phi)
x_ref = A * np.cos(2 * np.pi * f * t_fine)  # Reference wave with phi = 0

# 5. Finding the first positive peak inside the first cycle for annotation
# Unshifted cosine peaks at t = 0 and t = T. Shifted left by 'shift', the peak at T moves to:
t_peak = T - (shift % T)          # 0.5 - 0.0625 = 0.4375 s
```

---

## 2. Dataset Setup & Reshaping into a 3D ML Tensor

### Experimental Paradigm: BCI Competition II (Dataset III - Graz)
* **Motor Imagery (MI) Task:** One subject imagined moving either their **Left Hand** (`label = 1`) or **Right Hand** (`label = 2`).
* **Trial Timeline ($9\text{ s}$ total):**
  * `0 – 2 s`: **Rest** (baseline period — no task).
  * `t = 2 s`: **Acoustic beep + fixation cross** appears (subject prepares and focuses eyes).
  * `t = 3 s`: **Visual arrow cue** ($\leftarrow$ or $\rightarrow$); subject imagines the hand movement until $t = 9\text{ s}$.
* **Recording Specs:**
  * **Channels ($3$):** Bipolar EEG channels over `C3` (left hemisphere motor cortex), `Cz` (vertex/midline), and `C4` (right hemisphere motor cortex).
  * **Sampling Rate ($f_s$):** $128\text{ Hz}$ ($\Delta t = \frac{1}{128} \approx 7.81\text{ ms}$). Total time samples per trial = $9\text{ s} \times 128\text{ Hz} = 1152\text{ samples}$.
  * **Bandpass Filter:** $0.5\text{ to }30\text{ Hz}$ (isolates $\mu$ and $\beta$ rhythms while removing slow drift and $50/60\text{ Hz}$ power-line noise).
  * **Trials:** $140$ labeled trials ($70$ `LEFT`, $70$ `RIGHT`).

### Tensor Axis Convention: MATLAB vs. Python BCI/ML
In the raw MATLAB `.mat` file, `x_train` is stored as `(time, channels, trials)` = `(1152, 3, 140)`.
In Python BCI and Deep Learning pipelines (`scikit-learn`, `PyTorch`, `MNE`), **samples/trials must be Axis 0**: `(trials, channels, time)` = `(140, 3, 1152)`.
* **One Trial** `X[i]` is a 2D matrix of shape `(channels, time)` = `(3, 1152)`.
* **All Trials** `X` form a 3D tensor of shape `(trials, channels, time)` = `(140, 3, 1152)`.

$$\text{raw: }\underbrace{(1152}_{\text{Axis 0}}, \underbrace{3}_{\text{Axis 1}}, \underbrace{140)}_{\text{Axis 2}} \xrightarrow{\text{np.transpose(raw, (2, 1, 0))}} X\text{: }\underbrace{(140}_{\text{Old Axis 2}}, \underbrace{3}_{\text{Old Axis 1}}, \underbrace{1152)}_{\text{Old Axis 0}}$$

### Code & Line-by-Line Intuition

```python
from scipy.io import loadmat

mat = loadmat('dataset_BCIcomp1.mat') # Dictionary mapping variable names to NumPy arrays
raw = mat['x_train']                  # Shape: (1152, 3, 140) -> (time, channels, trials)

# y_train has shape (140, 1) (2D column vector). .ravel() flattens it to a 1D vector (140,)
# Needed so we can use 'y == 1' as a 1D boolean mask along Axis 0 of X!
y = mat['y_train'].ravel()            

# Permute axes: new axis 0 = old axis 2 (trials), new axis 1 = old axis 1 (channels), new axis 2 = old axis 0 (time)
X = np.transpose(raw, (2, 1, 0))      # Shape: (140, 3, 1152)
# Verification: raw[500, 0, 7] == X[7, 0, 500] == -0.03223

fs = 128
channel_names = ['C3', 'Cz', 'C4']
class_names = {1: 'LEFT', 2: 'RIGHT'}
class_colors = {1: 'cyan', 2: 'orange'}

n_trials, n_channels, n_times = X.shape
times = np.arange(n_times) / fs       # [0.000, 0.0078125, ..., 8.992] seconds
```

---

## 3. Part 1: Slicing the 3D Tensor

### Example 1a: One Index Per Dimension
For `X[trials, channels, time]`:
* **Single integer (`0`):** Extracts that index and **collapses/removes** that dimension.
* **Colon (`:`):** Keeps all elements along that dimension.
* **Slice (`a:b`):** Keeps a range of indices along that dimension.
* **Boolean mask (`window` or `y == 1`):** Keeps only positions where the mask is `True`.

```python
window = (times >= 3) & (times < 5)   # 1D boolean mask (length 1152): True for 256 samples (2s * 128Hz)
c4 = channel_names.index('C4')        # Finds integer index of 'C4' -> 2
```

| Code Expression | What it Selects | Output Shape | Rank |
| :--- | :--- | :--- | :--- |
| `X[0]` | Trial 0 (all 3 channels, all 1152 time points) | `(3, 1152)` | 2D Matrix |
| `X[0, 0]` | Trial 0, Channel `C3` (all 1152 time points) | `(1152,)` | 1D Vector |
| `X[0, 0, 384]` | Trial 0, `C3`, at sample $384$ ($t = \frac{384}{128} = 3.0\text{ s}$) | `()` | 0D Scalar |
| `X[:, c4]` | All 140 trials, Channel `C4`, all 1152 time points | `(140, 1152)` | 2D Matrix |
| `X[:10]` | First 10 trials, all channels, all time points | `(10, 3, 1152)` | 3D Tensor |
| `X[:, :, window]` | All trials, all channels, time cropped to $[3\text{ s}, 5\text{ s})$ | `(140, 3, 256)` | 3D Tensor |
| `X[y == 1]` | All `LEFT` trials ($70$ trials), all channels, all time | `(70, 3, 1152)` | 3D Tensor |

---

### Example 1b: Combining Masks (The NumPy Advanced Indexing Trap)

> [!danger] Why `X[y == 2, 0, window]` and `X[0, :, window]` Fail!
> When you combine **two boolean masks** (or **an integer and a boolean mask separated by `:`**) inside the *same* brackets `X[...]`, NumPy triggers **Advanced Indexing**:
> 1. **Paired Broadcasting Crash (`X[y == 2, 0, window]`):** NumPy does *not* filter Axis 0 and then Axis 2 sequentially. Instead, it tries to pair the $70$ `True` indices of `y == 2` element-by-element with the $256$ `True` indices of `window`. Since $70 \neq 256$, it raises `IndexError: shape mismatch: indexing arrays could not be broadcast together with shapes (70,) (256,)`.
> 2. **Silent Axis Transposition Trap (`X[0, :, window]`):** Here, Axis 0 uses an integer (`0`, advanced indexing), Axis 1 uses a slice (`:`, basic slicing), and Axis 2 uses a boolean mask (`window`, advanced indexing). Whenever a basic slice `:` separates two advanced indices, **NumPy moves the advanced-indexed dimensions to the very front of the output array!** Instead of `(channels, time) = (3, 256)`, the masked time axis jumps to the front, yielding `(256, 3)`!

#### The Fix: Two-Step Chained Slicing (`X[...][...]`)
Always apply one mask/index per bracket pair and verify `.shape`:

```python
# ❌ CRASHES: IndexError: shape mismatch (70,) vs (256,)
# X[y == 2, 0, window]

# ✅ CORRECT Two-Step Slicing:
# Step 1: X[y == 2] -> (70, 3, 1152) | Step 2: [:, 0, window] -> (70, 256)
right_c3_win = X[y == 2][:, 0, window]    # Shape: (70, 256)

# ❌ SILENT BUG: Moves masked time axis to the front!
wrong_order = X[0, :, window]             # Shape: (256, 3) -> (time, channels)

# ✅ CORRECT Two-Step Slicing:
# Step 1: X[0] -> (3, 1152) | Step 2: [:, window] -> (3, 256)
correct_order = X[0][:, window]           # Shape: (3, 256) -> (channels, time)
```

---

## 4. Part 2: Visualising Trials with Their Labels

### Example 2a: One Trial, All Channels (Vertical Offset Stacking)
Because all EEG channels oscillate around zero, plotting them directly on top of each other creates an unreadable tangle. We shift each channel `ch` vertically by `-ch * spacing` (negative so `ch = 0` (`C3`) stays at the top of the plot at $y = 0$, `Cz` is at $y = -0.8$, and `C4` is at $y = -1.6$).

```python
trial = 0
spacing = 0.8

fig, ax = plt.subplots(figsize=(16, 5))
for ch in range(n_channels):
    # Subtract ch * spacing to stack channels vertically from top to bottom
    ax.plot(times, X[trial, ch] - ch * spacing, color=class_colors[y[trial]], linewidth=0.8)

# Place Y-axis tick labels at the shifted baselines [0.0, -0.8, -1.6]
ax.set_yticks([-ch * spacing for ch in range(n_channels)])
ax.set_yticklabels(channel_names)
```

### Example 2b: Several Trials, One Channel (Low SNR Intuition)
* **`np.where(y == 1)[0][:3]`:** `np.where(y == 1)` returns a 1-element tuple containing the array of indices where `y == 1`. Indexing `[0]` extracts that 1D index array, and `[:3]` grabs the first 3 `LEFT` trial indices (`[0, 5, 6]`).
* **Biological Intuition:** Looking at 3 single trials of `LEFT` vs. 3 single trials of `RIGHT` on channel `C3`, **you cannot tell the class by eye**. Raw single-trial EEG has a very low **Signal-to-Noise Ratio (SNR)** because ongoing background brain activity is much larger than the task-related signal. This motivates statistical averaging (Part 3) and Machine Learning.

### Example 2c: All Trials as One 2D Image (ERP Image / Raster Plot)
For a single channel (`C3`), slicing across all trials and time points gives a 2D matrix of shape `(trials, time)` = `(140, 1152)`, which can be plotted as a heatmap image where **each horizontal row is one trial**.

```python
ch = channel_names.index('C3')

# 1. Sort trials by class label: all 70 LEFT (1) trials first, then all 70 RIGHT (2) trials.
# kind='stable' preserves the original chronological trial order *within* each class!
order = np.argsort(y, kind='stable')      
data = X[order, ch, :]                    # Shape: (140, 1152)

# 2. Robust Color Scaling (99th Percentile Clipping)
# If we used np.max(np.abs(data)), 1 or 2 extreme artifact spikes would stretch the color scale
# and make the other 138 trials look completely flat/gray. The 99th percentile saturates the top 1%
# of extreme values so normal EEG fluctuations remain clearly visible!
vlim = np.percentile(np.abs(data), 99)

# 3. Create a narrow 1:40 side-by-side subplot: left strip shows class color, right shows EEG heatmap
fig, (ax_lab, ax) = plt.subplots(1, 2, figsize=(17, 7), gridspec_kw={'width_ratios': [1, 40]}, sharey=True)

# Convert sorted labels (140,) into RGB colors (140, 3), then add a dummy width axis -> (140, 1, 3) image
label_rgb = np.array([plt.matplotlib.colors.to_rgb(class_colors[v]) for v in y[order]])[:, None, :]
ax_lab.imshow(label_rgb, aspect='auto', extent=[0, 1, n_trials, 0])

# Plot the (140, 1152) matrix with symmetric diverging colormap (-vlim to +vlim)
img = ax.imshow(data, aspect='auto', cmap='RdBu_r', vmin=-vlim, vmax=vlim,
                extent=[times[0], times[-1], n_trials, 0])
ax.axhline(np.sum(y == 1), color='white', linewidth=2) # Horizontal divider at row 70
```

* **Diagnostic Value:** In this heatmap, a few horizontal rows appear drastically darker red or brighter blue than the rest—these are **candidate outlier/artifact trials** (e.g., muscle clenching, blinking, or electrode movement) that we will clean in Part 4.

---

## 5. Part 3: Statistics Across Trials & Signal-to-Noise Ratio (SNR)

### Example 3a: Mean (ERP), Standard Deviation, and Standard Error
When we slice `left_Cz = X[y == 1, channel_names.index('Cz'), :]`, we get a 2D matrix of shape `(70, 1152)` $\rightarrow$ `(trials, time)`.
Computing statistics along **`axis=0`** collapses the `trials` dimension and computes a separate value **at every single time point** ($1152$ time points):

1. **Mean Across Trials — Event-Related Potential (ERP):**

$$\text{ERP}(t) = \bar{x}(t) = \frac{1}{N} \sum_{i=1}^{N} x_i(t)$$

   * **Why do deflections after the beep ($2\text{ s}$) and arrow ($3\text{ s}$) survive while everything else flattens out?**
     Every single trial $x_i(t) = s(t) + \eta_i(t)$ consists of a **time-locked, phase-locked brain response** $s(t)$ (evoked by the auditory beep at $2\text{ s}$ and visual arrow cue at $3\text{ s}$) plus **random background EEG noise** $\eta_i(t)$ that is *not* phase-locked to the stimulus. When you average across $N=70$ trials, random positive and negative peaks in $\eta_i(t)$ **destructively interfere and cancel out toward $0$**, whereas the time-locked evoked potentials $s(t)$ align at the exact same timestamp in every trial and reinforce!
2. **Sample Standard Deviation Across Trials (`std`):**

$$\sigma(t) = \sqrt{\frac{1}{N - 1} \sum_{i=1}^{N} \left(x_i(t) - \bar{x}(t)\right)^2}$$

   Measures **trial-to-trial variability** (how spread out the 70 individual trials are at time $t$). Computed with `ddof=1` (Bessel's correction, dividing by $N-1$ for an unbiased sample estimate).
3. **Standard Error of the Mean (`SEM`):**

$$\text{SEM}(t) = \frac{\sigma(t)}{\sqrt{N}}$$

   Measures the **uncertainty/precision of the estimated mean curve** $\bar{x}(t)$ itself. As $N \to \infty$, $\text{SEM} \to 0$, even though the trial-to-trial spread $\sigma(t)$ stays constant!

```python
left_Cz = X[y == 1, channel_names.index('Cz'), :]    # Shape: (70, 1152)

erp = left_Cz.mean(axis=0)                           # Shape: (1152,) - collapses 70 trials
std = left_Cz.std(axis=0, ddof=1)                    # Shape: (1152,) - sample std across trials
sem = std / np.sqrt(left_Cz.shape[0])                # Shape: (1152,) - std / sqrt(70)

# Plotting trick: matplotlib's plot(x, Y) expects x to match the FIRST dimension of Y.
# Since times is (1152,) and left_Cz is (70, 1152), we transpose left_Cz.T to (1152, 70)
# so matplotlib draws 70 separate lines (one per column) in a single call!
axes[0].plot(times, left_Cz.T, color='gray', alpha=0.15, linewidth=0.5)
```

---

### Example 3b: The $\frac{1}{\sqrt{N}}$ Noise Reduction Law
If background EEG noise $\eta_i(t)$ during the rest baseline ($0\text{–}2\text{ s}$, where no stimulus response exists) is independent and identically distributed (i.i.d.) across trials with standard deviation $\sigma_{\text{noise}}$, then the variance of the average of $N$ trials is:

$$\text{Var}\left(\frac{1}{N}\sum_{i=1}^{N} \eta_i\right) = \frac{1}{N^2} \cdot N \sigma_{\text{noise}}^2 = \frac{\sigma_{\text{noise}}^2}{N} \implies \sigma_{\text{mean\_noise}} = \frac{\sigma_{\text{noise}}}{\sqrt{N}}$$

* **Intuition:** To cut random background noise in half, you must average **$4\times$ as many trials** ($\sqrt{4} = 2$). Averaging $N=70$ trials reduces baseline noise by a factor of $\sqrt{70} \approx 8.37\times$!

```python
baseline = times < 2                      # Boolean mask for rest period (0 to 2 s)
noise_1 = left_Cz[0, baseline].std()      # Baseline noise level of a single trial (N = 1)

# As N increases from 1 -> 5 -> 20 -> 70, the empirical baseline std (mean_N[baseline].std())
# shrinks closely following the theoretical prediction: noise_1 / np.sqrt(N)
```

---

## 6. Part 4: Z-Scoring & Outlier Trial Rejection

A **z-score** standardizes data into units of standard deviation from the mean:

$$z = \frac{x - \mu}{\sigma}$$

In a 3D EEG tensor `X` of shape `(trials, channels, time)` = `(140, 3, 1152)`, **the `axis` over which you compute $\mu$ and $\sigma$ completely changes the biological meaning of the normalization**, and **`keepdims=True`** is the secret to making NumPy broadcasting work effortlessly.

### Why `keepdims=True` is Essential for 3D Broadcasting
If you compute `X.mean(axis=2)` *without* `keepdims=True`, the shape collapses from `(140, 3, 1152)` to `(140, 3)`. If you then try `X - base_mean`, NumPy fails to broadcast `(140, 3, 1152)` with `(140, 3)` because it aligns dimensions from the right!
With **`keepdims=True`**, collapsed axes are retained as size-`1` singleton dimensions (e.g., `(1, 3, 1)` or `(140, 3, 1)`), allowing NumPy to automatically stretch (broadcast) them across the 3D tensor.

---

### Example 4a: Global Channel Z-Scoring (`axis=(0, 2)`)
* **What it computes:** Pools all $140$ trials (Axis 0) and all $1152$ time points (Axis 2) together for each channel ($140 \times 1152 = 161,280$ samples per channel).
* **Shapes:** `ch_mean` and `ch_std` have shape **`(1, 3, 1)`** (one global mean and std for `C3`, one for `Cz`, one for `C4`).
* **Purpose:** Equalizes overall amplitude scale differences across electrodes (e.g., if `Cz` has higher impedance or gain than `C3`) while preserving relative amplitude differences *between* trials. After this transform, every channel has a global mean of $0.00000$ and a global std of $1.00000$.

```python
# Compute mean & std across trials (axis 0) and time (axis 2) -> Shape: (1, 3, 1)
ch_mean = X.mean(axis=(0, 2), keepdims=True)
ch_std = X.std(axis=(0, 2), keepdims=True)

# Broadcast (140, 3, 1152) with (1, 3, 1) -> Shape: (140, 3, 1152)
X_chz = (X - ch_mean) / ch_std
```

---

### Example 4b: Trial-Specific Baseline Z-Scoring (`axis=2` on Baseline Slice)
* **What it computes:** For *each individual trial* and *each channel*, computes the mean and std **only during that trial's own rest baseline ($0.5\text{ to }2\text{ s}$)** along the time axis (`axis=2`). *(Note: The first $0.5\text{ s}$ is skipped to avoid edge/settling artifacts at the start of the trial).*
* **Shapes:** `base_mean` and `base_std` have shape **`(140, 3, 1)`** ($140 \times 3 = 420$ independent means and stds—one for each trial-channel pair).
* **Purpose:** Measures how much the active motor imagery period ($3\text{–}9\text{ s}$) deviates from **that specific trial's own resting state**, removing slow trial-to-trial drift or fatigue fluctuations across the recording session. After this transform, every trial's baseline window ($0.5\text{–}2\text{ s}$) has a mean of $\approx 0$ and a standard deviation of $1.0$.

```python
base = (times >= 0.5) & (times < 2)                       # 1D boolean mask for 0.5s - 2.0s

# Slice baseline time points (140, 3, 192), then collapse time (axis=2) keeping dims -> (140, 3, 1)
base_mean = X[:, :, base].mean(axis=2, keepdims=True)    
base_std = X[:, :, base].std(axis=2, keepdims=True)

# Broadcast subtract & divide across the full 1152 time points of each trial -> (140, 3, 1152)
X_basez = (X - base_mean) / base_std
```

| Z-Score Method | Slicing & Axis | Shape of `mean` / `std` | What is Being Compared? |
| :--- | :--- | :--- | :--- |
| **4a: Per-Channel Z-Score** | `X` over `axis=(0, 2)` | `(1, 3, 1)` | Any sample vs. the **entire session recording** of that channel |
| **4b: Baseline Z-Score** | `X[:, :, base]` over `axis=2` | `(140, 3, 1)` | Any sample in a trial vs. **that same trial's own rest period** ($0.5\text{–}2\text{ s}$) |

---

### Example 4c: Automated Outlier Trial Rejection
In Example 2c, we saw extreme artifact rows in the heatmap. We can automatically filter out corrupted trials using our channel z-scored tensor `X_chz`:

```python
# 1. np.abs(X_chz) -> Magnitude of z-score at every point: shape (140, 3, 1152)
# 2. .max(axis=(1, 2)) -> Finds the single most extreme z-score peak across ALL channels (axis 1)
#    and ALL time points (axis 2) for each trial -> returns 1D array of shape (140,)
# 3. <= 6 -> 1D boolean mask of length 140: True if the trial NEVER exceeds 6 standard deviations
keep = np.abs(X_chz).max(axis=(1, 2)) <= 6

# Filter the raw tensor and labels using the boolean trial mask
X_clean, y_clean = X[keep], y[keep]
print(f"{len(y_clean)} clean trials: LEFT {np.sum(y_clean == 1)}, RIGHT {np.sum(y_clean == 2)}")
```

> [!info] Practical Machine Learning Trade-offs in Outlier Rejection
> 1. **Threshold Selection Trade-off:** Choosing the cutoff $|z| \le \theta$ balances **data quality** against **sample size**:
>    * Strict threshold ($|z| > 5$ rejected): Removes $25$ trials (leaves only $115$ trials—risks starving your ML classifier of training data).
>    * Moderate threshold ($|z| > 6$ rejected): Removes a balanced number of severe artifact trials while preserving class balance.
>    * Loose threshold ($|z| > 8$ rejected): Removes only $3$ extreme trials (leaves subtle artifacts in the dataset).
> 2. **Masking / Distortion Caveat:** Notice that `ch_mean` and `ch_std` in Example 4a were computed *before* removing the outliers. Because extreme outliers inflate `ch_std`, they slightly shrink the z-scores of all other points (an effect known in statistics as **outlier masking**). In advanced pipelines, one can either use robust statistics (median and interquartile range / MAD) or recompute the z-score normalization on `X_clean` after dropping the corrupted trials.