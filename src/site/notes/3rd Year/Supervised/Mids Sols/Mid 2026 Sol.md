---
{"dg-publish":true,"permalink":"/3rd-year/supervised/mids-sols/mid-2026-sol/","dg-note-properties":{}}
---

# **Q1**

## **A- Bagging Vs Boosting**
    
- Bagging: Trains Models in Parallel, dataset gets split into equal sized bootstrap, output is simple majority voting / averaging
    <br>
- Boosting: Trains models sequentially, wrongly classified points get higher weights, output is weighted average which strong learners gets higher weights

---
## B- Effect of K in KNN:
    
- Small K: Sensitive to noise overfitting
	<br>
- Large K: smooth decision boundary underfitting
        <br>
- Best way to choose K: use cross Validation and Select the K-value that gives the best performance

> TA Sol: Elbow method

---
## C- Naive Bayes

- **1) Priors**: 
  $$P(Yes)= \frac{n_{yes}}{n_{total}} =\frac{6}{10}, P(No)= \frac{n_{no}}{n_{total}} =\frac{4}{10}$$
    
- **2)** $P(Yes | Senior, income) = \frac{P(Senior, Low | Yes) P(Yes)}{P(Senior, Low)}$ 
  
	- $P(Senior, Low | Yes) \ P(Yes) = P(Senior|Yes) \ P(Low|Yes) \ P(Yes)$ 
        
    
    - $= \frac{3}{5} \times \frac{1}{5} \times \frac{6}{10} = 0.05$
        <br>
- $P(No | Senior, Low) = \frac{P(Senior, Low | No) P(No)}{P(Senior, Low)}$
    
    - $P(Senior|No) P(Low|No) P(No) = \frac{1}{4} \times \frac{2}{4} \times \frac{4}{10} = 0.05$
            <br>
- $P(Yes | Test Sample) = P(No | Test Sample)$
    <br>
- cannot decide or choose the class with the largest prior probability which is class (Yes)   

---
# **Q2**

## **A-** 

- Convolutional Layer: detect Local Patterns (edges, texture)

- weight sharing: same Filter applies across the image reduce # of parameters.
	
- Pooling: down-samples, reduce spatial dimensions reduces overfitting

	---
## **B-**
Input: $x (3\times 2) = \begin{bmatrix}2 & 2\\ 0 & 3\\ 1 & 2\end{bmatrix} , W (2\times 2) = \begin{bmatrix}0.5 & -1\\ 1 & 0.5\end{bmatrix}$, No Padding, Stride = 1 $o/p_{size} = (3-2+1) \times (2-2+1) = 2 \times 1$
    
### **1) Conv o/p**:
- $x_1 = (2 \times 0.5) + (2 \times -1) + (0 \times 1) + (3 \times 0.5) = 0.5$
    
- $x_2 = (0 \times 0.5) + (3 \times -1) + (1 \times 1) + (2 \times 0.5) = -1$
    
- $O/P = \begin{bmatrix} 0.5 \\ -1 \end{bmatrix}$
    
### **2) apply sigmoid on the Conv** 
    
- $sigmoid(0.5) = \frac{1}{1+e^{-0.5}} = 0.6225$
	
- $sigmoid(-1) = \frac{1}{1+e^{1}} = 0.269$
	

### **3) Calculate the Loss**

![Pasted image 20260527234515.png](/img/user/imgs/Pasted%20image%2020260527234515.png)

- $Net_3 = 1 \times 0.6225 + 1 \times 0.269 = 0.8915, O_3 = \frac{1}{1+e^{-0.8915}} = 0.71$
    
- $Net_4 = 1 \times 0.6225 + 1 \times 0.289 = 0.8915, O_4 = \frac{1}{1+e^{-0.8915}} = 0.71$
    
- $Loss = \frac{1}{2} [(1.5 - 0.71)^2 + (1 - 0.71)^2] = 0.3541$
        
### **4) BP Steps:** 
    
1. Compute error $(t_j - o_j)$
	
2. Compute $(t_j - o_j)o_j(1 - o_j)$
	
3. propagate error to conv. Layer
	
4. update weights using gradient descent
	

### 5) Update weights

![Pasted image 20260527234946.png](/img/user/imgs/Pasted%20image%2020260527234946.png)

- $O_1 = 0.6225, O_2 = 0.269, O_3 = O_4 = 0.71$
    
---
#### at O/P layer
    
- $\delta_j = (t_j - O_j) \ O_j(1 - O_j) ; \ \Delta W_{ji} = \eta \delta_j O_i$
    
- $W_{ji}^{new} = W_{ji}^{old} + \Delta W_{ji}$
    
- $\delta_3 = (t_3 - O_3)O_3(1 - O_3) = (1.5 - 0.71)(0.71)(1 - 0.71) = 0.163$
        
- $\delta_4 = (t_4 - O_4)O_4(1 - O_4) = (1 - 0.71)(0.71)(1 - 0.71) = 0.0597$

---

- $\Delta W_{31} = (0.1) \times 0.163 \times 0.6225 = 0.01$
    
	- $W_{31}^{new} = W_{31}^{old} + \Delta W_{31} = 1 + 0.01 = 1.01$

