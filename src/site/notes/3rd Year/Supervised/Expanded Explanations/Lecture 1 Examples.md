---
{"dg-publish":true,"permalink":"/3rd-year/supervised/expanded-explanations/lecture-1-examples/","dg-note-properties":{}}
---


### Part 1: K-Nearest Neighbors (KNN) Classification

Let’s classify a new data point based on its similarity to existing training data using Euclidean distance.

**The Setup:**

- **Training Data:**
    
    - $P_1$: (2, 4) $\rightarrow$ Class **A**
        
    - $P_2$: (4, 2) $\rightarrow$ Class **A**
        
    - $P_3$: (6, 8) $\rightarrow$ Class **B**
        
    - $P_4$: (8, 6) $\rightarrow$ Class **B**
        
- **Test Point ($P_t$):** (4, 5)
    
- **Hyperparameter:** $K = 3$
    

**Step 1: Calculate Euclidean Distances**

The formula is $d = \sqrt{(x_2 - x_1)^2 + (y_2 - y_1)^2}$.

- Distance to $P_1$: $\sqrt{(4 - 2)^2 + (5 - 4)^2} = \sqrt{4 + 1} = \mathbf{2.24}$
    
- Distance to $P_2$: $\sqrt{(4 - 4)^2 + (5 - 2)^2} = \sqrt{0 + 9} = \mathbf{3.00}$
    
- Distance to $P_3$: $\sqrt{(4 - 6)^2 + (5 - 8)^2} = \sqrt{4 + 9} = \mathbf{3.61}$
    
- Distance to $P_4$: $\sqrt{(4 - 8)^2 + (5 - 6)^2} = \sqrt{16 + 1} = \mathbf{4.12}$
    

**Step 2: Sort and Select the $K$ Nearest Neighbors**

Sorting the distances in ascending order:

1. $P_1$ (Distance: 2.24) $\rightarrow$ Class A
    
2. $P_2$ (Distance: 3.00) $\rightarrow$ Class A
    
3. $P_3$ (Distance: 3.61) $\rightarrow$ Class B
    

**Step 3: Majority Voting**

Among the 3 nearest neighbors, Class A appears twice and Class B appears once.

**Result:** The test point (4, 5) is classified as **Class A**.

For regression problems, you apply the exact same steps, except instead of taking majority vote, we simply take the average of K-nearest.

---

### Part 2: Linear Regression

For the following regression examples, we will use a tiny dataset to predict a target $Y$ from a single feature $X$.

**Dataset:**

- $X$: [1, 2, 3]
    
- $Y$: [2, 3, 5]
    

We are trying to fit the line equation: $\hat{y} = mx + b$ (where $m$ is the weight/slope, and $b$ is the bias/intercept).

#### Method A: The Normal Equation (Analytical)

The Normal Equation finds the exact optimal weights in one mathematical operation. The formula is:

$$W = (X^T X)^{-1} X^T Y$$

**Step 1: Create the Augmented Matrix $X$ and Vector $Y$**

We add a column of 1s to $X$ to account for the bias term ($b$).

$$X = \begin{bmatrix} 1 & 1 \\ 1 & 2 \\ 1 & 3 \end{bmatrix}, \quad Y = \begin{bmatrix} 2 \\ 3 \\ 5 \end{bmatrix}$$

**Step 2: Calculate $X^T X$**

$$X^T = \begin{bmatrix} 1 & 1 & 1 \\ 1 & 2 & 3 \end{bmatrix}$$

$$X^T X = \begin{bmatrix} 1 & 1 & 1 \\ 1 & 2 & 3 \end{bmatrix} \begin{bmatrix} 1 & 1 \\ 1 & 2 \\ 1 & 3 \end{bmatrix} = \begin{bmatrix} 3 & 6 \\ 6 & 14 \end{bmatrix}$$

**Step 3: Calculate the Inverse $(X^T X)^{-1}$**

The determinant is $(3 \times 14) - (6 \times 6) = 42 - 36 = 6$.

$$(X^T X)^{-1} = \frac{1}{6} \begin{bmatrix} 14 & -6 \\ -6 & 3 \end{bmatrix} = \begin{bmatrix} 7/3 & -1 \\ -1 & 1/2 \end{bmatrix}$$

