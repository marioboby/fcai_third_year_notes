---
{"dg-publish":true,"permalink":"/3rd-year/image-processing/expanded-explanations/lecture-8/","dg-note-properties":{}}
---

## Applying noise using noise models to images

In digital image processing, degradation is typically modeled as an additive process:

$$g(x,y) = f(x,y) + \eta(x,y)$$

Where $f(x,y)$ is the original clean image, $\eta(x,y)$ is the noise component generated from a specific Probability Density Function (PDF), and $g(x,y)$ is the resulting corrupted image.

Let's look at a simple $3 \times 3$ patch of an image. Assume this patch is a solid, flat mid-gray area where every pixel has an intensity value of 100 (on a standard 0–255 8-bit scale).

$$f(x,y) = \begin{bmatrix} 100 & 100 & 100 \\ 100 & 100 & 100 \\ 100 & 100 & 100 \end{bmatrix}$$

Here is how two different noise models would numerically attack this matrix:

### **Example 1: Adding Impulse (Salt-and-Pepper) Noise**

Salt-and-Pepper noise doesn't add a continuous value; it acts as a probability-based replacement.

**1. Define the PDF Rules:**

- Probability of Salt ($P_s$): **10%** (0.1). The pixel becomes 255 (pure white).
    
- Probability of Pepper ($P_p$): **20%** (0.2). The pixel becomes 0 (pure black).
    
- Probability of remaining unchanged: **70%** (0.7). The pixel stays 100.
    

**2. The Random Process:**

The computer evaluates every single pixel and generates a random probability number between 0.00 and 1.00 for each one. Let's assume the computer rolls the following random numbers for our 9 pixels:

$$\text{Random Rolls} = \begin{bmatrix} 0.45 & \mathbf{0.05} & 0.88 \\ \mathbf{0.12} & 0.76 & \mathbf{0.25} \\ 0.91 & 0.55 & \mathbf{0.08} \end{bmatrix}$$

**3. Applying the Thresholds:**

- If roll < 0.10 $\rightarrow$ Pixel becomes **255** (Salt).
    
- If 0.10 $\le$ roll < 0.30 $\rightarrow$ Pixel becomes **0** (Pepper).
    
- If roll $\ge$ 0.30 $\rightarrow$ Pixel stays **100**.
    

**4. The Final Corrupted Matrix, $g(x,y)$:**

$$g(x,y) = \begin{bmatrix} 100 & \mathbf{255} & 100 \\ \mathbf{0} & 100 & \mathbf{0} \\ 100 & 100 & \mathbf{255} \end{bmatrix}$$

_Notice how the noise completely obliterated the original data in the affected pixels, creating extreme black and white spikes._

---

### **Example 2: Adding Gaussian Noise**

Gaussian noise is strictly additive. It generates random numerical offsets based on a bell curve. Most offsets will be small (near the mean), but a few will be large.

**1. Define the PDF Rules:**

- **Mean ($\bar{z}$): 0**. (The noise is centered around zero, meaning it will brighten and darken pixels equally).
    
- **Standard Deviation ($\sigma$): 15**. (Controls how wide the bell curve is. A higher $\sigma$ means more severe noise).
    

**2. The Random Process:**

The computer samples 9 values from a Gaussian distribution with $\bar{z}=0$ and $\sigma=15$. Here are the generated noise values, $\eta(x,y)$:

$$\eta(x,y) = \begin{bmatrix} +12 & -5 & +3 \\ -22 & +31 & -8 \\ +1 & -15 & +6 \end{bmatrix}$$

**3. The Additive Math ($g = f + \eta$):**

We simply add the noise matrix to the original image matrix. _(Note: If a value drops below 0 or exceeds 255, we clip it)._

$$g(x,y) = \begin{bmatrix} 100 & 100 & 100 \\ 100 & 100 & 100 \\ 100 & 100 & 100 \end{bmatrix} + \begin{bmatrix} +12 & -5 & +3 \\ -22 & +31 & -8 \\ +1 & -15 & +6 \end{bmatrix}$$

**4. The Final Corrupted Matrix, $g(x,y)$:**

$$g(x,y) = \begin{bmatrix} 112 & 95 & 103 \\ 78 & 131 & 92 \\ 101 & 85 & 106 \end{bmatrix}$$

_Notice how unlike Salt-and-Pepper, Gaussian noise affects absolutely every pixel, but the original 100 intensity is still "hidden" within the noisy values._

---
