---
{"dg-publish":true,"permalink":"/3rd-year/image-processing/before-mid/lecture-3-intensity-transformation-and-spatial-filtering-ch3/","dg-note-properties":{}}
---


## 1. Image Enhancement Fundamentals

Image enhancement is the process of manipulating an image so the result is more suitable than the original for a _specific application_.

- **Key Goals:** Highlighting interesting details, removing noise, and making images more visually appealing .
<br>
- **Problem-Oriented:** Enhancement techniques are highly specific; a method used for enhancing X-ray images may not be suitable for satellite images .

### Spatial Domain Methods

Spatial domain operations operate directly on the pixels of the image and can be reduced to the form:

$$g(x,y) = T[f(x,y)]$$

Where $f(x,y)$ is the input image, $g(x,y)$ is the processed image, and $T$ is an operator defined over a specific neighborhood around $(x,y)$ .

### Point Processing

The simplest spatial domain operation occurs when the neighborhood is just the pixel itself $(1 \times 1)$. It takes the form:

$$s = T(r)$$

Where $r$ is the original image pixel value, $s$ is the processed image pixel value, and $T$ is the transformation mapping .

---

## 2. Basic Intensity Transformation Functions

There are three basic types of mathematical functions used frequently for image enhancement: Linear, Logarithmic, and Power-Law .

### A. Linear (Identity & Negative)

- **Image Negatives:** The negative of an image with intensity levels in the range $[0, L-1]$ is given by:
    
    $$s = L - 1 - r$$
    
- **Application:** Highly suitable for enhancing white or gray details embedded in large dark/black regions of an image .
    

### B. Logarithmic

- **Log Transformation:** Maps a narrow range of low (dark) input gray-level values into a wider range of output values.
    
    $$s = c * \log(1 + r)$$
    
- **Application:** Particularly useful for expanding the dark pixels in an image while compressing the higher-level values. It is famously used to reveal more detail in the Fourier spectrum of an image .
    
- **Inverse Log:** Performs the exact opposite transformation.
    

### C. Power-Law (Gamma)

- **Gamma Transformation:** Maps values based on an exponent $\gamma$ .
    
    $$s = c * r^\gamma$$
    
- **Behavior:** Fractional values of $\gamma$ (where $\gamma < 1$) map a narrow range of dark input values into a wider range of output values. Higher values of $\gamma$ (where $\gamma > 1$) do the opposite.
    
- **Gamma Correction:** Display monitors often do not respond linearly to different intensities (they naturally darken images). A power-law transformation is used to precondition the image before display to correct this hardware phenomenon .
    

---

## 3. Piecewise-Linear Transformation Functions

Unlike standard mathematical functions, piecewise linear functions can be arbitrarily complex, allowing for highly customized intensity mappings .

### A. Contrast Stretching

Contrast is the difference between the minimum and maximum pixel intensity in an image . Low contrast can result from poor illumination, lack of dynamic range in the sensor, or wrong lens aperture settings .

![Pasted image 20260227224343.png](/img/user/imgs/Pasted%20image%2020260227224343.png)


- **Definition:** A process that expands the range of intensity levels in an image so that it spans the full available intensity range of the display device (also known as normalization) .
<br>
- **Min-Max Stretching Equation:**
    $$I_{new} = (I - Min)\frac{NewMax - NewMin}{Max - Min} + NewMin$$
    _Where $I$ is the input intensity, $I_{new}$ is the output, $(NewMin, NewMax)$ is the target range (usually 0 to 255), and $(Min, Max)$ is the original intensity range_ .
    <br>
- **Global vs. Local:**  
	- _Global:_ Increases contrast across the entire image uniformly.
	- _Local:_ Divides the image into small regions and performs contrast enhancement on each region independently .

### B. Thresholding

Thresholding converts a grayscale image into a binary image by comparing every pixel to a specific threshold value $m$ (or $k$) .

$$s = \begin{cases} 1 & \text{If } r \ge m \\ 0 & \text{If } r < m \end{cases}$$

- **Process:** If a pixel's intensity is greater than or equal to the threshold, it is mapped to 1 (or 255/White). If it is below the threshold, it is mapped to 0 (Black) .

To see numerical examples, [[3rd Year/Image Processing/Expanded Explanations/Lecture 3\|here]]

## Summary

| **Transformation Type**                    | **Mathematical Formula**                                               | **Behavior / Characteristics**                                                                                                                   | **Primary Application**                                                                                                  |
| ------------------------------------------ | ---------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------ | ------------------------------------------------------------------------------------------------------------------------ |
| **Image Negative** (Linear)                | $s = L - 1 - r$                                                        | Reverses the intensity levels of an image.                                                                                                       | Enhancing white or gray details embedded in large dark/black regions (e.g., medical X-rays).                             |
| **Logarithmic**                            | $s = c \times \log(1 + r)$                                             | Expands a narrow range of dark input values while compressing higher-level values.                                                               | Revealing hidden details in images with massive dynamic ranges, like the Fourier spectrum.                               |
| **Power-Law (Gamma)**                      | $s = c \times r^\gamma$                                                | Maps values based on an exponent.<br><br>• $\gamma < 1$: Lightens image (expands darks).<br><br>• $\gamma > 1$: Darkens image (expands brights). | **Gamma Correction:** Preconditioning images to display correctly on monitors that do not respond linearly to intensity. |
| **Contrast Stretching** (Piecewise-Linear) | $I_{new} = (I - Min)\frac{NewMax - NewMin}{Max - Min} + NewMin$        | Stretches the original intensity range to span a new, wider target range (usually $0$ to $255$).                                                 | Normalizing images with poor illumination or fixing low dynamic range sensor captures.                                   |
| **Thresholding** (Piecewise-Linear)        | $s = 1 \text{ if } r \ge m$<br><br>  <br><br>$s = 0 \text{ if } r < m$ | Maps pixels above a threshold $m$ to White (1/255) and below to Black (0).                                                                       | Binarizing an image to segment or isolate specific objects from the background.                                          |
(Note: $r$ is the input intensity, $s$ is the output intensity, $L$ is the number of intensity levels, $c$ is a scaling constant, and $m$ is the threshold value).