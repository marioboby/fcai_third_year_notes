---
{"dg-publish":true,"permalink":"/3rd-year/image-processing/sheets/sheet-1-sol/","dg-note-properties":{}}
---


# Q1

![Pasted image 20260404182203.png](/img/user/imgs/Pasted%20image%2020260404182203.png)

The image is a $7 \times 7$ matrix, meaning the total number of pixels is $N \times M = 49$. The intensity values range from 0 to 7, giving us a dynamic range where $L = 8$.

By counting the occurrences of each pixel intensity in the given matrix, we get the base frequency data.

---

### **1.1 Histogram**

The histogram $h(r_k)$ represents the frequency of each pixel intensity $r_k$ across the image.

|**Intensity (rk​)**|**0**|**1**|**2**|**3**|**4**|**5**|**6**|**7**|
|---|---|---|---|---|---|---|---|---|
|**Count ($h(r_k)$)**|1|4|9|9|8|8|3|7|

---

### **1.2 Cumulative Histogram**

The cumulative histogram $H(r_k)$ is calculated by taking the running sum of the histogram frequencies up to each intensity level.

|**Intensity (rk​)**|**0**|**1**|**2**|**3**|**4**|**5**|**6**|**7**|
|---|---|---|---|---|---|---|---|---|
|**Cumulative Count ($H(r_k)$)**|1|5|14|23|31|39|42|49|

---

### **1.3 Histogram Equalization**

To perform histogram equalization, we map the original intensity values $r_k$ to new values $s_k$ using the cumulative probability function. The mapping formula is:

$$s_k = \text{round} \left( \frac{L-1}{N \times M} \times H(r_k) \right)$$

Given $L-1 = 7$ and $N \times M = 49$, our multiplier is $\frac{7}{49} = \frac{1}{7} \approx 0.1428$.

**Mapping Calculations:**

- $s_0 = \text{round}(1 \times \frac{1}{7}) = \text{round}(0.14) = 0$
    
- $s_1 = \text{round}(5 \times \frac{1}{7}) = \text{round}(0.71) = 1$
    
- $s_2 = \text{round}(14 \times \frac{1}{7}) = \text{round}(2.00) = 2$
    
- $s_3 = \text{round}(23 \times \frac{1}{7}) = \text{round}(3.28) = 3$
    
- $s_4 = \text{round}(31 \times \frac{1}{7}) = \text{round}(4.43) = 4$
    
- $s_5 = \text{round}(39 \times \frac{1}{7}) = \text{round}(5.57) = 6$
    
- $s_6 = \text{round}(42 \times \frac{1}{7}) = \text{round}(6.00) = 6$
    
- $s_7 = \text{round}(49 \times \frac{1}{7}) = \text{round}(7.00) = 7$
    

**Equalized Histogram:**

Notice that original intensities 5 and 6 both map to the new intensity 6. Therefore, we add their original frequencies together (8 + 3 = 11).

|**Intensity (sk​)**|**0**|**1**|**2**|**3**|**4**|**5**|**6**|**7**|
|---|---|---|---|---|---|---|---|---|
|**New Count**|1|4|9|9|8|0|11|7|

---

### **1.4 Robust Contrast Stretching (Truncation)**

Truncating allows us to ignore outliers (the extreme dark and bright pixels) when setting our new minimum and maximum bounds for the contrast stretch.

**Step 1: Calculate Truncation Bounds**

- **10% Dark Truncation:** $10\% \text{ of } 49 \text{ pixels} = 4.9 \approx 5 \text{ pixels}$.
    
    Looking at the cumulative histogram, discarding the 5 darkest pixels perfectly removes all pixels with intensity 0 and 1. The new minimum valid intensity is **$r_{min} = 2$**.
    
- **20% Light Truncation:** $20\% \text{ of } 49 \text{ pixels} = 9.8 \approx 10 \text{ pixels}$.
    
    Counting from the brightest end: we have 7 pixels of intensity 7, and 3 pixels of intensity 6. Discarding the top 10 pixels removes all 7s and 6s. The new maximum valid intensity is **$r_{max} = 5$**.
    

