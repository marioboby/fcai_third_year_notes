---
{"dg-publish":true,"permalink":"/4th-year/1st-term/ga-ns/numerical-examples/4-gan-types/","dg-note-properties":{}}
---



  
###  When to Use BiGAN (Bidirectional GAN) vs. Normal GAN

Use a **BiGAN** instead of a standard GAN when the task requires **mapping real data back into the latent space** ($x \rightarrow z$) in addition to generating samples ($z \rightarrow x$). Because a vanilla GAN lacks an encoder, it cannot infer the latent representation of a given real input.

  

**Key Use Cases:**

  

1. **Unsupervised Feature Learning:** Extracting rich, semantic latent representations from unlabeled datasets to use in downstream tasks (e.g., classification).
    
      
    
2. **Anomaly Detection:** Encoding a test sample $x$ into $z = E(x)$, reconstructing $\hat{x} = G(E(x))$, and measuring the reconstruction/discriminator discrepancy—anomalies fail to reconstruct accurately.
    
      
    
3. **Semantic Image Editing:** Projecting a real image into its latent code $z$, modifying specific attributes in latent space (e.g., expression, lighting), and decoding it back into a realistic edited image.
    
      
    

## BiGAN: Complete Numerical Forward-Propagation

### 1. Problem Setup & Network Architecture

- **Activation Function:** Sigmoid $\sigma(a) = \frac{1}{1 + e^{-a}}$ applied in every layer of $E$, $G$, and $D$.
    
      
    
- **Inputs:**
    
      
    - Real image vector: $X = \begin{bmatrix} 0.8 & 0.6 \end{bmatrix}^T \in \mathbb{R}^2$
        
          
        
    - Prior latent noise vector: $z = \begin{bmatrix} 0.2 & 0.4 \end{bmatrix}^T \in \mathbb{R}^2$
        
          
        

|**Component**|**Architecture**|**Input / Output**|
|---|---|---|
|**Encoder ($E$)**|$2 \rightarrow 3 \rightarrow 2$|$X \in \mathbb{R}^2 \implies E(X) \in \mathbb{R}^2$|
|**Generator ($G$)**|$2 \rightarrow 3 \rightarrow 2$|$z \in \mathbb{R}^2 \implies G(z) \in \mathbb{R}^2$|
|**Discriminator ($D$)**|$4 \rightarrow 3 \rightarrow 1$|Concatenated pair $[x, z] \in \mathbb{R}^4 \implies P(y) \in (0, 1)$|

### 2. Encoder Forward Pass $E(X)$

#### Layer 1 (Hidden Layer: $h_1, h_2, h_3$)

- **Parameters:**
    
      
    
    $$W_{E1} = \begin{bmatrix} 0.2 & 0.3 \\ 0.4 & 0.1 \\ 0.5 & 0.2 \end{bmatrix}, \quad b_{E1} = \begin{bmatrix} 0.1 \\ 0.2 \\ 0.1 \end{bmatrix}$$
    

|**Neuron**|**Weighted Sum (a=∑wixi+ba = \sum w_i x_i + ba=∑wi​xi​+b)**|**Activation σ(a)\sigma(a)σ(a)**|
|---|---|---|
|**$h_1$**|$0.2(0.8) + 0.3(0.6) + 0.1 = \mathbf{0.4400}$|$\sigma(0.4400) = \mathbf{0.6083}$|
|**$h_2$**|$0.4(0.8) + 0.1(0.6) + 0.2 = \mathbf{0.5800}$|$\sigma(0.5800) = \mathbf{0.6411}$|
|**$h_3$**|$0.5(0.8) + 0.2(0.6) + 0.1 = \mathbf{0.6200}$|$\sigma(0.6200) = \mathbf{0.6502}$|

- **Hidden Output:** $H_E = \begin{bmatrix} 0.6083 & 0.6411 & 0.6502 \end{bmatrix}^T$
    
      
    

#### Layer 2 (Latent Output Layer: $z_{E1}, z_{E2}$)

- **Parameters:**
    
      
    
    $$W_{E2} = \begin{bmatrix} 0.2 & 0.3 & 0.4 \\ 0.3 & 0.2 & 0.1 \end{bmatrix}, \quad b_{E2} = \begin{bmatrix} 0.1 \\ 0.2 \end{bmatrix}$$
    

