---
{"dg-publish":true,"permalink":"/3rd-year/supervised/before-mid/lecture-4-optimizers-hyperparameter-tuning/","dg-note-properties":{}}
---



### 1. Performance Evaluation

Before you can improve a model, you need to know how to measure its current performance. The lecture highlights that as model complexity increases, training error naturally decreases, but test error will eventually start to increase—creating a convex curve that represents overfitting.

#### Evaluating Regression Models

For regression (predicting continuous values), the slides define two primary metrics:

- **Mean Absolute Error (MAE):** The average of the absolute differences between predictions and actual values. Closer to 0 is better.
    
    $$MAE = \frac{1}{m}\sum_{i=1}^{m}|y_{i}-\hat{y}_{i}|$$
    
- **Mean Squared Error (MSE):** The average of the squared differences. _(Note: The slide formula uses $\overline{y}_i$, which typically denotes the mean, but in this context, it represents the predicted value comparable to $\hat{y}_i$)_.
    
    $$MSE = \frac{1}{m}\sum_{i=1}^{m}(y_{i}-\overline{y}_{i})^{2}$$
    

#### Evaluating Classification Models

For classification, performance is broken down using a **Confusion Matrix**, which tracks:

- **True Positives (TP) / True Negatives (TN):** Cases your model classified correctly.
    
- **False Positives (FP) / False Negatives (FN):** Cases your model classified incorrectly.
    

From this matrix, we derive several critical formulas:

- **Accuracy:** $\frac{TP+TN}{P+N}$ (Overall correctness).
    
- **Error Rate:** $\frac{FP+FN}{P+N}$ (Overall incorrectness).
    
- **Recall (Sensitivity):** $\frac{TP}{P}$ (Out of all actual positives, how many did we find?) "Completeness" .
    
- **Precision:** $\frac{TP}{TP+FP}$ (Out of all predicted positives, how many were actually positive?) "Soundness" .
    
- **F1 Score:** $\frac{2 \times precision \times recall}{precision + recall}$ (The harmonic mean of precision and recall, useful when classes are imbalanced).

![Pasted image 20260331014652.png](/img/user/imgs/Pasted%20image%2020260331014652.png)

