---
{"dg-publish":true,"permalink":"/4th-year/1st-term/selected-topics/numerical-examples/3-dense-net/","dg-note-properties":{}}
---

### 1. Convolutional Layer Sizing, Parameter Counts, and MACs (Slide 3)

- **Spatial Output Sizing Rule**:
    
      
    
    $$\text{Output Size} = \left\lfloor \frac{n + 2p - f}{s} \right\rfloor + 1$$
    
    - **Example Values**: Input dimension $n = 224$, kernel size $f = 7$, padding $p = 3$, stride $s = 2$.
        
          
        
    - **Calculation**:
        
          
        
        $$\left\lfloor \frac{224 + 2(3) - 7}{2} \right\rfloor + 1 = \left\lfloor \frac{223}{2} \right\rfloor + 1 = 111 + 1 = 112$$
        
- **Convolutional Weight Accounting (No Bias due to BN)**:
    
      
    
    $$\text{Parameters} = f \times f \times C_{\text{in}} \times C_{\text{out}}$$
    
    - **Example Values**: Kernel $3 \times 3$, input channels $C_{\text{in}} = 128$, output channels $C_{\text{out}} = 32$.
        
          
        
    - **Calculation**:
        
          
        
        $$3 \times 3 \times 128 \times 32 = 9 \times 128 \times 32 = 36,864\text{ weights}$$
        
- **Convolutional Computational Cost (MACs)**:
    
      
    
    $$\text{MACs} = \text{Parameters} \times H_{\text{out}} \times W_{\text{out}}$$
    
    - **Example Values**: $36,864$ parameters across an output resolution of $14 \times 14$.
        
          
        
    - **Calculation**:
        
          
        
        $$36,864 \times 14 \times 14 = 36,864 \times 196 = 7,225,344\text{ MACs}$$
        
- **Batch Normalization Parameter Count**:
    
      
    
    $$\text{Learnable Parameters} = 2 \times C \quad (\gamma \text{ scale and } \beta \text{ shift per channel})$$
    
    - **Example**: For $C = 64$ channels $\rightarrow 64 \times 2 = 128\text{ parameters}$.
        
          
        
- **Average Pooling & Channel Concatenation**:
    
      
    - **Average Pooling ($2 \times 2, s=2$)**: $0$ parameters; transforms $56 \times 56 \rightarrow 28 \times 28$.
        
          
        
    - **Concatenation**: $0$ parameters; stacks $64\text{ channels} + 32\text{ channels} \rightarrow 96\text{ channels}$.
        
          
        

### 2. Vanishing Gradient Decay Model (Slide 4)

- **Decay Rule**: Assuming each layer scales backpropagated gradients by a constant factor of $0.5$, the gradient reaching Layer 1 after traversing $L$ layers is:
    
      
    
    $$\text{Gradient at Layer 1} = 0.5^L$$
    

| **Layers ($L$)** | **Mathematical Term** | **Resulting Gradient Scale**       |
| ---------------- | --------------------- | ---------------------------------- |
| **$L = 1$**      | $0.5^1$               | $0.5$                              |
| **$L = 5$**      | $0.5^5$               | $0.03125$                          |
| **$L = 10$**     | $0.5^{10}$            | $0.0009765625 \approx 0.00098$     |
| **$L = 20$**     | $0.5^{20}$            | $0.00000095367 \approx 0.00000095$ |

### 3. Layer Connection Combinatorics Across Topologies (Slides 5, 7)

- **Topology Formulas for a Block of $L$ Layers**:
    
      
    - **Plain CNN**: $L$ connections
        
          
        
    - **ResNet**: $2L$ connections ($L\text{ main} + L\text{ skip additions}$)
        
          
        
    - **DenseNet**: $\sum_{i=1}^L i = \frac{L(L+1)}{2}$ direct concatenations
        
          
        