|**Latent Neuron**|**Weighted Sum (aaa)**|**Activation σ(a)\sigma(a)σ(a)**|
|---|---|---|
|**$z_{E1}$**|$0.2(0.6083) + 0.3(0.6411) + 0.4(0.6502) + 0.1 = \mathbf{0.6741}$|$\sigma(0.6741) = \mathbf{0.6624}$|
|**$z_{E2}$**|$0.3(0.6083) + 0.2(0.6411) + 0.1(0.6502) + 0.2 = \mathbf{0.5757}$|$\sigma(0.5757) = \mathbf{0.6401}$|

- **Encoder Output:**
    
      
    
    $$E(X) = \begin{bmatrix} 0.6624 \\ 0.6401 \end{bmatrix}$$
    
      
    

### 3. Generator Forward Pass $G(z)$

#### Layer 1 (Hidden Layer: $g_1, g_2, g_3$)

- **Parameters:** _(Corrected bias vector length to 3)_
    
      
    
    $$W_{G1} = \begin{bmatrix} 0.3 & 0.2 \\ 0.1 & 0.4 \\ 0.5 & 0.3 \end{bmatrix}, \quad b_{G1} = \begin{bmatrix} 0.1 \\ 0.1 \\ 0.2 \end{bmatrix}$$
    

| **Neuron** | **Weighted Sum (($a = \sum w_i x_i + b$))**   | **Activation $\sigma(a)$**         |
| ---------- | --------------------------------------------- | ---------------------------------- |
| **$g_1$**  | $0.3(0.2) + 0.2(0.4) + 0.1 = \mathbf{0.2400}$ | $\sigma(0.2400) = \mathbf{0.5597}$ |
| **$g_2$**  | $0.1(0.2) + 0.4(0.4) + 0.1 = \mathbf{0.2800}$ | $\sigma(0.2800) = \mathbf{0.5695}$ |
| **$g_3$**  | $0.5(0.2) + 0.3(0.4) + 0.2 = \mathbf{0.4200}$ | $\sigma(0.4200) = \mathbf{0.6035}$ |

- **Hidden Output:** $H_G = \begin{bmatrix} 0.5597 & 0.5695 & 0.6035 \end{bmatrix}^T$
    
      
    

#### Layer 2 (Generated Image Layer: $G_1, G_2$)

- **Parameters:**
    
      
    
    $$W_{G2} = \begin{bmatrix} 0.2 & 0.4 & 0.3 \\ 0.3 & 0.2 & 0.5 \end{bmatrix}, \quad b_{G2} = \begin{bmatrix} 0.1 \\ 0.1 \end{bmatrix}$$
    

|**Image Neuron**|**Weighted Sum (aaa)**|**Activation σ(a)\sigma(a)σ(a)**|
|---|---|---|
|**$G_1$**|$0.2(0.5597) + 0.4(0.5695) + 0.3(0.6035) + 0.1 = \mathbf{0.6208}$|$\sigma(0.6208) = \mathbf{0.6504}$|
|**$G_2$**|$0.3(0.5597) + 0.2(0.5695) + 0.5(0.6035) + 0.1 = \mathbf{0.6836}$|$\sigma(0.6836) = \mathbf{0.6645}$|

- **Generator Output:**
    
      
    
    $$G(z) = \begin{bmatrix} 0.6504 \\ 0.6645 \end{bmatrix}$$
    
      
    

### 4. Joint Image–Latent Pair Construction

Unlike a standard GAN, the BiGAN Discriminator evaluates joint pairs $(x, z) \in \mathbb{R}^4$:

  

|**Pair Type**|**Image Part**|**Latent Part**|**Concatenated Discriminator Input (qqq)**|
|---|---|---|---|
|**Real Pair ($q_R$)**|$X = [0.8000, 0.6000]$|$E(X) = [0.6624, 0.6401]$|$q_R = [0.8000, 0.6000, 0.6624, 0.6401]$|
|**Fake Pair ($q_F$)**|$G(z) = [0.6504, 0.6645]$|$z = [0.2000, 0.4000]$|$q_F = [0.6504, 0.6645, 0.2000, 0.4000]$|

### 5. Discriminator Forward Pass $D(x, z)$

- **Layer 1 Parameters:** _(Corrected bias vector length to 3)_
    
      
    
    $$W_{D1} = \begin{bmatrix} 0.2 & 0.3 & 0.1 & 0.4 \\ 0.3 & 0.1 & 0.2 & 0.2 \\ 0.4 & 0.2 & 0.3 & 0.1 \end{bmatrix}, \quad b_{D1} = \begin{bmatrix} 0.1 \\ 0.1 \\ 0.1 \end{bmatrix}$$
    
