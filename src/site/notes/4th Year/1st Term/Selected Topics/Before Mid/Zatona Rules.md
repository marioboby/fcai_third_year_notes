---
{"dg-publish":true,"permalink":"/4th-year/1st-term/selected-topics/before-mid/zatona-rules/","dg-note-properties":{}}
---


---

# CNN Architectures: Rules & Equations Master Reference

## Lecture 1: Introduction to CNN Concepts

### 1. Convolution & Spatial Dimension Rules
* **Output Dimension (No Padding, Stride $s=1$):**
  $$\text{Output} = (n - f + 1) \times (n - f + 1)$$
* **Output Dimension (With Padding $P$, Stride $s=1$):**
  $$\text{Output} = (n + 2P - f + 1) \times (n + 2P - f + 1)$$
* **"Same" Padding Rule** (preserves original spatial dimensions $H \times W$):
  $$P = \frac{f - 1}{2}$$
* **General Output Dimension (With Padding $P$ and Stride $s$):**
  $$n_{H}^{[l]} = \left\lfloor \frac{n_{H}^{[l-1]} + 2p^{[l]} - f^{[l]}}{s^{[l]}} + 1 \right\rfloor, \quad n_{W}^{[l]} = \left\lfloor \frac{n_{W}^{[l-1]} + 2p^{[l]} - f^{[l]}}{s^{[l]}} + 1 \right\rfloor$$

### 2. Tensor Shapes & Parameter Rules
* **Input Volume Shape:** $n_H^{[l-1]} \times n_W^{[l-1]} \times n_c^{[l-1]}$
* **Filter Shape Rule:** Each filter must match the input's channel depth: $f^{[l]} \times f^{[l]} \times n_c^{[l-1]}$
* **Output Volume Shape:** $n_H^{[l]} \times n_W^{[l]} \times n_c^{[l]}$ (where $n_c^{[l]}$ is the number of filters in layer $l$).
* **Batch Activation Tensor Shape ($m$ examples):** $m \times n_H^{[l]} \times n_W^{[l]} \times n_c^{[l]}$
* **Bias Tensor Shape:** $(1, 1, 1, n_c^{[l]})$ — 1 scalar bias per filter.
* **Total Weights (Parameters) in Layer $l$:**
  $$\text{Weights} = \left(f^{[l]} \times f^{[l]} \times n_c^{[l-1]}\right) \times n_c^{[l]}$$
  $$\text{Total Parameters} = \text{Weights} + n_c^{[l]} \text{ (biases)}$$

### 3. Forward & Backward Pass Equations
* **Convolution Dot Product (Pre-activation):**
  $$a_{r,c} = \sum_{i=-A}^{A} \sum_{j=-B}^{B} x_{r-i,\, c-j} \cdot w_{ij}$$
* **Convolution Gradient w.r.t. Weights:**
  $$\frac{da_{r,c}}{dw_{ij}} = \sum_{i=-A}^{A} \sum_{j=-B}^{B} x_{r-i,\, c-j}$$
* **Max Pooling Activation & Gradient Routing:**
  $$i_{\max}, j_{\max} = \arg\max x_{ij}$$
  $$\frac{da_{r,c}}{dx_{ij}} = \begin{cases} 1 & \text{if } i = i_{\max},\, j = j_{\max} \\ 0 & \text{otherwise} \end{cases}$$

### 4. Architectural & Training Rules
* **Pooling Channel Rule:** Pooling (Max or Average) reduces spatial dimensions ($H, W$) but **never changes the number of channels** ($n_c$).
* **VGG Receptive Field Stacking Rule:** Stacking smaller $3 \times 3$ filters (stride 1) replicates the effective receptive field of larger filters with significantly fewer parameters:
  * Two $3 \times 3$ layers $\equiv$ one $5 \times 5$ receptive field ($18$ vs. $25$ params $\rightarrow$ **28% reduction**).
  * Three $3 \times 3$ layers $\equiv$ one $7 \times 7$ receptive field (**44.9% reduction**).
  * Five $3 \times 3$ layers $\equiv$ one $11 \times 11$ receptive field (**62.8% reduction**).
