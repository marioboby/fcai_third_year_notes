---
{"dg-publish":true,"permalink":"/4th-year/1st-term/ga-ns/numerical-examples/1-autoregressive-models-direct-method-and-fvsbn/","dg-note-properties":{}}
---

# **<font color="#00b050">By: Youssef Amgad</font>**

### Direct Method

The simplest way to generate an image is to predict it pixel-by-pixel using conditional probability.

#### 1. Setup & Preprocessing

Assume 3 binarized training images of dimension $2 \times 2$:

  

$$I_{1}=\begin{bmatrix}0 & 0\\ 1 & 1\end{bmatrix}, \quad I_{2}=\begin{bmatrix}0 & 1\\ 1 & 0\end{bmatrix}, \quad I_{3}=\begin{bmatrix}1 & 1\\ 0 & 0\end{bmatrix}$$

  

Flatten into 1D arrays of binary pixels $(x_1, x_2, x_3, x_4)$:

  

- $I_{1} = [0, 0, 1, 1]$
    
      
    
- $I_{2} = [0, 1, 1, 0]$
    
      
    
- $I_{3} = [1, 1, 0, 0]$
    
      
    

Decomposition by the probability chain rule:

  

$$P(x_1, x_2, x_3, x_4) = P(x_1) \cdot P(x_2 \mid x_1) \cdot P(x_3 \mid x_1, x_2) \cdot P(x_4 \mid x_1, x_2, x_3)$$

  

#### 2. Conditional Probability Calculations

Extracted directly by frequency matching across the 3 training samples:

> [!Note] هنعمل ايه بالضبط
> اللي هيحصل هنا بالضبط اني هشوف قيمة كل بيكسل بالترتيب في كل الصور و احسب احتمالية كل واحدة، بحيث هبدأ بأول بيكسل و اشوف احتمال قيمها المختلفة "اللي هو 0 و 1" علي مستوى كل الصور، بعدها أول ما أبدأ اخش في ال Conditional من تاني بيكسل فما بعدها، اشوف كل ال Combinations للبيكسلات اللي قبلي الموجودة في الصور بتاعتي، و أبدأ احسب احتمال ظهورهم مع كل قيم البيكسل اللي أنا واقف عليها
  

- Pixel 1 ($x_1$):
    
      
    - $P(x_1 = 1) = \frac{1}{3}$ (only $I_3$ starts with 1)
        
          
        
    - $P(x_1 = 0) = \frac{2}{3}$ ($I_1$ and $I_2$ start with 0)
        
          
        
- Pixel 2 ($x_2$):
    
      
    - If $x_1 = 1$: $P(x_2 = 1 \mid x_1 = 1) = 1$ (from $I_3$)
        
          
        
    - If $x_1 = 0$: $P(x_2 = 1 \mid x_1 = 0) = \frac{1}{2}$ (from $I_2$), $P(x_2 = 0 \mid x_1 = 0) = \frac{1}{2}$ (from $I_1$)
        
          
        



- Pixel 3 ($x_3$):
    
      
    - If $x_1 = 1, x_2 = 1$: $P(x_3 = 0 \mid 1, 1) = 1$ (from $I_3$)
        
          
        
    - If $x_1 = 0, x_2 = 0$: $P(x_3 = 1 \mid 0, 0) = 1$ (from $I_1$)
        
          
        
    - If $x_1 = 0, x_2 = 1$: $P(x_3 = 1 \mid 0, 1) = 1$ (from $I_2$)
        
          

> هنا مثلا، شوفنا الأول كل ال Combinations للبيكسلات اللي قبل البيكسل التالتة الموجودة في الصور عندي (اللي هما تلت احتمالات بس)، و احسب بقي احتمالية ظهورهم مع كل قيم البيكسل اللي أنا واقف عليها "في الحالة دي، المقام بيبقي عدد الصور اللي فيها Combinations البيكسلات اللي قبلي" 

