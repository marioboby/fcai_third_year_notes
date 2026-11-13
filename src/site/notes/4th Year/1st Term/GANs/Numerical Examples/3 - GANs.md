---
{"dg-publish":true,"permalink":"/4th-year/1st-term/ga-ns/numerical-examples/3-ga-ns/","dg-note-properties":{}}
---

  

## Task 1: GAN Forward Pass & Loss Calculation

### Problem Setup & Network Parameters

- **Input Noise Vector ($z$):**
    
      
    
    $$z = \begin{bmatrix} 1 \\ 1 \end{bmatrix}$$
    
- **Real Image Vector ($x_{\text{real}}$):**
    
      
    
    $$x_{\text{real}} = \begin{bmatrix} 1 \\ 0 \\ 0 \\ 1 \end{bmatrix}$$
    
- **Activation Function:** Sigmoid $\sigma(u) = \frac{1}{1 + e^{-u}}$ applied to all layers (Generator hidden, Generator output, Discriminator hidden, and Discriminator output).
    
      
    
- **Weights & Biases:** All weights $W = 1$ and all biases $b = 0$ across both networks, **except** for the Discriminator's output layer weights, which are $W_{\text{out}}^{(D)} = \begin{bmatrix} 0.5 & 0.5 \end{bmatrix}$.
    
      
    
![Pasted image 20261113164827.png](/img/user/imgs/Pasted%20image%2020261113164827.png)
### Step 1: Generator Forward Pass $G(z)$

1. **Hidden Layer ($h_1, h_2$):**
    
      
    - Pre-activation:
        
          
        
        $$z_{h_1} = (1 \cdot 1) + (1 \cdot 1) + 0 = 2$$
        
    - Activation:
        
          
        
        $$h_1 = \sigma(2) = \frac{1}{1 + e^{-2}} \approx \mathbf{0.8808}$$
        
    - By symmetry ($W=1, b=0$), $h_2 = \sigma(2) \approx \mathbf{0.8808}$ (written as $0.8803$ in the handwritten scan—use $0.8808$ for exact precision).
        
          
        
2. **Output Layer / Generated Image ($x_{\text{gen}}$):**
    
      
    - Each of the 4 output pixels receives inputs from $h_1$ and $h_2$:
        
          
        
        $$z_{\text{out}} = (0.8808 \cdot 1) + (0.8808 \cdot 1) + 0 = 1.7616$$
        
    - Activation:
        
          
        
        $$\sigma(1.7616) = \frac{1}{1 + e^{-1.7616}} \approx \mathbf{0.8534}$$
        
    - Generated Image Vector:
        
          
        
        $$x_{\text{gen}} = G(z) = \begin{bmatrix} 0.8534 \\ 0.8534 \\ 0.8534 \\ 0.8534 \end{bmatrix}$$
        
          
        

### Step 2: Discriminator Forward Pass on Fake Image $D(G(z))$

1. **Hidden Layer ($h_3, h_4$):**
    
      
    - Pre-activation (summing all 4 pixels from $x_{\text{gen}}$):
        
          
        
        $$z_{h_3} = 4 \times (0.8534 \cdot 1) + 0 = 3.4136$$
        
    - Activation:
        
          
        
        $$h_3 = \sigma(3.4136) = \frac{1}{1 + e^{-3.4136}} \approx \mathbf{0.9681}$$
        
    - By symmetry, $h_4 = \mathbf{0.9681}$.
        
          
        
2. **Output Layer $D(G(z))$:**
    
      
    - Pre-activation (using output weights $0.5$ and $0.5$):
        
          
        
        $$z_{D_{\text{fake}}} = (0.9681 \cdot 0.5) + (0.9681 \cdot 0.5) + 0 = 0.9681$$
        
    - Activation:
        
          
        
        $$D(G(z)) = \sigma(0.9681) = \frac{1}{1 + e^{-0.9681}} \approx \mathbf{0.7247}$$
        
          
        

### Step 3: Discriminator Forward Pass on Real Image $D(x_{\text{real}})$

