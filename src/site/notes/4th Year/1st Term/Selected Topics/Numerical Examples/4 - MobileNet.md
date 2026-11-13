---
{"dg-publish":true,"permalink":"/4th-year/1st-term/selected-topics/numerical-examples/4-mobile-net/","dg-note-properties":{}}
---


  

## 1. Standard vs. Depthwise Separable Convolution (Toy Layer)

### Setup & Dimensional Parameters

- **Input dimensions:** $D_F \times D_F \times M$
    
      
    
- **Output dimensions:** $D_G \times D_G \times N$ with $D_G = 4$
    
      
    
- **Channels:** Input channels $M = 3$, Output channels $N = 5$
    
      
    
- **Kernel size:** $D_K = 3$ ($3 \times 3$ filter)
    
      
    

### Calculation Breakdown

- **Standard Convolution:**
    
      
    - $\text{Parameters} = D_K^2 \cdot M \cdot N = 3^2 \cdot 3 \cdot 5 = 135$
        
          
        
    - $\text{Mult-adds} = D_K^2 \cdot M \cdot N \cdot D_G^2 = 3 \cdot 3 \cdot 3 \cdot 5 \cdot 16 = 2,160$
        
          
        
- **Depthwise Convolution (Stage 1):**
    
      
    - $\text{Parameters} = D_K^2 \cdot M = 3^2 \cdot 3 = 27$
        
          
        
    - $\text{Mult-adds} = D_K^2 \cdot M \cdot D_G^2 = 9 \cdot 3 \cdot 16 = 432$
        
          
        
    - $\text{Output shape} = 4 \times 4 \times 3$ (channel count remains unchanged)
        
          
        
- **Pointwise Convolution (Stage 2):**
    
      
    - $\text{Parameters} = M \cdot N = 3 \cdot 5 = 15$
        
          
        
    - $\text{Mult-adds} = M \cdot N \cdot D_G^2 = 3 \cdot 5 \cdot 16 = 240$
        
          
        
    - $\text{Output shape} = 4 \times 4 \times 5$
        
          
        
- **Theoretical Savings Ratio:**
    
      
    
    $$\frac{1}{N} + \frac{1}{D_K^2} = \frac{1}{5} + \frac{1}{9} = 0.20 + 0.111 = 0.311 \implies 3.2\times\text{ fewer operations}$$
    
    _(Comparison: for $D_K = 3, N = 256$, ratio is $0.0039 + 0.111 = 0.115 \implies 8.7\times\text{ fewer operations}$)_
    
      
    

## 2. BatchNorm and ReLU6 Value Walkthrough

### Setup

- **Batch values:** $x = [2, 4, 6]$
    
      
    
- **Batch statistics:** Mean $\mu = 4$, Standard deviation $\sigma = 1.63$
    
      
    
- **BatchNorm learnable parameters:** Scale $\gamma = 2$, Shift $\beta = 1$
    
      
    
- **Formulae:**
    
      
    - $\text{BN}(x) = \gamma \cdot \frac{x - \mu}{\sqrt{\sigma^2 + \epsilon}} + \beta$
        
          
        
    - $\text{ReLU6}(x) = \min(\max(0, x), 6)$
        
          
        

### Step-by-Step Values

|**Step**|**Value 1**|**Value 2**|**Value 3**|
|---|---|---|---|
|**Input ($x$)**|$2$|$4$|$6$|
|**Normalize ($\frac{x - \mu}{\sigma}$)**|$-1.22$|$0$|$1.22$|
|**BN ($\gamma=2, \beta=1$)**|$-1.45$|$1.00$|$3.45$|
|**ReLU6**|$0$|$1.00$|$3.45$|

- **Additional Scalar ReLU6 Clamping Examples:**
    
      
    - $-2 \rightarrow 0$
        
          
        
    - $3.5 \rightarrow 3.5$
        
          
        
    - $8 \rightarrow 6$
        
          
        

## 3. Width Multiplier ($\alpha$) Scaling Impact

### Setup

- **Layer parameters:** Input channels $M = 64$, Output channels $N = 128$, Kernel size $D_K = 3$, Feature map size $D_F = 14$ ($D_F^2 = 196$)
    
      
    

### Comparison: $\alpha = 1.0$ vs. $\alpha = 0.5$