example on it [[3rd Year/Supervised/Expanded Explanations/Lecture 4 Examples#Example On Confusion Matrix\|here]]

---

### 2. Hyperparameter Tuning

As discussed in previous lectures, hyperparameters are external configurations set manually by the user (like the learning rate $\eta$, $K$ in KNN, or the number of trees in a Random Forest).

The standard search process is:

1. Divide data into train, validation, and test sets.
    
2. Optimize internal parameters on the training set.
    
3. Search for the best hyperparameters using the validation set.
    
4. Alternate until finalized, then do a final assessment on the test set.
    

**Search Methods:**

- **Grid Search:** Tests every single possible combination on a manually specified grid. It is highly exhaustive but extremely expensive and time-consuming, making it unfeasible for deep neural networks.
    
- **Random Search:** Samples random combinations from a broad range, eventually narrowing down the search area based on where the best results are found. This is much more efficient for large search spaces.
    

---

### 3. Optimizer Types

Optimizers are the specific algorithms used to update the weights of your model during training. They aim to ==accelerate convergence==, prevent getting stuck in local minimums, and simplify learning rate adjustments.

**1. Standard Gradient Descent** Updates weights by taking a step in the negative direction of the gradient.

$$w_{k+1} = w_{k} - \eta\nabla f_{w_{k}}(x^{i})$$

**2. Momentum Optimizer** Standard gradient descent can get stuck or oscillate. Momentum fixes this by adding a fraction of the _previous_ update to the current update. It acts like a ball rolling down a hill, gaining inertia to push through flat spots or small bumps.

زي اي حاجة بتتدحرج علي تل, كل ما تفضل ماشي في اتجاه ثابت, تكتسب سرعة اكبر في نفس الاتجاه دهز

$$\Delta w_{ji}(n) = -\eta\delta_i x_j + \alpha\Delta w_{ji}(n-1)$$

**3. AdaGrad (Adaptive Gradient)** Instead of using one global learning rate ($\eta$) for all parameters, AdaGrad adapts the learning rate for each specific parameter. It does this by dividing the learning rate by the square root of $r_t$ (the accumulated sum of all past squared gradients).

$$r_t = r_{t-1} + g_t^2$$

$$\Delta w_t = -\frac{\eta}{\epsilon + \sqrt{r_t}}g_t$$

_Problem:_ Because $r_t$ strictly increases, the learning rate eventually shrinks to zero, stopping training entirely.

**4. RMSProp (Root Mean Square Propagation)** RMSProp fixes AdaGrad's "shrinking to zero" problem by introducing an attenuation coefficient (decay factor $\beta$). Instead of accumulating _all_ past gradients, it keeps a moving average, allowing the model to continue learning. It is highly effective for Recurrent Neural Networks (RNNs).

$$r_t = \beta r_{t-1} + (1-\beta)g_t^2$$

**5. Adam (Adaptive Moment Estimation)** Adam is essentially the combination of Momentum and RMSProp, and it is the default choice for most modern deep learning.

- It calculates the moving average of the gradient ($m_t$, the First Moment, like Momentum).
    
- It calculates the moving average of the squared gradient ($v_t$, the Second Moment, like RMSProp).
    
- It applies a bias correction ($\hat{m}_t$ and $\hat{v}_t$) to prevent these values from being biased toward zero at the start of training.
    
- The default parameters are highly stable: $\beta_1 = 0.9$, $\beta_2 = 0.999$, $\epsilon = 10^{-8}$, and $\eta = 0.001$.

---

تلخيصة القوانين كلها تحت اهي سيبك من القوانين اللي فوق

Here is a complete comparison of the five optimization algorithms, followed by a step-by-step numerical walkthrough for two epochs.

### 1. Optimizer Comparison & Equations

Each optimizer attempts to solve the core problem of standard gradient descent: finding the minimum error efficiently without getting stuck in flat regions or overshooting the target. Let $g_t$ represent the gradient at step $t$, $\eta$ represent the learning rate, and $w_t$ represent the weight at step $t$.

**1. Stochastic Gradient Descent (SGD)**

- **Concept:** The baseline method. It calculates the gradient of the loss and takes a step of size $\eta$ in the opposite direction. It is simple but can be slow and easily gets stuck in local minima.
    
- **Equation:**
    
    $$w_{t} = w_{t-1} - \eta g_t$$
    

**2. Momentum**

- **Concept:** Adds an "inertia" term ($v_t$). Instead of just relying on the current gradient, it remembers the direction of the previous updates (controlled by $\beta$). This helps push the optimizer through flat surfaces and dampens oscillations.
    
- **Equations:**
    
    $$v_t = \beta v_{t-1} + \eta g_t$$
    
    $$w_t = w_{t-1} - v_t$$
    

**3. AdaGrad (Adaptive Gradient)**

- **Concept:** Abandons the idea of a single global learning rate. It divides the learning rate by the square root of $G_t$ (the sum of all past squared gradients). Features that update frequently get smaller learning rates, while rare features get larger ones.
    
- **Equations:**
    
    $$G_t = G_{t-1} + g_t^2$$
    
    $$w_t = w_{t-1} - \frac{\eta}{\sqrt{G_t} + \epsilon} g_t$$
    

**4. RMSProp (Root Mean Square Propagation)**

- **Concept:** Fixes AdaGrad's fatal flaw (the learning rate shrinking to zero). Instead of accumulating _all_ past squared gradients, it uses an exponentially decaying moving average ($s_t$), controlled by $\beta$. This keeps the learning rate adaptable but prevents it from dying out.
    
- **Equations:**
    
    $$s_t = \beta s_{t-1} + (1-\beta)g_t^2$$
    
    $$w_t = w_{t-1} - \frac{\eta}{\sqrt{s_t} + \epsilon} g_t$$
    

**5. Adam (Adaptive Moment Estimation)**

- **Concept:** The industry standard. It combines the "inertia" of Momentum (First Moment, $m_t$) with the "adaptive learning rate" of RMSProp (Second Moment, $v_t$). It also includes bias correction ($\hat{m}_t$ and $\hat{v}_t$) to ensure the initial steps aren't artificially small.
    
- **Equations:**
    
    $$m_t = \beta_1 m_{t-1} + (1-\beta_1)g_t$$
    
    $$v_t = \beta_2 v_{t-1} + (1-\beta_2)g_t^2$$
    
    $$\hat{m}_t = \frac{m_t}{1-\beta_1^t} \quad \text{and} \quad \hat{v}_t = \frac{v_t}{1-\beta_2^t}$$
    
    $$w_t = w_{t-1} - \frac{\eta}{\sqrt{\hat{v}_t} + \epsilon} \hat{m}_t$$
    

---

### 2. Numerical Example (2 Epochs)

Let's trace these optimizers mathematically using a static gradient to see how they behave differently. For simplicity in the hand calculations, we will ignore the tiny $\epsilon$ (usually 1e-8) as its only purpose is preventing division by zero.

**Initial Conditions:**

- Starting Weight ($w_0$): 1.0
    
- Constant Gradient ($g_t$): 0.5
    
- Learning Rate ($\eta$): 0.1
    
- Momentum/RMSProp Factor ($\beta$): 0.9
    
- Adam Factors ($\beta_1$, $\beta_2$): 0.9, 0.999
    
- All accumulators ($v_0, G_0, s_0, m_0$): 0
    

#### Epoch 1 ($t=1$)

- **SGD**
    
    - $w_1 = 1.0 - 0.1(0.5) = \mathbf{0.95}$
        
- **Momentum**
    
    - $v_1 = 0.9(0) + 0.1(0.5) = 0.05$
        
    - $w_1 = 1.0 - 0.05 = \mathbf{0.95}$
        
- **AdaGrad**
    
    - $G_1 = 0 + (0.5)^2 = 0.25$
        
    - $w_1 = 1.0 - \frac{0.1}{\sqrt{0.25}}(0.5) = 1.0 - (0.2 \times 0.5) = \mathbf{0.90}$
        
- **RMSProp**
    
    - $s_1 = 0.9(0) + 0.1(0.25) = 0.025$
        
    - $w_1 = 1.0 - \frac{0.1}{\sqrt{0.025}}(0.5) = 1.0 - (0.6324 \times 0.5) = \mathbf{0.684}$
        
- **Adam**
    
    - $m_1 = 0.9(0) + 0.1(0.5) = 0.05$
        
    - $v_1 = 0.999(0) + 0.001(0.25) = 0.00025$
        
    - $\hat{m}_1 = \frac{0.05}{1 - 0.9^1} = \frac{0.05}{0.1} = 0.5$
        
    - $\hat{v}_1 = \frac{0.00025}{1 - 0.999^1} = \frac{0.00025}{0.001} = 0.25$
        
    - $w_1 = 1.0 - \frac{0.1}{\sqrt{0.25}}(0.5) = \mathbf{0.90}$
        

#### Epoch 2 ($t=2$)

- **SGD**
    
    - $w_2 = 0.95 - 0.1(0.5) = \mathbf{0.90}$
        
- **Momentum**
    
    - $v_2 = 0.9(0.05) + 0.1(0.5) = 0.045 + 0.05 = 0.095$
        
    - $w_2 = 0.95 - 0.095 = \mathbf{0.855}$ _(Notice how the step size is increasing due to momentum)_
        
- **AdaGrad**
    
    - $G_2 = 0.25 + (0.5)^2 = 0.50$
        
    - $w_2 = 0.90 - \frac{0.1}{\sqrt{0.50}}(0.5) = 0.90 - (0.1414 \times 0.5) = \mathbf{0.829}$ _(Notice how the effective learning rate shrank)_
        
- **RMSProp**
    
    - $s_2 = 0.9(0.025) + 0.1(0.25) = 0.0475$
        
    - $w_2 = 0.684 - \frac{0.1}{\sqrt{0.0475}}(0.5) = 0.684 - (0.4588 \times 0.5) = \mathbf{0.455}$
        
- **Adam**
    
    - $m_2 = 0.9(0.05) + 0.1(0.5) = 0.095$
        
    - $v_2 = 0.999(0.00025) + 0.001(0.25) = 0.00049975$
        
    - $\hat{m}_2 = \frac{0.095}{1 - 0.9^2} = \frac{0.095}{0.19} = 0.5$
        
    - $\hat{v}_2 = \frac{0.00049975}{1 - 0.999^2} = \frac{0.00049975}{0.001999} = 0.25$
        
    - $w_2 = 0.90 - \frac{0.1}{\sqrt{0.25}}(0.5) = 0.90 - 0.1 = \mathbf{0.80}$

more on optimizers [[3rd Year/Supervised/Expanded Explanations/Lecture 4 Examples#More On Optimizers\|here]]