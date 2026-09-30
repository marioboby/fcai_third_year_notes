---
{"dg-publish":true,"permalink":"/3rd-year/reasoning/finals-sol/final-25-sol/","dg-note-properties":{}}
---


# Q1
## **A) Use the Resolution Algorithm to prove that "Alice is accepted"**

**1. Define Predicates and Constants:**

- $H(x)$: $x$ has high grades
    
- $R(x)$: $x$ has strong recommendations
    
- $A(x)$: $x$ is accepted
    
- $W(x)$: $x$ works hard
    
- $a$: Alice
    

**2. Translate the story into First-Order Logic:**

1. Everyone who has high grades or has strong recommendations is accepted:
    
    $$\forall x \, ((H(x) \lor R(x)) \rightarrow A(x))$$
    
2. Everyone who works hard has high grades:
    
    $$\forall x \, (W(x) \rightarrow H(x))$$
    
3. Alice did not work hard but she has strong recommendations:
    
    $$\neg W(a) \land R(a)$$
    

- **Goal to prove:** $A(a)$
    

**3. Convert to Conjunctive Normal Form (CNF):**

- **From Premise 1:**
    
    - $\neg (H(x) \lor R(x)) \lor A(x)$
    
    - Apply De Morgan's Law: $(\neg H(x) \land \neg R(x)) \lor A(x)$
    
    - Distribute: $(\neg H(x) \lor A(x)) \land (\neg R(x) \lor A(x))$
    
    - **C1:** $\neg H(x) \lor A(x)$
        
    - **C2:** $\neg R(y) \lor A(y)$
        
- **From Premise 2:**
    
    - **C3:** $\neg W(z) \lor H(z)$
        
- **From Premise 3:**
    
    - **C4:** $\neg W(a)$
        
    - **C5:** $R(a)$
        
- **Negated Goal:**
    
    - **C6:** $\neg A(a)$
        

**4. Resolution Steps:**

We only need a few steps to reach a contradiction because her strong recommendations alone are sufficient for acceptance.

- **Step 1:** Resolve **C6** ($\neg A(a)$) with **C2** ($\neg R(y) \lor A(y)$) by substituting $y = a$:
    
    - _Result:_ **C7:** $\neg R(a)$
        
- **Step 2:** Resolve **C7** ($\neg R(a)$) with **C5** ($R(a)$):
    
    - _Result:_ **$\square$ (Contradiction)**
        

**Conclusion:** By deriving the empty clause, we prove the original goal is true. **Alice is accepted.**

---

### **B) Translate the following statements into predicate logic**

**Constants:** $s$ (Sarah), $t$ (Tom), $m$ (Mike)

**Predicates:** $Animal(x)$, $Loves(x,y)$, $Dog(x)$, $Cat(x)$, $Barks(x)$, $Dangerous(x)$, $Owns(x,y)$, $Friendly(x)$

**a. Sarah loves every animal.**

$$\forall x \, (Animal(x) \rightarrow Loves(s, x))$$

**b. Dogs and cats are animals.**

$$\forall x \, ((Dog(x) \lor Cat(x)) \rightarrow Animal(x))$$

**c. Everything that barks and is not dangerous is a dog.**

$$\forall x \, ((Barks(x) \land \neg Dangerous(x)) \rightarrow Dog(x))$$

**d. Tom owns a dog and it's friendly.**

$$\exists x \, (Dog(x) \land Owns(t, x) \land Friendly(x))$$

**e. Mike owns every pet that Tom owns.**

$$\forall x \, (Owns(t, x) \rightarrow Owns(m, x))$$