| **Block Depth ($L$)**                 | **Plain ($L$)** | **ResNet ($2L$)** | **DenseNet $\frac{L(L+1)}{2}$** |
| ------------------------------------- | --------------- | ----------------- | ------------------------------- |
| **$L = 4$**                           | $4$             | $8$               | $\frac{4 \times 5}{2} = 10$     |
| **$L = 6$** _(DenseNet-121 Block 1)_  | $6$             | $12$              | $\frac{6 \times 7}{2} = 21$     |
| **$L = 12$** _(DenseNet-121 Block 2)_ | $12$            | $24$              | $\frac{12 \times 13}{2} = 78$   |
| **$L = 16$** _(DenseNet-121 Block 4)_ | $16$            | $32$              | $\frac{16 \times 17}{2} = 136$  |
| **$L = 24$** _(DenseNet-121 Block 3)_ | $24$            | $48$              | $\frac{24 \times 25}{2} = 300$  |

### 4. Addition vs. Concatenation Tensor Walkthrough (Slide 6)

- **Inputs**:
    
      
    
    $$A = \begin{bmatrix} 1 & 2 \\ 3 & 4 \end{bmatrix} \quad (\text{shape } 2 \times 2 \times 1), \quad F(A) = \begin{bmatrix} 0 & 1 \\ 1 & 0 \end{bmatrix} \quad (\text{shape } 2 \times 2 \times 1)$$
    
- **ResNet Addition ($A + F(A)$)**:
    
      
    
    $$\begin{bmatrix} 1+0 & 2+1 \\ 3+1 & 4+0 \end{bmatrix} = \begin{bmatrix} 1 & 3 \\ 4 & 4 \end{bmatrix} \quad (\text{shape } 2 \times 2 \times 1)$$
    
    - _Result_: Information is irreversibly entangled into 1 channel.
        
          
        
- **DenseNet Concatenation ($[A, F(A)]$ )**:
    
      
    
    $$\text{Channel 1} = \begin{bmatrix} 1 & 2 \\ 3 & 4 \end{bmatrix}, \quad \text{Channel 2} = \begin{bmatrix} 0 & 1 \\ 1 & 0 \end{bmatrix} \quad (\text{shape } 2 \times 2 \times 2)$$
    
    - _Result_: Features remain unmixed; subsequent layers learn individual channel weights.
        
          
        

### 5. Dense Block Channel Growth Traces (Slides 8, 9)

- **Channel Growth Formulas**:
    
      
    - **Input channels to Layer $l$**: $C_{\text{in}}(l) = k_0 + k(l - 1)$
        
          
        
    - **Block output channels**: $C_{\text{out}} = k_0 + k \cdot L$
        
          
        
- **Toy Example ($k_0 = 2, k = 2, L = 4$)**:
    
      
    - Layer 1 input ($x_0$): $2 + 2(0) = 2\text{ ch}$; generates $x_1$ ($2\text{ ch}$)
        
          
        
    - Layer 2 input ($[x_0, x_1]$): $2 + 2(1) = 4\text{ ch}$; generates $x_2$ ($2\text{ ch}$)
        
          
        
    - Layer 3 input ($[x_0, x_1, x_2]$): $2 + 2(2) = 6\text{ ch}$; generates $x_3$ ($2\text{ ch}$)
        
          
        
    - Layer 4 input ($[x_0, x_1, x_2, x_3]$): $2 + 2(3) = 8\text{ ch}$; generates $x_4$ ($2\text{ ch}$)
        
          
        
    - Final Block Output: $k_0 + k \cdot L = 2 + 2(4) = 10\text{ channels}$.
        
          
        
- **DenseNet-121 Block 1 Trace ($k_0 = 64, k = 32, L = 6$)**:
    
      
    

|**Layer ($L$)**|**Input Channels (64+32(l−1)64 + 32(l-1)64+32(l−1))**|**New Maps (kkk)**|**Output Channels After Concat**|
|---|---|---|---|
|**Layer 1**|$64$|$32$|$64 + 32 = 96$|
|**Layer 2**|$96$|$32$|$96 + 32 = 128$|
|**Layer 3**|$128$|$32$|$128 + 32 = 160$|
|**Layer 4**|$160$|$32$|$160 + 32 = 192$|
|**Layer 5**|$192$|$32$|$192 + 32 = 224$|
|**Layer 6**|$224$|$32$|$224 + 32 = \mathbf{256}$|

