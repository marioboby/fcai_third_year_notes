---
{"dg-publish":true,"permalink":"/3rd-year/reasoning/after-mid/lecture-8-uncertainty/","dg-note-properties":{}}
---



The final lecture introduces **Uncertainty Management in Rule-based Expert Systems**, moving away from exact mathematics and into how systems handle ambiguity.

In classical logic, knowledge is assumed to be perfect: a statement is either exactly TRUE or exactly FALSE. However, most real-world problems do not provide clear-cut facts. Uncertainty is defined as the lack of exact knowledge needed to reach a perfectly reliable conclusion.

### **Sources of Uncertain Knowledge**

Expert systems must navigate several sources of ambiguity:

- **Weak Implications:** It is often difficult to establish concrete correlations between the "IF" condition and the "THEN" action of a rule.
    
- **Imprecise Language:** Human language is inherently ambiguous, using vague terms like "often," "sometimes," or "frequently".
    
- **Unknown Data:** When data is missing, the system must accept an "unknown" value and proceed with approximate reasoning.
    
- **Combining Expert Views:** Large systems rely on multiple experts who frequently have contradictory opinions and conflicting rules.
    

---

### **Certainty Factors (CF) Theory**

To handle this uncertainty, the lecture introduces **Certainty Factors**, an approach originally developed for the MYCIN expert system.

A Certainty Factor (cf) measures an expert's degree of belief in a hypothesis.

- It operates on a scale from **-1.0** (Definitely False / maximum disbelief) to **+1.0** (Definitely True / maximum belief).
    
- A value of **0** generally represents an "Unknown" state.
    

When a rule fires, the certainty factor assigned to its conclusion is propagated through the reasoning chain. Here is how the mathematics work for different rule structures:

#### **1. Single Premise Rules**

If a rule relies on a single piece of evidence, you simply multiply the certainty of the evidence by the certainty assigned to the rule.

$$cf(H,E) = cf(E) \times cf$$

#### **2. Conjunctive (AND) Rules**

When a rule requires multiple conditions to be true (e.g., IF A is true AND B is true), the system takes the **minimum** certainty factor among all the conditions, and multiplies it by the rule's certainty.

$$cf(H, E_1 \cap E_2) = \min[cf(E_1), cf(E_2)] \times cf$$

#### **3. Disjunctive (OR) Rules**

When a rule requires only one of several conditions to be true (e.g., IF A is true OR B is true), the system takes the **maximum** certainty factor among the conditions, and multiplies it by the rule's certainty.

$$cf(H, E_1 \cup E_2) = \max[cf(E_1), cf(E_2)] \times cf$$

---

### **Combining Certainty Factors**

The most complex part of this theory occurs when **two different rules lead to the exact same conclusion**. Common sense dictates that if two independent pieces of evidence support the same hypothesis, our confidence in that hypothesis should increase.

To calculate this, the system merges the individual certainty factors ($cf_1$ and $cf_2$) using a specific piecewise equation depending on their signs:

- **If both are positive (Incremental Belief):**
    
    $$cf = cf_1 + cf_2 \times (1 - cf_1)$$
    
- **If both are negative (Incremental Disbelief):**
    
    $$cf = cf_1 + cf_2 \times (1 + cf_1)$$
    
- **If they have opposite signs (Conflicting Rules):**
    
    $$cf = \frac{cf_1 + cf_2}{1 - \min[|cf_1|, |cf_2|]}$$
    

This is the step-by-step breakdown of how an expert system resolves the medical diagnosis tree in **Example 2**.

![Pasted image 20260511211424.png](/img/user/imgs/Pasted%20image%2020260511211424.png)

The goal of this network is to determine the final Certainty Factor (CF) for the hypothesis **"HAVING COLD"**. The system does this by evaluating 7 interconnected rules based on patient symptoms.

_(Note: Because the exact patient inputs are partially cut off in the document snippet, I will use a realistic set of assumed patient symptoms to demonstrate the exact mathematical mechanics the system uses to reach a conclusion)._

**Assumed Patient Evidence (Inputs):**

- **$cf(\text{fever} < 37.5)$** = $0.3$
    
- **$cf(\text{fever} > 37.5)$** = $0.8$
    
- **$cf(\text{cough} > 24\text{h})$** = $0.8$
    
- **$cf(\text{cough} > 48\text{h})$** = $0.6$
    
