---
{"dg-publish":true,"permalink":"/3rd-year/image-processing/sheets/segmentation-sheet-sol/","dg-note-properties":{}}
---

# Q1

Use histogram thresholding method to detect a suitable threshold to segment the following image into foreground region and background region (Assuming σ1= σ2 for background and foreground region, Probability of background region=0.5) 

2 3 2 5 6 
1 2 3 4 5 
6 7 3 2 7 
0 0 3 2 2

## Sol

To find the suitable threshold to segment the image, we can use optimal statistical thresholding.

Given the assumptions:

1. **Equal variances** ($\sigma_1 = \sigma_2$)
    
2. **Equal prior probabilities** ($P(\text{background}) = 0.5$, which means $P(\text{foreground}) = 0.5$)
    

Under these conditions, the optimal threshold $T$ according to Bayes' decision rule is the exact average of the two class means:

$$T = \frac{\mu_1 + \mu_2}{2}$$

### **Step 1: Partition the Pixels**

We have a total of 20 pixels. Because the probability of the background region is given as **$0.5$** (50%), exactly 10 pixels belong to the background class and 10 pixels belong to the foreground class.

By sorting the pixels by intensity, we can split them perfectly in half:

- **Background Pixels (Lowest 10):** 0, 0, 1, 2, 2, 2, 2, 2, 2, 3
    
- **Foreground Pixels (Highest 10):** 3, 3, 3, 4, 5, 5, 6, 6, 7, 7
    

### **Step 2: Calculate the Exact Means**

Now, we calculate the average intensity for both the background ($\mu_1$) and the foreground ($\mu_2$).

- **Background Mean ($\mu_1$):**
    
    $$Sum = (0 \times 2) + (1 \times 1) + (2 \times 6) + (3 \times 1) = 0 + 1 + 12 + 3 = 16$$
    
    $$\mu_1 = \frac{16}{10} = 1.6$$
    
- **Foreground Mean ($\mu_2$):**
    
    $$Sum = (3 \times 3) + (4 \times 1) + (5 \times 2) + (6 \times 2) + (7 \times 2) = 9 + 4 + 10 + 12 + 14 = 49$$
    
    $$\mu_2 = \frac{49}{10} = 4.9$$
    

### **Step 3: Round the Means**

Following your instruction to round $\mu_1$ and $\mu_2$ to the nearest whole numbers:

- $\mu_1 = 1.6 \rightarrow$ **$2$**
    
- $\mu_2 = 4.9 \rightarrow$ **$5$**
    

### **Step 4: Calculate the Optimal Threshold**

For optimal statistical thresholding where the variances are equal ($\sigma_1 = \sigma_2$) and the probabilities are equal ($P=0.5$), the threshold $T$ falls exactly halfway between the two means:

$$T = \frac{\mu_1 + \mu_2}{2}$$

$$T = \frac{2 + 5}{2} = \frac{7}{2}$$

$$T = 3.5 \approx 4$$

Depending on whether your segmentation rule is strictly less than/greater than, you would use **$T = 4$** to separate the regions (where intensities $\le 4$ are background, and $\gt 4$ are foreground).
  
---

# Q2

For the following image:- 
7 7 1 1 2 
7 8 0 1 2 
6 4 2 3 2 
5 6 3 2 3 

Compute the gradient magnitudes and directions using the Sobel masks. Use the obtained results and suitable threshold values to obtain the edges.

## Sol
### 1. Horizontal and Vertical Gradients ($G_x$ and $G_y$)

Using the standard Sobel masks ($G_x$ for horizontal gradients/vertical edges, $G_y$ for vertical gradients/horizontal edges), we compute the values for every pixel, treating any out-of-bounds pixel as $0$.

**Vertical Gradient ($G_y$):**

$$G_y = \begin{bmatrix} 22 & -19 & -19 & 4 & -3 \\ 27 & -24 & -21 & 5 & -6 \\ 22 & -17 & -13 & 2 & -9 \\ 16 & -8 & -9 & 0 & -7 \end{bmatrix}$$

**Horizontal Gradient ($G_y$):**

$$G_x = \begin{bmatrix} 22 & 23 & 9 & 4 & 5 \\ -5 & -6 & 1 & 5 & 2 \\ -6 & -3 & 5 & 6 & 3 \\ -16 & -16 & -11 & -10 & -7 \end{bmatrix}$$

### 2. Gradient Magnitudes and Directions

The magnitude is calculated as $M = \sqrt{G_x^2 + G_y^2}$ and the direction as $\theta = \arctan(G_y / G_x)$.

