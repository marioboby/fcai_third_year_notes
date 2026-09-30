---
{"dg-publish":true,"permalink":"/3rd-year/image-processing/expanded-explanations/lecture-7/","dg-note-properties":{}}
---


# How the order parameter ($n$) in the Butterworth filter mathematically bridges the gap between the Ideal and Gaussian filters?

To understand this, let's look at the formula for the Butterworth Lowpass Filter (BLPF):

$$H(u,v) = \frac{1}{1 + [D(u,v)/D_0]^{2n}}$$

- **$D(u,v)$**: The current distance of a frequency point from the center of the spectrum.
    
- **$D_0$**: The cutoff frequency (the chosen distance where we want to start blocking high frequencies).
    
- **$n$**: The **order** of the filter. This is the magic number.
    

The magic of the Butterworth filter lies entirely in that exponent, $2n$. By simply changing the value of $n$, we fundamentally alter the geometry of the filter's shape in the frequency domain.

### **How the Order ($n$) Changes the Shape**

Imagine looking at a 1-D cross-section (a side profile) of the filter. Here is what happens as we change $n$:

1. **Low Order ($n = 1$) - The Gaussian Twin:**
    
    When $n=1$, the transition from the passband (allowing frequencies, where $H \approx 1$) to the stopband (blocking frequencies, where $H \approx 0$) is very slow and gradual. At this low order, the Butterworth curve behaves almost exactly like a **Gaussian filter**. Because the transition is so smooth, it produces absolutely no "ringing" artifacts in the final image.
    
2. **Moderate Order ($n = 2$ or $3$) - The "Sweet Spot":**
    
    As $n$ increases to 2, the "slope" of the cutoff becomes a bit steeper. It holds onto the low frequencies a little better than a Gaussian, but drops off faster once it hits $D_0$. A slight amount of ringing might technically be present, but it is usually completely imperceptible to the human eye. This is why $n=2$ is the standard default for image processing.
    
3. **High Order ($n \to \infty$) - The Ideal Shape:**
    
    Consider what happens to the math when $n$ becomes very large (e.g., $n = 20$ or higher).
    
    - If $D < D_0$ (inside the cutoff), the fraction $D/D_0$ is less than 1. A number less than 1 raised to a massive power approaches 0. So, $H = 1 / (1 + 0) = \mathbf{1}$.
        
    - If $D > D_0$ (outside the cutoff), the fraction $D/D_0$ is greater than 1. A number greater than 1 raised to a massive power approaches infinity. So, $H = 1 / (1 + \infty) = \mathbf{0}$.
        
    
    This creates an immediate, vertical drop-off at $D_0$. The filter abruptly snaps from 1 to 0. This is the exact definition of the **Ideal filter**, which creates severe ringing artifacts.






<iframe src=""D:\FCAI\ملخصات\Cognitive Science\FCAI_Third-Year_Second-Term\Image Processing\Expanded Explanations\visualizer.html"" width="100%" height="600px" style="border:none;"></iframe>
<iframe src="file:///C:/Users/YourName/Documents/ObsidianVault/butterworth.html" width="100%" height="600px" style="border:none;"></iframe>

<iframe src="file:///C:/Users/YourName/Documents/ObsidianVault/butterworth.html" width="100%" height="600px" style="border:none;"></iframe>

