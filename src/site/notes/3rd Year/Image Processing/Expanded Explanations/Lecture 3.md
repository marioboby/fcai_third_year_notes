---
{"dg-publish":true,"permalink":"/3rd-year/image-processing/expanded-explanations/lecture-3/","dg-note-properties":{}}
---

## Practical Exercise: Min-Max Contrast Stretching

**The Scenario:**

You have captured an image, but due to poor illumination, it suffers from low contrast. When you analyze the image histogram, you find that the pixel intensities only range from a minimum of **100** to a maximum of **151** (a very narrow, gray-washed image).

You want to stretch this contrast to fill the standard 8-bit dynamic range, which goes from **0** to **255**.

**The Formula:**

$$I_{new} = (I - Min)\frac{NewMax - NewMin}{Max - Min} + NewMin$$

**Step 1: Identify your variables**

- **Min (original minimum):** 100
- **Max (original maximum):** 151
- **NewMin (target minimum):** 0
- **NewMax (target maximum):** 255

**Step 2: Plug the limits into the scaling factor**

$$\text{Scaling Factor} = \frac{255 - 0}{151 - 100} = \frac{255}{51} = 5$$

**Step 3: Create your simplified transformation equation**

Now substitute the scaling factor back into the formula. Your specific transformation function for every pixel $I$ in this image is:

$$I_{new} = (I - 100) \times 5$$

**Step 4: Test it on specific pixels!**

Let's see what happens to the pixels in the original image:

- **The darkest pixel ($I = 100$):** $I_{new} = (100 - 100) \times 5 = 0 \times 5 = \mathbf{0}$ _(Now pure black)_
- **The brightest pixel ($I = 151$):** $I_{new} = (151 - 100) \times 5 = 51 \times 5 = \mathbf{255}$ _(Now pure white)_
- **A mid-tone pixel ($I = 120$):** $I_{new} = (120 - 100) \times 5 = 20 \times 5 = \mathbf{100}$

**Conclusion:** By applying this simple linear operation to every pixel, you have successfully stretched the narrow 51-level range into a full 256-level range, significantly enhancing the visual contrast!

![Pasted image 20260227225212.png](/img/user/imgs/Pasted%20image%2020260227225212.png)

---

## Graph meaning

This slide explains a fundamental concept in digital image processing known as a **Piecewise Linear Transformation**, specifically used for **Contrast Stretching**.

It shows how you can change the brightness and contrast of an image by mapping the original pixel values (input) to new pixel values (output) using a mathematical graph.

Here is a breakdown of the concepts shown on the slide:

![Pasted image 20260227230751.png](/img/user/imgs/Pasted%20image%2020260227230751.png)


### The Basics of the Graph

- **X-axis (Input Intensity level, $r$):** Represents the original pixel values of your image. It ranges from $0$ (pure black) to $L-1$ (pure white). For a standard 8-bit image, $L-1$ is 255.
    <br>
- **Y-axis (Output Intensity level, $s$):** Represents the new pixel values after the transformation.
    <br>
- **The Control Points $(r_1, s_1)$ and $(r_2, s_2)$:** These are the "hinges" of the line. By moving these two points around the graph, you alter the shape of the line, which completely changes how the image's contrast is manipulated.
    

---

### The Three Scenarios Explained

**1. Linear Function**

- **What the slide says:** When $r_1 = s_1$ and $r_2 = s_2$.
    
- **What it means:** If the $x$ and $y$ coordinates are identical for both points, the points fall perfectly on a straight 45-degree diagonal line going from $(0,0)$ to $(L-1, L-1)$. In this case, every input pixel maps to the exact same output pixel ($s = r$).
    
- **Result:** The image doesn't change at all. This is called an "identity transformation."
    

**2. Thresholding Function**

- **What the slide says:** When $r_1 = r_2$, $s_1 = 0$, and $s_2 = L-1$.
    
- **What it means:** If $r_1$ and $r_2$ are the same, the middle section of the line becomes perfectly vertical. Because $s_1$ is at the very bottom (0) and $s_2$ is at the very top ($L-1$), it creates a hard cutoff. Any input pixel value below that vertical line becomes pure black ($0$), and any value above it becomes pure white ($L-1$).
    
- **Result:** This turns a grayscale image into a completely binary (black and white only) image. It's often used to separate objects from a background.
    

**3. Min-Max Stretching**

- **What the slide says:** When $(r_1, s_1) = (r_{min}, 0)$ and $(r_2, s_2) = (r_{max}, L-1)$.
    
- **What it means:** Imagine an image that looks washed out because its darkest pixel isn't truly black (let's say $r_{min}$ is 50) and its brightest pixel isn't truly white (let's say $r_{max}$ is 200). This transformation takes that lowest existing value ($r_{min}$) and forces it down to $0$ (pure black). It takes the highest existing value ($r_{max}$) and forces it up to $L-1$ (pure white).
    
- **Result:** It stretches out the intermediate pixel values to fill the entire available dynamic range, making darks darker, lights lighter, and significantly improving the overall contrast of a dull image.