**Step 2: Contrast Stretching Formula**

We map the valid range $[2, 5]$ to the full dynamic range $[0, 7]$. Any values below $r_{min}$ clip to 0, and any values above $r_{max}$ clip to 7.

$$I_{new} = (I - Min)\frac{NewMax - NewMin}{Max - Min} + NewMin$$
$$I_{new} = (I - 2) \times \frac{7 - 0}{5 - 2} + 0$$

$$I_{new} = (I - 2) \times \frac{7}{3}$$
**Mapping Calculations:**

- **$r_k < 2$ (Intensities 0, 1):** Clipped to **0** since both will result in negatives
    
- **$r_k = 2$:** $\text{round}( \frac{0}{3} \times 7 ) =$ **0**
    
- **$r_k = 3$:** $\text{round}( \frac{1}{3} \times 7 ) = \text{round}(2.33) =$ **2**
    
- **$r_k = 4$:** $\text{round}( \frac{2}{3} \times 7 ) = \text{round}(4.67) =$ **5**
    
- **$r_k = 5$:** $\text{round}( \frac{3}{3} \times 7 ) = \text{round}(7.00) =$ **7**
    
- **$r_k > 5$ (Intensities 6, 7):** Clipped to **7**
    

**Final Stretched Histogram:**

We compile the frequencies of the original pixels based on their newly mapped destinations.

- Mapped to 0: Old intensities 0, 1, 2 (1 + 4 + 9 = 14)
    
- Mapped to 2: Old intensity 3 (9)
    
- Mapped to 5: Old intensity 4 (8)
    
- Mapped to 7: Old intensities 5, 6, 7 (8 + 3 + 7 = 18)
    

|**Intensity (sk​)**|**0**|**1**|**2**|**3**|**4**|**5**|**6**|**7**|
|---|---|---|---|---|---|---|---|---|
|**New Count**|14|0|9|0|0|8|0|18|

---

# Q2

![Pasted image 20260404185418.png](/img/user/imgs/Pasted%20image%2020260404185418.png)