### 6. Bottleneck Layer vs. Direct Conv: Break-Even Analysis (Slides 11, 12)

- **Bottleneck Invariant**: The $3 \times 3$ conv in a DenseNet-B layer always takes $4k$ input channels and produces $k$ output channels:
    
      
    
    $$\text{Weights}_{3\times3} = 3 \times 3 \times 4k \times k = 36k^2$$
    
    - For $k = 32 \rightarrow 36(32^2) = 9 \times 128 \times 32 = 36,864\text{ weights}$.
        
          
        
- **Case Study: Block 3, Layer 24 ($C_{\text{in}} = 992$, $k = 32$, spatial size $14 \times 14 = 196\text{ positions}$)**:
    
      
    - **Design A (Direct $3 \times 3$, $992 \rightarrow 32$)**:
        
          
        - Parameters: $9 \times 992 \times 32 = \mathbf{285,696}$
            
              
            
        - MACs: $285,696 \times 196 = \mathbf{55,996,416}$
            
              
            
    - **Design B (Bottleneck $1 \times 1: 992 \rightarrow 128$, then $3 \times 3: 128 \rightarrow 32$)**:
        
          
        - Parameters: $(1 \times 1 \times 992 \times 128) + (9 \times 128 \times 32) = 126,976 + 36,864 = \mathbf{163,840}$
            
              
            
        - MACs: $163,840 \times 196 = \mathbf{32,112,640}$
            
              
            
        - Savings: $\approx \mathbf{43\%}$ fewer parameters and MACs.
            
              
            
- **Early Layer Comparison (Block 1, Layer 6 — $C_{\text{in}} = 224, k = 32$)**:
    
      
    - Direct $3 \times 3$: $9 \times 224 \times 32 = \mathbf{64,512}$ parameters.
        
          
        
    - Bottleneck: $(224 \times 128) + 36,864 = 28,672 + 36,864 = \mathbf{65,536}$ parameters.
        
          
        
    - _Break-even Point_: $9 C_{\text{in}} k = C_{\text{in}}(4k) + 36k^2 \implies 5 C_{\text{in}} k = 36k^2 \implies C_{\text{in}} = 7.2k$. For $k=32$, the threshold is $\mathbf{\approx 230\text{ channels}}$.
        
          
        

### 7. Transition Layer Step-by-Step Numerical Walkthrough (Slide 15)

- **Setup**: Transition 1 receives Dense Block 1 output ($56 \times 56 \times 256$, $m = 256, \theta = 0.5$).
    
      
    
- **Step 1 (BN + ReLU)**: Shape unchanged ($56 \times 56 \times 256$).
    
      
    
- **Step 2 (Conv $1 \times 1, \theta = 0.5$)**:
    
      
    - Output channels: $\lfloor 0.5 \times 256 \rfloor = 128$
        
          
        
    - Conv weights: $256 \times 128 = 32,768$
        
          
        
    - Adding BN ($128 \times 2 = 256$ params per conv/BN stage): $32,768 + 512 = 33,280\text{ total params}$
        
          
        
- **Step 3 ($2 \times 2$ Average Pooling, $s = 2$)**:
    
      
    - Spatial output: $\lfloor(56 - 2)/2\rfloor + 1 = 28 \rightarrow 28 \times 28 \times 128$
        
          
        
    - Patch-wise mean aggregation over a $4 \times 4$ slice:
        
          
        
        $$\begin{bmatrix} 1 & 3 & 2 & 4 \\ 5 & 7 & 6 & 8 \\ 3 & 1 & 2 & 0 \\ 4 & 2 & 0 & 2 \end{bmatrix} \xrightarrow{\text{AvgPool}} \begin{bmatrix} \frac{1+3+5+7}{4} & \frac{2+4+6+8}{4} \\ \frac{3+1+4+2}{4} & \frac{2+0+0+2}{4} \end{bmatrix} = \begin{bmatrix} 4.0 & 5.0 \\ 2.5 & 1.0 \end{bmatrix}$$
        
