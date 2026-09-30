---
{"dg-publish":true,"permalink":"/3rd-year/reasoning/mids-sol/mid-2025-sol/","dg-note-properties":{}}
---


# Q1

## **A) Consider the following production system:** 

- **Rule 1:** IF shape is long AND color is yellow THEN fruit is banana
    
- **Rule 2:** IF shape is round AND color is red AND size is medium THEN then fruit is apple
    
- **Rule 3:** IF shape is round AND color is red AND size is small THEN then fruit is cherry
    
- **Rule 4:** IF skin smell THEN perfumed
    
- **Rule 5:** IF fruit is lemon OR fruit is orange OR fruit is pomelo OR fruit is grapefruit THEN citrus fruit
    
- **Rule 6:** IF size is medium AND color is yellow AND perfumed THEN then fruit is lemon
    
- **Rule 7:** IF size is medium AND color is green THEN fruit is kiwi
    
- **Rule 8:** IF size is big AND perfumed AND color is orange AND citrus fruit THEN fruit is grapefruit
    
- **Rule 9:** IF perfumed AND color is orange AND size is medium THEN fruit is orange
    
- **Rule 10:** IF perfumed AND color is red AND size is small AND no seeds THEN fruit is strawberry
    
- **Rule 11:** IF diameter < 2 cm THEN size is small
    
- **Rule 12:** IF diameter > 10 cm THEN size is big
    
- **Rule 13:** IF diameter > 2 cm AND diameter < 10 cm THEN size is medium
    

The user inputs the facts: The fruit has no seed, a 7 cm diameter, smelling skin, orange color. Establish by forward chaining and backward chaining that the fruit is a citrus fruit.

### Sol: 

**Initial Facts:**

- no seed
    
- diameter = 7 cm
    
- skin smell
    
- color is orange
    

**Goal:**

- Prove: **citrus fruit**
    

---

### 1. Forward Chaining 

In forward chaining, we start with the known facts and continuously apply the rules until we reach our goal.

**Iteration 1:**

- Check **Rule 4**: `IF skin smell THEN perfumed`
    
    - Since we have the fact "skin smell", this rule fires.
        
    - _New Fact derived:_ **perfumed**
        
- Check **Rule 13**: `IF diameter > 2 cm AND diameter < 10 cm THEN size is medium`
    
    - Since our diameter is 7 cm (which is > 2 and < 10), this rule fires.
        
    - _New Fact derived:_ **size is medium**
        
- _Current Facts:_ `[no seed, diameter = 7 cm, skin smell, color is orange, perfumed, size is medium]`.
    

**Iteration 2:**

- Check **Rule 9**: `IF perfumed AND color is orange AND size is medium THEN fruit is orange`
    
    - We now have all three facts in our working memory ("perfumed", "color is orange", and "size is medium"). The rule fires.
        
    - _New Fact derived:_ **fruit is orange**
        
- _Current Facts:_ `[no seed, diameter = 7 cm, skin smell, color is orange, perfumed, size is medium, fruit is orange]`.
    

**Iteration 3:**

- Check **Rule 5**: `IF fruit is lemon OR fruit is orange OR fruit is pomelo OR fruit is grapefruit THEN citrus fruit`
    
    - Since we have derived "fruit is orange", the OR condition is satisfied. The rule fires.
        
    - _New Fact derived:_ **citrus fruit**
        

**Conclusion:** The goal **citrus fruit** has been successfully established.

---

### 2. Backward Chaining 

In backward chaining, we start with the goal and work backward, setting sub-goals to see if they can be supported by the initial facts.

**Main Goal:** Prove **citrus fruit**

- To prove "citrus fruit", we scan the rules for a conclusion that matches. We find **Rule 5**.
    
- **Rule 5** requires proving at least one of the following sub-goals (OR condition): `fruit is lemon`, `fruit is orange`, `fruit is pomelo`, or `fruit is grapefruit`.
    

**Attempt Sub-goal 1:** Prove **fruit is lemon**

- We look at **Rule 6**, which concludes "fruit is lemon".
    