_(Note: Assuming we don't need a Predicate for Pet)._

---

### **C) Prove or disprove the goal `friend1(X) & friend2(X)` by using backward chaining (Look Lecture 4).**

**Initial Facts Database:**

- $h1 = True$
    
- $h2 = True$
    

**Goal:** Prove `friend1(X) , friend2(X)`

**Evaluating Sub-goal 1: Prove `friend1(X)`**

- **Rule 1** says IF $h$ is true THEN $Friend1(X)$ is true.
    
    - _Check Database:_ $h$ is not in the database. Rule 1 fails.
        
- **Rule 2** says IF $a$ is true AND $b$ is true THEN $Friend1(X)$ is true.
    
    - _Attempt to prove `a`:_
        
        - **Rule 4** says IF $not(d)$ is true THEN $a$ is true. (This requires $d$ to be false).
            
        - _Attempt to prove `d`:_
            
            - **Rule 8** says IF $h1$ is true AND $h2$ is true THEN $d$ is true.
                
            - _Check Database:_ $h1$ is true, and $h2$ is true. Therefore, **$d$ is true**.
                
        - Since $d$ is true, $not(d)$ is false. Therefore, **Rule 4 fails**, meaning **$a$ is false**.
            
    - Because $a$ is false, the condition for Rule 2 ($a \land b$) fails. Rule 2 fails.
        
- **Conclusion for Sub-goal 1:** $Friend1(X)$ cannot be proven and is evaluated as **False**.
    
---

**Evaluating Sub-goal 2: Prove `friend2(X)`**

- Matching friend2(X):

- R3: IF c(X) is true THEN Friend2(X) is true

	- Matching c(X) :

	- R7: IF not(e) is true THEN c(2) is true

		- Matching e 

		- R9: IF h2 is true  AND h3 is true THEN e is true

		- h2 is true and h3 is false then e is false then R9 is false

	- Return to R7:

	- R7: IF not(e) is true THEN c(2) is true

	- e is false, then not(e) become true
	
	- So, c(2) is true then R7 is true 

- Return to R3:

- R3: IF c(X) is true THEN Friend2(X) is true

 - c(X) is true for X=2 

then Friend2(X) is true where X=2

---

# Q2

## A) **Goal:** Determine the final Certainty Factor for both "tomorrow is rain" and "tomorrow is dry".

**1. Identify Initial User Input Facts:**

- $CF(\text{today is rain}) = 1$
    
- $CF(\text{rainfall is low}) = 0.8$
    
- $CF(\text{temperature is cold}) = 0.9$
    
- _(Note: Facts like "today is dry", "temperature is warm", and "sky is overcast" are not provided, so their CF is implicitly $0$, meaning rules 2, 5, and 6 will not fire)._
    

**2. Evaluate the Relevant Rules:**

For rules with an `AND` premise, we take the minimum CF of the given facts. Then, we multiply the premise's CF by the Rule's CF to get the conclusion's CF.

- **Rule 1: If today is rain $\rightarrow$ tomorrow is rain {CF = 0.5}**
    
    - Premise CF = $CF(\text{today is rain}) = 1$
        
    - Conclusion CF = $1 \times 0.5 = 0.5$
        
    - **Result 1:** $CF_1(\text{tomorrow is rain}) = 0.5$
        
- **Rule 3: If today is rain AND rainfall is low $\rightarrow$ tomorrow is dry {CF = 0.6}**
    
    - Premise CF = $\min(CF(\text{today is rain}), CF(\text{rainfall is low}))$
        
    - Premise CF = $\min(1, 0.8) = 0.8$
        
    - Conclusion CF = $0.8 \times 0.6 = 0.48$
        
    - **Result 2:** $CF_1(\text{tomorrow is dry}) = 0.48$
        
- **Rule 4: If today is rain AND rainfall is low AND temperature is cold $\rightarrow$ tomorrow is dry {CF = 0.7}**
    
    - Premise CF = $\min(CF(\text{today is rain}), CF(\text{rainfall is low}), CF(\text{temperature is cold}))$
        
    - Premise CF = $\min(1, 0.8, 0.9) = 0.8$
        
    - Conclusion CF = $0.8 \times 0.7 = 0.56$
        
    - **Result 3:** $CF_2(\text{tomorrow is dry}) = 0.56$
        

**3. Combine Certainty Factors for the Same Conclusions:**

When multiple rules lead to the same conclusion and both CFs are positive, we combine them using the formula: $CF_{combine}(CF_1, CF_2) = CF_1 + CF_2 \times (1 - CF_1)$.

- **For "tomorrow is rain":**
    
    - Only Rule 1 fired for this conclusion.
        
    - **Final $CF(\text{tomorrow is rain}) = 0.5$**
        
- **For "tomorrow is dry":**
    
    - Rules 3 and 4 both concluded "tomorrow is dry".
        
    - $CF_{combine}(0.48, 0.56) = 0.48 + 0.56 \times (1 - 0.48)$
        
    - $CF_{combine} = 0.48 + 0.56 \times (0.52)$
        
    - $CF_{combine} = 0.48 + 0.2912 = 0.7712$
        
    - **Final $CF(\text{tomorrow is dry}) = 0.7712$**
        

### **Final Answer:**

- **CF for tomorrow is rain:** $0.5$
    
- **CF for tomorrow is dry:** $0.7712$
    
    _(Since $0.7712 > 0.5$, the expert system would strongly lean toward concluding that tomorrow is dry)._

---

## B) 

![Pasted image 20260520221410.png](/img/user/imgs/Pasted%20image%2020260520221410.png)


![Pasted image 20260520222118.png](/img/user/imgs/Pasted%20image%2020260520222118.png)

![Pasted image 20260520224154.png](/img/user/imgs/Pasted%20image%2020260520224154.png)
### **Rule 1: Output is Low ($LW$), Clipped at 0.25**

Based on your handwritten sketch, we look at the falling slope from **x = 2** to **x = 7** (Total Base = 5).

Using the similar triangles method: the tip of the triangle under the clip is $5 \times 0.25 = 1.25$.

Therefore, the clipped shape is split into:

1. A Rectangle from $x = 2$ to $5.75$
    
2. A Triangle from $x = 5.75$ to $7$
    

![Pasted image 20260520224013.png](/img/user/imgs/Pasted%20image%2020260520224013.png)

**Shape 1: Rectangle**

- Base = $3.75$ _(which is $5 - 1.25$)_
    
- Height = $0.25$
    
- Area = $3.75 \times 0.25 = \mathbf{0.9375}$
    
- Center = $(3.75 / 2) = \mathbf{1.875}$
    

**Shape 2: Triangle (The Tail)**

- Base = $1.25$
    
- Height = $0.25$
    
- Area = $\frac{1}{2} \times 1.25 \times 0.25 = \mathbf{0.15625}$
    
- Center = $(1.25 / 3) \approx \mathbf{0.4167}$
    

**Rule 1 Totals:**

- **Total Area 1:** $0.9375 + 0.15625 = \mathbf{1.09375}$
    
- **Total Moment 1 ($\frac{\sum Area \times Center}{\sum Area} + StartingPoint$):** $$COA_{LW} = \frac{(0.9375 \times 1.875) + (0.15625 \times 0.4167)}{1.09375} + 2 = \frac{1.8229}{1.09375} + 2 = 1.66667 + 2 = \mathbf{3.66667}$$
    

### **Rule 2: Output is High ($H$), Clipped at 0.5**

Because the High ($H$) shape is symmetrical (it forms an even trapezoid after being clipped), we can bypass the individual shape decomposition and apply the special rule directly to the entire base.

![Pasted image 20260520230444.png](/img/user/imgs/Pasted%20image%2020260520230444.png)

- **Base:** $12 - 2 = \mathbf{10}$
    
- **Starting Point:** $\mathbf{2}$
    
- **Center H:** $(\frac{1}{2} \times Base) + Starting Point = (\frac{1}{2} \times 10) + 2 = 5 + 2 = \mathbf{7.0}$
    
- **Total Area H:** $\mathbf{3.75}$ _(Using trapezoid area: $\frac{10 + 5}{2} \times 0.5$)_
    

### **3. Complete COA**

Now we apply the final aggregation formula using the areas and centers of both shapes:

- **LW:** Area = $1.09375$, Center = $4.2023$
    
- **H:** Area = $3.75$, Center = $7.0$
    

$$COA = \frac{(Area_{LW} \times Center_{LW}) + (Area_H \times Center_H)}{Area_{LW} + Area_H}$$

$$COA = \frac{(1.09375 \times 3.66667) + (3.75 \times 7.0)}{1.09375 + 3.75}$$

$$COA = \frac{4.0104 + 26.25}{4.84375}$$

$$COA = \frac{30.2604}{4.84375} \approx \mathbf{6.247}$$
