---
{"dg-publish":true,"permalink":"/3rd-year/image-processing/before-mid/lecture-4-intensity-transformation-and-spatial-filtering-ch3-part2/","dg-note-properties":{}}
---


## 1. Piecewise-Linear Transformation Functions

These are functions that manipulate an image's contrast by mapping input pixel values to output pixel values using linear segments. The main types covered are contrast stretching, intensity-level slicing, and bit-plane slicing.

### Intensity-Level Slicing

- **Purpose**: This technique highlights a specific range of grey levels within an image.
<br>
- **Applications**: It is mostly used for enhancing specific features, such as in satellite imagery or medical X-rays.
    <br>
- **Approach 1 (Binary Masking)**: Displays all values within the range of interest as one value (e.g., pure white or 255) and all other intensities as another value (e.g., pure black or 0).
<br>
- **Approach 2 (Preserving Background)**: Brightens or darkens the desired range of intensities but leaves all other intensity levels in the image entirely unchanged, effectively preserving the original background.
<br>
![Pasted image 20260227235132.png](/img/user/imgs/Pasted%20image%2020260227235132.png)
### Bit-Plane Slicing

- **Concept**: Instead of highlighting intensity ranges, this technique isolates particular bits of the pixel values to highlight interesting aspects of an image's structure.
<br>
- **Structure**: In a standard 256-level grayscale image, each pixel is composed of 8 bits (one byte). The image can be viewed as being composed of eight separate 1-bit planes.
<br>
- **Plane 1 (LSB)**: Contains the lowest-order (least significant) bit of all pixels, which usually represents subtle details and noise.
<br>
- **Plane 8 (MSB)**: Contains the highest-order (most significant) bit, which holds the majority of the visually significant data and structural information.
<br>
- **Applications**: It is highly useful for analyzing the relative importance of each bit (determining quantization adequacy) and for image compression, as reconstructing an image with fewer higher-order planes can still yield a visually acceptable result.
    <br>
- **Example**: A pixel with a decimal value of 100 converts to the binary value $01100100$. The 3rd bit contributes a value of 4, the 6th bit contributes 32, and the 7th bit contributes 64 ($4 + 32 + 64 = 100$).

---

## 2. Histogram Processing

A histogram is a fundamental graph indicating the number of times each gray level occurs, showing the overall distribution of grey levels in an image.

- **Histogram Function**: Defined as $h(r_k) = n_k$.
<br>
- **Variables**: $r_k$ represents the $k$-th grey level, and $n_k$ is the total number of pixels in the image that have that specific grey level.
<br>
- **Normalized Histogram**: Defined as $p(r_k) = n_k / MN$, where $MN$ is the total number of pixels in the image.
<br>
- **Utility**: This information is vital for applications like image compression and image segmentation.

### Four Basic Image Types Based on Histograms

- **Dark image**: The histogram's components are concentrated strictly on the low (dark) side of the gray scale.
<br>
- **Bright image**: The histogram is heavily biased toward the high (light) side of the gray scale.
<br>
- **Low-contrast image**: The histogram is very narrow and centered toward the middle of the gray scale, lacking pure blacks and pure whites.
<br>
- **High-contrast image**: The histogram covers a broad range of the gray scale, with a relatively even distribution where very few vertical lines are significantly higher than the others.


---

## 3. Histogram Equalization

Histogram equalization is a mathematical technique used for adjusting image intensities to automatically enhance overall contrast. It works by mapping each pixel with level $r_k$ in the input image into a corresponding pixel with level $S_k$ in the output image, effectively spreading out the most frequent intensity values.

### The Core Formula

$$S_k = T(r_k) = (L-1)\sum_{j=0}^{k}p_r(r_j)$$

- $L$ is the total number of possible intensity levels (e.g., 8 for a 3-bit image, 256 for an 8-bit image).
- $k = 0, 1, 2, ..., L-1$.
- $p_r(r_j)$ is the probability of occurrence of gray level $r_j$.

### Calculation Example

Suppose we have a 3-bit image ($L=8$) with a total of 4096 pixels ($MN=4096$).

![Pasted image 20260401223128.png](/img/user/imgs/Pasted%20image%2020260401223128.png)

1. **Find Probabilities**: Divide the count of pixels for each level ($n_k$) by the total pixels ($4096$) to get $p_r(r_k)$.
    
    - For $r_0=0$: $790 / 4096 = 0.19$.
    - For $r_1=1$: $1023 / 4096 = 0.25$.
    - For $r_2=2$: $850 / 4096 = 0.21$.

2. **Apply Formula**: Multiply the max intensity level ($L-1$, which is $7$) by the cumulative sum of probabilities up to that level.
    
    - $s_0 = 7 \times (0.19) = 1.33$ (rounds to 1).
    - $s_1 = 7 \times (0.19 + 0.25) = 3.08$ (rounds to 3).
    - $s_2 = 7 \times (0.19 + 0.25 + 0.21) = 4.55$ (rounds to 5).
    
3. **Result**: The original clustered gray levels (0, 1, 2) are now stretched out into new mapping values (1, 3, 5), creating a more uniform distribution and higher contrast.

## Comparison between contrast stretching and histogram equalization

While both contrast stretching and histogram equalization are techniques used to improve the contrast of an image, they achieve this goal using entirely different mathematical approaches.

### 1. Contrast Stretching (Min-Max Stretching)

Contrast stretching is a **linear** (or piecewise-linear) transformation. It works by taking the narrow range of intensity values present in a low-contrast image and pulling them apart to fill the entire available dynamic range (e.g., from $0$ to $255$ in an 8-bit image).

- **How it works:** It identifies the darkest pixel ($r_{min}$) and the brightest pixel ($r_{max}$) in the original image. It then applies a formula to map $r_{min}$ to absolute black ($0$) and $r_{max}$ to absolute white ($255$), scaling all the values in between linearly.
<br>
- **The Result:** The overall "shape" of the histogram remains the same; it is just stretched out horizontally like a rubber band.


### 2. Histogram Equalization

Histogram equalization is a **non-linear** transformation. Instead of just stretching the boundaries, it mathematically redistributes the pixel intensities so that they are as evenly distributed as possible across the entire range.

- **How it works:** It uses the image's cumulative distribution function (CDF). It finds the most frequent intensity values (the tallest peaks in the histogram) and spreads them out, while compressing the less frequent values.
<br>
- **The Result:** The original shape of the histogram is completely changed. The output image will have a roughly "flat" or uniform histogram where all gray levels occur with relatively similar frequency. This often reveals hidden details in very dark or very bright regions but can sometimes produce unnatural-looking results.

| **Feature**             | **Contrast Stretching**                                          | **Histogram Equalization**                                                             |
| ----------------------- | ---------------------------------------------------------------- | -------------------------------------------------------------------------------------- |
| **Transformation Type** | Linear (or Piecewise-Linear)                                     | Non-linear                                                                             |
| **Primary Goal**        | Expands the existing range of pixels to fill the max/min limits. | Flattens the histogram to distribute pixels evenly.                                    |
| **Effect on Histogram** | Keeps the relative shape, but makes it wider.                    | Completely alters the shape, aiming for a uniform distribution.                        |
| **Visual Result**       | Usually looks natural, simply correcting "washed out" images.    | Maximizes contrast heavily, which can sometimes look artificial or amplify noise.      |
| **Complexity**          | Computationally simple (relies on min/max values).               | Computationally more complex (requires calculating probabilities and cumulative sums). |