[[3rd Year/Image Processing/Before Mid/Lecture 4 - Intensity Transformation and Spatial Filtering - Ch3 - Part2#2. Histogram Equalization\|When a question asks to modify a histogram into a "uniformly distributed" one across a specific interval, it is asking you to perform **Histogram Equalization**.]]



- Total number of pixels: $N \times M = 25$
    
- Target interval $[0, 7]$ means our dynamic range $L = 8$, so $L-1 = 7$.
    
- Our equalization multiplier will be $\frac{L-1}{N \times M} = \frac{7}{25} = 0.28$.
    

### **Step 1: Calculate Frequencies and Cumulative Frequencies**

By counting the occurrences of each intensity ($r_k$) in the matrix, we build our standard histogram $h(r_k)$ and cumulative histogram $H(r_k)$.

| **Intensity (rk​)**       | **0** | **1** | **2** | **3** | **4** | **5** | **6** | **7** |
| ------------------------- | ----- | ----- | ----- | ----- | ----- | ----- | ----- | ----- |
| **Count ($h(r_k)$)**      | 5     | 4     | 4     | 4     | 2     | 2     | 4     | 0     |
| **Cumulative ($H(r_k)$)** | 5     | 9     | 13    | 17    | 19    | 21    | 25    | 25    |

---

### **Step 2: Apply the Equalization Formula**

Now we map the original intensities ($r_k$) to their new uniform intensities ($s_k$) using the formula:

$$s_k = \text{round} \left( 0.28 \times H(r_k) \right)$$

- $s_0 = \text{round}(0.28 \times 5) = \text{round}(1.40) = \mathbf{1}$
    
- $s_1 = \text{round}(0.28 \times 9) = \text{round}(2.52) = \mathbf{3}$
    
- $s_2 = \text{round}(0.28 \times 13) = \text{round}(3.64) = \mathbf{4}$
    
- $s_3 = \text{round}(0.28 \times 17) = \text{round}(4.76) = \mathbf{5}$
    
- $s_4 = \text{round}(0.28 \times 19) = \text{round}(5.32) = \mathbf{5}$
    
- $s_5 = \text{round}(0.28 \times 21) = \text{round}(5.88) = \mathbf{6}$
    
- $s_6 = \text{round}(0.28 \times 25) = \text{round}(7.00) = \mathbf{7}$
    
- $s_7 = \text{round}(0.28 \times 25) = \text{round}(7.00) = \mathbf{7}$
    

---

### **Step 3: The Modified (Equalized) Histogram**

To get the final uniformly distributed histogram, we assign the original pixel counts to their newly calculated intensity levels. Notice that both original intensities **3** and **4** are mapped to the new intensity **5**, so their counts are added together ($4 + 2 = 6$).

|**New Intensity (sk​)**|**0**|**1**|**2**|**3**|**4**|**5**|**6**|**7**|
|---|---|---|---|---|---|---|---|---|
|**New Count**|0|5|0|4|4|6|2|4|

---

# Q3

![Pasted image 20260404190802.png](/img/user/imgs/Pasted%20image%2020260404190802.png)

### **1. First Marked Pixel: (2) at position row 0, column 1**

Because this pixel is on the top edge of the image, a $3 \times 3$ window centered on it will go out of bounds. The most standard approach in image processing is to assume **zero-padding** for the out-of-bounds pixels.

**The $3 \times 3$ neighborhood (with zero-padding on top):**

$$\begin{bmatrix} 0 & 0 & 0 \\ 1 & 2 & 2 \\ 2 & 2 & 2 \end{bmatrix}$$

The values in this window are: $0, 0, 0, 1, 2, 2, 2, 2, 2$

- **a) Standard Average filter:** Sum the values and divide by 9.
    
    - $\frac{0 + 0 + 0 + 1 + 2 + 2 + 2 + 2 + 2}{9} = \frac{11}{9} \approx 1.22$
    
    - _(Note: If your class uses integer rounding, this becomes **1**. If your class ignores out-of-bounds pixels entirely instead of zero-padding, the average would be $\frac{11}{6} \approx 1.83$)._
    
- **b) Median filter:** Sort the values from lowest to highest and pick the middle (5th) value.
    
    - Sorted: $0, 0, 0, 1, \mathbf{2}, 2, 2, 2, 2$
    
    - The median is **2**.
    
- **c) Min filter:** Select the smallest value in the neighborhood.
    
    - The minimum is **0**. _(Or 1, if ignoring padded pixels)._
    

---

### **2. Second Marked Pixel: (7) at position row 2, column 2**

This is an inner pixel, so it has a complete set of 8 surrounding neighbors within the image boundaries. No padding is needed.

**The $3 \times 3$ neighborhood:**

$$\begin{bmatrix} 2 & 2 & 2 \\ 1 & 7 & 3 \\ 1 & 2 & 1 \end{bmatrix}$$

The values in this window are: $2, 2, 2, 1, 7, 3, 1, 2, 1$

- **a) Standard Average filter:** Sum the values and divide by 9.
    
    - $\frac{2 + 2 + 2 + 1 + 7 + 3 + 1 + 2 + 1}{9} = \frac{21}{9} = 2.33$
    
    - _(Note: Rounded to the nearest integer, this is **2**)._
    
- **b) Median filter:** Sort the 9 values from lowest to highest and pick the middle (5th) value.
    
    - Sorted: $1, 1, 1, 2, \mathbf{2}, 2, 2, 3, 7$
    
    - The median is **2**.
    
- **c) Min filter:** Select the smallest value in the neighborhood.
    
    - The minimum is **1**.
      
---

# Q4

![Pasted image 20260404191511.png](/img/user/imgs/Pasted%20image%2020260404191511.png)

First, let's extract the $3 \times 3$ neighborhood matrix centered around each pixel.

