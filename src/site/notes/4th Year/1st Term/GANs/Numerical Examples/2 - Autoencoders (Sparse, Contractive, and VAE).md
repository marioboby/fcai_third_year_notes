---
{"dg-publish":true,"permalink":"/4th-year/1st-term/ga-ns/numerical-examples/2-autoencoders-sparse-contractive-and-vae/","dg-note-properties":{}}
---

### Sparse Autoencoder (SAE): Numerical Example

#### 1. Setup & Formula

A Sparse Autoencoder regularizes activations $a_j \in (0, 1)$ across a dataset or batch of size $M$ by penalizing the empirical average activation $\hat{\rho}_j$ when it diverges from a target sparsity parameter $\rho$:

  

$$\hat{\rho}_j = \frac{1}{M}\sum_{m=1}^{M} a_j(x^{(m)})$$

The penalty is calculated using the Bernoulli Kullback-Leibler (KL) divergence summed over all $s$ hidden units:

  

$$\mathcal{L}_\rho = \sum_{j=1}^{s} \text{KL}(\rho \parallel \hat{\rho}_j) = \sum_{j=1}^{s} \left(\rho \log\frac{\rho}{\hat{\rho}_j} + (1 - \rho)\log\frac{1 - \rho}{1 - \hat{\rho}_j}\right)$$

- **Batch size**: $M = 100$
    
      
    
- **Target sparsity**: $\rho = 0.10$ ($10\%$)
    
      
    
- **Hidden layer size**: $s = 2$ neurons
    
      
    
- **Regularization weight**: $\lambda$
    
      
    

#### 2. Forward Pass & Empirical Activation Rates

Pass all 100 samples through the encoder to obtain the average activation for each hidden neuron:

  

- For Neuron 1 ($j=1$):
    
      
    
    $$\hat{\rho}_1 = \frac{1}{100}\sum_{i=1}^{100} a_1(x^{(i)}) = 0.30$$
    
    _(Neuron 1 is firing at $3\times$ the target rate $\rho = 0.10$)_
    
      
    
- For Neuron 2 ($j=2$):
    
      
    
    $$\hat{\rho}_2 = \frac{1}{100}\sum_{i=1}^{100} a_2(x^{(i)}) = 0.08$$
    
    _(Neuron 2 is firing very close to target rate $\rho = 0.10$)_
    
      
    

#### 3. Sparsity Penalty Calculation (KL Divergence)

Using natural logarithms:

  

- **Neuron 1 Penalty ($\text{KL}_1$):**
    
      
    
    $$\text{KL}(\rho \parallel \hat{\rho}_1) = 0.1 \log\left(\frac{0.1}{0.3}\right) + 0.9 \log\left(\frac{0.9}{0.7}\right)$$
    
    $$\text{KL}_1 = 0.1 \log(0.3333) + 0.9 \log(1.2857) = 0.1(-0.4771) + 0.9(0.1091) = 0.05048$$
    
- **Neuron 2 Penalty ($\text{KL}_2$):**
    
      
    
    $$\text{KL}(\rho \parallel \hat{\rho}_2) = 0.1 \log\left(\frac{0.1}{0.08}\right) + 0.9 \log\left(\frac{0.9}{0.92}\right)$$
    
    $$\text{KL}_2 = 0.1 \log(1.25) + 0.9 \log(0.97826) = 0.1(0.0969) + 0.9(-0.0095) \approx 0.0011$$
    
- **Total Sparsity Loss ($\mathcal{L}_\rho$):**
    
      
    
    $$\mathcal{L}_\rho = \text{KL}_1 + \text{KL}_2 = 0.05048 + 0.0011 = 0.05158$$
    

#### 4. Decoder Pass & Reconstruction Error (MSE)

For a single sample with hidden activations $h = [0.8, 0.2]$:

  

- **Original vector ($x$):** $x = [1, 0, 1, 0]$ ($n = 4$)
    
      
    
