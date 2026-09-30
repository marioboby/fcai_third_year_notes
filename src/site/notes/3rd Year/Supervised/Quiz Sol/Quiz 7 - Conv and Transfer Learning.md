---
{"dg-publish":true,"permalink":"/3rd-year/supervised/quiz-sol/quiz-7-conv-and-transfer-learning/","dg-note-properties":{}}
---


# Q1 - CNN  

```python
from tensorflow.keras.models import Sequential
from tensorflow.keras.layers import Conv2D, MaxPooling2D, Flatten, Dense,
Dropout

model = Sequential([
	Conv2D(32, (3, 3), padding='same', activation='relu', input_shape=(28,
	28, 1)),
	Conv2D(32, (3, 3), activation='relu'),
	MaxPooling2D((2, 2)),
	Conv2D(64, (3, 3), padding='same', activation='relu'),
	Conv2D(64, (3, 3), activation='relu'),
	MaxPooling2D((2, 2)),
	Flatten(),
	Dense(256, activation='relu'),
	Dropout(0.5),
	Dense(10, activation='softmax')
])
```

### CNN Parameter Table

| **Layer**                             | **Output Shape** | **# Parameters** | **Calculation**                              |
| ------------------------------------- | ---------------- | ---------------- | -------------------------------------------- |
| **Conv2D(32, (3,3), padding='same')** | `(28, 28, 32)`   | 320              | `(3 * 3 * 1 + 1) * 32`                       |
| **Conv2D(32, (3,3))**                 | `(26, 26, 32)`   | 9,248            | `(3 * 3 * 32 + 1) * 32`                      |
| **MaxPooling2D((2,2))**               | `(13, 13, 32)`   | 0                | Max pooling has no learnable parameters      |
| **Conv2D(64, (3,3), padding='same')** | `(13, 13, 64)`   | 18,496           | `(3 * 3 * 32 + 1) * 64`                      |
| **Conv2D(64, (3,3))**                 | `(11, 11, 64)`   | 36,928           | `(3 * 3 * 64 + 1) * 64`                      |
| **MaxPooling2D((2,2))**               | `(5, 5, 64)`     | 0                | Max pooling has no learnable parameters      |
| **Flatten**                           | `(1600,)`        | 0                | Reshaping operation only (5 * 5 * 64 = 1600) |
| **Dense(256)**                        | `(256,)`         | 409,856          | `(1600 + 1) * 256`                           |
| **Dropout(0.5)**                      | `(256,)`         | 0                | Regularization operation only                |
| **Dense(10)**                         | `(10,)`          | 2,570            | `(256 + 1) * 10`                             |
| **TOTAL**                             | —                | **477,418**      | Sum of all layer parameters                  |

### Summary totals:

- **All Trainable?** Yes (There are no frozen layers or `BatchNormalization` layers tracking moving statistics).
    
- **Total Trainable Parameters:** 477,418
    
- **Total Non-Trainable Parameters:** 0
    

### Quick reminder on the formulas used:

1. **Conv2D Parameters:** $$((filter_{height} * filter_{width} * input_{channels}) + 1 \ bias \ per \ filter) * output_channels$$
    
2. **Dense Parameters:** $$(input_{units} + 1 \ bias \ per \ neuron) * output \ units$$
    
3. **Valid Padding (Default Conv2D):** Reduces spatial dimensions by `filter_size - 1` (e.g., $\frac{28 - 3 + 2 \times 0}{1} + 1) = 26$).
    
4. **Max Pooling (2x2):** Reduces dimensions by half, discarding remainders (e.g., $11 \div 2 = 5.5 \rightarrow 5$).

---

# Q2 - Transfer Learning

### Parameter Table

| **Layer / Section**        | **# Parameters** | **Trainable?** | **Calculation**                              |
| -------------------------- | ---------------- | -------------- | -------------------------------------------- |
| **VGG16 base (frozen)**    | 14,714,688       | No             | Given (pre-trained weights)                  |
| **GlobalAveragePooling2D** | 0                | No             | Pooling operations have no learnable weights |
| **Dense(256)**             | 131,328          | Yes            | `(512 inputs + 1 bias) * 256 units`          |
| **Dense(10)**              | 2,570            | Yes            | `(256 inputs + 1 bias) * 10 units`           |
| **TOTAL**                  | **14,848,586**   | —              | Sum of all parameters                        |

### Totals Breakdown

|**Metric**|**Value**|
|---|---|
|**Total Parameters**|14,848,586|
|**Total Trainable**|133,898 _(131,328 + 2,570)_|
|**Total Non-Trainable**|14,714,688 _(The frozen VGG16 base)_|

### Trainable Status (Before vs. After)

|**Total Trainable (before)**|**Total Trainable (after setting trainable=True)**|
|---|---|
|**133,898**|**14,848,586**|

**Why?**

Initially, we set `base_model.trainable = False`, which "freezes" the 14.7 million weights in the VGG16 base. During backpropagation, only the weights in the new Dense layers (133,898 parameters) are updated. If we later set `base_model.trainable = True` (a process called fine-tuning), we unfreeze the base. Now, the optimizer will update _all_ 14,848,586 parameters across both the VGG16 base and the custom Dense layers.

### Conceptual Question

**Why do we use `include_top=False` in transfer learning?**

**Reason:** The "top" of VGG16 refers to its final fully connected Dense layers, which are hardcoded to output probabilities for the 1,000 specific classes of the ImageNet dataset. By setting `include_top=False`, we discard this ImageNet-specific classification head but keep the convolutional base. This allows us to use VGG16 as a generalized feature extractor and attach our own custom "top" (like the `Dense(10)` layer) to classify images into our own specific categories.

> The `GlobalAveragePooling2D` layer take the average of the **entire** feature map, turning the 
> `7 * 7 * 512` 3D block, into a 1D vector with 512 numbers, each is the average of its respective feature map