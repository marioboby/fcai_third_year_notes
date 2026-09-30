---
{"dg-publish":true,"permalink":"/3rd-year/reasoning/after-mid/lecture-6-neuro-fuzzy-system-mamdani-approach/","dg-note-properties":{}}
---



### **Introduction to Neuro-Fuzzy Systems**
The lecture focuses on how to combine Neural Networks (NN) with Fuzzy Logic to develop a **Neuro-Fuzzy System (NFS)**. The primary goal of an NFS is to improve the performance of a fuzzy reasoning tool (or fuzzy logic controller) by representing it using the learning structure of a neural network. 

While you can combine these two concepts in different ways—such as a "Fuzzy Neural Network" where individual neurons use fuzzy set theory —the Neuro-Fuzzy System is far more popular and practical for solving real-world problems. The lecture specifically concentrates on an NFS based on the **Mamdani Approach**.

---

### **The 5-Layer Architecture of an NFS**
An NFS using the Mamdani approach is structured into five distinct layers, each performing a specific function in the fuzzy inference process:

* **Layer 1 (Input Layer):** This layer uses a linear transfer function. Its purpose is simply to receive the real crisp inputs and pass them forward. The outputs of this layer are exactly the same as the corresponding inputs.
  <br>
* **Layer 2 (Fuzzification Layer):** This layer takes the crisp inputs and determines their degree of belonging (membership) to different fuzzy linguistic terms (like "Near", "Far", "Small", "Medium"). It uses defined membership function distributions, such as right-angled or isosceles triangles.
    <br>
* **Layer 3 (AND Operation Layer):** This layer looks at the combinations of fuzzified inputs to form the antecedents ("If" parts) of the fuzzy rules. It performs logical AND operations, typically using the "minimum" mathematical function, to determine the firing strength of each combination.
    <br>
* **Layer 4 (Fuzzy Inference Layer):** This layer maps the activated combinations to their corresponding output consequences ("Then" parts) based on a predefined Rule Base. It outputs the set of fired rules along with their respective strengths.
    <br>
* **Layer 5 (Defuzzification Layer):** This final layer aggregates the fuzzy outputs from Layer 4 and converts them back into a single, usable crisp number. The lecture highlights the **Center of Sums Method** for this calculation:
    
    $$\text{Crisp Output} = \frac{\sum_{i=1}^{r} A_i f_i}{\sum_{i=1}^{r} A_i}$$
    *(Where $A_i$ is the area of the respective fired rule shape, and $f_i$ is the center of that area).*

To optimize this system, the network can be tuned using methods like Batch mode training, Backpropagation (BP) algorithms, or Genetic Algorithms (GA).

---

### **Step-by-Step Numerical Example Walkthrough**
The bulk of the lecture is dedicated to a numerical example to calculate the prediction deviation of the system. 

**The Setup:**
* **Inputs:** $I_1 = 1.6$, $I_2 = 18.0$
* **Target Output:** $O = 9.0$
* **Linguistic Terms:** 
	- $I_1$: Near (NR), Far (FR), Very Far (VFR).
	- $I_2$: Small (SM), Medium (M), Large (LR).
	- Output $O$: Low (LW), High (H), Very High (VH).

Here is how the data flows through the 5 layers:

#### **Layer 1**
The inputs are passed directly through.
* $1_{O1} = I_1 = 1.6$
* $1_{O2} = I_2 = 18.0$

#### **Layer 2 (Fuzzification)**
We determine where the inputs fall on the modified triangular membership graphs.
* For **$I_1 = 1.6$**: It falls between the "Near" (NR) and "Far" (FR) triangles. Based on the graph slopes, the calculated membership values are:
    * $\mu_{NR} = 0.25$
    * $\mu_{FR} = 0.75$
* For **$I_2 = 18.0$**: It falls between the "Small" (SM) and "Medium" (M) triangles. 
    * $\mu_{SM} = 0.272727$
    * $\mu_{M} = 0.727272$

![Pasted image 20260520221807.png](/img/user/imgs/Pasted%20image%2020260520221807.png)

#### **Layer 3 (AND Operation)**
Because we have 2 non-zero states for $I_1$ and 2 non-zero states for $I_2$, there are 4 combinations (rules) that activate. We use the **minimum** value to find the strength of each combination:
1.  **NR AND SM:** $\min(0.25, 0.272727) = \mathbf{0.25}$
2.  **NR AND M:** $\min(0.25, 0.727272) = \mathbf{0.25}$
3.  **FR AND SM:** $\min(0.75, 0.272727) = \mathbf{0.272727}$
4.  **FR AND M:** $\min(0.75, 0.727272) = \mathbf{0.727272}$