* **Data Augmentation Rules:**
  * Must be applied **only to the training set**—never to validation or test sets.
  * Always split the dataset into train/validation/test **before** applying augmentation.
  * For class imbalance, augment the minority class until sample counts match the majority class.

---

## Lecture 2: ResNet and $1 \times 1$ Convolutions

### 1. Residual Learning & Skip Connection Equations
* **Plain Layer Mapping:** $H(x) = F(x)$
* **Residual Block Output (Identity Shortcut):**
  $$H(x) = F(x) + x$$
  *(If $F(x) = 0$, the block learns an identity mapping, allowing gradients to flow directly through the bypass path without vanishing).*
* **Two-Layer Residual Block Forward Pass:**
  $$z^{[l+1]} = W^{[l+1]}a^{[l]} + b^{[l+1]}, \quad a^{[l+1]} = g(z^{[l+1]})$$
  $$z^{[l+2]} = W^{[l+2]}a^{[l+1]} + b^{[l+2]}$$
  $$a^{[l+2]} = g\left(z^{[l+2]} + a^{[l]}\right)$$
* **Dimension Mismatch Rule:** Element-wise addition ($F(x) + x$) requires identical spatial and channel dimensions. If dimensions change (e.g., via stride-2 downsampling or channel expansion), either:
  1. Pad the shortcut with extra zero entries, or
  2. Apply a **projection shortcut** using a $1 \times 1$ convolution ($W_s x$) with matching stride:
     $$H(x) = F(x) + W_s x$$

### 2. $1 \times 1$ Convolution (Network-in-Network) Rules
* **Channel Control:** Changes channel depth ($n_c^{[l]} \rightarrow n_c^{[l+1]}$) for dimensionality reduction (bottlenecks) or expansion without altering spatial dimensions $H \times W$ (unless stride $s > 1$ is used).
* **Non-Linearity:** Adds an extra activation function (e.g., ReLU) across channels at every spatial pixel without spatial neighborhood mixing.

### 3. ResNet Architectural Design Rules
* **ResNet-34 Spatial/Channel Scaling Rules:**
  1. Layers with the **same output feature map size** have the **same number of filters**.
  2. When the **feature map size is halved** (via stride-2 convolution, not pooling), the **number of filters is doubled** to preserve time complexity (FLOPs) per layer.
* **ResNet-50 / 101 / 152 Bottleneck Rule:** Replaces 2-layer blocks ($3\times3 \rightarrow 3\times3$) with 3-layer bottlenecks ($1\times1$ reduce $\rightarrow 3\times3$ process $\rightarrow 1\times1$ expand).
* **Total Weighted Layer Count Formula (Bottleneck Variants):**
  $$\text{Total Layers} = (\text{Number of Residual Blocks} \times 3) + 1 \text{ (initial } 7\times7 \text{ conv)} + 1 \text{ (FC layer)}$$

---

## Lecture 3: DenseNet and Its Variants

### 1. Core Connectivity & Channel Growth Equations
* **Layer Connectivity Equation:** Every layer $l$ receives the channel-wise concatenation of all preceding feature maps in the block:
  $$x_l = H_l\left([x_0, x_1, \dots, x_{l-1}]\right)$$
  where $H_l$ is a composite pre-activation function: $\text{BN} \rightarrow \text{ReLU} \rightarrow \text{Conv}$.
* **Direct Connections in a Block of $L$ Layers:**
  $$\text{Connections} = 1 + 2 + \dots + L = \frac{L(L+1)}{2}$$
  *(Compare with ResNet: $2L$, Plain CNN: $L$).*
* **Input Channels to Layer $l$** (given block input channels $k_0$ and growth rate $k$):
  $$C_{\text{in}}(l) = k_0 + k \cdot (l - 1)$$
* **Total Output Channels of a Dense Block ($L$ layers):**
  $$C_{\text{out}} = k_0 + k \cdot L$$