**Step 4: Calculate $X^T Y$**

$$X^T Y = \begin{bmatrix} 1 & 1 & 1 \\ 1 & 2 & 3 \end{bmatrix} \begin{bmatrix} 2 \\ 3 \\ 5 \end{bmatrix} = \begin{bmatrix} 10 \\ 23 \end{bmatrix}$$

**Step 5: Multiply to get $W$**

$$W = \begin{bmatrix} 7/3 & -1 \\ -1 & 1/2 \end{bmatrix} \begin{bmatrix} 10 \\ 23 \end{bmatrix} = \begin{bmatrix} (70/3) - 23 \\ -10 + (23/2) \end{bmatrix} = \begin{bmatrix} 1/3 \\ 3/2 \end{bmatrix}$$

**Result:** The optimal bias $b \approx \mathbf{0.33}$ and optimal weight $m = \mathbf{1.5}$.

---

#### Method B: Gradient Descent (Iterative)

![Gradient Descent optimization, AI generated](https://encrypted-tbn1.gstatic.com/licensed-image?q=tbn:ANd9GcQuitrfANnlyXtWGyIefBWrugHp7maMF6uK3KMD0X2myURHHVRDbIpJcYTXEgCD62551g5rwaIo9QAGUinRI2mCs-QFSSSX30Fdcc0WSEpNPcDT0Wo)

Instead of matrix inversion, Gradient Descent updates weights iteratively.

- **Initialization:** $m = 0, b = 0$
    
- **Learning Rate ($\alpha$):** 0.05
    
- **Gradient Formula for $m$:** $\frac{\partial J}{\partial m} = -\frac{2}{n} \sum x_i(y_i - \hat{y}_i)$
    
- **Gradient Formula for $b$:** $\frac{\partial J}{\partial b} = -\frac{2}{n} \sum (y_i - \hat{y}_i)$
    

Below is exactly one iteration (epoch) for the three different types of gradient descent using our dataset. Since initial weights are 0, $\hat{y}$ is initially 0 for all points.

##### 1. Batch Gradient Descent (BGD)

BGD calculates the error across the **entire dataset** ($n=3$) before making a single update.

- Errors ($y - \hat{y}$): (2 - 0) = 2, (3 - 0) = 3, (5 - 0) = 5.
    
- $\frac{\partial J}{\partial m} = -\frac{2}{3} [1(2) + 2(3) + 3(5)] = -\frac{2}{3} [2 + 6 + 15] = -\frac{46}{3} \approx -15.33$
    
- $\frac{\partial J}{\partial b} = -\frac{2}{3} [2 + 3 + 5] = -\frac{20}{3} \approx -6.67$
    

**Update:**

- $m_{new} = 0 - 0.05(-15.33) = \mathbf{0.766}$
    
- $b_{new} = 0 - 0.05(-6.67) = \mathbf{0.333}$
    

##### 2. Stochastic Gradient Descent (SGD)

SGD updates the weights after evaluating **a single, randomly chosen data point**. Let's assume the first point chosen is $(x=1, y=2)$. Here, $n=1$.

- Error: (2 - 0) = 2.
    
- $\frac{\partial J}{\partial m} = -2 [1(2)] = -4$
    
- $\frac{\partial J}{\partial b} = -2 [2] = -4$
    

**Update:**

- $m_{new} = 0 - 0.05(-4) = \mathbf{0.20}$
    
- $b_{new} = 0 - 0.05(-4) = \mathbf{0.20}$
    
    _(The algorithm would then immediately perform another update using the next single data point)._
    

##### 3. Mini-Batch Gradient Descent (MBGD)

MBGD uses a small subset of the data. Let's use a **batch size of 2**, picking the first two points: $(1, 2)$ and $(2, 3)$. Here, $n=2$.

- Errors: 2, 3.
    
- $\frac{\partial J}{\partial m} = -\frac{2}{2} [1(2) + 2(3)] = -[2 + 6] = -8$
    
- $\frac{\partial J}{\partial b} = -\frac{2}{2} [2 + 3] = -5$
    

**Update:**

- $m_{new} = 0 - 0.05(-8) = \mathbf{0.40}$
    
- $b_{new} = 0 - 0.05(-5) = \mathbf{0.25}$