- Pixel 4 ($x_4$):
    
      
    - If $x_1 = 1, x_2 = 1, x_3 = 0$: $P(x_4 = 0) = 1$ (from $I_3$)
        
          
        
    - If $x_1 = 0, x_2 = 0, x_3 = 1$: $P(x_4 = 1) = 1$ (from $I_1$)
        
          
        
    - If $x_1 = 0, x_2 = 1, x_3 = 1$: $P(x_4 = 0) = 1$ (from $I_2$)
        
          
        

#### 3. Sampling 5 New Images

Because every conditional branch collapses to a probability of 1 after branching, sampling strictly reproduces training instances:

  

1. Sample $x_1 = 0 \implies$ sample $x_2 \mid 0 \to 1 \implies x_3, x_4$ forced to $1, 0 \implies [0, 1, 1, 0]$ (Matches $I_2$)
    
      
    
2. Sample $x_1 = 0 \implies$ sample $x_2 \mid 0 \to 0 \implies x_3, x_4$ forced to $1, 1 \implies [0, 0, 1, 1]$ (Matches $I_1$)
    
      
    
3. Sample $x_1 = 1 \implies x_2, x_3, x_4$ forced to $1, 0, 0 \implies [1, 1, 0, 0]$ (Matches $I_3$)
    
      
    
4. Sample $x_1 = 0 \implies$ sample $x_2 \mid 0 \to 1 \implies [0, 1, 1, 0]$ (Matches $I_2$)
    
      
    
5. Sample $x_1 = 1 \implies [1, 1, 0, 0]$ (Matches $I_3$)
    
      
    

### Problems with the Direct Method 

1. <font color="#ff0000">Curse of Dimensionality</font>: Storing all conditional probabilities requires an exponentially large lookup table ($𝑂(2^𝑁)$ for $N$ binary pixels). For a tiny 28x28 image, you need 2784 table entries, which is computationally impossible. 
   
2. <font color="#ff0000">Zero Frequency / Overfitting</font>: If a specific pixel combination never appeared in the training set (e.g., $𝑥_1 = 1,𝑥_2 = 0$), its probability is exactly 0. The model can never generate novel combinations; it only memorizes and repeats the training data. 

### Fully Visible Sigmoid Belief Network (FVSBN): Numerical Example (1 Iteration)

To fix the exponential table size of the direct method, FVSBN replaces the lookup table with Logistic Regression. Instead of tracking every exact sequence, it learns weights (𝛼) that represent how much each previous pixel influences the current one. 

#### 1. Setup & Formula

FVSBN models conditional probabilities via logistic regression:

  

$$P(x_i = 1 \mid x_{<i}) = \sigma\left(\alpha_0^{(i)} + \sum_{j < i} \alpha_j^{(i)} x_j\right)$$

  

Task: Predict pixel $x_2$ given pixel $x_1$:

  

$$P(x_2 = 1 \mid x_1) = \sigma(\alpha_0 + \alpha_1 x_1)$$

  recall our 3 images:
  $$I_{1}=\begin{bmatrix}0 & 0\\ 1 & 1\end{bmatrix}, \quad I_{2}=\begin{bmatrix}0 & 1\\ 1 & 0\end{bmatrix}, \quad I_{3}=\begin{bmatrix}1 & 1\\ 0 & 0\end{bmatrix}$$

- **Initial weights**: $\alpha_0 = 0$, $\alpha_1 = 0.5$
    
      
    
- **Learning rate**: $\eta = 1.0$
    
      
    
- **Training pairs $(x_1 \to x_2)$**:
    
      
    - From $I_1$: $x_1 = 0 \to \text{Target } y = 0$
        
          
        
    - From $I_2$: $x_1 = 0 \to \text{Target } y = 1$
        
          
        
    - From $I_3$: $x_1 = 1 \to \text{Target } y = 1$
        
          
        

#### 2. Forward Pass (Predictions)

- For $I_1\ (x_1 = 0)$:
    
      
    
    $$\hat{y}_1 = \sigma(0 + 0.5 \cdot 0) = \sigma(0) = 0.5$$
    
      
    
