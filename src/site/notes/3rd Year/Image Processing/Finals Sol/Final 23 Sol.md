---
{"dg-publish":true,"permalink":"/3rd-year/image-processing/finals-sol/final-23-sol/","dg-note-properties":{}}
---



> شرح الاجابات يا اما تحت السؤال هحط سبب الاجابة الصح, يا هحط جنب كل اختيار غلط السبب اني مختارتوش

## **1.** Image restoration usually uses a model that is based on -----------------------

==a) Additive noise==

b) Multiplication noise

c) Subtractive noise

d) None of the above

![Pasted image 20260607153043.png](/img/user/imgs/Pasted%20image%2020260607153043.png)

## **2.** The process of moving a filter mask over the image, and computing the sum of products at each location is called -----------------------

==a) Correlation==

b) Rotation

c) Linearity

d) Convolution

![Pasted image 20260607153435.png](/img/user/imgs/Pasted%20image%2020260607153435.png)

## **3.** Which of the following expressions is used to denote spatial domain enhancement?

==a) $g(x,y) = T[f(x,y)]$==

b) $g(xy) = T[f(xy)]$

c) $g(x+y) = T[f(x+y)]$

d) $g(x-y) = T[f(x-y)]$


## **4.** Which of the following filters remove the pepper noise?

a) Maximum filter 

b) Median filter 

c) Contra harmonic mean filter

==d) All are correct==

> Max filters remove pepper (black) noise, median removes both, and contra harmonic (with positive Q) removes pepper.
## **5.** Which of the following filters remove the salt-and-pepper noise?

a) Average filter

==b) Median filter==

c) Sobel filter

d) Laplacian filter

> as Q4
## **6.** Convolution is usually used in the --------------- domain(s).

==a) Spatial==

b) Frequency (uses multiplication, not convolution)

c) Both spatial and frequency

d) None of the above

## **7.** Ideal filters can be -------------------

a) LPF (low pass filter)

b) HPF (high pass filter)

c) BPF (band pass filter)

==d) All are correct.==

## **8.** The idea behind -------------- is to increase the dynamic range of the gray levels in the image being processed.

a) Intensity-level slicing

==b) Contrast stretching==

c) Bit-plane slicing

d) None of the above.

## **9.** In Contra harmonic Mean, negative values of $Q$ eliminate ---------------- noise

a) Periodic

b) Pepper

==c) Salt==

d) None is correct

## **10.** In bit plane slicing the most of the information of an image is contained by -------------

a) Lowest order plane

b) Mid order plane

==c) Highest order plane==

d) All are correct

## **11.** If $f(x, y)$ represents an input image, and $\nabla^2f(x,y)$ represents Laplacian, then if a high boost filtered image is given by:

![Pasted image 20260607155036.png](/img/user/imgs/Pasted%20image%2020260607155036.png)

For what value of $A$ this high boost filtering becomes the standard Laplacian sharpening filter?

a) 0

==b) 1==

c) -1

d) $\infty$

> As the equation for 4, or 8-Centered Laplacian: $g(x,y) = f(x,y) + c[\nabla^2 f(x,y)]$, where c is 1 if center was positive, -1 otherwise

## **12.** Applying the logical AND operation with an input image, and a mask is the same as applying ----------- arithmetic operation.

a) Addition

b) Subtraction

==c) Multiplication==

d) Division

## **13.** Alpha trimmed mean filter is useful in ----------------------

a) Salt noise

b) Pepper noise

c) Salt-and-pepper noise

==d) Combination of salt-and-pepper and Gaussian noise==

## **14.** Which of the following is the transfer function of a gray level slicing method?

![Pasted image 20260607162947.png](/img/user/imgs/Pasted%20image%2020260607162947.png)

> Highlights a specific range of intensities $[A, B]$. Can either set all other pixels to black (binary mapping) or preserve their original values.
> 
> This specific piecewise function isolates a band of intensities and pushes everything else to the original values.

## **15.** Apply the Laplacian filter for the highlighted pixel in the below image. Use the Laplacian filter with size 3x3 and has 5 in the center. The resulting corresponding pixel value is ---------------

