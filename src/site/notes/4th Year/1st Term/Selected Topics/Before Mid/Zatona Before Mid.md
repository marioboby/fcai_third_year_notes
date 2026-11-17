---
{"dg-publish":true,"permalink":"/4th-year/1st-term/selected-topics/before-mid/zatona-before-mid/","dg-note-properties":{}}
---




The provided slides cover the evolutionary trajectory of modern Convolutional Neural Networks across four primary families: **Classic Baselines (LeNet-5 & VGG)**, **Residual Networks (ResNet)**, **Densely Connected Networks (DenseNet)**, and **Mobile/Edge Architectures (MobileNetV1, V2, V3)**.

  

## 1. High-Level Architectural Comparison

|**Architecture**|**Main Motivation**|**Core Mechanism / Equation**|**Key Building Blocks**|**Main Variations Present**|**Primary Pros**|**Primary Cons**|
|---|---|---|---|---|---|---|
|**LeNet-5**|Digitize document reading (handwritten zip codes/digits) efficiently with automated feature extraction.|Weight sharing and local receptive fields over 2D topology.|$5\times5$ Conv, $2\times2$ Avg Pool, Dense (FC) layers, Softmax.|Baseline architecture for $32\times32$ digit inputs.|Greatly reduced parameters vs. fully connected nets; shift/translation invariance.|Limited capacity; fails on large-scale natural color images (e.g., ImageNet).|
|**VGGNet**|Deepen feature hierarchies while standardizing conv filters across the entire network.|Factorize large spatial receptive fields into consecutive $3\times3$ convolutions.|Stacks of $3\times3$ Conv (stride 1, `same`), $2\times2$ Max Pool (stride 2), three massive FC layers ($4096 \to 4096 \to 1000$).|VGG-16, VGG-19 (differing in depth of conv stacks per stage).|Homogeneous design; deeper non-linearities with fewer params than $5\times5$/$7\times7$ filters.|Extremely parameter-heavy (~138M params); massive compute (~15.3–19.6 GFLOPs); high activation memory.|
|**ResNet**|Solve the degradation/optimization problem where plain deep networks suffer higher training error.|Residual learning with identity skip connections: $\mathcal{H}(x) = \mathcal{F}(x) + x$.|**Basic Block** (two $3\times3$ convs) and **Bottleneck Block** ($1\times1 \to 3\times3 \to 1\times1$).|ResNet-18, ResNet-34 (Basic Blocks); ResNet-50, ResNet-101, ResNet-152 (Bottleneck Blocks).|Eliminates vanishing gradients; trains up to 152+ layers easily; keeps channel sizes moderate.|Element-wise addition mixes feature representations directly; redundant channel learning.|
|**DenseNet**|Maximize information flow and reuse low-level features without relearning redundant filters.|Dense connectivity via channel concatenation: $x_l = H_l([x_0, x_1, \dots, x_{l-1}])$.|**Dense Layer** (pre-activation BN-ReLU-Conv), **Dense Block**, and **Transition Layer** ($1\times1$ Conv + $2\times2$ Avg Pool).|Basic DenseNet, DenseNet-B, DenseNet-C, DenseNet-BC; scaled depths: 121, 169, 201, 264.|High parameter efficiency (~8M params for 121); strong gradient flow via short paths; mitigates overfitting.|High memory footprint and memory access cost (MAC) due to continuous concatenation buffers.|
|**MobileNetV1**|Run efficient CNN inference on resource-constrained mobile, drone, and IoT hardware.|Factorize standard convolution into Depthwise (spatial) + Pointwise (channel mixing).|Depthwise Separable Conv block ($3\times3$ DW Conv + BN + ReLU6 followed by $1\times1$ PW Conv + BN + ReLU6).|Baseline ($1.0\times, 224$), Width Multipliers ($\alpha \in \{0.75, 0.5, 0.25\}$), Resolution Multipliers ($\rho$).|8–9× reduction in mult-adds; fast GEMM implementation on mobile GPUs.|Information loss from aggressive non-linearities in low-channel manifolds; no residual paths.|
|**MobileNetV2**|Prevent information destruction caused by ReLU in narrow channel spaces.|Inverted Residuals with Linear Bottlenecks (expand $\to$ filter $\to$ project).|Inverted Residual Block ($1\times1$ Expansion + ReLU6 $\to 3\times3$ DW Conv + ReLU6 $\to 1\times1$ Linear Projection).|Scaled across width multiplier $\alpha$ and expansion factor $t=6$.|Preserves feature manifold; better accuracy (72.0%) with lower params (3.4M) and FLOPs (300M) vs. V1.|Residual shortcuts only execute when input/output dimensions and strides match ($s=1$, $C_{in}=C_{out}$).|
|**MobileNetV3**|Push mobile CPU latency boundaries using automated search and lightweight attention.|Hardware-aware NAS + channel-wise attention + computationally cheap non-linearities.|Inverted residual updated with Squeeze-and-Excitation (SE) and hard-swish ($h\text{-swish}$).|**V3-Large** (15 blocks, high accuracy) and **V3-Small** (11 blocks, ultra-low resource).|Highest accuracy-to-latency trade-off (75.2% top-1 at 219M mult-adds); integer/quantization friendly.|Fragmented search-derived layer configurations make manual customization less intuitive.|