- It requires: `size is medium AND color is yellow AND perfumed`.
    
- _Check Facts:_ Our known fact is "color is orange", not yellow.
    
- _Result:_ Sub-goal fails. Move to the next OR condition.
    

**Attempt Sub-goal 2:** Prove **fruit is orange**

- We look at **Rule 9**, which concludes "fruit is orange".
    
- It requires three conditions: `perfumed AND color is orange AND size is medium`. We evaluate them one by one:
    
    1. **Check `color is orange`:** This is given as an initial fact. **(True)**
        
    2. **Check `perfumed`:** This is not an initial fact. Set as a new sub-goal.
        
        - Find a rule concluding "perfumed": **Rule 4**.
            
        - Rule 4 requires `skin smell`.
            
        - "skin smell" is an initial fact. **(True)** -> Therefore, `perfumed` is **True**.
            
    3. **Check `size is medium`:** This is not an initial fact. Set as a new sub-goal.
        
        - Find a rule concluding "size is medium": **Rule 13**.
            
        - Rule 13 requires `diameter > 2 cm AND diameter < 10 cm`.
            
        - Our initial fact is "diameter = 7 cm", which satisfies the math condition. **(True)** -> Therefore, `size is medium` is **True**.
            

**Conclusion:** Because all three prerequisites for Rule 9 (`perfumed`, `color is orange`, and `size is medium`) have been evaluated as True, the sub-goal **fruit is orange** is proven True. Because "fruit is orange" is True, it satisfies the OR condition in Rule 5, successfully proving the ultimate goal: **citrus fruit**.

---

## **B) Use the Resolution Algorithm to prove that "Alice is successful" based on the following story:**

1. Everyone who works hard and gets a promotion is successful.
    
2. Everyone who is dedicated or talented can work hard.
    
3. Alice is talented but not dedicated.
    
4. Everyone who is talented gets a promotion.
    
5. Prove that Alice is successful.

### Sol

To prove that "Alice is successful" using the Resolution Algorithm, we must first translate the story into First-Order Logic, convert those statements into Conjunctive Normal Form (CNF) clauses, negate our goal, and then resolve the clauses to find a contradiction (an empty clause).

### 1. Define the Predicates

- $W(x)$: $x$ works hard
    
- $P(x)$: $x$ gets a promotion
    
- $S(x)$: $x$ is successful
    
- $D(x)$: $x$ is dedicated
    
- $T(x)$: $x$ is talented
    
- $a$: Alice
    

### 2. Translate to First-Order Logic

1. Everyone who works hard and gets a promotion is successful:
    
    $\forall x \, ((W(x) \land P(x)) \rightarrow S(x))$
    
2. Everyone who is dedicated or talented can work hard:
    
    $\forall x \, ((D(x) \lor T(x)) \rightarrow W(x))$
    
3. Alice is talented but not dedicated:
    
    $T(a) \land \neg D(a)$
    
4. Everyone who is talented gets a promotion:
    
    $\forall x \, (T(x) \rightarrow P(x))$
    

**Goal to prove:** $S(a)$ (Alice is successful)

### 3. Convert to Conjunctive Normal Form (CNF)

We remove implications ($\rightarrow$) using the rule $A \rightarrow B \equiv \neg A \lor B$, and distribute to form standalone clauses.

- **From Premise 1:**
    
    - $\neg (W(x) \land P(x)) \lor S(x)$
    
    - **C1:** $\neg W(x) \lor \neg P(x) \lor S(x)$
    
- **From Premise 2:**
    
    - $\neg (D(x) \lor T(x)) \lor W(x)$
    
    - Apply De Morgan's Law: $(\neg D(x) \land \neg T(x)) \lor W(x)$
    
    - Distribute: $(\neg D(x) \lor W(x)) \land (\neg T(x) \lor W(x))$
    
    - **C2:** $\neg D(x) \lor W(x)$
    
    - **C3:** $\neg T(x) \lor W(x)$
    
- **From Premise 3:**
    
    - **C4:** $T(a)$
    
    - **C5:** $\neg D(a)$
    