- **Layer 2 Parameters:**
    
      
    
    $$W_{D2} = \begin{bmatrix} 0.3 & 0.4 & 0.2 \end{bmatrix}, \quad b_{D2} = 0.1$$
    
      
    

#### A. Real Pair Forward Pass: $D(q_R)$

- **Hidden Layer:**
    
      
    - $d_1$: $a = 0.2(0.8000) + 0.3(0.6000) + 0.1(0.6624) + 0.4(0.6401) + 0.1 = \mathbf{0.7623} \implies \sigma(0.7623) = \mathbf{0.6818}$
        
          
        
    - $d_2$: $a = 0.3(0.8000) + 0.1(0.6000) + 0.2(0.6624) + 0.2(0.6401) + 0.1 = \mathbf{0.6605} \implies \sigma(0.6605) = \mathbf{0.6594}$
        
          
        
    - $d_3$: $a = 0.4(0.8000) + 0.2(0.6000) + 0.3(0.6624) + 0.1(0.6401) + 0.1 = \mathbf{0.8027} \implies \sigma(0.8027) = \mathbf{0.6906}$ _(Note: PDF had a `7.4` typo for `0.4`, though `0.8027` was computed using `0.4`)_
        
          
        
- **Output Layer:**
    
      
    
    $$a_{D,R} = 0.3(0.6818) + 0.4(0.6594) + 0.2(0.6906) + 0.1 = \mathbf{0.7064}$$
    
    $$D(q_R) = \sigma(0.7064) = \mathbf{0.6696}$$
    
      
    

#### B. Fake Pair Forward Pass: $D(q_F)$

- **Hidden Layer:**
    
      
    - $d_1$: $a = 0.2(0.6504) + 0.3(0.6645) + 0.1(0.2000) + 0.4(0.4000) + 0.1 = \mathbf{0.6094} \implies \sigma(0.6094) = \mathbf{0.6478}$
        
          
        
    - $d_2$: $a = 0.3(0.6504) + 0.1(0.6645) + 0.2(0.2000) + 0.2(0.4000) + 0.1 = \mathbf{0.4816} \implies \sigma(0.4816) = \mathbf{0.6181}$
        
          
        
    - $d_3$: $a = 0.4(0.6504) + 0.2(0.6645) + 0.3(0.2000) + 0.1(0.4000) + 0.1 = \mathbf{0.5931} \implies \sigma(0.5931) = \mathbf{0.6441}$
        
          
        
- **Output Layer:**
    
      
    
    $$a_{D,F} = 0.3(0.6478) + 0.4(0.6181) + 0.2(0.6441) + 0.1 = \mathbf{0.6704}$$
    
    $$D(q_F) = \sigma(0.6704) = \mathbf{0.6616}$$
    
      
    

### 6. Adversarial Losses & Summary

#### Discriminator Binary Cross-Entropy Loss ($L_D$)

$D$ wants $D(q_R) \to 1$ and $D(q_F) \to 0$:

  

$$L_D = -\ln(D(q_R)) - \ln(1 - D(q_F))$$

- **Real Pair Loss:** $L_{\text{real}} = -\ln(0.6696) = \mathbf{0.4011}$
    
      
    
- **Fake Pair Loss:** $L_{\text{fake}} = -\ln(1 - 0.6616) = -\ln(0.3384) = \mathbf{1.0835}$
    
      
    
- **Total Discriminator Loss:**
    
      
    
    $$L_D = 0.4011 + 1.0835 = \mathbf{1.4846}$$
    
      
    

#### Encoder–Generator Minimax Objective ($L_{EG}$)

In the minimax formulation $\min_{G, E} \max_{D} V(D, E, G)$, the joint minimization objective for $E$ and $G$ is the exact negation of $L_D$ (using natural log $\ln$):

  

$$L_{EG} = \ln(D(q_R)) + \ln(1 - D(q_F)) = -0.4011 - 1.0835 = \mathbf{-1.4846}$$

  

#### Final Summary Table

|**Quantity**|**Value**|
|---|---|
|**Encoder Output $E(X)$**|$[0.6624, 0.6401]$|
|**Generator Output $G(z)$**|$[0.6504, 0.6645]$|
|**Discriminator Real Prediction $D(q_R)$**|$0.6696$|
|**Discriminator Fake Prediction $D(q_F)$**|$0.6616$|
|**Real-Pair BCE Loss**|$0.4011$|
|**Fake-Pair BCE Loss**|$1.0835$|
|**Total Discriminator Loss ($L_D$)**|$\mathbf{1.4846}$|
|**Encoder–Generator Minimax Objective ($L_{EG}$)**|$\mathbf{-1.4846}$|
