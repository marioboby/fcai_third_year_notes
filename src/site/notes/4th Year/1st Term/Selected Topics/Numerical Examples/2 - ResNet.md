---
{"dg-publish":true,"permalink":"/4th-year/1st-term/selected-topics/numerical-examples/2-res-net/","dg-note-properties":{}}
---


# ResNet Architecture & Skip Connections

## 1. Custom Basic Block (3× Conv)

_Note: Standard ResNet Basic Blocks use two $3 \times 3$ convolutions; this walkthrough uses a custom three-layer variant._

  

### Setup

- **Input Tensor ($X$):** $32 \times 32 \times 64$ ($H \times W \times C$)
    
      
    
- **Padding:** `same` (preserves spatial resolution)
    
      
    

|**Layer**|**Filters**|**Kernel**|**Stride**|**Output Shape**|**Activation**|
|---|---|---|---|---|---|
|**Conv1**|64|$3 \times 3$|1|$32 \times 32 \times 64$|ReLU|
|**Conv2**|64|$3 \times 3$|1|$32 \times 32 \times 64$|ReLU|
|**Conv3**|64|$3 \times 3$|1|$32 \times 32 \times 64$|None (Linear)|

```
Input X (32×32×64)
   │
   ├───> [Conv 3×3 + ReLU] ──> [Conv 3×3 + ReLU] ──> [Conv 3×3] ──> F(X)
   │                                                                  │
   └──────────────────────── (Identity Shortcut) ────────────────────> (+) ──> [ReLU] ──> Output (32×32×64)
```

$$\boxed{Y = \text{ReLU}(F(X) + X)}$$

The original input $X$ is added element-wise to the transformed feature map $F(X)$ without passing through extra parameters.

  

## 2. Bottleneck Block

Designed to reduce computational cost by compressing channels before running spatial convolutions.

  

### Setup

- **Input Tensor ($X$):** $32 \times 32 \times 256$
    
      
    

|**Layer**|**Type / Purpose**|**Filters**|**Kernel**|**Stride**|**Output Shape**|
|---|---|---|---|---|---|
|**Conv1**|Reduce channels|64|$1 \times 1$|1|$32 \times 32 \times 64$|
|**Conv2**|Spatial feature extraction|64|$3 \times 3$|1|$32 \times 32 \times 64$|
|**Conv3**|Restore / expand channels|256|$1 \times 1$|1|$32 \times 32 \times 256$|

```
Input X (32×32×256)
   │
   ├───> [1×1 Conv (Reduce)] ──> [3×3 Conv] ──> [1×1 Conv (Expand)] ──> F(X)
   │                                                                      │
   └──────────────────────── (Identity Shortcut) ────────────────────────> (+) ──> [ReLU] ──> Output (32×32×256)
```

### Parameter Efficiency Comparison

For transforming $256 \to 256$ channels (excluding biases):

  

- **Direct $3 \times 3$ Conv:**
    
      
    
    $$3 \times 3 \times 256 \times 256 = 589{,}824\text{ parameters}$$
    
- **Bottleneck Design ($1 \times 1 \to 3 \times 3 \to 1 \times 1$):**
    
      
    
    $$(1 \times 1 \times 256 \times 64) + (3 \times 3 \times 64 \times 64) + (1 \times 1 \times 64 \times 256) = 16{,}384 + 36{,}864 + 16{,}384 = 69{,}632\text{ parameters}$$
    

> **Result:** Approximately **$8.47\times$ fewer parameters** with comparable representational capacity.
> 
>   

## 3. Skip Connections: Same vs. Different Dimensions

Element-wise addition requires identical tensor shapes across all axes: $(H, W, C)$.

  

### Case A: Matching Dimensions (Identity Shortcut)

- **Input ($X$):** $32 \times 32 \times 64$
    
      
    
- **Main Path Output ($F(X)$):** $32 \times 32 \times 64$
    
      
    

$$\boxed{Y = \text{ReLU}(F(X) + X)}$$

No transformation needed on the shortcut; direct tensor addition.

  

### Case B: Mismatched Dimensions (Projection Shortcut)

- **Input ($X$):** $32 \times 32 \times 64$
    
      
    
- **Main Path Output ($F(X)$):** $16 \times 16 \times 128$
    
      
    

Because spatial resolution halves ($32 \to 16$) and channel depth doubles ($64 \to 128$), the shortcut path requires a projection layer:

  

- **Projection Layer:** $1 \times 1$ Conv with **$\text{stride} = 2$** and **$128\text{ filters}$** ($W_s$)
    
      
    

```
Input X (32×32×64)
   │
   ├───> [Main Path: Conv layers, Stride 2] ────────────> F(X) (16×16×128)
   │                                                              │
   └───> [Shortcut Path: 1×1 Conv, Stride 2, 128 Filters] ─> W_s·X (16×16×128)
                                                                  │
                                                                 (+) ──> [ReLU] ──> Output (16×16×128)
```

$$\boxed{Y = \text{ReLU}(F(X) + W_s X)}$$

## 4. Summary Matrix

|**Attribute**|**Same Dimensions**|**Different Dimensions**|
|---|---|---|
|**Shortcut Type**|Identity mapping|Projection ($1 \times 1$ Conv)|
|**Spatial Resolution ($H \times W$)**|Maintained|Downsampled (via $\text{stride} > 1$)|
|**Channels ($C$)**|Maintained|Matched (via filter count)|
|**Added Parameters**|0|$(1 \times 1 \times C_{\text{in}} \times C_{\text{out}})$|
|**Formulation**|$Y = \text{ReLU}(F(X) + X)$|$Y = \text{ReLU}(F(X) + W_s X)$|

### Core Takeaway

Instead of fitting an underlying mapping $\mathcal{H}(X)$ directly, the residual block optimizes the residual mapping:

  

$$F(X) = \mathcal{H}(X) - X$$

Driving $F(X) \to 0$ allows identity mappings to propagate gradients directly through deep layers during backpropagation, mitigating the vanishing gradient problem.