**Gradient Magnitudes ($M$):**

_(Rounded to one decimal place)_

$$M = \begin{bmatrix} 31.1 & 29.8 & 21.0 & 5.7 & 5.8 \\ 27.5 & 24.7 & 21.0 & 7.1 & 6.3 \\ 22.8 & 17.3 & 13.9 & 6.3 & 9.5 \\ 22.6 & 17.9 & 14.2 & 10.0 & 9.9 \end{bmatrix}$$

**Gradient Directions ($\theta$):**

_(Calculated in degrees, accounting for quadrants)_

$$\theta = \begin{bmatrix} 45.0^\circ & 129.5^\circ & 154.6^\circ & 45.0^\circ & 121.0^\circ \\ -10.5^\circ & -166.0^\circ & 177.3^\circ & 45.0^\circ & 161.6^\circ \\ -15.3^\circ & -170.0^\circ & 158.9^\circ & 71.6^\circ & 161.6^\circ \\ -45.0^\circ & -116.6^\circ & -129.3^\circ & -90.0^\circ & -135.0^\circ \end{bmatrix}$$

### 3. Edge Detection using Thresholding

Looking at the entire padded magnitude matrix, there is a clear division between the higher magnitudes on the left and the lower magnitudes on the right. The lower group peaks at $10.0$, and the higher group starts at $13.9$.

A suitable threshold to separate the edges from the background is **$T = 12$**.

- If $M(y,x) \ge 12$, it is an edge (1).
    
- If $M(y,x) < 12$, it is background (0).
    

**Final Binary Edge Map:**

$$Edges = \begin{bmatrix} 1 & 1 & 1 & 0 & 0 \\ 1 & 1 & 1 & 0 & 0 \\ 1 & 1 & 1 & 0 & 0 \\ 1 & 1 & 1 & 0 & 0 \end{bmatrix}$$

_(Note: The padding artificially increases the gradient magnitude along the top and left borders because the algorithm detects the sharp contrast between the 0s in the padding and the high intensity values of 6, 7, and 8 on the edge of the image)._

---

# Q3

Apply Laplcian mask on the following image, 

2 3 1 5 
6 0 8 2 
5 6 7 1

### **Step 1: Apply the Laplacian Mask**

The provided Laplacian mask is a $3 \times 3$ filter. To apply it to the $3 \times 4$ image, we use convolution. Assuming standard zero-padding for the original image boundaries, the formula for this specific mask is:

$$R(y,x) = 8 \cdot I(y,x) - \sum \text{8-neighbors}(I(y,x))$$

Let's calculate the result for each pixel:

**Row 1:**

- **$R(1,1)$**: $8(2) - (0+0+0 + 0+3 + 0+6+0) = 16 - 9 =$ **$7$**
    
- **$R(1,2)$**: $8(3) - (0+0+0 + 2+1 + 6+0+8) = 24 - 17 =$ **$7$**
    
- **$R(1,3)$**: $8(1) - (0+0+0 + 3+5 + 0+8+2) = 8 - 18 =$ **$-10$**
    
- **$R(1,4)$**: $8(5) - (0+0+0 + 1+0 + 8+2+0) = 40 - 11 =$ **$29$**
    

**Row 2:**

- **$R(2,1)$**: $8(6) - (0+2+3 + 0+0 + 0+5+6) = 48 - 16 =$ **$32$**
    
- **$R(2,2)$**: $8(0) - (2+3+1 + 6+8 + 5+6+7) = 0 - 38 =$ **$-38$**
    
- **$R(2,3)$**: $8(8) - (3+1+5 + 0+2 + 6+7+1) = 64 - 25 =$ **$39$**
    
- **$R(2,4)$**: $8(2) - (1+5+0 + 8+0 + 7+1+0) = 16 - 22 =$ **$-6$**
    

**Row 3:**

- **$R(3,1)$**: $8(5) - (0+6+0 + 0+6 + 0+0+0) = 40 - 12 =$ **$28$**
    
- **$R(3,2)$**: $8(6) - (6+0+8 + 5+7 + 0+0+0) = 48 - 26 =$ **$22$**
    
- **$R(3,3)$**: $8(7) - (0+8+2 + 6+1 + 0+0+0) = 56 - 17 =$ **$39$**
    
- **$R(3,4)$**: $8(1) - (8+2+0 + 7+0 + 0+0+0) = 8 - 17 =$ **$-9$**
    

**Resulting Matrix after Laplacian Filter ($R$):**

$$\begin{bmatrix} 7 & 7 & -10 & 29 \\ 32 & -38 & 39 & -6 \\ 28 & 22 & 39 & -9 \end{bmatrix}$$