## 2. Granular Architectural Deep-Dives

### LeNet-5 & VGGNet (The Classical Baselines)

- **Core Motivation & Concept:** LeNet-5 introduced automated visual feature learning to replace hand-crafted descriptors. VGG addressed the parameter inefficiency of large kernels (e.g., AlexNet's $11\times11$ and $5\times5$) by stacking homogeneous $3\times3$ convolutions. Two consecutive $3\times3$ convolutions produce an effective receptive field of $5\times5$ while saving 28% of parameters ($2 \times 3^2 = 18$ vs. $5^2 = 25$); three $3\times3$ layers replicate a $7\times7$ field, saving 44.9%.
    
      
    
- **Layer Mechanics:**
    
      
    - _LeNet-5:_ Conv ($5\times5, s=1$, no pad) $\to$ Avg Pool ($2\times2, s=2$) $\to$ Conv ($5\times5$) $\to$ Avg Pool $\to$ FC (120) $\to$ FC (84) $\to$ Softmax (10).
        
          
        
    - _VGG-16:_ Five convolutional stages utilizing stacks of 2 or 3 $3\times3$ convs followed by $2\times2$ Max Pooling, culminating in three Fully Connected layers (4096, 4096, 1000).
        
          
        
- **Pros & Cons:** VGG provides clean structural modularity and excellent feature representations. However, the three terminal FC layers account for over 100M of its 138M parameters, creating severe computational bottlenecks and high memory consumption.
    
      
    

### ResNet (Residual Networks)

- **Core Motivation & Concept:** As plain networks grow deeper, training and validation error degrade due to optimization difficulties rather than overfitting. ResNet re-parameterizes the mapping: rather than fitting $\mathcal{H}(x)$ directly, the stacked layers fit $\mathcal{F}(x) = \mathcal{H}(x) - x$, outputting $\mathcal{H}(x) = \mathcal{F}(x) + x$. If added layers are redundant, learning $\mathcal{F}(x) = 0$ is trivial, allowing the network to easily collapse to an identity mapping.
    
      
    
- **Blocks & Layers:**
    
      
    - **Basic Block (ResNet-18, 34):** Two consecutive $3\times3$ convolutions with Batch Normalization and ReLU.
        
          
        
    - **Bottleneck Block (ResNet-50, 101, 152):** Stacks three layers: $1\times1$ Conv (channel reduction, e.g., $256 \to 64$) $\to 3\times3$ Conv (spatial processing, 64) $\to 1\times1$ Conv (channel expansion, $64 \to 256$). This reduces computational complexity from quadratic in wide spaces to compact processing.
        
          
        
    - **Dimension Matching:** When downsampling or channel counts double, shortcuts use either zero-padding or a $1\times1$ projection convolution with stride 2.
        
          
        
- **Variations:**
    
      
    - _ResNet-18 / 34:_ $[2, 2, 2, 2]$ and $[3, 4, 6, 3]$ 2-layer blocks.
        
          
        
    - _ResNet-50 / 101 / 152:_ $[3, 4, 6, 3]$, $[3, 4, 23, 3]$, and $[3, 8, 36, 3]$ 3-layer bottleneck blocks.
        
          
        

### DenseNet (Densely Connected Convolutional Networks)

- **Core Motivation & Concept:** Unlike ResNet which adds feature maps and inadvertently blends representations, DenseNet concatenates all preceding feature maps directly. Every layer receives the collective knowledge of all preceding layers:
    
      
    
    $$\mathbf{x}_l = H_l([\mathbf{x}_0, \mathbf{x}_1, \dots, \mathbf{x}_{l-1}])$$
    
    A block of $L$ layers contains $L(L+1)/2$ direct connections, enabling continuous feature reuse and short gradient backpropagation paths.
    
      
    
- **Blocks & Mechanics:**
    
      
    - **Growth Rate ($k$):** The number of new channels output by each individual dense layer (typically $k=12$ for CIFAR, $k=32$ for ImageNet). Layer $l$ receives $k_0 + k(l-1)$ input channels.
        
          
        
    - **Bottleneck Layer (DenseNet-B):** A pre-activation sequence of $\text{BN} \to \text{ReLU} \to \text{Conv } 1\times1$ (compressing accumulated inputs to a fixed $4k$ channels) $\to \text{BN} \to \text{ReLU} \to \text{Conv } 3\times3$ (producing $k$ channels).
        
          
        
    - **Transition Layer (DenseNet-C):** Placed between Dense Blocks to enable downsampling and prevent channel explosion: $\text{BN} \to \text{ReLU} \to \text{Conv } 1\times1$ (with compression factor $\theta=0.5$, reducing channels $m \to \lfloor\theta m\rfloor$) $\to \text{AvgPool } 2\times2$ (stride 2).
        
          
        
- **Variations:**
    
      
    - _DenseNet (basic):_ No bottleneck ($1\times1$), $\theta = 1.0$.
        
          
        
    - _DenseNet-B:_ Bottleneck included, $\theta = 1.0$.
        
          
        
    - _DenseNet-C:_ No bottleneck, compression $\theta = 0.5$.
        
          
        
    - _DenseNet-BC:_ Both bottleneck ($4k$) and compression ($\theta = 0.5$) applied.
        
          
        
    - _ImageNet Depths (DenseNet-BC, $k=32$):_ **121** ($[6, 12, 24, 16]$), **169** ($[6, 12, 32, 32]$), **201** ($[6, 12, 48, 32]$), and **264** ($[6, 12, 64, 48]$).
        
          
        

### MobileNet Family (V1, V2, V3)

#### MobileNetV1

- **Core Concept:** Decomposes a standard convolution ($D_K \times D_K \times M \times N$) into:
    
      
    1. _Depthwise (DW) Convolution:_ Applies a single $D_K \times D_K$ spatial filter per channel ($M$ filters total).
        
          
        
    2. _Pointwise (PW) Convolution:_ A $1\times1$ convolution that linearly combines the $M$ channels into $N$ output channels.
        
          
        
- **Computational Savings:**
    
      
    
    $$\frac{\text{Cost}_{\text{Separable}}}{\text{Cost}_{\text{Standard}}} = \frac{D_K^2 \cdot M \cdot D_G^2 + M \cdot N \cdot D_G^2}{D_K^2 \cdot M \cdot N \cdot D_G^2} = \frac{1}{N} + \frac{1}{D_K^2}$$
    
    For $3\times3$ kernels ($D_K=3$) and large $N$, this reduces parameters and FLOPs by ~8 to 9×.
    
      
    
- **Hyperparameter Knobs:**
    
      
    - _Width Multiplier ($\alpha \in (0, 1]$):_ Uniformly scales channel counts ($M \to \alpha M, N \to \alpha N$). Parameters and FLOPs scale by $\approx \alpha^2$.
        
          
        
    - _Resolution Multiplier ($\rho \in (0, 1]$):_ Downsamples the input resolution ($D_F \to \rho D_F$). FLOPs scale by $\approx \rho^2$, leaving parameter counts unchanged.
        
          
        
- **Activation:** Uses **ReLU6** ($\min(\max(0, z), 6)$) to prevent activations from exploding, stabilizing low-precision fixed-point/INT8 quantization on mobile hardware.
    
      
    

#### MobileNetV2

- **Linear Bottlenecks:** ReLU operations zero out negative values, irreversibly collapsing information in low-dimensional representations. MobileNetV2 retains full linearity at the narrow output projection of each block.
    
      
    
- **Inverted Residual Block:** Flips the ResNet paradigm (which went Wide $\to$ Narrow $\to$ Wide). MobileNetV2 follows a **Narrow $\to$ Wide $\to$ Narrow** structure:
    
      
    1. $1\times1$ Conv: Expands input channels by expansion factor $t=6$ ($C_{in} \to t C_{in}$) + BN + ReLU6.
        
          
        
    2. $3\times3$ Depthwise Conv: Applies spatial filtering in the expanded high-dimensional space + BN + ReLU6.
        
          
        
    3. $1\times1$ Projection Conv: Compresses back to narrow output channels ($t C_{in} \to C_{out}$) + BN (Linear, no activation).
        
          
        
- **Residual Shortcuts:** Direct identity connections connect the low-dimensional bottlenecks, executing exclusively when stride $s=1$ and $C_{in} = C_{out}$.
    
      
    

#### MobileNetV3

- **Neural Architecture Search (NAS):** Uses platform-aware MnasNet to discover the coarse global network structure, paired with NetAdapt to fine-tune layer-by-layer channel widths for target mobile CPU latency.
    
      
    
- **Squeeze-and-Excitation (SE):** Introduces channel-wise attention into the inverted residual block at negligible cost ($<2\%$ extra params):
    
      
    - _Squeeze:_ Global average pooling collapses spatial maps to $1\times1\times C$.
        
          
        
    - _Excite:_ A two-layer FC bottleneck ($C \to C/r \to C$) outputs channel scale factors via hard-sigmoid.
        
          
        
    - _Scale:_ Channel-wise multiplication rescales the feature maps.
        
          
        
- **Hard-Swish ($h\text{-swish}$):** Standard Swish ($x \cdot \sigma(x)$) improves gradient flow and convergence but requires computationally expensive exponentials. MobileNetV3 introduces a piecewise linear approximation:
    
      
    
    $$\text{hard-sigmoid}(x) = \frac{\text{ReLU6}(x + 3)}{6}, \quad h\text{-swish}(x) = x \cdot \frac{\text{ReLU6}(x + 3)}{6}$$
    
    This removes all expensive division and transcendental functions, matching Swish accuracy within $\sim0.14$ while remaining hardware- and quantization-friendly.
    
      
    
- **Variations:**
    
      
    - **V3-Large:** 15 bottleneck blocks, final stage $960 \to 1280$ channels, 5.4M params, 219M mult-adds, 75.2% top-1 ImageNet.
        
          
        
    - **V3-Small:** 11 bottleneck blocks, final stage $576 \to 1024$ channels, 2.5M params, 56M mult-adds, 67.4% top-1 ImageNet.
        
          
        

## 3. Structural Comparison: Feature Combination Paradigms

```
ResNet Block:       x ────────────────────────────(+)──> H(x) = F(x) + x
                         └─> [Conv] ─> [Conv] ─────┘
                         (Element-wise Addition: fixed channels, mixed features)

DenseNet Block:     x ────────────────────────────[Concat]──> [x, H(x)]
                         └─> [BN-ReLU-Conv] ────────┘
                         (Channel Concatenation: growing channels, preserved features)

MobileNetV2 Block:  x ────────────────────────────(+)──> (Only if s=1, Cin=Cout)
                         └─> [1x1 Expand] ─> [3x3 DW] ─> [1x1 Project (Linear)] ──┘
                         (Inverted Residual: narrow -> wide -> narrow)
```

1. **Addition vs. Concatenation:**
    
      
    - ResNet merges features via element-wise summation, preserving tensor dimensions but permanently blending representation details.
        
          
        
    - DenseNet preserves all earlier states intact along the channel axis, offloading the choice of feature weighting to subsequent layers, but generating massive memory access traffic.
        
          
        
2. **Standard vs. Inverted Bottlenecks:**
    
      
    - ResNet bottlenecks compress channels first to make $3\times3$ standard convolutions cheaper ($256 \to 64 \to 256$).
        
          
        
    - MobileNetV2 expands channels first ($24 \to 144 \to 24$) because depthwise convolutions are already light, ensuring spatial operations occur in a rich, high-dimensional space without manifold collapse.