### 2. Bottleneck & Transition Layer Equations
* **Spatial Invariance Rule:** Downsampling **never occurs inside a dense block**; every layer uses padding $1$ on $3 \times 3$ convolutions so $H \times W$ stays constant for concatenation.
* **No-Bias Rule:** Convolutions omit bias terms ($\text{bias}=\text{False}$) because Batch Normalization follows/precedes every convolution.
* **Conv Parameters & Cost (MACs):**
  $$\text{Params} = f \times f \times C_{\text{in}} \times C_{\text{out}}$$
  $$\text{MACs} = \text{Params} \times H_{\text{out}} \times W_{\text{out}}$$
* **DenseNet-B (Bottleneck Layer):** Adds a $1 \times 1$ conv before the $3 \times 3$ conv to reduce any input depth $C_{\text{in}}$ down to $4k$ channels.
  * Fixed $3 \times 3$ conv weight count (independent of depth):
    $$3 \times 3 \times 4k \times k = 36k^2$$
  * *Break-even rule:* Bottleneck saves parameters/MACs once $C_{\text{in}} > \approx 230$ channels.
* **Transition Layer Compression & Downsampling** (with compression factor $0 < \theta \le 1$, default $\theta = 0.5$):
  $$\text{Channels: } m \longrightarrow \lfloor \theta \cdot m \rfloor \quad (\text{via } 1\times1 \text{ conv})$$
  $$\text{Spatial: } H \times W \longrightarrow \frac{H}{2} \times \frac{W}{2} \quad (\text{via } 2\times2 \text{ AvgPool, } s=2)$$

### 3. Depth & Variant Rules
* **ImageNet DenseNet-BC Depth Formula** (4 blocks with $n_1, n_2, n_3, n_4$ layers, 2 convs per layer):
  $$L = 1 \text{ (stem)} + 2(n_1 + n_2 + n_3 + n_4) + 3 \text{ (transitions)} + 1 \text{ (FC)}$$
* **CIFAR DenseNet-BC Depth Formula** (3 blocks with $n$ layers each):
  $$L = 1 + 2(3n) + 2 + 1 = 6n + 4$$
* **Variant Taxonomy:**
  * **DenseNet (Basic):** No bottleneck, $\theta = 1$.
  * **DenseNet-B:** $1 \times 1$ bottleneck ($4k$), $\theta = 1$.
  * **DenseNet-C:** No bottleneck, compression $\theta < 1$ ($\theta = 0.5$).
  * **DenseNet-BC:** Both bottleneck ($4k$) and compression ($\theta = 0.5$).

---

## Lecture 4: MobileNet (V1, V2, V3)

### 1. Standard vs. Depthwise Separable Convolution (MobileNetV1)
*Given input $D_F \times D_F \times M$, kernel $D_K \times D_K$, output $D_G \times D_G \times N$:*
* **Standard Convolution:**
  $$\text{Params}_{\text{std}} = D_K^2 \cdot M \cdot N$$
  $$\text{Mult-Adds}_{\text{std}} = D_K^2 \cdot M \cdot N \cdot D_G^2$$
* **Step 1 — Depthwise Convolution** (spatial filtering per channel, no channel mixing):
  $$\hat{G}(k, l, m) = \sum_{i, j} K(i, j, m) \cdot F(k+i, l+j, m)$$
  $$\text{Params}_{\text{DW}} = D_K^2 \cdot M, \quad \text{Mult-Adds}_{\text{DW}} = D_K^2 \cdot M \cdot D_G^2$$
* **Step 2 — Pointwise Convolution** ($1 \times 1$ channel mixing):
  $$\text{Params}_{\text{PW}} = M \cdot N, \quad \text{Mult-Adds}_{\text{PW}} = M \cdot N \cdot D_G^2$$
* **Total Depthwise Separable Cost:**
  $$\text{Params}_{\text{DS}} = D_K^2 \cdot M + M \cdot N$$
  $$\text{Mult-Adds}_{\text{DS}} = D_K^2 \cdot M \cdot D_G^2 + M \cdot N \cdot D_G^2$$