|**Metric**|**Baseline (α=1.0)**|**Scaled (α=0.5)**|
|---|---|---|
|**Channels ($M \rightarrow N$)**|$64 \rightarrow 128$|$32 \rightarrow 64$|
|**Depthwise Mult-adds**|$9 \cdot 64 \cdot 196 = 112,896$|$9 \cdot 32 \cdot 196 = 56,448$|
|**Pointwise Mult-adds**|$64 \cdot 128 \cdot 196 = 1,605,632$|$32 \cdot 64 \cdot 196 = 401,408$|
|**Total Mult-adds**|$1,718,528$|$457,856$ ($0.27\times \approx \alpha^2$)|
|**Parameters**|$576 + 8,192 = 8,768$|$288 + 2,048 = 2,336$|

## 4. Resolution Multiplier ($\rho$) Scaling Impact

### Setup

- **Input Resolution:** Scaled from $224 \rightarrow 160$ ($\rho = \frac{160}{224} = 0.714$)
    
      
    
- **Feature Map Size:** $D_F$ shrinks from $14 \rightarrow 10$ ($D_F^2 = 100$)
    
      
    
- **Channels & Kernel:** $M = 64, N = 128, D_K = 3$
    
      
    

### Comparison: $\rho = 1.0$ vs. $\rho = 0.714$

|**Metric**|**Baseline (ρ=1.0,DF​=14)**|**Scaled (ρ=0.714,DF​=10)**|
|---|---|---|
|**Depthwise Mult-adds**|$9 \cdot 64 \cdot 196 = 112,896$|$9 \cdot 64 \cdot 100 = 57,600$|
|**Pointwise Mult-adds**|$64 \cdot 128 \cdot 196 = 1,605,632$|$64 \cdot 128 \cdot 100 = 819,200$|
|**Total Mult-adds**|$1,718,528$|$876,800$ ($0.51\times \approx \rho^2$)|
|**Parameters**|$8,768$|$8,768$ (unchanged)|

## 5. Linear Bottlenecks & Information Loss (MobileNetV2)

### Vector Preservation Test

- **Input 3-channel vector:** $(-1.5, 2.0, -0.5)$
    
      
    
- **Vector after ReLU:** $(0, 2.0, 0)$
    
      
    
- **Information loss:** $2$ out of $3$ values lost ($66.7\%$)
    
      
    

## 6. MobileNetV2 Inverted Residual Block Layer Walkthrough

### Setup

- **Input & Output channels:** $24 \rightarrow 24$
    
      
    
- **Expansion factor:** $t = 6$ ($24 \cdot 6 = 144$ intermediate channels)
    
      
    
- **Kernel:** $3 \times 3$ depthwise
    
      
    

### Cost & Parameter Breakdown

|**Sub-Layer**|**Channel Transform**|**Computation / Parameter Formula**|**Parameter Count**|
|---|---|---|---|
|**$1 \times 1$ Expand ($t=6$)**|$24 \rightarrow 144$|$24 \cdot 144$|$3,456$|
|**$3 \times 3$ Depthwise**|$144 \rightarrow 144$|$9 \cdot 144$|$1,296$|
|**$1 \times 1$ Project (Linear)**|$144 \rightarrow 24$|$144 \cdot 24$|$3,456$|
|**Block Total**|$24 \rightarrow 144 \rightarrow 24$|—|**$8,208$**|

- **Computational Load:** On a $28 \times 28$ feature map ($784$ pixels):
    
      
    
    $$\text{Mult-adds} = 8,208 \cdot 784 \approx 6.4\text{ M}$$
    
- **Comparison:** A standard $3 \times 3$ convolution from $144 \rightarrow 144$ channels requires $3^2 \cdot 144 \cdot 144 = 186,624\text{ parameters}$ ($22.7\times$ more).
    
      
    

## 7. Squeeze-and-Excitation (SE) End-to-End Walkthrough (MobileNetV3)

### Setup

- **Input Tensor:** $C = 4$ channels, each of spatial size $2 \times 2$
    
      
    
- **Reduction Ratio:** $r = 2$ (bottleneck dimension $= C / r = 2$)
    
      
    
- **Total Parameter Cost:** $\frac{2C^2}{r} = \frac{2 \cdot 4^2}{2} = 8 + 8 = 16\text{ parameters}$
    
      
    

### Input Maps ($2 \times 2$ per channel)

$$\text{Ch 1} = \begin{bmatrix} 1 & 3 \\ 2 & 2 \end{bmatrix}, \quad \text{Ch 2} = \begin{bmatrix} 4 & 4 \\ 4 & 4 \end{bmatrix}, \quad \text{Ch 3} = \begin{bmatrix} 0 & 1 \\ -1 & 0 \end{bmatrix}, \quad \text{Ch 4} = \begin{bmatrix} 5 & 7 \\ 6 & 6 \end{bmatrix}$$

### Execution Pipeline