- **$cf(\text{sneezing})$** = $0.6$
    
- **$cf(\text{headache})$** = $0.4$
    
- **$cf(\text{nasal congestion})$** = $0.5$
    

---

### **Phase 1: Resolving the Intermediate Hypotheses**

We now have two sub-conclusions to figure out: "soar troth" and "cold symptoms."

**1. Calculate "soar troth" (Rules 3 & 4)**

- **Rule 3:** IF cough > 24h THEN soar troth {cf = 0.5}
    
    $$cf(R_3) = 0.8 \times 0.5 = \mathbf{0.40}$$
    
- **Rule 4:** IF cough > 48h THEN soar troth {cf = 1.0}
    
    $$cf(R_4) = 0.6 \times 1.0 = \mathbf{0.60}$$
    
- **Combine (Incremental Belief):** $0.40 + 0.60 \times (1 - 0.40) = \mathbf{0.76}$
    
    _(Intermediate $CF$ for "soar troth" = $0.76$)_
    

**2. Calculate "cold symptoms" (Rules 1 & 2)**

- **Rule 1:** IF fever < 37.5 THEN cold symptoms {cf = 0.5}
    
    $$cf(R_1) = 0.3 \times 0.5 = \mathbf{0.15}$$
    
- **Rule 2:** IF fever > 37.5 THEN cold symptoms {cf = 0.9}
    
    $$cf(R_2) = 0.8 \times 0.9 = \mathbf{0.72}$$
    
- **Combine (Incremental Belief):** $0.15 + 0.72 \times (1 - 0.15) = 0.15 + 0.612 = \mathbf{0.762}$
    
    _(Intermediate $CF$ for "cold symptoms" = $0.762$)_
    

---

### **Phase 2: Evaluating the Final Hypothesis Rules**

Now we evaluate the three rules that point to **"HAVING COLD"**, using our newly calculated intermediate CFs where necessary.

- **Rule 5:** IF cold symptoms AND sneezing THEN having cold {cf = -0.2}
    
    - $\text{Premise} = \min(\text{cold symptoms: } 0.762,\ \text{sneezing: } 0.6) = 0.6$
        
    - $$cf(R_5) = 0.6 \times (-0.2) = \mathbf{-0.12}$$
        
        _(Notice this rule creates disbelief because the CF is negative)._
        
- **Rule 6:** IF soar troth THEN having cold {cf = 0.5}
    
    - $\text{Premise} = \text{soar troth: } 0.76$
        
    - $$cf(R_6) = 0.76 \times 0.5 = \mathbf{0.38}$$
        
- **Rule 7:** IF headache AND nasal congestion THEN having cold {cf = 0.7}
    
    - $\text{Premise} = \min(\text{headache: } 0.4,\ \text{nasal congestion: } 0.5) = 0.4$
        
    - $$cf(R_7) = 0.4 \times 0.7 = \mathbf{0.28}$$
        

---

### **Phase 3: The Final Combination**

We have three independent rules firing for the final diagnosis:

- $R_5 = -0.12$
    
- $R_6 = 0.38$
    
- $R_7 = 0.28$
    

**Merge 1: Combine the positive evidence ($R_6$ and $R_7$)**

Using the Incremental Belief formula:

$$cf_{new1} = 0.38 + 0.28 \times (1 - 0.38)$$

$$cf_{new1} = 0.38 + (0.28 \times 0.62) = \mathbf{0.5536}$$

**Merge 2: Combine the positive result with the negative evidence ($R_5$)**

Because we are combining a positive ($0.5536$) and a negative ($-0.12$), we must use the **Conflicting Rules** formula: $cf = \frac{cf_1 + cf_2}{1 - \min[|cf_1|, |cf_2|]}$

$$cf_{final} = \frac{0.5536 + (-0.12)}{1 - \min(|0.5536|, |-0.12|)}$$

$$cf_{final} = \frac{0.4336}{1 - 0.12}$$

$$cf_{final} = \frac{0.4336}{0.88} = \mathbf{0.4927}$$

### **Conclusion**

With the corrected tree structure, the system uses the conflicting evidence (the presence of sneezing combined with cold symptoms slightly reduces the likelihood of a standard cold in this specific rule base) to temper its final diagnosis. The final Certainty Factor for **"HAVING COLD"** is **$0.49$** (or $49\%$ confidence).