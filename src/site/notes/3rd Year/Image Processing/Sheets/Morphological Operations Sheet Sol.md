---
{"dg-publish":true,"permalink":"/3rd-year/image-processing/sheets/morphological-operations-sheet-sol/","dg-note-properties":{}}
---

# Q1

## Sol


**Given:**

$$A = \begin{bmatrix} 0 & 0 & 1 & 0 & 0 & 0 \\ 0 & 1 & 1 & 1 & 0 & 0 \\ 0 & 1 & 1 & 1 & 1 & 0 \end{bmatrix}$$

$$B = \begin{bmatrix} \mathbf{1} & 1 \\ 1 & 1 \end{bmatrix}$$

_(The bold $\mathbf{1}$ represents the origin of the structure element at the top-left corner)._

### **(a) Dilation of Image A by SE B**

**Rule:** For dilation, we place the origin of $B$ on every pixel in $A$. Is there any 1-pixel in B that meets a 1-pixel in A? if yes, output is 1, 0 otherwise

**Resulting Dilation:**

$$A \oplus B = \begin{bmatrix} 1 & 1 & 1 & 1 & 0 & 0 \\ 1 & 1 & 1 & 1 & 1 & 0 \\ 1 & 1 & 1 & 1 & 1 & 0 \end{bmatrix}$$

### **(b) Erosion of Image A by SE B**

**Rule:** For erosion, we place the origin of $B$ on every pixel in $A$. A pixel in the output image is set to `1` **only if** the entire structuring element $B$ is fully contained within the foreground (`1`s) of $A$. Since $B$ is a $2 \times 2$ block of `1`s, we are looking for any $2 \times 2$ square of `1`s in $A$.

Checking the original image $A$:

- Placing the origin at $(1,1)$ covers the $2 \times 2$ area of $1$s from rows 1-2, columns 1-2. $\rightarrow$ **Output 1**
    
- Placing the origin at $(1,2)$ covers the $2 \times 2$ area of $1$s from rows 1-2, columns 2-3. $\rightarrow$ **Output 1**
    
- No other $2 \times 2$ blocks of `1`s exist in the image without overlapping a `0` or going out of bounds.
    

**Resulting Erosion:**

$$A \ominus B = \begin{bmatrix} 0 & 0 & 0 & 0 & 0 & 0 \\ 0 & 1 & 1 & 0 & 0 & 0 \\ 0 & 0 & 0 & 0 & 0 & 0 \end{bmatrix}$$

---

# Q2

The image

$$A = \begin{bmatrix} 0 & 1 & 0 & 0 & 0 & 0 \\ 1 & 1 & 1 & 1 & 1 & 1 \\ 0 & 0 & 1 & 1 & 1 & 1 \\ 0 & 0 & 1 & 1 & 0 & 1 \\ 0 & 1 & 1 & 1 & 1 & 1 \\ 0 & 0 & 1 & 1 & 0 & 0\end{bmatrix}$$

The given structuring element $B$ is a $3 \times 3$ cross. Its origin is at the center pixel.

$$B = \begin{bmatrix} 0 & 1 & 0 \\ 1 & 1 & 1 \\ 0 & 1 & 0 \end{bmatrix}$$

This means for **Erosion**, the center pixel and its 4 direct neighbors (top, bottom, left, right) must all be `1` in the image for the output to be `1`. For **Dilation**, if any of those 5 pixels in the structuring element overlaps with a `1` in the image, the output center becomes `1`.

### **(a) Opening Operation ($A \circ B$)**

The opening of an image is an **Erosion followed by a Dilation**: $A \circ B = (A \ominus B) \oplus B$. It typically removes small isolated objects and thin protrusions.

**Step 1: Compute Erosion ($E = A \ominus B$)**

We scan Image $A$ to find where the entire cross shape fits perfectly inside the `1`s.

- The cross fits at **(2,3)** because the center and its neighbors (top at 1,3; bottom at 3,3; left at 2,2; right at 2,4) are all `1`.
    
- The cross fits at **(4,2)**.
    
- The cross fits at **(4,3)**.
    
- It does not fit anywhere else (boundary pixels lack neighbors, and other `1`s touch `0`s).
    

**Eroded Matrix ($E$):**