- **Reconstructed vector ($\hat{x}$):** $\hat{x} = [0.9, 0.1, 0.8, 0.2]$
    
      
    

Calculate the Mean Squared Error across dimensions:

  

$$\mathcal{L}_{\text{MSE}} = \frac{1}{4} \sum_{i=1}^{4} (x_i - \hat{x}_i)^2$$

$$\mathcal{L}_{\text{MSE}} = \frac{(1 - 0.9)^2 + (0 - 0.1)^2 + (1 - 0.8)^2 + (0 - 0.2)^2}{4} = \frac{0.01 + 0.01 + 0.04 + 0.04}{4} = \frac{0.10}{4} = 0.025$$

#### 5. Total Loss Calculation

$$\mathcal{L}_{\text{total}} = \mathcal{L}_{\text{MSE}} + \lambda\mathcal{L}_\rho = 0.025 + 0.05158\lambda$$
---
### Contractive Autoencoder (CAE): Numerical Example

#### 1. Setup & Formula

A Contractive Autoencoder adds an explicit penalty directly to the objective function using the squared Frobenius norm of the encoder's Jacobian matrix $J_f(x)$:

  

$$\Vert{}J_f(x)\Vert{}_F^2 = \sum_{i,j} \left(\frac{\partial h_j(x)}{\partial x_i}\right)^2$$

$$\mathcal{J}_{\text{CAE}}(\theta) = \sum_{x \in \mathcal{D}_n} \left(L(x, g(f(x))) + \lambda \Vert{}J_f(x)\Vert{}_F^2\right)$$

- **Input vector ($x$):**
    
      
    
    $$x = \begin{bmatrix} x_1 \\ x_2 \end{bmatrix} = \begin{bmatrix} 1 \\ 2 \end{bmatrix}$$
    
- **Encoder functions ($y = f(x)$):**
    
      
    
    $$y_1 = 2x_1 + x_2$$
    
    $$y_2 = x_1 + 3x_2$$
    

#### 2. Forward Pass (Outputs)

Evaluate encoder activations at the input point $(1, 2)$:

  

- $y_1 = 2(1) + (2) = 4$
    
      
    
- $y_2 = (1) + 3(2) = 7$
    
      
    
- **Output vector ($y$):**
    
      
    
    $$y = \begin{bmatrix} y_1 \\ y_2 \end{bmatrix} = \begin{bmatrix} 4 \\ 7 \end{bmatrix}$$
    

#### 3. Jacobian Partial Derivatives

Compute first-order derivatives for each output dimension with respect to each input dimension:

  

- For $y_1 = 2x_1 + x_2$:
    
      
    - $\frac{\partial y_1}{\partial x_1} = 2$
        
          
        
    - $\frac{\partial y_1}{\partial x_2} = 1$
        
          
        
- For $y_2 = x_1 + 3x_2$:
    
      
    - $\frac{\partial y_2}{\partial x_1} = 1$
        
          
        
    - $\frac{\partial y_2}{\partial x_2} = 3$
        
          
        

Construct the full Jacobian matrix:

  

$$J_f(x) = \begin{bmatrix} \frac{\partial y_1}{\partial x_1} & \frac{\partial y_1}{\partial x_2} \\ \frac{\partial y_2}{\partial x_1} & \frac{\partial y_2}{\partial x_2} \end{bmatrix} = \begin{bmatrix} 2 & 1 \\ 1 & 3 \end{bmatrix}$$

#### 4. Contractive Penalty Calculation (Squared Frobenius Norm)

Sum the squares of all elements in the Jacobian matrix:

$$\Vert{}J_f(x)\Vert{}_F^2 = \left(\frac{\partial y_1}{\partial x_1}\right)^2 + \left(\frac{\partial y_1}{\partial x_2}\right)^2 + \left(\frac{\partial y_2}{\partial x_1}\right)^2 + \left(\frac{\partial y_2}{\partial x_2}\right)^2$$