- $\Delta W_{32} = \eta \delta_3 O_2 = 0.1 \times 0.163 \times 0.269 = 0.004$
    
    - $W_{32}^{new} = 1 + 0.004 = 1.004$
        
- $\Delta W_{41} = \eta \delta_4 O_1 = 0.1 \times 0.0597 \times 0.6225 = 0.0037$
    
    - $W_{41}^{new} = 1 + 0.0037 = 1.0037$
        
- $\Delta W_{42} = \eta \delta_4 O_2 = 0.1 \times 0.0597 \times 0.269 = 0.002$
    
    - $W_{42}^{new} = 1 + 0.002 = 1.002$

---

### at hidden layer (kernel weights)

### **Gathering the Known Values**

From the earlier steps in the problem, we have the following values for the Output Layer ($\delta_k$) and the Dense Weights ($W_{kj}$) connecting the hidden Conv layer to the output layer:

- $\eta = 0.1$
    
- **Output Layer Errors ($\delta_k$):** $\delta_3 = 0.163$, $\delta_4 = 0.0597$
    
- **Dense Layer Weights ($W_{kj}$):** $W_{31} = 1.01$, $W_{32} = 1.004$, $W_{41} = 1.0037$, and $W_{42} = 1.002$.
    
- **Hidden Layer Outputs ($O_j$):** $O_1 = 0.6225$, $O_2 = 0.269$
    

For the convolutional layer, the original kernel $W$ and the input regions it was applied to are:

- **Original Kernel:** $W = \begin{bmatrix} W_{11} & W_{12} \\ W_{21} & W_{22} \end{bmatrix} = \begin{bmatrix} 0.5 & -1 \\ 1 & 0.5 \end{bmatrix}$
    
- **Input Region for $O_1$ ($X^{(1)}$):** $\begin{bmatrix} 2 & 2 \\ 0 & 3 \end{bmatrix}$
    
- **Input Region for $O_2$ ($X^{(2)}$):** $\begin{bmatrix} 0 & 3 \\ 1 & 2 \end{bmatrix}$



### **1. Recalculating Hidden Layer Errors ($\delta_1$ and $\delta_2$)**


$$\delta_j = \left( \sum_k W_{kj}^{new} \delta_k \right) \cdot O_j(1 - O_j)$$

**For $\delta_1$ (associated with $O_1$):**

$$\delta_1 = (W_{31}\delta_3 + W_{41}\delta_4) \cdot O_1(1 - O_1)$$

$$\delta_1 = (1.01 \times 0.163 + 1.0037 \times 0.0597) \cdot 0.6225(1 - 0.6225)$$

$$\delta_1 = (0.16463 + 0.05992) \cdot 0.23499$$

$$\delta_1 = 0.22455 \times 0.23499 \approx \mathbf{0.05277}$$

**For $\delta_2$ (associated with $O_2$):**

$$\delta_2 = (W_{32}\delta_3 + W_{42}\delta_4) \cdot O_2(1 - O_2)$$

$$\delta_2 = (1.004 \times 0.163 + 1.002 \times 0.0597) \cdot 0.269(1 - 0.269)$$

$$\delta_2 = (0.16365 + 0.05982) \cdot 0.19664$$

$$\delta_2 = 0.22347 \times 0.19664 \approx \mathbf{0.04394}$$

### **2. Updating the Kernel Weights**

Now we apply these new $\delta$ values to the kernel weight update formula:

$$\Delta W_{mn} = \eta \sum_j \delta_j X^{(j)}_{mn}$$

**Updating $W_{11}$:**

$$\Delta W_{11} = 0.1 \times (\delta_1(2) + \delta_2(0)) = 0.1 \times (0.05277 \times 2) = 0.01055$$

$$W_{11}^{new} = 0.5 + 0.01055 = \mathbf{0.51055}$$

**Updating $W_{12}$:**

$$\Delta W_{12} = 0.1 \times (\delta_1(2) + \delta_2(3)) = 0.1 \times (0.05277(2) + 0.04394(3))$$

$$\Delta W_{12} = 0.1 \times (0.10554 + 0.13182) = 0.023736$$

$$W_{12}^{new} = -1 + 0.023736 = \mathbf{-0.976264}$$

**Updating $W_{21}$:**

$$\Delta W_{21} = 0.1 \times (\delta_1(0) + \delta_2(1)) = 0.1 \times (0.04394 \times 1) = 0.00439$$

$$W_{21}^{new} = 1 + 0.00439 = \mathbf{1.00439}$$

**Updating $W_{22}$:**

$$\Delta W_{22} = 0.1 \times (\delta_1(3) + \delta_2(2)) = 0.1 \times (0.05277(3) + 0.04394(2))$$

$$\Delta W_{22} = 0.1 \times (0.15831 + 0.08788) = 0.02462$$

$$W_{22}^{new} = 0.5 + 0.02462 = \mathbf{0.52462}$$

### **Final Updated Kernel**

Using the updated dense weights, the new convolutional filter is slightly different:

$$W^{new} = \begin{bmatrix} 0.51055 & -0.97626 \\ 1.00439 & 0.52462 \end{bmatrix}$$