- For $I_2\ (x_1 = 0)$:
    
      
    
    $$\hat{y}_2 = \sigma(0 + 0.5 \cdot 0) = \sigma(0) = 0.5$$
    
      
    
- For $I_3\ (x_1 = 1)$:
    
      
    
    $$\hat{y}_3 = \sigma(0 + 0.5 \cdot 1) = \sigma(0.5) \approx 0.6225$$
    
      
    

#### 3. Loss Calculation (Mean Squared Error)

$$\text{MSE} = \frac{1}{3}\sum_{k=1}^{3} (\hat{y}_k - y_k)^2$$

  

- $\text{Error}_1 = (0.5 - 0)^2 = 0.25$
    
      
    
- $\text{Error}_2 = (0.5 - 1)^2 = (-0.5)^2 = 0.25$
    
      
    
- $\text{Error}_3 = (0.6225 - 1)^2 = (-0.3775)^2 \approx 0.1425$
    
      
    
- $\text{Total MSE} = \frac{0.25 + 0.25 + 0.1425}{3} = \frac{0.6425}{3} \approx 0.2142$
    
      
    
احنا حلينا مع الدكتور لغاية هنا، باقي الخطوات ممكن تعرفها زيادة بس هي مجرد حسبة gradients و chain rule عادي
#### 4. Gradient Calculation (MSE with Sigmoid Derivative)

Derivative of sigmoid is $\hat{y}(1 - \hat{y})$:

  

$$\frac{\partial L}{\partial w} = \frac{2}{N}\sum (\hat{y} - y) \cdot [\hat{y}(1 - \hat{y})] \cdot x_{\text{input}}$$

  

Per-sample delta terms $(\hat{y} - y) \cdot [\hat{y}(1 - \hat{y})]$:

  

- $I_1: (0.5 - 0) \cdot [0.5(1 - 0.5)] = 0.5 \cdot 0.25 = 0.125$
    
      
    
- $I_2: (0.5 - 1) \cdot [0.5(1 - 0.5)] = -0.5 \cdot 0.25 = -0.125$
    
      
    
- $I_3: (0.6225 - 1) \cdot [0.6225(1 - 0.6225)] = -0.3775 \cdot 0.235 = -0.0887$
    
      
    

**Gradient for bias $\alpha_0$** ($x_{\text{input}} = 1$):

  

$$\frac{\partial L}{\partial \alpha_0} = \frac{2}{3}(0.125 - 0.125 - 0.0887) = \frac{2}{3}(-0.0887) = -0.0591$$

  

**Gradient for weight $\alpha_1$** ($x_{\text{input}} = x_1$):

  

$$\frac{\partial L}{\partial \alpha_1} = \frac{2}{3}[(0.125 \cdot 0) + (-0.125 \cdot 0) + (-0.0887 \cdot 1)] = \frac{2}{3}(-0.0887) = -0.0591$$

  

#### 5. Weight Update (Gradient Descent)

With $\eta = 1.0$:

  

$$\alpha_0^{(\text{new})} = \alpha_0 - \eta\left(\frac{\partial L}{\partial \alpha_0}\right) = 0 - (1.0 \cdot -0.0591) = 0.0591$$

  

$$\alpha_1^{(\text{new})} = \alpha_1 - \eta\left(\frac{\partial L}{\partial \alpha_1}\right) = 0.5 - (1.0 \cdot -0.0591) = 0.5591$$

### The Problem with FVSBN

While FVSBN solves the $𝑂(2^𝑁)$ memory issue of the direct method, it introduces an $𝑂(𝑁^2)$ parameter issue (each block has its own model, $\frac{n*(n+1)}{2}$). Every conditional probability $P(X_i|X<i)$ requires its own independent set of weights. There is no parameter sharing. For a modern image size, calculating independent logistic regressions for every single pixel interaction leads to massive overfitting and painfully slow training times. 