$$E = \begin{bmatrix} 0 & 0 & 0 & 0 & 0 & 0 \\ 0 & 0 & 0 & 0 & 0 & 0 \\ 0 & 0 & 0 & \mathbf{1} & 0 & 0 \\ 0 & 0 & 0 & 0 & 0 & 0 \\ 0 & 0 & \mathbf{1} & \mathbf{1} & 0 & 0 \\ 0 & 0 & 0 & 0 & 0 & 0 \end{bmatrix}$$

**Step 2: Compute Dilation on $E$ ($O = E \oplus B$)**

Now, we take the eroded image $E$ and place the cross structuring element on every `1`, replacing it with the full cross shape.

- Cross at (2,3) adds `1`s at: (1,3), (2,2), (2,3), (2,4), (3,3).
    
- Cross at (4,2) adds `1`s at: (3,2), (4,1), (4,2), (4,3), (5,2).
    
- Cross at (4,3) adds `1`s at: (3,3), (4,2), (4,3), (4,4), (5,3).
    

**Final Opening Output ($A \circ B$):**

$$A \circ B = \begin{bmatrix} 0 & 0 & 0 & 0 & 0 & 0 \\ 0 & 0 & 0 & 1 & 0 & 0 \\ 0 & 0 & 1 & 1 & 1 & 0 \\ 0 & 0 & 1 & 1 & 0 & 0 \\ 0 & 1 & 1 & 1 & 1 & 0 \\ 0 & 0 & 1 & 1 & 0 & 0 \end{bmatrix}$$

### **(b) Closing Operation ($A \bullet B$)**

The closing of an image is a **Dilation followed by an Erosion**: $A \bullet B = (A \oplus B) \ominus B$. It typically fills in small holes and connects nearby objects.

**Step 1: Compute Dilation ($D = A \oplus B$)**

We scan Image $A$. A pixel in the output becomes `1` if it, or any of its 4 direct neighbors, is `1` in Image $A$.

Because the `1`s are so dense in Image $A$, almost the entire image becomes `1`. Let's identify the pixels that remain `0` (meaning the pixel and its top, bottom, left, and right neighbors are all `0` in $A$):

- **(3,0)**: Pixel is `0`. Top(2,0)=`0`, Bottom(4,0)=`0`, Right(3,1)=`0`. -> Remains `0`.
    
- **(5,0)**: Pixel is `0`. Top(4,0)=`0`, Right(5,1)=`0`. -> Remains `0`.
    
- Every other `0` in the original image touches at least one `1`.
    

**Dilated Matrix ($D$):**

$$D = \begin{bmatrix} 1 & 1 & 1 & 1 & 1 & 1 \\ 1 & 1 & 1 & 1 & 1 & 1 \\ 1 & 1 & 1 & 1 & 1 & 1 \\ \mathbf{0} & 1 & 1 & 1 & 1 & 1 \\ 1 & 1 & 1 & 1 & 1 & 1 \\ \mathbf{0} & 1 & 1 & 1 & 1 & 1 \end{bmatrix}$$

**Step 2: Compute Erosion on $D$ ($C = D \ominus B$)**

Now we erode $D$ with the cross.

_(Note: since we assumed zero padding, all pixels on the bounderies will result in zero)._

A pixel will become `0` only if the cross overlaps with one of the `0`s in $D$.

- Overlap with the `0` at (3,0) forces `0`s at: **(3,0), (2,0), (4,0), (3,1)**.
    
- Overlap with the `0` at (5,0) forces `0`s at: **(5,0), (4,0), (5,1)**.
    

Every other location fully contains `1`s.

**Final Closing Output ($A \bullet B$):**

$$A \bullet B = \begin{bmatrix} 0 & 0 & 0 & 0 & 0 & 0 \\ 0 & 1 & 1 & 1 & 1 & 0 \\ \mathbf{0} & 1 & 1 & 1 & 1 & 0 \\ \mathbf{0} & \mathbf{0} & 1 & 1 & 1 & 0 \\ \mathbf{0} & 1 & 1 & 1 & 1 & 0 \\ \mathbf{0} & \mathbf{0} & 0 & 0 & 0 & 0 \end{bmatrix}$$

---

# Q3

To extract the boundary of an image using mathematical morphology, we use the standard boundary extraction formula:

$$A = \begin{bmatrix} 1 & 1 & 1 & 0 & 1 & 1 & 1 & 1 & 1 & 0 \\ 1 & 1 & 1 & 0 & 1 & 1 & 1 & 1 & 1 & 0 \\ 1 & 1 & 1 & 1 & 1 & 1 & 1 & 1 & 1 & 1 \\ 1 & 1 & 1 & 1 & 1 & 1 & 1 & 1 & 1 & 1 \\ 1 & 1 & 1 & 1 & 1 & 1 & 1 & 1 & 1 & 1 \end{bmatrix}$$