![Pasted image 20260607163039.png](/img/user/imgs/Pasted%20image%2020260607163039.png)

a) 32

==b) -13==

c) -10

d) 33

Using the composite kernel $\begin{bmatrix} 0 & -1 & 0 \\ -1 & 5 & -1 \\ 0 & -1 & 0 \end{bmatrix}$

![Pasted image 20260607163123.png](/img/user/imgs/Pasted%20image%2020260607163123.png)

## **16.** The resulting pixel in question 15 will be more --------------------- than the input pixel.

==a) Sharpen==

b) Smooth

c) Blur

d) None of the above.

## **17.** Laplacian filter in question 15 applies the ----------------- derivative.

a) First

==b) Second==

c) Third

d) None of the above.

## **18.** Calculate the alpha trimmed mean filter of the following pixel values where $d=0$.

![Pasted image 20260607163913.png](/img/user/imgs/Pasted%20image%2020260607163913.png)

==a) ~ 351==

b) ~ 229

c) ~ 526

d) 344

> Just Average Filter: $\frac{304 + 366 + 325 + 350 + 425 + 335}{6} \approx 351$

## **19.** Calculate the alpha trimmed mean filter of the pixel values in question 18 where $d=1$.

a) ~ 351

b) ~ 229

c) ~ 526

==d) 344==

> $d$ can't be odd, so we'll suppose it's the number of values to omit at each end "basically removing the highest and lowest numbers", then averaging the rest

## **20.** Apply the midpoint filter for the highlighted pixel in the below sub image using a 3x3 window size. [Assume zero padding]. The resulting corresponding pixel value is ----------------.

![Pasted image 20260607164839.png](/img/user/imgs/Pasted%20image%2020260607164839.png)

a) 0

==b) 5==

c) 10

d) 15

> $\frac{max + min}{2} = \frac{10 + 0 (from \ padding)}{2} = 5$

## **21.** The First derivative approximation says the values of constant intensities must be:

==a) Zero==

b) One

c) Negative

d) Positive

## **22.** Thresholding gives a:

a) Larger image

b) Grayscale image

c) Color Image

==d) Binary Image==

## **23.** In the splitting and merge algorithm, the criteria used to split images into sub-regions is:

a) Pixel intensity

b) Mean

==c) Variance==

d) Discontinuity

![Pasted image 20260607185527.png](/img/user/imgs/Pasted%20image%2020260607185527.png)

## **24.** Main operation(s) of morphological operations are:

a) Erosion

b) Dilation

c) Opening

==d) Both a and b==

## **25.** Full containment of the SE in an image is required in:

==a) Erosion==

b) Dilation

c) Opening

d) Closing

## **26.** Hit and miss operation is used for:

a) Noise removal

==b) Shape detection==

c) Image thresholding

d) Color detection

## **27.** Hold out method for data collection means:

a) A training set is generated by randomly selecting N samples using replacement. The samples not selected for training are used for testing (**bootstraping**).

==b) Split the training set by using two-thirds for training and the other third for testing.==

c) Use all available data to design a classifier. Then use the same data again to test the classifier (**Resubstituting**).

d) The data set is divided into $k$ subsets. For different runs, one of the $k$ subsets is used as the test set and the other $k-1$ subsets are put together to form a training set (K fold cross-validation method).

## **28.** F1 score combines ............and ..............into a single measure.

a) Accuracy and miss classification rate

b) Sensitivity and Specificity

==c) Precision and recall==

d) Recall and Sensitivity

## **29.** To solve the sensitivity of noise for the second derivative approach of edge detection, we can use the following mask:

==a) LOG==

b) Zero crossing

c) Sobel Mask

d) Laplacian mask

> The Gaussian blur in LoG handles the noise sensitivity of the standard Laplacian.

## **30.** Dilation, causes objects to ..............; erosion causes objects to ..............

==a) Grow, shrink==

b) Shrink, grow

c) Grow, grow

d) Shrink, shrink

## **31.** For the following image, the image histogram will be as follows:

| p1  | p2  | p3  | p4  | p5  |
| --- | --- | --- | --- | --- |
| 5   | 2   | 3   | 6   | 1   |
| 4   | 3   | 5   | 5   | 4   |
| 4   | 8   | 4   | 5   | 2   |
| 8   | 9   | 2   | 4   | 6   |
| 2   | 1   | 3   | 8   | 7   |

a)

**Grey-level:** 1 | 2 | 3 | 4 | 5 | 6 | 7 | 8 | 9

**Frequency:** 3 | 2 | 4 | 6 | 4 | 1 | 1 | 3 | 1

b)

**Grey-level:** 1 | 2 | 3 | 4 | 5 | 6 | 7 | 8 | 9

**Frequency:** 3 | 2 | 5 | 4 | 5 | 3 | 2 | 1 | 1

==c)==

**Grey-level:** 1 | 2 | 3 | 4 | 5 | 6 | 7 | 8 | 9

**Frequency:** 2 | 4 | 3 | 5 | 4 | 2 | 1 | 3 | 1

d)

**Grey-level:** 1 | 2 | 3 | 4 | 5 | 6 | 7 | 8 | 9

**Frequency:** 2 | 4 | 3 | 5 | 2 | 1 | 1 | 4 | 1

## **32.** Get the appropriate thresholding value to segment the image in problem 31 using the global thresholding method:

a) 3

b) 7

c) $(a*b)/2$

==d) $(a+b)/2$==

==since we don't know how the regions are split, we'll assume a is the average intensity of first region A1, and B is the average intensity of second region B1==

## **33.** For the following error matrix, find the overall accuracy:

![Pasted image 20260607190004.png](/img/user/imgs/Pasted%20image%2020260607190004.png)

a) 22.1 %

==b) 77.8 %==

c) 38.9 %

d) 88.9 %

![Pasted image 20260607190020.png](/img/user/imgs/Pasted%20image%2020260607190020.png)

## **34.** For the error matrix in problem 33, find the F1-score for class Forest:

==a) 0.81==

b) 0.79

c) 0.40

d) 0.83

![Pasted image 20260607190040.png](/img/user/imgs/Pasted%20image%2020260607190040.png)

## **35.** For the error matrix in problem 33, the precision of class water is........, while the recall is ...............

a) 0.63, 0.77

b) 0.36, 0.22

==c) 0.77, 0.63==

d) 0.22, 0.36

![Pasted image 20260607190059.png](/img/user/imgs/Pasted%20image%2020260607190059.png)

## **36.** In the corresponding figure, the small squares have dimensions 10x10, the large square has dimensions 20x20, and a circle has a radius of $r=15$. The required operation to obtain Figure (b) from Figure (a) is:

![Pasted image 20260607190409.png](/img/user/imgs/Pasted%20image%2020260607190409.png)

==a) Opening Operation==

b) Closing Operation

c) Dilation Operation

d) Erosion Operation

> since I don't see a difference, I'll assume he means removing the small squares, that requires erosion, then dilation to get the surviving objects back to their original sizes, hence: **Opening**

## **37.** To perform the required operation in problem 36, SE can be square with the following dimension:

==a) 20 x 20==

b) 5 x 5

c) 30 x 30

d) 10 x 10

- If we use a $10 \times 10$ SE, it perfectly fits inside the $10 \times 10$ noise squares. They would survive the erosion as a single pixel and be re-inflated by the dilation.
    
- Therefore, the SE must be _larger_ than the noise we want to remove, but small enough to fit inside the objects we want to keep.
    
- A **$20 \times 20$ (a)** square SE is perfect. The $10 \times 10$ noise cannot contain it, so they are erased. The $20 \times 20$ square perfectly contains it (surviving as 1 pixel), and the circle (diameter 30) easily contains it. After dilation, the large square and circle are restored!

## **38.** Get gradient magnitude Sobel response for the marked pixel:

| p1  | p2  | p3  | p4  | p5  |
| --- | --- | --- | --- | --- |
| 15  | 20  | 13  | 11  | 1   |
| 4   | 3   | 15  | 3   | 4   |
| 4   | 8   | 4   | 5   | 2   |
| 8   | 9   | 8   | 3   | 6   |
| 2   | 1   | 3   | 8   | 7   |

a) 36

b) 12

==c) 38==

d) 0

![Pasted image 20260607190315.png](/img/user/imgs/Pasted%20image%2020260607190315.png)

## **39.** The extracted boundary using 8-connected SE of the corresponding object using morphological operation will be:

*assuming inner boundary*

**Original Object:**

|q|w|e|r|t|y|
|---|---|---|---|---|---|
|1|1|1|0|1|1|
|1|1|1|0|1|1|
|1|1|1|1|1|1|
|1|1|1|1|1|1|

a)

0 0 0 0 0 0

0 1 0 0 0 0

0 1 0 0 0 0

0 0 0 0 0 0

==b)==

1 1 1 0 1 1

1 0 1 0 1 1

1 0 1 1 1 1

1 1 1 1 1 1

c)

1 1 1 0 1 1

1 1 1 0 1 1

1 1 1 1 1 1

1 1 1 1 1 1

d)

0 0 0 1 0 0

0 1 0 1 0 0

0 1 0 0 0 0

0 0 0 0 0 0

> Boundary = Original Image - Eroded Image
> 
> During erosion, you slide this 3 x 3 grid over every pixel. The rule for erosion is strict: A pixel remains 1 ONLY IF its entire 3 x 3 neighborhood is completely filled with 1s. If even a single neighbor is 0 (or if the pixel is on the edge of the image and touches the "outside" boundaries), it turns into a 0.

![Pasted image 20260607191917.png](/img/user/imgs/Pasted%20image%2020260607191917.png)

*only these 2 pixels will stay 1*

Original Image:
1 1 1 0 1 1
1 1 1 0 1 1
1 1 1 1 1 1
1 1 1 1 1 1

Minus Eroded Image:
0 0 0 0 0 0
0 1 0 0 0 0
0 1 0 0 0 0
0 0 0 0 0 0

Equals Final Boundary:
1 1 1 0 1 1
1 0 1 0 1 1
1 0 1 1 1 1
1 1 1 1 1 1
## **40.** For the following image, the corresponding hough transform matrix for only the highlighted pixel with index (1,1) will be:

_(Hint: $\Theta = [0, \frac{\pi}{2}, \pi]$, $\rho = [0,1,2]$)_

**Image Matrix:**

|a|b|c|
|---|---|---|
|1|1|0|
|1|1|0|
|1|1|1|

==a)==

|**Θ**|**0**|**2π​**|**π**|
|---|---|---|---|
|$\rho=0$|0|0|1|
|$\rho=1$|1|1|0|
|$\rho=2$|0|0|0|

b)

|**Θ**|**0**|**2π​**|**π**|
|---|---|---|---|
|$\rho=0$|1|1|1|
|$\rho=1$|1|1|1|
|$\rho=2$|1|1|1|

c)

|**Θ**|**0**|**2π​**|**π**|
|---|---|---|---|
|$\rho=0$|0|0|0|
|$\rho=1$|0|0|0|
|$\rho=2$|0|0|0|

d)

|**Θ**|**0**|**2π​**|**π**|
|---|---|---|---|
|$\rho=0$|0|0|0|
|$\rho=1$|1|1|1|
|$\rho=2$|0|0|0|

$$\rho = x \cos(\theta) + y \sin(\theta)$$

- **For $\theta = 0$:** $\rho = 1(1) + 1(0) = \mathbf{1}$. (This gets a vote at $\rho=1, \theta=0$)
    
- **For $\theta = \frac{\pi}{2}$:** $\rho = 1(0) + 1(1) = \mathbf{1}$. (This gets a vote at $\rho=1, \theta=\frac{\pi}{2}$)
    
- **For $\theta = \pi$**: $\rho = 1(-1) + 1(0) = -1 \rightarrow \text{Clip to 0} \rightarrow \mathbf{0}$