- **From Premise 4:**
    
    - **C6:** $\neg T(x) \lor P(x)$
    
- **Negated Goal:**
    
    - To use resolution by refutation, we negate what we want to prove.
    
    - **C7:** $\neg S(a)$
    

### 4. Resolution Steps

Now we perform resolution on our set of clauses {C1, C2, C3, C4, C5, C6, C7} to derive a contradiction (an empty clause, often denoted as $\square$ or $\bot$).

- **Step 1:** Resolve **C6** ($\neg T(x) \lor P(x)$) with **C4** ($T(a)$)
    
    - Substitute $x = a$ into C6: $\neg T(a) \lor P(a)$
    
    - $\neg T(a)$ and $T(a)$ cancel out.
    
    - **C8: $P(a)$** _(Meaning: Alice gets a promotion)_
    
- **Step 2:** Resolve **C3** ($\neg T(x) \lor W(x)$) with **C4** ($T(a)$)
    
    - Substitute $x = a$ into C3: $\neg T(a) \lor W(a)$
    
    - $\neg T(a)$ and $T(a)$ cancel out.
    
    - **C9: $W(a)$** _(Meaning: Alice works hard)_
    
- **Step 3:** Resolve **C1** ($\neg W(x) \lor \neg P(x) \lor S(x)$) with **C7** ($\neg S(a)$)
    
    - Substitute $x = a$ into C1: $\neg W(a) \lor \neg P(a) \lor S(a)$
    
    - $S(a)$ and $\neg S(a)$ cancel out.
    
    - **C10: $\neg W(a) \lor \neg P(a)$**
    
- **Step 4:** Resolve **C10** ($\neg W(a) \lor \neg P(a)$) with **C8** ($P(a)$)
    
    - $\neg P(a)$ and $P(a)$ cancel out.
    
    - **C11: $\neg W(a)$**
    
- **Step 5:** Resolve **C11** ($\neg W(a)$) with **C9** ($W(a)$)
    
    - $\neg W(a)$ and $W(a)$ cancel out, leaving nothing.
    
    - **C12: $\perp$ (Contradiction)**
    

### Conclusion

By negating the goal ($\neg S(a)$) and using the resolution algorithm, we reached a logical contradiction (the empty clause). Therefore, the original premise must be true. **Alice is successful.**

---

## **C) Translate the following statements into predicate logic:**

a. Every student who studies passes the exam.

b. Mathematics and Physics are subjects.

c. Anything that is taught by a teacher is a subject.

d. Alice studies Mathematics and has passed the exam.

e. Bob studies everything that Alice studies.

### Sol

**Constants:**

- $a$: Alice
    
- $b$: Bob
    
- $m$: Mathematics
    
- $p$: Physics
    
- $e$: the exam
    

**Predicates:**

- $Student(x)$: $x$ is a student
    
- $Studies(x)$: $x$ studies
    
- $Studies(x, y)$: $x$ studies subject $y$
    
- $Passes(x, y)$: $x$ passes $y$
    
- $Subject(x)$: $x$ is a subject
    
- $Teacher(x)$: $x$ is a teacher
    
- $Teaches(x, y)$: $x$ teaches $y$
    

---

**a. Every student who studies passes the exam.**

$$\forall x \, ((Student(x) \land Studies(x)) \rightarrow Passes(x, e))$$

**b. Mathematics and Physics are subjects.**

$$Subject(m) \land Subject(p)$$

**c. Anything that is taught by a teacher is a subject.**

$$\forall x \, (\exists y \, (Teacher(y) \land Teaches(y, x)) \rightarrow Subject(x))$$

_(Alternatively: $\forall x \, \forall y \, ((Teacher(y) \land Teaches(y, x)) \rightarrow Subject(x))$)_

**d. Alice studies Mathematics and has passed the exam.**

$$Studies(a, m) \land Passes(a, e)$$

**e. Bob studies everything that Alice studies.**

$$\forall x \, (Studies(a, x) \rightarrow Studies(b, x))$$