$$\Vert{}J_f(x)\Vert{}_F^2 = 2^2 + 1^2 + 1^2 + 3^2 = 4 + 1 + 1 + 9 = 15$$

#### 5. Total Loss

$$\mathcal{J}_{\text{CAE}}(\theta) = \sum_{x \in \mathcal{D}_n} \left(L(x, g(f(x))) + \lambda 15 \right)$$

---

### Variational Autoencoder (VAE)

#### 1. Setup & Formula

A Variational Autoencoder maps input data into a parameterized latent distribution $q_\phi(z \mid x) = \mathcal{N}(\mu, \sigma^2)$ characterized by predicted mean ($\mu$) and standard deviation ($\sigma$) vectors.

  

- **Latent Dimension**: $D = 2$ neurons ($z \in \mathbb{R}^2$)
    
      
    
- **Reparameterization Trick**: Ensures differentiability by decoupling stochasticity:
    
      
    
    $$z = \mu + \sigma \odot \epsilon, \quad \text{where } \epsilon \sim \mathcal{N}(0, I)$$
    
      
    
- **Reconstruction Loss (MSE)**:
    
      
    
    $$\mathcal{L}_{\text{rec}} = \frac{1}{n} \sum_{i=1}^{n} (x_i - \hat{x}_i)^2$$
    
      
    
- **Latent Regularization (KL Divergence to Standard Normal $\mathcal{N}(0, I)$)**:
    
      
    
    $$D_{\text{KL}}\left(\mathcal{N}(\mu, \sigma^2) \,\parallel\, \mathcal{N}(0, 1)\right) = -\frac{1}{2} \sum_{j=1}^{D} \left(1 + \ln(\sigma_j^2) - \mu_j^2 - \sigma_j^2\right)$$
    
      
    
- **Total Loss**:
    
      
    
    $$\mathcal{L}_{\text{total}} = \mathcal{L}_{\text{rec}} + \beta \mathcal{L}_{\text{KL}}$$
    
      
    

#### 2. Sample 1 ($I_1$) Walkthrough

**Input & Encoder Output:**

  

- **Input Vector ($x^{(1)}$)**: $x = [0, 1]$
    
      
    
- **Predicted Distribution Parameters**:
    
      
    - $\mu_1 = 0.2, \quad \mu_2 = -0.1$
        
          
        
    - $\sigma_1 = 0.25, \quad \sigma_2 = 0.5$
        
          
        

**Reparameterization Trick (Latent Sampling):** Draw random noise vector $\epsilon = [1.0, -0.8]$:

  

- $z_1 = \mu_1 + \sigma_1 \epsilon_1 = 0.2 + (0.25)(1.0) = 0.45$
    
      
    
- $z_2 = \mu_2 + \sigma_2 \epsilon_2 = -0.1 + (0.5)(-0.8) = -0.50$
    
      
    
- **Latent Vector**: $z = [0.45, -0.50]$
    
      
    

**Decoder Pass (Reconstruction):** Pass $z$ through the decoder network to reconstruct the original input:

  

- Reconstructed output: $\hat{x}^{(1)} = [0.269, 0.622]$
    
      
    

**Reconstruction Loss ($\mathcal{L}_{\text{rec}}^{(1)}$):**

  

$$\mathcal{L}_{\text{rec}}^{(1)} = \frac{(0 - 0.269)^2 + (1 - 0.622)^2}{2} = \frac{0.0724 + 0.1429}{2} \approx 0.311$$

  

**KL Divergence Loss ($\mathcal{L}_{\text{KL}}^{(1)}$):** Compute dimension-wise divergence against standard normal prior:

  

- **Dimension 1**:
    
      
    
    $$\text{KL}_{11} = -\frac{1}{2} \left(1 + \ln(0.25^2) - 0.2^2 - 0.25^2\right) \approx 0.9375$$
    
      
    
- **Dimension 2**:
    
      
    
    $$\text{KL}_{12} = -\frac{1}{2} \left(1 + \ln(0.5^2) - (-0.1)^2 - 0.5^2\right) \approx 0.0451$$
    
      
    
