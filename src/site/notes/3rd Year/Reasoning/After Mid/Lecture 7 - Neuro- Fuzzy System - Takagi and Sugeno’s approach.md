---
{"dg-publish":true,"permalink":"/3rd-year/reasoning/after-mid/lecture-7-neuro-fuzzy-system-takagi-and-sugeno-s-approach/","dg-note-properties":{}}
---

While Mamdani (from Lecture 6) outputs fuzzy shapes that need to be geometrically "defuzzified," the Takagi-Sugeno model uses mathematical linear equations to determine the output of each rule. This makes it highly efficient and very popular in machine learning algorithms.

This is a breakdown of the 6-layer ANFIS architecture and the step-by-step numerical example covered in the slides.

### **The 6-Layer ANFIS Architecture**

Unlike the 5-layer Mamdani model, ANFIS uses 6 layers to process the data:

- **Layer 1 (Input Layer):** Passes the crisp inputs forward unchanged using a linear transfer function.
    
- **Layer 2 (Fuzzification Layer):** Calculates the membership values ($\mu$) for the inputs based on their respective linguistic terms (e.g., Low, High).
    
- **Layer 3 (Firing Strength):** _Key Difference from Mamdani._ Instead of taking the minimum value, this layer calculates the **product** (multiplication) of the $\mu$ values to determine the firing strength ($w$) of each rule.
    
- **Layer 4 (Normalization):** Calculates the _normalized_ firing strength ($\bar{w}$) by dividing a rule's strength by the total sum of all firing strengths.
    
- **Layer 5 (Rule Output):** Applies the first-order Sugeno equation to calculate the actual output ($y$) of each specific rule: $y_i = a_i I_1 + b_i I_2 + c_i$ (where $a, b, c$ are predefined coefficients). It then multiplies this output by the normalized firing strength from Layer 4.
    
- **Layer 6 (Overall Output):** Sums up all the weighted outputs from Layer 5 to produce the final single crisp number.
    

---

### **Step-by-Step Numerical Example Walkthrough**

We want to find the deviation in prediction for a specific training scenario.

**The Setup:**

- **Inputs:** $I_1 = 1.1$, $I_2 = 6.0$.
    
- **Target Output:** $O = 2.3$.
    
- **Base Widths ($d_1, d_2$):** Using the provided min-max normalization formula on the normalized weights ($0.3$ and $0.5$), the real base widths are calculated as $d_1 = 1.01$ and $d_2 = 5.0$.
    

#### **Layer 1**

The inputs pass directly through.

- $1_{O1} = I_1 = 1.1$
    
- $1_{O2} = I_2 = 6.0$
    

#### **Layer 2 (Fuzzification)**

Based on the modified right-angled triangle graphs (using the calculated $d_1$ and $d_2$ base widths):

- For **$I_1 = 1.1$**:
    
    - $\mu_{LW} = 0.900990$
        
    - $\mu_{H} = 0.099009$
        
- For **$I_2 = 6.0$**:
    
    - $\mu_{SM} = 0.8$
        
    - $\mu_{LR} = 0.2$
        

#### **Layer 3 (Firing Strengths)**

We multiply the membership values to find the strength ($w$) of the 4 activated rules:

1. $w_1$ (LW and SM) = $0.900990 \times 0.8 = \mathbf{0.720792}$
    
2. $w_2$ (LW and LR) = $0.900990 \times 0.2 = \mathbf{0.180198}$
    
3. $w_3$ (H and SM) = $0.099009 \times 0.8 = \mathbf{0.079207}$
    
4. $w_4$ (H and LR) = $0.099009 \times 0.2 = \mathbf{0.019802}$
    

#### **Layer 4 (Normalization)**

We normalize the weights by dividing each $w$ by the sum of all $w$'s.

_(Note: Because of how these specific triangles are structured, the sum of $w_1 + w_2 + w_3 + w_4$ happens to equal $1.0$. Therefore, dividing by $1.0$ means the normalized weights ($\bar{w}$) are identical to the raw weights in this specific instance)_.

- $\bar{w}_1 = 0.720792$
    
- $\bar{w}_2 = 0.180198$
    
- $\bar{w}_3 = 0.079207$
    
- $\bar{w}_4 = 0.019802$
    

#### **Layer 5 (Rule Outputs)**

First, we use the rule coefficients ($a_i, b_i, c_i$) from the provided table to calculate the mathematical output ($y$) for each rule:

- $y_1 = (0.2 \times 1.1) + (0.3 \times 6.0) + 0.10 = \mathbf{2.12}$
    
- $y_2 = (0.2 \times 1.1) + (0.4 \times 6.0) + 0.11 = \mathbf{2.73}$
    
- $y_3 = (0.3 \times 1.1) + (0.3 \times 6.0) + 0.13 = \mathbf{2.26}$
    
- $y_4 = (0.3 \times 1.1) + (0.4 \times 6.0) + 0.14 = \mathbf{2.87}$
    

Next, Layer 5 multiplies these rule outputs by their respective normalized firing strengths:

- $5_{O1} = \bar{w}_1 \times y_1 = 0.720792 \times 2.12 = \mathbf{1.528079}$
    
- $5_{O2} = \bar{w}_2 \times y_2 = 0.180198 \times 2.73 = \mathbf{0.491941}$
    
- $5_{O3} = \bar{w}_3 \times y_3 = 0.079207 \times 2.26 = \mathbf{0.179008}$
    
- $5_{O4} = \bar{w}_4 \times y_4 = 0.019802 \times 2.87 = \mathbf{0.056832}$
    

#### **Layer 6 (Final Calculation)**

The overall predicted output is simply the sum of the Layer 5 values:

$$6_{O1} = 1.528079 + 0.491941 + 0.179008 + 0.056832 = \mathbf{2.255860}$$

Finally, to find the deviation in prediction, we subtract our predicted output from the known target output:

- Target Output = $2.3$
    
- Predicted Output = $2.255860$
    
- **Deviation:** $2.3 - 2.255860 = \mathbf{0.044140}$