|**Step**|**Operation / Weights**|**Computation**|**Result**|
|---|---|---|---|
|**1. Squeeze**|Global spatial average per channel|$\left[\frac{1+3+2+2}{4}, \frac{4+4+4+4}{4}, \frac{0+1-1+0}{4}, \frac{5+7+6+6}{4}\right]$|$z = [2, 4, 0, 6]$|
|**2. FC1 + ReLU**|$W_1 = \begin{bmatrix} 0.5 & 0.5 & 0 & 0 \\ 0 & 0 & 0.5 & 0.5 \end{bmatrix}$|$\begin{bmatrix} 0.5(2) + 0.5(4) \\ 0.5(0) + 0.5(6) \end{bmatrix} = \begin{bmatrix} 3 \\ 3 \end{bmatrix} \xrightarrow{\text{ReLU}}$|$h = [3, 3]$|
|**3. FC2**|$W_2 = \begin{bmatrix} 0.5 & 0.5 \\ 0.25 & 0.25 \\ -0.5 & -0.5 \\ 0 & 0.5 \end{bmatrix}$|$\begin{bmatrix} 0.5(3)+0.5(3) \\ 0.25(3)+0.25(3) \\ -0.5(3)-0.5(3) \\ 0(3)+0.5(3) \end{bmatrix}$|$[3, 1.5, -3, 1.5]$|
|**4. Hard-Sigmoid**|$\frac{\text{ReLU6}(x+3)}{6}$|$\left[\frac{\text{ReLU6}(6)}{6}, \frac{\text{ReLU6}(4.5)}{6}, \frac{\text{ReLU6}(0)}{6}, \frac{\text{ReLU6}(4.5)}{6}\right]$|$s = [1.0, 0.75, 0.0, 0.75]$|

### Rescaling Step (Output Maps)

- **Channel 1:** $\times 1.00 \implies \begin{bmatrix} 1 & 3 \\ 2 & 2 \end{bmatrix}$
    
      
    
- **Channel 2:** $\times 0.75 \implies \begin{bmatrix} 3 & 3 \\ 3 & 3 \end{bmatrix}$
    
      
    
- **Channel 3:** $\times 0.00 \implies \begin{bmatrix} 0 & 0 \\ 0 & 0 \end{bmatrix}$ _(switched off)_
    
      
    
- **Channel 4:** $\times 0.75 \implies \begin{bmatrix} 3.75 & 5.25 \\ 4.50 & 4.50 \end{bmatrix}$
    
      
    

## 8. Swish vs. Hard-Swish ($\text{h-swish}$) Activation Comparison

### Mathematical Definitions

- $\text{swish}(x) = x \cdot \sigma(x) = \frac{x}{1 + e^{-x}}$
    
      
    
- $\text{hard-sigmoid}(x) = \frac{\text{ReLU6}(x + 3)}{6}$
    
      
    
- $\text{h-swish}(x) = x \cdot \frac{\text{ReLU6}(x + 3)}{6}$
    
      
    

### Value Comparison Table

|**x**|**σ(x)**|**hard-sigmoid(x)**|**swish(x)**|**h-swish(x)**|**ReLU(x)**|
|---|---|---|---|---|---|
|**$-3$**|$0.05$|$0.00$|$-0.14$|$0.00$|$0$|
|**$-1$**|$0.27$|$0.33$|$-0.27$|$-0.33$|$0$|
|**$0$**|$0.50$|$0.50$|$0.00$|$0.00$|$0$|
|**$1$**|$0.73$|$0.67$|$0.73$|$0.67$|$1$|
|**$3$**|$0.95$|$1.00$|$2.86$|$3.00$|$3$|

- **Step Examples:**
    
      
    - For $x = 1$: $1 \cdot \frac{\text{ReLU6}(4)}{6} = \frac{4}{6} \approx 0.67$
        
          
        
    - For $x = -1$: $-1 \cdot \frac{\text{ReLU6}(2)}{6} = -\frac{2}{6} \approx -0.33$
        
          
        
- **Max error:** Curves differ by at most $\approx 0.14$ (occurring at $\vert{}x\vert{} = 3$).
    
      
    

## 9. Comprehensive Full Numerical Example (Slides 30–33)

### Goal & Configuration

- **Input Tensor:** $6 \times 6 \times 3$
    
      
    
- **Output Goal:** $4 \times 4 \times 5$
    
      
    
- **Kernels:** $3 \times 3$, Stride $1$, No Padding
    
      
    

### Input Feature Maps ($6 \times 6 \times 3$)