- **Total KL for $I_1$**:
    
      
    
    $$\mathcal{L}_{\text{KL}}^{(1)} = \text{KL}_{11} + \text{KL}_{12} = 0.9375 + 0.0451 = 0.9826$$
    
      
    

#### 3. Sample 2 ($I_2$) Walkthrough

**Input & Encoder Output:**

  

- **Input Vector ($x^{(2)}$)**: $x = [1, 0]$
    
      
    
- **Predicted Distribution Parameters**:
    
      
    - $\mu_1 = 0.2, \quad \mu_2 = 0.3$
        
          
        
    - $\sigma_1 = 0.8, \quad \sigma_2 = 0.5$
        
          
        

**Reparameterization Trick (Latent Sampling):** Draw random noise vector $\epsilon = [-1.0, 1.0]$:

  

- $z_1 = \mu_1 + \sigma_1 \epsilon_1 = 0.2 + (0.8)(-1.0) = -0.60$
    
      
    
- $z_2 = \mu_2 + \sigma_2 \epsilon_2 = 0.3 + (0.5)(1.0) = 0.80$
    
      
    
- **Latent Vector**: $z = [-0.60, 0.80]$
    
      
    

**Decoder Pass (Reconstruction):** Pass $z$ through the decoder network to reconstruct the input:

  

- Reconstructed output: $\hat{x}^{(2)} = [0.775, 0.378]$
    
      
    

**Reconstruction Loss ($\mathcal{L}_{\text{rec}}^{(2)}$):**

  

$$\mathcal{L}_{\text{rec}}^{(2)} = \frac{(1 - 0.775)^2 + (0 - 0.378)^2}{2} = \frac{0.0506 + 0.1429}{2} \approx 0.161$$

  

**KL Divergence Loss ($\mathcal{L}_{\text{KL}}^{(2)}$):** Compute dimension-wise divergence against standard normal prior:

  

- **Dimension 1**:
    
      
    
    $$\text{KL}_{21} = -\frac{1}{2} \left(1 + \ln(0.8^2) - 0.2^2 - 0.8^2\right) \approx 0.0631$$
    
      
    
- **Dimension 2**:
    
      
    
    $$\text{KL}_{22} = -\frac{1}{2} \left(1 + \ln(0.5^2) - 0.3^2 - 0.5^2\right) \approx 0.3630$$
    
      
    
- **Total KL for $I_2$**:
    
      
    
    $$\mathcal{L}_{\text{KL}}^{(2)} = \text{KL}_{21} + \text{KL}_{22} = 0.0631 + 0.3630 = 0.4261$$
    
      
    

#### 4. Total Loss Calculation (Weighted by $\beta$)

Combining reconstruction and regularized latent distribution penalties with regularization weight $\beta$:

  

- **Total Loss for Sample 1 ($I_1$)** (with $\beta = 0.5$):
    
      
    
    $$\mathcal{L}_1 = \mathcal{L}_{\text{rec}}^{(1)} + \beta \mathcal{L}_{\text{KL}}^{(1)} = 0.311 + 0.5(0.9826) = 0.311 + 0.4913 \approx 0.7545$$
    
      
    
- **Total Loss for Sample 2 ($I_2$)** (with $\beta = 0.5$):
    
      
    
    $$\mathcal{L}_2 = \mathcal{L}_{\text{rec}}^{(2)} + \beta \mathcal{L}_{\text{KL}}^{(2)} = 0.161 + 0.5(0.4261) = 0.161 + 0.21305 \approx 0.3741$$
    
      
    

#### 5. Image Generation via Decoder Sampling

To generate a completely novel image, bypass the encoder entirely:

  

1. Sample a random point directly from the standard normal latent prior: $z_{\text{gen}} \sim \mathcal{N}(0, I)$
    
      
    
2. Pass $z_{\text{gen}}$ into the decoder network $g(z)$ to synthesize a new output image $\hat{x}_{\text{gen}}$