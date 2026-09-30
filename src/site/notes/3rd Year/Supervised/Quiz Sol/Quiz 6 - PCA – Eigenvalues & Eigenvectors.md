---
{"dg-publish":true,"permalink":"/3rd-year/supervised/quiz-sol/quiz-6-pca-eigenvalues-and-eigenvectors/","dg-note-properties":{}}
---

Given the covariance matrix:

C = $\begin{bmatrix} 5 & 2 \\ 2 & 5 \end{bmatrix}$

Compute:
1. The eigenvalues of C.
2. The corresponding eigenvectors (normalised).
3. Construct the matrix Q sorted by |eigenvalue| descending.

## Sol

ده حرفيا [[3rd Year/Supervised/Quiz Sol/Quiz 5 - PCA – Mean & Covariance Matrix#4. The Eigenvalues of $C$\|كويز 5]]

### 3. Construct the matrix Q 

To construct the feature vector matrix $Q$ (also known as the projection matrix or weight matrix), we place the normalized eigenvectors as columns. The columns must be sorted in descending order based on the absolute value of their corresponding eigenvalues ($|\lambda|$).

From our previous steps:

- **Principal Component 1 (PC1):** $\lambda_1 = 7$, $v_1 = \begin{pmatrix} \frac{1}{\sqrt{2}} \\ \frac{1}{\sqrt{2}} \end{pmatrix}$
    
- **Principal Component 2 (PC2):** $\lambda_2 = 3$, $v_2 = \begin{pmatrix} -\frac{1}{\sqrt{2}} \\ \frac{1}{\sqrt{2}} \end{pmatrix}$
    

Since $7 > 3$, the matrix $Q$ is constructed with $v_1$ as the first column and $v_2$ as the second column:

$$Q = \begin{bmatrix} \frac{1}{\sqrt{2}} & -\frac{1}{\sqrt{2}} \\ \frac{1}{\sqrt{2}} & \frac{1}{\sqrt{2}} \end{bmatrix}$$

## Bonus (ما أصل خلاص متبقاش غير خطوة)

### 4. Transform the Data (1D Projection)

To reduce the dimensionality of the data from 2-D to 1-D using only one principal component, we discard the lesser components and construct a reduced matrix $Q_1$ using only the eigenvector associated with the largest eigenvalue (PC1).

$$Q_1 = \begin{bmatrix} \frac{1}{\sqrt{2}} \\ \frac{1}{\sqrt{2}} \end{bmatrix}$$

To transform the data into this new 1-D subspace, we project the **mean-shifted vectors** ($x_i$) onto our chosen principal component. The formula for the transformed scalar value $y_i$ is:

$$y_i = Q_1^T x_i = \begin{bmatrix} \frac{1}{\sqrt{2}} & \frac{1}{\sqrt{2}} \end{bmatrix} \begin{bmatrix} x_{i,x} \\ x_{i,y} \end{bmatrix}$$

Let's apply this to each mean-shifted vector from Step 2:

- **For $x_1 = \begin{pmatrix} -3 \\ 1 \end{pmatrix}$:**
    
    $$y_1 = \begin{bmatrix} \frac{1}{\sqrt{2}} & \frac{1}{\sqrt{2}} \end{bmatrix} \begin{bmatrix} -3 \\ 1 \end{bmatrix} = \left(\frac{1}{\sqrt{2}} \times -3\right) + \left(\frac{1}{\sqrt{2}} \times 1\right) = \frac{-2}{\sqrt{2}} = -\sqrt{2} \approx -1.414$$
    
- **For $x_2 = \begin{pmatrix} -1 \\ -3 \end{pmatrix}$:**
    
    $$y_2 = \begin{bmatrix} \frac{1}{\sqrt{2}} & \frac{1}{\sqrt{2}} \end{bmatrix} \begin{bmatrix} -1 \\ -3 \end{bmatrix} = \left(\frac{1}{\sqrt{2}} \times -1\right) + \left(\frac{1}{\sqrt{2}} \times -3\right) = \frac{-4}{\sqrt{2}} = -2\sqrt{2} \approx -2.828$$
    
- **For $x_3 = \begin{pmatrix} 1 \\ -1 \end{pmatrix}$:**
    
    $$y_3 = \begin{bmatrix} \frac{1}{\sqrt{2}} & \frac{1}{\sqrt{2}} \end{bmatrix} \begin{bmatrix} 1 \\ -1 \end{bmatrix} = \left(\frac{1}{\sqrt{2}} \times 1\right) + \left(\frac{1}{\sqrt{2}} \times -1\right) = \frac{0}{\sqrt{2}} = 0$$
    
- **For $x_4 = \begin{pmatrix} 3 \\ 3 \end{pmatrix}$:**
    
    $$y_4 = \begin{bmatrix} \frac{1}{\sqrt{2}} & \frac{1}{\sqrt{2}} \end{bmatrix} \begin{bmatrix} 3 \\ 3 \end{bmatrix} = \left(\frac{1}{\sqrt{2}} \times 3\right) + \left(\frac{1}{\sqrt{2}} \times 3\right) = \frac{6}{\sqrt{2}} = 3\sqrt{2} \approx 4.243$$
    

**Final Transformed Data (1D):**

The original 2-D coordinates have been successfully compressed into a single dimension along the axis of maximum variance:

- $F_1 \rightarrow y_1 = -\sqrt{2}$
    
- $F_2 \rightarrow y_2 = -2\sqrt{2}$
    
- $F_3 \rightarrow y_3 = 0$
    
- $F_4 \rightarrow y_4 = 3\sqrt{2}$