$$\text{Channel 1} = \begin{bmatrix} 1 & 2 & 3 & 0 & 1 & 2 \\ 4 & 5 & 6 & 1 & 0 & 3 \\ 7 & 8 & 9 & 2 & 1 & 0 \\ 1 & 3 & 5 & 7 & 9 & 2 \\ 0 & 1 & 4 & 6 & 8 & 1 \\ 3 & 5 & 7 & 9 & 2 & 0 \end{bmatrix}, \quad \text{Channel 2} = \begin{bmatrix} 2 & 0 & 1 & 3 & 5 & 7 \\ 1 & 4 & 2 & 0 & 3 & 6 \\ 7 & 8 & 5 & 1 & 2 & 4 \\ 3 & 0 & 6 & 9 & 8 & 1 \\ 1 & 2 & 3 & 4 & 5 & 6 \\ 0 & 1 & 7 & 8 & 9 & 2 \end{bmatrix}, \quad \text{Channel 3} = \begin{bmatrix} 0 & 1 & 2 & 3 & 4 & 5 \\ 6 & 7 & 8 & 9 & 0 & 1 \\ 2 & 3 & 4 & 5 & 6 & 7 \\ 8 & 9 & 0 & 1 & 2 & 3 \\ 4 & 5 & 6 & 7 & 8 & 9 \\ 1 & 2 & 3 & 4 & 5 & 6 \end{bmatrix}$$

### Step 1: Depthwise Stage ($3$ kernels of size $3 \times 3$)

- **Kernels:**
    
      
    
    $$K_1 = \begin{bmatrix} 1 & 0 & -1 \\ 1 & 0 & -1 \\ 1 & 0 & -1 \end{bmatrix}, \quad K_2 = \begin{bmatrix} 0 & 1 & 0 \\ 0 & 1 & 0 \\ 0 & 1 & 0 \end{bmatrix}, \quad K_3 = \begin{bmatrix} 1 & 1 & 1 \\ 0 & 0 & 0 \\ -1 & -1 & -1 \end{bmatrix}$$
    
- **Position $(0,0)$ check for Channel 1:**
    
      
    
    $$(1 + 4 + 7) - (3 + 6 + 9) = 12 - 18 = -6$$
    
- **Depthwise Output Maps ($4 \times 4 \times 3$):**
    
      
    
    $$\hat{G}_1 = \begin{bmatrix} -6 & 12 & 16 & -2 \\ -8 & 6 & 10 & 5 \\ -10 & -3 & 0 & 12 \\ -12 & -13 & -3 & 19 \end{bmatrix}, \quad \hat{G}_2 = \begin{bmatrix} 12 & 8 & 4 & 10 \\ 12 & 13 & 10 & 13 \\ 10 & 14 & 14 & 15 \\ 3 & 16 & 21 & 22 \end{bmatrix}, \quad \hat{G}_3 = \begin{bmatrix} -6 & -6 & -6 & -6 \\ 4 & 14 & 14 & 4 \\ -6 & -6 & -6 & -6 \\ 11 & 1 & -9 & -9 \end{bmatrix}$$
    

### Step 2: Pointwise Stage & Output Verification

- **Pointwise filter vector:** $p = [1, -1, 2]$
    
      
    
- **Position $(0,0)$ calculation:**
    
      
    
    $$\text{Vector at }(0,0) = (-6, 12, -6)$$
    
    $$\text{Output}(0,0) = 1 \cdot (-6) + (-1) \cdot 12 + 2 \cdot (-6) = -6 - 12 - 12 = -30$$
    
- **Final Resulting $4 \times 4$ Output Slice:**
    
      
    
    $$G = \begin{bmatrix} -30 & -8 & 0 & -24 \\ -12 & 21 & 28 & 0 \\ -32 & -29 & -26 & -15 \\ 7 & -27 & -42 & -21 \end{bmatrix}$$
    
    _(A standard $3 \times 3 \times 3$ filter whose slices are $K_1, K_2, K_3$ scaled by $p$ produces the exact same $-30$ value at $(0,0)$)_
    

### Final Cost Comparison Table

|**Metric**|**Standard Convolution**|**Depthwise Separable Convolution**|**Savings Factor**|
|---|---|---|---|
|**Multiplications**|$16 \cdot 27 \cdot 5 = 2,160$|$432 + 240 = 672$|$3.2\times\text{ fewer}$|
|**Additions**|$16 \cdot 26 \cdot 5 = 2,080$|$384 + 160 = 544$|$3.8\times\text{ fewer}$|
|**Total Operations**|$4,240$|$1,216$|**$3.5\times\text{ fewer}$**|
|**Parameters**|$27 \cdot 5 = 135$|$27 + 15 = 42$|**$3.2\times\text{ fewer}$**|