#### **Layer 4 (Fuzzy Inference)**
Using the colored Rule Base grid from the slides, we map the 4 activated combinations to their specific output consequence and attach the strengths calculated in Layer 3:
1.  If $I_1$ is NR AND $I_2$ is SM $\rightarrow$ **$O$ is LW** (Strength: 0.25)
2.  If $I_1$ is NR AND $I_2$ is M $\rightarrow$ **$O$ is LW** (Strength: 0.25)
3.  If $I_1$ is FR AND $I_2$ is SM $\rightarrow$ **$O$ is H** (Strength: 0.272727)
4.  If $I_1$ is FR AND $I_2$ is M $\rightarrow$ **$O$ is H** (Strength: 0.727272)

![Pasted image 20260511210449.png](/img/user/imgs/Pasted%20image%2020260511210449.png)

#### **Layer 5 (Defuzzification)**
We now have four "fired" fuzzy shapes. Because the strengths act as a ceiling, the triangular output shapes are cut off (truncated) at their respective strength levels, turning them into trapezoids (or smaller shapes). 

Using geometric formulas (as shown in the handwritten notes), the area ($A$) and center of area ($f$) are calculated for each of the four shapes:
* **Rule 1 (LW):** $A_1 = 0.9625$, $f_1 = 6.466667$
* **Rule 2 (LW):** $A_2 = 0.9625$, $f_2 = 6.466667$
* **Rule 3 (H):** $A_3 = 2.072724$, $f_3 = 9.4$
* **Rule 4 (H):** $A_4 = 4.072722$, $f_4 = 9.4$

Finally, the **Center of Sums** formula is applied to these values:
$$5_{O1} = \frac{(0.9625 \times 6.466) + (0.9625 \times 6.466) + (2.072 \times 9.4) + (4.072 \times 9.4)}{0.9625 + 0.9625 + 2.072724 + 4.072722}$$
$$\mathbf{5_{O1} = 8.700328}$$

The **Defuzzification Layer (Layer 5)** is the critical final step in a Neuro-Fuzzy System. Its entire purpose is to translate the fuzzy, linguistic conclusions reached by the network back into a concrete, real-world number that a system can actually use.

If a fuzzy controller is managing an air conditioner, the rules might determine that the cooling fan needs to spin at a speed that is a combination of "Medium" and "Fast." The defuzzification layer calculates exactly how many Revolutions Per Minute (RPM) that combination translates to.

### 1. The Hand-off from Layer 4 (Clipping)

Before Layer 5 can do its math, it receives the "fired" rules from Layer 4.

Each fired rule has a specific **strength** (a number between 0 and 1 calculated by the AND operations). This strength acts as a horizontal blade that slices off the top of the output membership triangles.

- If a rule has a strength of `0.25`, the output triangle is truncated horizontally at the `0.25` mark on the y-axis.
    
- The original triangle is transformed into a trapezoid. The stronger the rule fires, the taller the resulting trapezoid.
    

### 2. The Geometric Breakdown

Layer 5 looks at each of these resulting shapes (the trapezoids) and calculates two specific properties for each one:

- **Area ($A_i$):** How much "weight" or influence this specific rule should have on the final decision. A taller, wider shape has more area and therefore more pull.
    
- **Center of Area ($f_i$):** The balance point (centroid) of that specific shape on the x-axis.
    

_(Note: Calculating the area and centroid of a trapezoid requires breaking it down geometrically, which is why your lecture notes show specific equations for finding these values based on the base widths and the cutoff heights)._

### 3. The Center of Sums Calculation

Once Layer 5 has the Area ($A$) and Center ($f$) for every fired rule, it aggregates them using a weighted average formula:

$$\text{Crisp Output} = \frac{(A_1 \cdot f_1) + (A_2 \cdot f_2) + ... + (A_n \cdot f_n)}{A_1 + A_2 + ... + A_n}$$

**Why use Center of Sums?** There are other defuzzification methods (like the _Center of Gravity_ method), but Center of Sums is highly popular in Neuro-Fuzzy Systems because it is computationally faster. Instead of trying to calculate the complex mathematical union of overlapping shapes, Center of Sums simply adds the areas together, even if they overlap. This speed is crucial when training the network through many iterations.

---
#### **Final Calculation**
The final step is to find the deviation in prediction. We subtract our predicted output from the known target output:
* Target Output = $9.0$
* Predicted Output = $8.700328$
* **Deviation:** $9.0 - 8.700328 = \mathbf{0.299672}$