### **Step 2: Apply Zero-Crossing**

A zero-crossing occurs wherever there is a sign change between adjacent pixels. A standard way to detect this is to check if a pixel has an opposite sign to any of its neighbors (often checking horizontal, vertical, and diagonal neighbors).

We'll **"count zero padding as positive"**. This means when we check the boundary pixels of our result matrix $R$, we treat the imaginary padded pixels surrounding the matrix as positive ($+$).

Let's look at the **Signs Matrix** of our result $R$:

$$\begin{bmatrix} + & + & - & + \\ + & - & + & - \\ + & + & + & - \end{bmatrix}$$

Now, we evaluate each pixel to see if it borders a pixel (or the positive padding) of the opposite sign:

- **$R(1,1)$** $[+]$: Borders $R(2,2)$ $[-]$. **(Edge)**
    
- **$R(1,2)$** $[+]$: Borders $R(1,3)$ $[-]$ and $R(2,2)$ $[-]$. **(Edge)**
    
- **$R(1,3)$** $[-]$: Borders multiple $[+]$ pixels and the $[+]$ padding. **(Edge)**
    
- **$R(1,4)$** $[+]$: Borders $R(1,3)$ $[-]$ and $R(2,4)$ $[-]$. **(Edge)**
    
- **$R(2,1)$** $[+]$: Borders $R(2,2)$ $[-]$. **(Edge)**
    
- **$R(2,2)$** $[-]$: Borders multiple $[+]$ pixels. **(Edge)**
    
- **$R(2,3)$** $[+]$: Borders $R(1,3)$ $[-]$, $R(2,2)$ $[-]$, $R(2,4)$ $[-]$, and $R(3,4)$ $[-]$. **(Edge)**
    
- **$R(2,4)$** $[-]$: Borders multiple $[+]$ pixels and the $[+]$ padding. **(Edge)**
    
- **$R(3,1)$** $[+]$: Borders $R(2,2)$ $[-]$. **(Edge)**
    
- **$R(3,2)$** $[+]$: Borders $R(2,2)$ $[-]$. **(Edge)**
    
- **$R(3,3)$** $[+]$: Borders $R(2,4)$ $[-]$ and $R(3,4)$ $[-]$. **(Edge)**
    
- **$R(3,4)$** $[-]$: Borders multiple $[+]$ pixels and the $[+]$ padding. **(Edge)**
    

**Final Binary Zero-Crossing Map:**

Because the high-pass Laplacian filter created intense frequency oscillation on this specific image, **every single pixel** experiences a sign change with at least one of its neighbors. Therefore, the final binary edge map marks all pixels as edges ($1$):

$$\begin{bmatrix} 1 & 1 & 1 & 1 \\ 1 & 1 & 1 & 1 \\ 1 & 1 & 1 & 1 \end{bmatrix}$$

---

# Q4

Compute the gradient magnitude and angle of the following image using Sobel masks. Apply the local edge criteria (2 neighbor points belongs to the same edge if they have small gradient magnitude and angle differences) to find all the edges starting from the point with highest gradient magnitude. Use a magnitude difference threshold of 20% of the maximum magnitude difference and angle difference threshold of 10% of the maximum angel value. 

1 3 3 3 
1 5 6 7 
2 7 6 8 
4 5 5 5

## Sol

we will assume standard **zero-padding** around the boundaries of the image.

The standard Sobel masks used are:

$$G_y = \begin{bmatrix} -1 & 0 & 1 \\ -2 & 0 & 2 \\ -1 & 0 & 1 \end{bmatrix} \quad \text{and} \quad G_x = \begin{bmatrix} -1 & -2 & -1 \\ 0 & 0 & 0 \\ 1 & 2 & 1 \end{bmatrix}$$

_(Note: $G_y$ computes right-minus-left, and $G_x$ computes bottom-minus-top)._

### **Step 1: Compute $G_x$ and $G_y$**

Applying the convolution to every pixel in the $4 \times 4$ grid (treating out-of-bound pixels as 0), we get the following gradient matrices:

**Vertical Gradient ($G_y$):**

$$G_y = \begin{bmatrix} 11 & 9 & 2 & -12 \\ 20 & 16 & 5 & -21 \\ 24 & 14 & 4 & -23 \\ 17 & 6 & 1 & -16 \end{bmatrix}$$

**Horizontal Gradient ($G_x$):**

$$G_x = \begin{bmatrix} 7 & 17 & 24 & 20 \\ 6 & 12 & 15 & 13 \\ 6 & 2 & -4 & -5 \\ -11 & -22 & -27 & -22 \end{bmatrix}$$

