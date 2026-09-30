---
{"dg-publish":true,"permalink":"/3rd-year/supervised/before-mid/lecture-5-cnn/","dg-note-properties":{}}
---


## Image Filtering

بص على محاضرة ال Spatial Filtering في الايميج ارحم

[[3rd Year/Image Processing/Before Mid/Lecture 5 - Intensity Transformation and Spatial Filtering - Ch3 - Part3#A. Linear Smoothing Filters (Averaging)\|the image is what matters here]]

---

حابب أشارك معاكم strategy note عملتها لل2D Convolution، علشان لاحظت اني لما بحاول أعمل 2D Convolution بإيدي على ورقة بتوه جدًا وسط الأرقام، إن شاء الله تفيد حد =)
[Performing 2D Convolution](https://asserknowledgespace.vercel.app/strategies/performing-2d-convolution)

By: Asser Ahmed

---

### 1. Machine Learning vs. Deep Learning

Deep Learning (DL) is a highly effective subfield of Machine Learning (ML) that uses multiple layers to learn data representations and find complex patterns.

The fundamental difference between the two lies in **Feature Extraction**:

- **Traditional Machine Learning:** The pipeline requires human intervention to extract features. The flow is: _Input Data $\rightarrow$ Manual Feature Extraction $\rightarrow$ Classification Model $\rightarrow$ Output (e.g., "Car" or "Not Car")_. This can be highly time-consuming and limited by human understanding of the feature dimensions.
    
- **Deep Learning:** The network figures out the features on its own. The flow seamlessly merges the steps: _Input Data $\rightarrow$ Feature Extraction + Classification (handled internally by the network layers) $\rightarrow$ Output_.
    

---

### 2. Convolutional Neural Networks (CNNs)

When dealing with images (like a 32x32x3 RGB image), standard neural networks struggle because they flatten the image, losing the spatial relationship between pixels.

CNNs are designed specifically to solve this. They **preserve the spatial structure** of the image and use far fewer weights by sharing them across the entire image.

![Convolutional Neural Network architecture, AI generated](https://encrypted-tbn2.gstatic.com/licensed-image?q=tbn:ANd9GcT_DuZr5p4yya0UEi5nygyygOB8xZMVaMoaESiK-LY1Xr96EaXoQpxmr4_4d98_G16EjUoiejDMsDWObQ4M3e-uncUD5_NnSI98kgX1cwLkNUagxYk)

A standard CNN is a sequence of distinct layers:

#### A. The Convolution Layer (CONV)

This is the core building block of a CNN. Instead of connecting every pixel to a neuron, a CONV layer uses **filters** (also called kernels).

- **The Sliding Process:** If you have a 32x32x3 image and a 5x5x3 filter, the network "slides" (convolves) this filter spatially over the entire image. At every location, it computes the dot product between the filter's weights and the image's pixels.

> The Kernel should have the same depth of the previous layer
    
- **Weight Sharing:** The network ensures that all locations in the image are processed using the exact same weights from that filter.
    
- **Activation Maps:** Sliding one filter across the image produces a single 2D "activation map" (e.g., a 28x28x1 map). If your CONV layer uses 6 different filters, it will produce 6 separate activation maps stacked together (a 28x28x6 volume).
    

#### B. Activation Functions (ReLU)

Between the Convolution layers, the network intersperses activation functions (like ReLU) to introduce non-linearity, allowing the network to learn complex, non-linear patterns.

#### C. The Pooling Layer (POOL)

Pooling layers are used to downsample the activation maps, reducing the spatial dimensions (width and height) while keeping the most important information.

- **Max Pooling:** The most common technique. It looks at a single depth slice of the activation map and extracts the maximum value within a specific window.
    
- **Example:** If you use a 2x2 max pooling filter with a stride of 2 on a 4x4 matrix, it will divide the matrix into four 2x2 blocks and only keep the highest number from each block, resulting in a compacted 2x2 matrix.
    

#### D. The Fully Connected Layer (FC)

After the image has passed through multiple sequences of CONV, ReLU, and POOL layers, the high-level features are finally flattened and passed into Fully Connected layers. These act just like a standard neural network to make the final classification (e.g., predicting if the image is a car, truck, airplane, ship, or horse).

more [[3rd Year/Supervised/Expanded Explanations/Lecture 5\|here]]