- **Activation Volume Reduction**:
    
      
    - Input: $56 \times 56 \times 256 = 802,816\text{ values}$
        
          
        
    - Output: $28 \times 28 \times 128 = 100,352\text{ values}$
        
          
        
    - Ratio: $\frac{802,816}{100,352} = \mathbf{8\times\text{ reduction}}$ ($2\times\text{ channel compression} \times 4\times\text{ spatial pooling}$).
        
          
        

### 8. Stem and Classifier Parameter Sizing (Slide 18)

- **Stem Conv $7 \times 7, s=2, 64\text{ filters}$**:
    
      
    - Input $224 \times 224 \times 3 \rightarrow$ Output $112 \times 112 \times 64$
        
          
        
    - Parameters: $7 \times 7 \times 3 \times 64 = \mathbf{9,408\text{ weights}}$
        
          
        
- **Stem Max Pooling $3 \times 3, s=2$**:
    
      
    - Output shape: $56 \times 56 \times 64$ ($16\times$ fewer spatial pixels than the input: $56^2 = 3,136$ vs. $224^2 = 50,176$).
        
          
        
- **Global Average Pooling vs. Direct Flattening**:
    
      
    - Final feature map: $7 \times 7 \times 1,024$.
        
          
        
    - Global Average Pooling collapses $7 \times 7 \times 1,024 \rightarrow 1 \times 1 \times 1,024$ ($0$ parameters).
        
          
        
    - **FC Layer ($1,024 \rightarrow 1,000$)**:
        
          
        
        $$1,024 \times 1,000 + 1,000\text{ (bias)} = \mathbf{1,025,000\text{ weights}}$$
        
    - **Naive Flattening ($50,176 \rightarrow 1,000$)**:
        
          
        
        $$7 \times 7 \times 1,024 \times 1,000 = 50,176 \times 1,000 = \mathbf{50,176,000\text{ weights}} \quad (\approx 49\times\text{ larger})$$
        
          
        

### 9. DenseNet-121 Complete Layer-by-Layer Channel Trace (Slide 19)

|**Stage**|**Spatial Shape**|**Input Channels**|**Channels Added (L×k)**|**Block Output**|**Post-Transition (θ=0.5)**|
|---|---|---|---|---|---|
|**Dense Block 1 ($L=6$)**|$56 \times 56$|$64$|$6 \times 32 = 192$|$256$|$128$|
|**Dense Block 2 ($L=12$)**|$28 \times 28$|$128$|$12 \times 32 = 384$|$512$|$256$|
|**Dense Block 3 ($L=24$)**|$14 \times 14$|$256$|$24 \times 32 = 768$|$1,024$|$512$|
|**Dense Block 4 ($L=16$)**|$7 \times 7$|$512$|$16 \times 32 = 512$|$1,024$|None (Global Pool $\rightarrow 1,024$-d)|

- **Final Feature Dimension Without Compression ($\theta = 1$)**:
    
      
    
    $$64 + 192 + 384 + 768 + 512 = \mathbf{1,920\text{-d}}$$
    
      
    

### 10. Depth Counting Verification Across Variants (Slides 21, 27)

- **General Formula (ImageNet)**:
    
      
    
    $$\text{Depth} = 1\text{ (Stem)} + 2 \times \sum_{i=1}^4 n_i + 3\text{ (Transitions)} + 1\text{ (FC)} = 5 + 2\sum n_i$$
    
    - **DenseNet-121** ($[6, 12, 24, 16]$): $5 + 2(58) = 5 + 116 = \mathbf{121}$
        
          
        
    - **DenseNet-169** ($[6, 12, 32, 32]$): $5 + 2(82) = 5 + 164 = \mathbf{169}$
        
          
        
    - **DenseNet-201** ($[6, 12, 48, 32]$): $5 + 2(98) = 5 + 196 = \mathbf{201}$
        
          
        
    - **DenseNet-264** ($[6, 12, 64, 48]$): $5 + 2(130) = 5 + 260 = \mathbf{265}$ (named 264 by convention)
        
          
        