### **Step 2: Compute Gradient Magnitude and Angle**

- **Magnitude ($M$):** Calculated using $M = \sqrt{G_x^2 + G_y^2}$
    
- **Angle ($\theta$):** Calculated using $\theta = \arctan(G_y / G_x)$ mapped to $[0^\circ, 360^\circ)$
    

**Magnitude Matrix ($M$):**

_(Rounded to two decimal places)_

$$M = \begin{bmatrix} 13.04 & 19.24 & 24.08 & 23.32 \\ 20.88 & 20.00 & 15.81 & 24.70 \\ 24.74 & 14.14 & 5.66 & 23.54 \\ 20.25 & 22.80 & 27.02 & 27.20 \end{bmatrix}$$

**Angle Matrix ($\theta$):**

_(Rounded to one decimal place)_

$$\theta = \begin{bmatrix} 32.5^\circ & 62.1^\circ & 85.2^\circ & 121.0^\circ \\ 16.7^\circ & 36.9^\circ & 71.6^\circ & 148.2^\circ \\ 14.0^\circ & 8.1^\circ & 315.0^\circ & 192.3^\circ \\ 327.1^\circ & 285.3^\circ & 272.1^\circ & 234.0^\circ \end{bmatrix}$$

### **Step 3: Define the Thresholds**

1. **Magnitude Difference Threshold ($T_M$):** 20% of the maximum magnitude difference.
    
    - Maximum magnitude in the image: $27.20$ (at row 4, col 4)
        
    - Minimum magnitude in the image: $5.66$ (at row 3, col 3)
        
    - Maximum Difference = $27.20 - 5.66 = 21.54$
        
    - $T_M = 0.2 \times 21.54 =$ **$4.31$**
        
2. **Angle Difference Threshold ($T_A$):** 10% of the maximum angle value.
    
    - The theoretical maximum angle value in a full $360^\circ$ circle is $360^\circ$.
        
    - $T_A = 0.1 \times 360^\circ =$ **$36^\circ$**
        

### **Step 4: Apply Local Edge Linking**

We start at the point with the highest gradient magnitude, which is the bottom-right pixel **(row 4, col 4)**.

- **Current Edge Pixel:** $M = 27.20$, $\theta = 234.0^\circ$
    
- **Valid Neighbors to Check:** (4,3), (3,4), and (3,3).
    

For a neighbor to be linked to this edge, it must satisfy:

1. $|M_{neighbor} - 27.20| \le 4.31$
    
2. $|\theta_{neighbor} - 234.0^\circ| \lt 36^\circ$
    

**Checking Neighbors:**

- **Check (4,3):** $M = 27.02$, $\theta = 272.1^\circ$
    
    - Magnitude diff: $|27.20 - 27.02| = 0.18 \le 4.31$ **(Pass)**
        
    - Angle diff: $|234.0^\circ - 272.1^\circ| = 38.1^\circ \not\le 36^\circ$ **(Fail)**
        
- **Check (3,4):** $M = 23.54$, $\theta = 192.3^\circ$
    
    - Magnitude diff: $|27.20 - 23.54| = 3.66 \le 4.31$ **(Pass)**
        
    - Angle diff: $|234.0^\circ - 192.3^\circ| = 41.7^\circ \not\le 36^\circ$ **(Fail)**
        
- **Check (3,3):** $M = 5.66$, $\theta = 315.0^\circ$
    
    - Magnitude diff: $|27.20 - 5.66| = 21.54 \not\le 4.31$ **(Fail)**
        

### **Conclusion**

Although the neighboring pixels at (4,3) and (3,4) have very similar gradient magnitudes, their angle directions differ by slightly more than the strict 10% tolerance ($36^\circ$). Therefore, based exactly on the provided mathematical criteria, **no neighboring pixels are linked**, and the edge trace stops immediately at the starting point (4,4).

---

# Q5

For the following image:- 

0 0 1 1 0 
0 0 0 1 0 
0 0 0 1 0 
0 0 0 0 1 

Compute the linear Hough transform of the result edge image using four quantization levels for the angles from 0 to π and five uniform quantization levels of the radial distance from 0 to the maximum distance , Use a threshold of 3 to detect strong lines in the image