* **Computation Reduction Ratio:**
  $$\frac{\text{Mult-Adds}_{\text{DS}}}{\text{Mult-Adds}_{\text{std}}} = \frac{D_K^2 \cdot M \cdot D_G^2 + M \cdot N \cdot D_G^2}{D_K^2 \cdot M \cdot N \cdot D_G^2} = \frac{1}{N} + \frac{1}{D_K^2}$$
  *(For $3 \times 3$ kernels and large $N$, cost drops by $\approx 8\times$ to $9\times$).*
* **Downsampling Rule:** MobileNetV1 replaces pooling layers with **stride-2 depthwise convolutions**.

### 2. Width ($\alpha$) & Resolution ($\rho$) Multipliers
* **Width Multiplier $\alpha \in (0, 1]$** (scales channels $M \rightarrow \alpha M$, $N \rightarrow \alpha N$):
  $$\text{Mult-Adds} = D_K^2 \cdot \alpha M \cdot D_F^2 + \alpha M \cdot \alpha N \cdot D_F^2 \approx \alpha^2 \times \text{original}$$
  $$\text{Params} \approx \alpha^2 \times \text{original}$$
* **Resolution Multiplier $\rho \in (0, 1]$** (scales spatial maps $D_F \rightarrow \rho D_F$):
  $$\text{Mult-Adds} = D_K^2 \cdot M \cdot (\rho D_F)^2 + M \cdot N \cdot (\rho D_F)^2 \approx \rho^2 \times \text{original}$$
  $$\text{Params} = \text{unchanged}$$
* **Combined Scaling Rule:**
  $$\text{Mult-Adds}_{\text{combined}} \approx \alpha^2 \rho^2 \times \text{original}$$

### 3. MobileNetV2: Linear Bottlenecks & Inverted Residuals
* **Linear Bottleneck Rule:** Never apply ReLU after the final $1 \times 1$ projection into a low-dimensional (narrow) bottleneck, as zeroing negative values destroys information in low-channel spaces. Apply non-linearity (ReLU6) **only in the expanded (wide) space**.
* **Inverted Residual Block Flow** (narrow $\rightarrow$ wide $\rightarrow$ narrow, expansion factor $t$, typically $t=6$):
  1. $1 \times 1$ Expand ($C \rightarrow tC$) + BN + ReLU6
  2. $3 \times 3$ Depthwise ($tC \rightarrow tC$) + BN + ReLU6
  3. $1 \times 1$ Project ($tC \rightarrow C'$) + BN + **Linear (no activation)**
* **Shortcut Rule:** Residual skip connection links the narrow ends **only when $\text{stride} = 1$ and $C = C'$**.

### 4. MobileNetV3: Squeeze-and-Excitation (SE) & Activations
* **Batch Normalization:**
  $$\text{BN}(x) = \gamma \cdot \frac{x - \mu}{\sqrt{\sigma^2 + \epsilon}} + \beta$$
* **ReLU6:**
  $$\text{ReLU6}(x) = \min(\max(0, x), 6)$$
* **Swish & Hard-Swish (h-swish):**
  $$\sigma(x) = \frac{1}{1 + e^{-x}}, \quad \text{swish}(x) = x \cdot \sigma(x)$$
  $$\text{hard-sigmoid}(x) = \frac{\text{ReLU6}(x + 3)}{6}$$
  $$\text{h-swish}(x) = x \cdot \frac{\text{ReLU6}(x + 3)}{6}$$
  *(h-swish replaces $\sigma(x)$ with a piecewise linear approximation to avoid costly `exp()` operations and is used primarily in deeper layers).*
* **Squeeze-and-Excitation (SE) Block** (reduction ratio $r$):
  1. **Squeeze:** Global Average Pooling per channel ($H \times W \times C \rightarrow 1 \times 1 \times C$).
  2. **Excite:** Two FC layers ($C \rightarrow C/r \text{ [ReLU]} \rightarrow C \text{ [hard-sigmoid]}$) producing channel weights $s_c \in [0, 1]$.
  3. **Scale:** Multiply each input channel $c$ by $s_c$.
  * **SE Parameter Cost:**
    $$\text{Params}_{\text{SE}} = \frac{2C^2}{r}$$