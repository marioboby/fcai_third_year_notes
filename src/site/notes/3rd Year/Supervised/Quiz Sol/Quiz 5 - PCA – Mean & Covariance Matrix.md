---
{"dg-publish":true,"permalink":"/3rd-year/supervised/quiz-sol/quiz-5-pca-mean-and-covariance-matrix/","dg-note-properties":{}}
---


# Given the following 2-D feature vectors, compute Steps 1 and 2 of PCA:

F₁ = (2, 6), F₂ = (4, 2), F₃ = (6, 4), F₄ = (8, 8)

$X = \begin{bmatrix}2 & 4 & 6 & 8 \\ 6 & 2 & 4 & 8\end{bmatrix}$

Compute:
1. The mean vector m.
2. The shifted vectors
3. The covariance matrix
4. The eigenvalues of C.
5. The corresponding eigenvectors (normalised).

Show all steps.

## Sol

### 1. The Mean Vector ($m$)

To find the mean vector, we average the $x$-coordinates and the $y$-coordinates across all four points.

- **$x$-mean:** $m_x = \frac{2 + 4 + 6 + 8}{4} = \frac{20}{4} = 5$
    
- **$y$-mean:** $m_y = \frac{6 + 2 + 4 + 8}{4} = \frac{20}{4} = 5$
    

$$m = \begin{pmatrix} 5 \\ 5 \end{pmatrix}$$

### 2. The Shifted Vectors

To mean-center the data, we subtract the mean vector $m$ from each original feature vector $F_i$. Let the shifted vectors be $x_i = F_i - m$.

- **$x_1$:**
    
    $$x_1 = \begin{pmatrix} 2 \\ 6 \end{pmatrix} - \begin{pmatrix} 5 \\ 5 \end{pmatrix} = \begin{pmatrix} -3 \\ 1 \end{pmatrix}$$
    
- **$x_2$:**
    
    $$x_2 = \begin{pmatrix} 4 \\ 2 \end{pmatrix} - \begin{pmatrix} 5 \\ 5 \end{pmatrix} = \begin{pmatrix} -1 \\ -3 \end{pmatrix}$$
    
- **$x_3$:**
    
    $$x_3 = \begin{pmatrix} 6 \\ 4 \end{pmatrix} - \begin{pmatrix} 5 \\ 5 \end{pmatrix} = \begin{pmatrix} 1 \\ -1 \end{pmatrix}$$
    
- **$x_4$:**
    
    $$x_4 = \begin{pmatrix} 8 \\ 8 \end{pmatrix} - \begin{pmatrix} 5 \\ 5 \end{pmatrix} = \begin{pmatrix} 3 \\ 3 \end{pmatrix}$$
    

### 3. The Covariance Matrix ($C$)

First, we compute the scatter matrix (the sum of the outer products of the shifted vectors):

$$X X^T = \sum_{i=1}^{4} x_i x_i^T = \begin{bmatrix} -3 \\ 1 \end{bmatrix}\begin{bmatrix} -3 & 1 \end{bmatrix} + \begin{bmatrix} -1 \\ -3 \end{bmatrix}\begin{bmatrix} -1 & -3 \end{bmatrix} + \begin{bmatrix} 1 \\ -1 \end{bmatrix}\begin{bmatrix} 1 & -1 \end{bmatrix} + \begin{bmatrix} 3 \\ 3 \end{bmatrix}\begin{bmatrix} 3 & 3 \end{bmatrix}$$

Calculating each matrix:

$$X X^T = \begin{bmatrix} 9 & -3 \\ -3 & 1 \end{bmatrix} + \begin{bmatrix} 1 & 3 \\ 3 & 9 \end{bmatrix} + \begin{bmatrix} 1 & -1 \\ -1 & 1 \end{bmatrix} + \begin{bmatrix} 9 & 9 \\ 9 & 9 \end{bmatrix}$$

Adding them together yields the scatter matrix:

$$X X^T = \begin{bmatrix} 20 & 8 \\ 8 & 20 \end{bmatrix}$$

To find the covariance matrix $C$, we divide by $N$ (the population formulation, which yields clean integers for this specific textbook problem). _Note: Some statistical software divides by $N-1$ instead, which would result in fractions._

$$C = \frac{1}{4} \begin{bmatrix} 20 & 8 \\ 8 & 20 \end{bmatrix} = \begin{bmatrix} 5 & 2 \\ 2 & 5 \end{bmatrix}$$

### 4. The Eigenvalues of $C$

To find the eigenvalues ($\lambda$), we solve the characteristic equation $\det(C - \lambda I) = 0$.

$$\det \begin{bmatrix} 5-\lambda & 2 \\ 2 & 5-\lambda \end{bmatrix} = 0$$

Calculate the determinant:

$$(5-\lambda)(5-\lambda) - (2)(2) = 0$$

$$(5-\lambda)^2 - 4 = 0$$

$$(5-\lambda)^2 = 4$$

Taking the square root of both sides gives:

$$5-\lambda = 2 \quad \text{or} \quad 5-\lambda = -2$$

Solving for $\lambda$ yields the two eigenvalues (usually sorted in descending order for PCA):

- **$\lambda_1 = 7$** (Principal Component 1)
    
- **$\lambda_2 = 3$** (Principal Component 2)
    

### 5. The Corresponding Eigenvectors

To find the eigenvectors $v$, we substitute each eigenvalue back into $(C - \lambda I)v = 0$.

**For $\lambda_1 = 7$:**

$$\begin{bmatrix} 5-7 & 2 \\ 2 & 5-7 \end{bmatrix} \begin{pmatrix} v_x \\ v_y \end{pmatrix} = \begin{pmatrix} 0 \\ 0 \end{pmatrix}$$

$$\begin{bmatrix} -2 & 2 \\ 2 & -2 \end{bmatrix} \begin{pmatrix} v_x \\ v_y \end{pmatrix} = \begin{pmatrix} 0 \\ 0 \end{pmatrix}$$

This gives the equation $-2v_x + 2v_y = 0$, which simplifies to $v_x = v_y$.

An unnormalized vector is $\begin{pmatrix} 1 \\ 1 \end{pmatrix}$.

To normalize, we divide by its magnitude $\sqrt{1^2 + 1^2} = \sqrt{2}$:

$$v_1 = \begin{pmatrix} \frac{1}{\sqrt{2}} \\ \frac{1}{\sqrt{2}} \end{pmatrix}$$

**For $\lambda_2 = 3$:**

$$\begin{bmatrix} 5-3 & 2 \\ 2 & 5-3 \end{bmatrix} \begin{pmatrix} v_x \\ v_y \end{pmatrix} = \begin{pmatrix} 0 \\ 0 \end{pmatrix}$$

$$\begin{bmatrix} 2 & 2 \\ 2 & 2 \end{bmatrix} \begin{pmatrix} v_x \\ v_y \end{pmatrix} = \begin{pmatrix} 0 \\ 0 \end{pmatrix}$$

This gives the equation $2v_x + 2v_y = 0$, which simplifies to $v_x = -v_y$.

An unnormalized vector is $\begin{pmatrix} -1 \\ 1 \end{pmatrix}$.

Normalizing by its magnitude $\sqrt{(-1)^2 + 1^2} = \sqrt{2}$:

$$v_2 = \begin{pmatrix} -\frac{1}{\sqrt{2}} \\ \frac{1}{\sqrt{2}} \end{pmatrix}$$