- **Neighborhood for Pixel (5):**
    
    $$\begin{bmatrix} 2 & 6 & 6 \\ 1 & \mathbf{5} & 7 \\ 2 & 6 & 6 \end{bmatrix}$$
    
- **Neighborhood for Pixel (7):**
    
    $$\begin{bmatrix} 6 & 6 & 7 \\ 5 & \mathbf{7} & 5 \\ 6 & 6 & 6 \end{bmatrix}$$
    

---

### **a) Sobel Operator**

The Sobel operator calculates the gradient of the image using two kernels: one for horizontal changes ($G_x$) and one for vertical changes ($G_y$).

$$G_y = \begin{bmatrix} -1 & 0 & 1 \\ -2 & 0 & 2 \\ -1 & 0 & 1 \end{bmatrix} \quad \text{and} \quad G_x = \begin{bmatrix} -1 & -2 & -1 \\ 0 & 0 & 0 \\ 1 & 2 & 1 \end{bmatrix}$$

The total magnitude is typically calculated as $G = \sqrt{G_y^2 + G_x^2}$ (or approximated by $|G_y| + |G_x|$).

**1. Applying to Pixel (5):**

- **$G_y$:** $(-1 \times 2) + (1 \times 6) + (-2 \times 1) + (2 \times 7) + (-1 \times 2) + (1 \times 6)$
    
    $G_y = -2 + 6 - 2 + 14 - 2 + 6 = \mathbf{20}$
    
- **$G_x$:** $(-1 \times 2) + (-2 \times 6) + (-1 \times 6) + (1 \times 2) + (2 \times 6) + (1 \times 6)$
    
    $G_x = -2 - 12 - 6 + 2 + 12 + 6 = \mathbf{0}$
    
- **Magnitude:** $\sqrt{20^2 + 0^2} =$ **20**
    

**2. Applying to Pixel (7):**

- **$G_y$:** $(-1 \times 6) + (1 \times 7) + (-2 \times 5) + (2 \times 5) + (-1 \times 6) + (1 \times 6)$
    
    $G_y = -6 + 7 - 10 + 10 - 6 + 6 = \mathbf{1}$
    
- **$G_x$:** $(-1 \times 6) + (-2 \times 6) + (-1 \times 7) + (1 \times 6) + (2 \times 6) + (1 \times 6)$
    
    $G_x = -6 - 12 - 7 + 6 + 12 + 6 = \mathbf{-1}$
    
- **Magnitude:** $\sqrt{1^2 + (-1)^2} = \sqrt{2} \approx$ **1.41**
    

---

### **b) Laplacian Operator**

We'll calculate using the composite 4-neighbour $3 \times 3$ kernel:

$$\text{Composite Laplacian Kernel} = \begin{bmatrix} 0 & -1 & 0 \\ -1 & \mathbf{5} & -1 \\ 0 & -1 & 0 \end{bmatrix}$$

---

### **1. Applying to Pixel (5):**

For the pixel at row 1, column 1, we look at its immediate top, bottom, left, and right neighbors.

- **Top neighbor:** 6
    
- **Bottom neighbor:** 6
    
- **Left neighbor:** 1
    
- **Right neighbor:** 7
    
- **Center pixel:** 5
    

**Calculation:**

- Sum of the 4 neighbors: $6 + 6 + 1 + 7 = 20$
    
- Multiply neighbors by $-1$: $-20$
    
- Center multiplied by $5$: $5 \times 5 = 25$
    
- **True Output:** $25 - 20 =$ **5**

---

### **2. Applying to Pixel (7):**

For the pixel at row 1, column 2, we again look at its immediate 4-connected neighbors.

- **Top neighbor:** 6
    
- **Bottom neighbor:** 6
    
- **Left neighbor:** 5 (this is the pixel we just calculated in the step above)
    
- **Right neighbor:** 5
    
- **Center pixel:** 7
    

**Calculation:**

Sum of the 4 neighbors: $6 + 6 + 5 + 5 = 22$

Negate the sum: $-22$

Center multiplied by 5: $7 \times (5) = 35$

**Output:** $35 - 22 =$ **13**