- **General Formula (CIFAR DenseNet-BC, 3 Blocks, $n$ layers/block)**:
    
      
    
    $$\text{Depth} = 1\text{ (Stem)} + 2 \times (3n) + 2\text{ (Transitions)} + 1\text{ (FC)} = 6n + 4$$
    
    - For $L = 100 \implies 6n + 4 = 100 \implies 6n = 96 \implies \mathbf{n = 16\text{ layers per block}}$.
        
          
        

### 11. Full Parameter Distribution of DenseNet-121 (Slide 26)

|**Architectural Component**|**Parameter Count**|**Share of Total**|
|---|---|---|
|**Stem + final BN**|$11,584$|$0.1\%$|
|**Dense Block 1 ($6$ layers)**|$335,040$|$4.2\%$|
|**Transition 1**|$33,280$|$0.4\%$|
|**Dense Block 2 ($12$ layers)**|$919,680$|$11.5\%$|
|**Transition 2**|$132,096$|$1.7\%$|
|**Dense Block 3 ($24$ layers)**|$2,837,760$|**$35.6\%$**|
|**Transition 3**|$526,336$|$6.6\%$|
|**Dense Block 4 ($16$ layers)**|$2,158,080$|$27.0\%$|
|**Classifier (FC $1024 \rightarrow 1000$)**|$1,025,000$|**$12.8\%$**|
|**Total**|**$7,978,856$**|**$100.0\%$**|

### 12. Quantitative Impact of Compression Factor $\theta$ (Slide 28)

- Comparison of DenseNet-121 with $\theta = 1.0$ vs. $\theta = 0.5$:
    
      
    - **Channels Entering Blocks 2 / 3 / 4**:
        
          
        - $\theta = 1.0 \rightarrow 256 / 640 / 1,408$
            
              
            
        - $\theta = 0.5 \rightarrow 128 / 256 / 512$
            
              
            
    - **Classifier Vector Dimension**: $1,920\text{-d}$ vs. $1,024\text{-d}$
        
          
        
    - **Combined Transition Conv Weights ($T_1 + T_2 + T_3$)**:
        
          
        - For $\theta = 1.0$:
            
              
            
            $$256^2 + 640^2 + 1,408^2 = 65,536 + 409,600 + 1,982,464 = \mathbf{2,457,600\text{ weights}}$$
            
        - For $\theta = 0.5$:
            
              
            
            $$(256 \times 128) + (512 \times 256) + (1024 \times 512) = 32,768 + 131,072 + 524,288 = \mathbf{688,128\text{ weights}}$$
            
        - Weight reduction: $\frac{2,457,600}{688,128} \approx \mathbf{3.6\times\text{ fewer weights}}$.
            
              
            

### 13. ImageNet Variants & Cross-Model Benchmarking (Slides 25, 29, 32)

- **ImageNet DenseNet Variants ($k=32$)**:
    
      
    - **DenseNet-121**: $\approx 8\text{M}$ params, $2.9\text{ GFLOPs}$, Top-1: $74.9\%$ (paper) / $74.4\%$ (torchvision)
        
          
        
    - **DenseNet-169**: $\approx 14\text{M}$ params, Top-1: $76.2\%$
        
          
        
    - **DenseNet-201**: $\approx 20\text{M}$ params, Top-1: $77.4\%$
        
          
        
    - **DenseNet-264**: $\approx 33\text{M}$ params, Top-1: $77.8\%$
        
          
        
    - _Scaling Note_: $169 \rightarrow 264$ adds $+1.6\%$ top-1 for a $2.4\times$ parameter increase ($14\text{M} \rightarrow 33\text{M}$).
        
          
        
- **DenseNet-121 vs. ResNet-50 vs. VGG-16**:
    
      
    