1. **Hidden Layer ($h_3, h_4$):**
    
      
    - Pre-activation (summing all 4 pixels from $x_{\text{real}} = \begin{bmatrix} 1 & 0 & 0 & 1 \end{bmatrix}^T$):
        
          
        
        $$z_{h_3} = (1 \cdot 1) + (0 \cdot 1) + (0 \cdot 1) + (1 \cdot 1) + 0 = 2$$
        
    - Activation:
        
          
        
        $$h_3 = \sigma(2) = \frac{1}{1 + e^{-2}} \approx \mathbf{0.8808}$$
        
    - By symmetry, $h_4 = \mathbf{0.8808}$.
        
          
        
2. **Output Layer $D(x_{\text{real}})$:**
    
      
    - Pre-activation:
        
          
        
        $$z_{D_{\text{real}}} = (0.8808 \cdot 0.5) + (0.8808 \cdot 0.5) + 0 = 0.8808$$
        
    - Activation:
        
          
        
        $$D(x_{\text{real}}) = \sigma(0.8808) = \frac{1}{1 + e^{-0.8808}} \approx \mathbf{0.7070}$$
        
          
        

### Step 4: Loss Calculations (Minimax Formulation)

(Note: Deep learning loss functions use the **natural logarithm** $\ln$. My handwritten sheet calculated $-0.711$ and $-0.56$ because a calculator's $\log$ button defaults to base-10 $\log_{10}$; below are both the exact $\ln$ values and the $\log_{10}$ values for clarity.)

  

1. **Discriminator Loss ($L_D$):**
    
      
    - **Formula (Minimax Objective):**
        
          
        
        $$L_D = \ln(D(x_{\text{real}})) + \ln(1 - D(G(z)))$$
        
    - **Exact Calculation (Natural Log $\ln$):**
        
          
        
        $$L_D = \ln(0.7070) + \ln(1 - 0.7247) = -0.3467 + (-1.2899) = \mathbf{-1.6366}$$
        
    - (Base-10 calculator check: $\log_{10}(0.7070) + \log_{10}(0.2753) = -0.1506 - 0.5602 = -0.7108 \approx -0.711$)
        
          
        
2. **Generator Loss ($L_G$):**
    
      
    - **Formula (Original Saturating Minimax Loss):**
        
          
        
        $$L_G = \ln(1 - D(G(z)))$$
        
    - **Exact Calculation (Natural Log $\ln$):**
        
          
        
        $$L_G = \ln(1 - 0.7247) = \ln(0.2753) \approx \mathbf{-1.2899}$$
        
    - (Base-10 calculator check: $\log_{10}(0.2753) \approx -0.5602 \approx -0.56$)
          
        

## Task 2: Essay Questions & Neural Network Forward Pass

### Question 1: VAEs vs. GANs

|**Feature**|**Variational Autoencoders (VAEs)**|**Vanilla GANs**|
|---|---|---|
|**Architecture**|**Encoder–Decoder** network|**Generator–Discriminator** adversarial pair|
|**Latent Mapping**|**Bidirectional** ($x \rightarrow z$ via Encoder, $z \rightarrow x$ via Decoder)|**Unidirectional** ($z \rightarrow x$ only; no built-in encoder)|
|**Training Objective**|Maximizes the Evidence Lower Bound (**ELBO**): Reconstruction Loss + KL Divergence|**Minimax game** (Nash equilibrium between $G$ and $D$)|
|**Output Quality**|Smooth, often **blurry** (due to pixel-wise MSE/BCE averaging)|**Sharp, realistic**, and high-frequency detailed|
|**Training Stability**|Stable, well-behaved convergence|Unstable (prone to **mode collapse** and vanishing gradients)|

### Question 2: When to Use BiGAN (Bidirectional GAN) vs. Normal GAN

Use a **BiGAN** instead of a standard GAN when the task requires **mapping real data back into the latent space** ($x \rightarrow z$) in addition to generating samples ($z \rightarrow x$). Because a vanilla GAN lacks an encoder, it cannot infer the latent representation of a given real input.

  

**Key Use Cases:**

  

1. **Unsupervised Feature Learning:** Extracting rich, semantic latent representations from unlabeled datasets to use in downstream tasks (e.g., classification).
    
      
    
2. **Anomaly Detection:** Encoding a test sample $x$ into $z = E(x)$, reconstructing $\hat{x} = G(E(x))$, and measuring the reconstruction/discriminator discrepancy—anomalies fail to reconstruct accurately.
    
      
    
3. **Semantic Image Editing:** Projecting a real image into its latent code $z$, modifying specific attributes in latent space (e.g., expression, lighting), and decoding it back into a realistic edited image.
    
      
    

### Bonus Exercise (Page 4): Multi-Layer Perceptron Forward Pass

#### Given Parameters

- **Inputs:** $x_1 = 0.8, \quad x_2 = 0.6$
    
      
    
- **Hidden Layer (3 neurons: $h_1, h_2, h_3$):**
    
      
    - Weights from $x_1$: $w_{11} = 0.2, \quad w_{12} = 0.4, \quad w_{13} = 0.5$
        
          
        
    - Weights from $x_2$: $w_{21} = 0.3, \quad w_{22} = 0.1, \quad w_{23} = 0.2$
        
          
        
    - Biases: $b_1 = 0.1, \quad b_2 = 0.2, \quad b_3 = 0.1$
        
          
        
- **Output Layer (2 neurons: $h_4, h_5$):**
    
      
    - Weights to $h_4$: $w_{h_1 \to 4} = 0.2, \quad w_{h_2 \to 4} = 0.3, \quad w_{h_3 \to 4} = 0.4, \quad b_4 = 0$
        
          
        
    - Weights to $h_5$: $w_{h_1 \to 5} = 0.2, \quad w_{h_2 \to 5} = 0.1, \quad w_{h_3 \to 5} = 0.2, \quad b_5 = 0.1$
        
          
        
- **Activation Function:** Sigmoid $\sigma(z) = \frac{1}{1 + e^{-z}}$
    
      
    

#### Step-by-Step Verification

1. **Hidden Neuron $h_1$:**
    
      
    
    $$z_1 = (0.8 \times 0.2) + (0.6 \times 0.3) + 0.1 = 0.16 + 0.18 + 0.10 = \mathbf{0.44}$$
    
    $$h_1 = \sigma(0.44) = \frac{1}{1 + e^{-0.44}} \approx \mathbf{0.6083} \quad \text{(Correct)}$$
    
      
    
2. **Hidden Neuron $h_2$:**
    
      
    
    $$z_2 = (0.8 \times 0.4) + (0.6 \times 0.1) + 0.2 = 0.32 + 0.06 + 0.20 = \mathbf{0.58}$$
    
    $$h_2 = \sigma(0.58) = \frac{1}{1 + e^{-0.58}} \approx \mathbf{0.6411} \quad \text{(Correct)}$$
    
      
    
3. **Hidden Neuron $h_3$:**
    
      
    
    $$z_3 = (0.8 \times 0.5) + (0.6 \times 0.2) + 0.1 = 0.40 + 0.12 + 0.10 = \mathbf{0.62}$$
    
    $$h_3 = \sigma(0.62) = \frac{1}{1 + e^{-0.62}} \approx \mathbf{0.6502} \quad \text{(Correct)}$$
    
      
    
4. **Output Neuron $h_4$:**
    
      
    
    $$z_4 = (0.6083 \times 0.2) + (0.6411 \times 0.3) + (0.6502 \times 0.4) + 0$$
    
    $$z_4 = 0.1217 + 0.1923 + 0.2601 = \mathbf{0.5741}$$
    
    $$h_4 = \sigma(0.5741) = \frac{1}{1 + e^{-0.5741}} \approx \mathbf{0.6397}$$
    
    _(Correction: In the notes, $z_4$ was misadded as $0.674$ instead of $0.574$, which led to $\sigma(0.674) = 0.6624$.)_
    
      
    
5. **Output Neuron $h_5$:**
    
      
    
    $$z_5 = (0.6083 \times 0.2) + (0.6411 \times 0.1) + (0.6502 \times 0.2) + 0.1$$
    
    $$z_5 = 0.1217 + 0.0641 + 0.1300 + 0.1000 = \mathbf{0.4158}$$
    
    $$h_5 = \sigma(0.4158) = \frac{1}{1 + e^{-0.4158}} \approx \mathbf{0.6025}$$
    
    _(Correction: In the notes, $z_5$ was miscalculated as $0.5757$, giving $0.6401$. Recomputing the weighted sum with $b_5 = 0.1$ yields $0.4158$ and $h_5 \approx 0.6025$.)_