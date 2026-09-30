---
{"dg-publish":true,"permalink":"/3rd-year/cognitive-science/sheets/random-sheet/","dg-note-properties":{}}
---



![Pasted image 20260405233534.png](/img/user/imgs/Pasted%20image%2020260405233534.png)
### **(a) Conditional Probability Table**

In a Noisy-OR model, the probability that the effect $Y$ is absent ($Y=0$) is the product of the independent failure probabilities of each active cause $X_i$. 

Let $q_i$ be the probability that $Y=0$ given that *only* $X_i = 1$. The general Noisy-OR formula is:
$$P(Y=0 \mid X) = \prod_{i: X_i=1} q_i$$
$$P(Y=1 \mid X) = 1 - \prod_{i: X_i=1} q_i$$

First, we extract the failure probabilities $q_1, q_2, q_3$ from the given rows:
1.  **Find $q_2$:**
    $P(Y=1 \mid 0, 1, 0) = \frac{1}{3} \implies P(Y=0 \mid 0, 1, 0) = \frac{2}{3}$. Therefore, **$q_2 = \frac{2}{3}$**.
2.  **Find $q_3$:**
    $P(Y=1 \mid 0, 1, 1) = \frac{4}{5} \implies P(Y=0 \mid 0, 1, 1) = \frac{1}{5}$.
    Using the Noisy-OR formula: $q_2 \cdot q_3 = \frac{1}{5} \implies \frac{2}{3} \cdot q_3 = \frac{1}{5} \implies$ **$q_3 = \frac{3}{10}$**.
3.  **Find $q_1$:**
    $P(Y=1 \mid 1, 1, 1) = \frac{5}{6} \implies P(Y=0 \mid 1, 1, 1) = \frac{1}{6}$.
    Using the formula: $q_1 \cdot q_2 \cdot q_3 = \frac{1}{6} \implies q_1 \cdot \left(\frac{1}{5}\right) = \frac{1}{6} \implies$ **$q_1 = \frac{5}{6}$**.

Now, we calculate the missing values in the table:
* **Row 2 ($X_1=1, X_2=0, X_3=0$):** $P(Y=1) = 1 - q_1 = 1 - \frac{5}{6} =$ **$\frac{1}{6}$**
* **Row 4 ($X_1=0, X_2=0, X_3=1$):** $P(Y=1) = 1 - q_3 = 1 - \frac{3}{10} =$ **$\frac{7}{10}$**
* **Row 5 ($X_1=1, X_2=1, X_3=0$):** $P(Y=1) = 1 - (q_1 \cdot q_2) = 1 - \left(\frac{5}{6} \cdot \frac{2}{3}\right) = 1 - \frac{5}{9} =$ **$\frac{4}{9}$**
* **Row 6 ($X_1=1, X_2=0, X_3=1$):** $P(Y=1) = 1 - (q_1 \cdot q_3) = 1 - \left(\frac{5}{6} \cdot \frac{3}{10}\right) = 1 - \frac{1}{4} =$ **$\frac{3}{4}$**

**Completed Table:**

| $X_1$ | $X_2$ | $X_3$ | $P(Y=1 \mid X_1, X_2, X_3)$ |
| :--- | :--- | :--- | :--- |
| 0 | 0 | 0 | 0 |
| 1 | 0 | 0 | **1/6** |
| 0 | 1 | 0 | 1/3 |
| 0 | 0 | 1 | **7/10** |
| 1 | 1 | 0 | **4/9** |
| 1 | 0 | 1 | **3/4** |
| 0 | 1 | 1 | 4/5 |
| 1 | 1 | 1 | 5/6 |

---
![Pasted image 20260405233526.png](/img/user/imgs/Pasted%20image%2020260405233526.png)
### **a) Joint Probability Distribution**

The joint probability distribution for a Bayesian network is the product of the conditional probability distributions of each node given its parents.

Looking at the graph, we can identify the parents for each node:

- $A$: No parents
    
- $C$: No parents
    
- $B$: Parents are $A$ and $C$
    
- $D$: Parent is $C$
    
- $E$: Parent is $B$
    

Therefore, the joint probability distribution is:

$$P(A, B, C, D, E) = P(A) \cdot P(C) \cdot P(B \mid A, C) \cdot P(D \mid C) \cdot P(E \mid B)$$

---

### **b) Conditional Independence Assumptions**

The fundamental assumption of a Bayesian network (the Local Markov Property) is that **each node is conditionally independent of its non-descendants given its parents.** Applying this rule to each node in the model gives us the following conditional independence assumptions:

- **Node A:** Has no parents. Its non-descendants are $C$ and $D$.
    
    - Assumption: $A \perp C, D$ (A is independent of C and D)
        
- **Node C:** Has no parents. Its non-descendant is $A$.
    
    - Assumption: $C \perp A$ (C is independent of A)
        
- **Node B:** Parents are $A, C$. Its non-descendant is $D$.
    
    - Assumption: $B \perp D \mid A, C$
        
- **Node D:** Parent is $C$. Its non-descendants are $A, B, E$.
    
    - Assumption: $D \perp A, B, E \mid C$
        
- **Node E:** Parent is $B$. Its non-descendants are $A, C, D$.
    
    - Assumption: $E \perp A, C, D \mid B$
        

---

![Pasted image 20260405233749.png](/img/user/imgs/Pasted%20image%2020260405233749.png)

### **a) Joint Probability Distribution**

In an undirected model, the joint probability distribution is factored into potential functions (often denoted as $\phi$) over the maximal cliques (in this case, the pairs of connected nodes).

The joint probability distribution is:

$$p(A, B, C, D) = \frac{1}{Z} \phi(A, B) \phi(B, C) \phi(C, D)$$

Where $Z$ is the partition function (a normalization constant) that ensures all probabilities sum to 1:

$$Z = \sum_{A,B,C,D} \phi(A, B) \phi(B, C) \phi(C, D)$$

---

### **b) Potential Functions**

The problem states that each variable takes a value of 0 or 1, and it is 9 times more probable for neighboring variables to have equal values than different values.

Since the rule applies identically across the entire chain, we can use the same potential function for all pairs: $\phi(A,B) = \phi(B,C) = \phi(C,D)$.

We can define the potential function $\phi(X,Y)$ as:

- $\phi(X,Y) = 9$ if $X = Y$
    
- $\phi(X,Y) = 1$ if $X \neq Y$
    

Expressed as a table (or matrix) where the rows represent $X \in \{0,1\}$ and columns represent $Y \in \{0,1\}$:

|           | **Y=0** | **Y=1** |
| --------- | ------- | ------- |
| **$X=0$** | 9       | 1       |
| **$X=1$** | 1       | 9       |