|**Metric**|**DenseNet-121**|**ResNet-50**|**VGG-16**|
|---|---|---|---|
|**Weighted Layers**|$121$|$50$|$16$|
|**Parameters**|$\approx 8.0\text{M}$|$\approx 25.6\text{M}$|$\approx 138.4\text{M}$|
|**Compute (MACs/FLOPs)**|$\approx 2.9\text{ GFLOPs}$|$\approx 4.1\text{ GFLOPs}$|$\approx 15.5\text{ GFLOPs}$|
|**Torchvision Top-1 Accuracy**|$\approx 74.4\%$|$\approx 76.1\%$|$\approx 71.6\%$|

### 14. VRAM Concatenation Footprint: Naive Copying vs. Shared Buffer (Slide 33)

- **Setup**: Block 3 ($L = 24$ layers, $k_0 = 256, k = 32$, resolution $14 \times 14 = 196\text{ positions}$, `float32` $= 4\text{ bytes/element}$, batch size $= 1$).
    
      
    
- **Naive Concatenation Buffer Memory**:
    
      
    
    $$\text{Total Channels Stored} = \sum_{l=1}^{24} (256 + 32(l - 1)) = 24 \times \frac{256 + 992}{2} = 14,976\text{ channel maps}$$
    
    $$\text{Memory} = 14,976 \times 196 \times 4\text{ bytes} = 11,741,184\text{ bytes} \approx \mathbf{11.2\text{ MiB}}$$
    
- **Shared Buffer (Pleiss et al., 2017)**:
    
      
    
    $$\text{Channels Stored} = 1,024\text{ channel maps}$$
    
    $$\text{Memory} = 1,024 \times 196 \times 4\text{ bytes} = 802,816\text{ bytes} \approx \mathbf{0.77\text{ MiB}}$$
    
- **Memory Reduction Ratio**: $\frac{11.2}{0.77} \approx \mathbf{14.6\times\text{ less activation memory}}$.
    
      
    

### 15. Slide Practice Problems (Slides 37, 38)

- **Problem 1 (Dense Block Geometry)**:
    
      
    - Setup: $k_0 = 48, k = 12, L = 8$.
        
          
        
    - Input to Layer 5: $48 + 12(5 - 1) = 48 + 48 = \mathbf{96\text{ channels}}$
        
          
        
    - Block output: $48 + 12(8) = 48 + 96 = \mathbf{144\text{ channels}}$
        
          
        
    - Direct connections: $\frac{8 \times 9}{2} = \mathbf{36\text{ connections}}$
        
          
        
- **Problem 2 (Transition Layer Weights)**:
    
      
    - Setup: $\theta = 0.5$ following the block in Problem 1 ($m = 144$).
        
          
        
    - Output channels: $\lfloor 0.5 \times 144 \rfloor = \mathbf{72\text{ channels}}$
        
          
        
    - Conv $1 \times 1$ weights: $144 \times 72 = \mathbf{10,368\text{ weights}}$
        
          
        
- **Problem 3 (CIFAR-BC Layer Distribution)**:
    
      
    - Setup: $L = 100$, 3 blocks.
        
          
        
    - Formula: $6n + 4 = 100 \implies 6n = 96 \implies \mathbf{n = 16\text{ layers per block}}$
        
          
        
- **Problem 4 (Direct Conv vs. Bottleneck Parameter Cost)**:
    
      
    - Setup: $C_{\text{in}} = 96, k = 12$.
        
          
        
    - Direct $3 \times 3$: $9 \times 96 \times 12 = \mathbf{10,368\text{ weights}}$
        
          
        
    - Bottleneck ($4k = 48$ intermediate):
        
          
        
        $$(96 \times 48) + (9 \times 48 \times 12) = 4,608 + 5,184 = \mathbf{9,792\text{ weights}}$$
        
    - Difference: $\frac{10,368 - 9,792}{10,368} \approx \mathbf{5.6\%\text{ fewer weights}}$.
      
- **Problem 5 (Why can downsampling not happen inside a dense block?)**
  
  Concatenation needs equal H × W, so resolution changes only in transition layers