$$B = \begin{bmatrix} 1 & 1 & 1 \\ 1 & 1 & 1 \\1 & 1 & 1 \end{bmatrix}$$

$$Boundary(A) = A - (A \ominus B)$$

This means the boundary is found by first **eroding** Image A with the Structuring Element B, and then **subtracting** the eroded result from the original Image A.

Assuming standard zero-padding.

### **Step 1: Compute the Erosion ($A \ominus B$)**

**Rule:** We place the center of the $3 \times 3$ structuring element $B$ on every pixel in $A$. A pixel in the eroded image becomes `1` **if and only if** all 9 pixels of the structuring element completely overlap with `1`s in Image $A$. Otherwise, it becomes `0`.

- _Note: Because of zero-padding, all pixels on the outer border (row 1, row 5, col 1, col 10) will automatically become `0` since the structuring element goes out of bounds._
    

Scanning the inner pixels (Rows 2-4, Cols 2-9):

- **Row 2:** Only the centers at (2,2), (2,6), (2,7), and (2,8) are surrounded by a $3 \times 3$ block of solid `1`s.
    
- **Row 3:** Only the centers at (3,2), (3,6), (3,7), and (3,8) are surrounded by a $3 \times 3$ block of solid `1`s.
    
- **Row 4:** Because Rows 3, 4, and 5 are completely filled with `1`s, any center in Row 4 (from col 2 to col 9) will have a solid $3 \times 3$ block of `1`s.
    

**Eroded Matrix ($E = A \ominus B$):**

$$E = \begin{bmatrix} 0 & 0 & 0 & 0 & 0 & 0 & 0 & 0 & 0 & 0 \\ 0 & 1 & 0 & 0 & 0 & 1 & 1 & 1 & 0 & 0 \\ 0 & 1 & 0 & 0 & 0 & 1 & 1 & 1 & 0 & 0 \\ 0 & 1 & 1 & 1 & 1 & 1 & 1 & 1 & 1 & 0 \\ 0 & 0 & 0 & 0 & 0 & 0 & 0 & 0 & 0 & 0 \end{bmatrix}$$

### **Step 2: Subtract Eroded Image from Original Image ($A - E$)**

**Rule:** We perform a simple pixel-by-pixel subtraction (which is equivalent to logical `A AND NOT E`). This removes the "solid interior" of the shape, leaving only the outer perimeter.

**Calculation:**

$$\begin{bmatrix} 1 & 1 & 1 & 0 & 1 & 1 & 1 & 1 & 1 & 0 \\ 1 & 1 & 1 & 0 & 1 & 1 & 1 & 1 & 1 & 0 \\ 1 & 1 & 1 & 1 & 1 & 1 & 1 & 1 & 1 & 1 \\ 1 & 1 & 1 & 1 & 1 & 1 & 1 & 1 & 1 & 1 \\ 1 & 1 & 1 & 1 & 1 & 1 & 1 & 1 & 1 & 1 \end{bmatrix} - \begin{bmatrix} 0 & 0 & 0 & 0 & 0 & 0 & 0 & 0 & 0 & 0 \\ 0 & 1 & 0 & 0 & 0 & 1 & 1 & 1 & 0 & 0 \\ 0 & 1 & 0 & 0 & 0 & 1 & 1 & 1 & 0 & 0 \\ 0 & 1 & 1 & 1 & 1 & 1 & 1 & 1 & 1 & 0 \\ 0 & 0 & 0 & 0 & 0 & 0 & 0 & 0 & 0 & 0 \end{bmatrix}$$

**Final Boundary Output:**

$$Boundary = \begin{bmatrix} 1 & 1 & 1 & 0 & 1 & 1 & 1 & 1 & 1 & 0 \\ 1 & 0 & 1 & 0 & 1 & 0 & 0 & 0 & 1 & 0 \\ 1 & 0 & 1 & 1 & 1 & 0 & 0 & 0 & 1 & 1 \\ 1 & 0 & 0 & 0 & 0 & 0 & 0 & 0 & 0 & 1 \\ 1 & 1 & 1 & 1 & 1 & 1 & 1 & 1 & 1 & 1 \end{bmatrix}$$