look [[3rd Year/Image Processing/After Mid/Lecture 10 - Image Segmentation - Ch10 - Part2#The Accumulator Algorithm\|chapter 10.2]], this is the exact example from slides.

---

# Q6

Use region growing algorithm to find the region starting with two seed point (2,2) in the first cluster using a threshold of 2, and the other seed point is (1,6) using a threshold of 1 in the following image 

8 8 8 9 9 8 9 8 
8 2 3 8 7 6 (5) 9 
8 1 (1) 2 6 6 5 9 
8 2 2 3 2 7 6 9 
8 8 9 8 8 9 8 8

## Sol

To solve this using the Region Growing algorithm, we first need to establish the rules and coordinate system based on the image.

**Assumptions & Setup:**

1. **Coordinate System:** Based on the locations of the highlighted seeds `(1)` and `(5)`, the matrix uses **0-based indexing** `(row, col)`.
    
    - Seed 1: `(2,2)` perfectly lands on the value `1`.
        
    - Seed 2: `(1,6)` perfectly lands on the value `5`.
        
2. **Growing Criterion:** A neighboring pixel is added to a region if the absolute difference between its intensity and the _initial seed intensity_ is less than or equal to the given threshold $T$:
    
    $$|I(row, col) - I(seed)| \le T$$
    
3. **Connectivity:** We will use standard **8-connectivity** (horizontal, vertical, and diagonal neighbors).
    

### **Step 1: Grow Cluster 1**

- **Seed Point:** `(2,2)`
    
- **Seed Value:** `1`
    
- **Threshold:** `2`
    
- **Valid Intensity Range:** $[1 - 2, 1 + 2] = \textbf{[0, 3]}$
    

We start at `(2,2)` and continuously check its 8-neighbors to see if their values are between 0 and 3.

- **Initial Region:** `(2,2)` -> value `1`
    
- **Check neighbors of `(2,2)`:** * `(1,1)`=2 $\rightarrow$ Add
    
    - `(1,2)`=3 $\rightarrow$ Add
        
    - `(2,1)`=1 $\rightarrow$ Add
        
    - `(2,3)`=2 $\rightarrow$ Add
        
    - `(3,1)`=2 $\rightarrow$ Add
        
    - `(3,2)`=2 $\rightarrow$ Add
        
    - `(3,3)`=3 $\rightarrow$ Add
        
- **Check neighbors of newly added pixels:**
    
    - From `(3,3)`=3, we look right and find `(3,4)`=2 $\rightarrow$ Add.
        
- No other neighboring pixels fall within the $[0, 3]$ range.
    

**Final Pixels in Cluster 1:**

`(1,1)`, `(1,2)`, `(2,1)`, `(2,2)`, `(2,3)`, `(3,1)`, `(3,2)`, `(3,3)`, `(3,4)`

### **Step 2: Grow Cluster 2**

- **Seed Point:** `(1,6)`
    
- **Seed Value:** `5`
    
- **Threshold:** `1`
    
- **Valid Intensity Range:** $[5 - 1, 5 + 1] = \textbf{[4, 6]}$
    

We start at `(1,6)` and continuously check its 8-neighbors to see if their values are between 4 and 6.

- **Initial Region:** `(1,6)` -> value `5`
    
- **Check neighbors of `(1,6)`:**
    
    - `(1,5)`=6 $\rightarrow$ Add
        
    - `(2,5)`=6 $\rightarrow$ Add
        
    - `(2,6)`=5 $\rightarrow$ Add
        
- **Check neighbors of newly added pixels:**
    
    - From `(1,5)`=6 and `(2,5)`=6, we look left and find `(2,4)`=6 $\rightarrow$ Add.
        
    - From `(2,6)`=5, we look down and find `(3,6)`=6 $\rightarrow$ Add.
        
- No other neighboring pixels fall within the $[4, 6]$ range.
    

**Final Pixels in Cluster 2:**

`(1,5)`, `(1,6)`, `(2,4)`, `(2,5)`, `(2,6)`, `(3,6)`

### **Final Segmented Output**

If we map these two clusters back onto the original $5 \times 8$ grid (where **R1** is Region 1, **R2** is Region 2, and **-** is the unassigned background), the resulting segmentation map looks like this:

| pixel | **0** | **1**  | **2**  | **3**  | **4**  | **5**  | **6**  | **7** |
| ----- | ----- | ------ | ------ | ------ | ------ | ------ | ------ | ----- |
| **0** | -     | -      | -      | -      | -      | -      | -      | -     |
| **1** | -     | **R1** | **R1** | -      | -      | **R2** | **R2** | -     |
| **2** | -     | **R1** | **R1** | **R1** | **R2** | **R2** | **R2** | -     |
| **3** | -     | **R1** | **R1** | **R1** | **R1** | -      | **R2** | -     |
| **4** | -     | -      | -      | -      | -      | -      | -      | -     |