---
{"dg-publish":true,"permalink":"/3rd-year/reasoning/mids-sol/mid-2022-sol/","dg-note-properties":{}}
---


# **Q1**

## **A) Translate the following statements into predicate logic**

1. Mary loves everyone
    
2. Every student except George smiles.
    
3. Every student who walks talks
    
4. Every boy who loves Mary hates every other boy who Mary loves
    
5. No one who runs walks
    
6. There is a person who writes computer class

### Sol

- $Loves(x, y)$: $x$ loves $y$
    
- $Student(x)$: $x$ is a student
    
- $Smiles(x)$: $x$ smiles
    
- $Walks(x)$: $x$ walks
    
- $Talks(x)$: $x$ talks
    
- $Boy(x)$: $x$ is a boy
    
- $Hates(x, y)$: $x$ hates $y$
    
- $Runs(x)$: $x$ runs
    
- $Person(x)$: $x$ is a person
    
- $Writes(x, y)$: $x$ writes $y$
    

**1. Mary loves everyone**

$$\forall x \, Loves(Mary, x)$$

**2. Every student except George smiles.**

$$\forall x \, ((Student(x) \land x \neq George) \rightarrow Smiles(x)) \land \neg Smiles(George)$$

**3. Every student who walks talks**

$$\forall x \, ((Student(x) \land Walks(x)) \rightarrow Talks(x))$$

**4. Every boy who loves Mary hates every other boy who Mary loves**

$$\forall x \, \forall y \, ((Boy(x) \land Loves(x, Mary) \land Boy(y) \land Loves(Mary, y) \land x \neq y) \rightarrow Hates(x, y))$$

**5. No one who runs walks**

$$\forall x \, (Runs(x) \rightarrow \neg Walks(x))$$

_(Alternatively: $\neg \exists x \, (Runs(x) \land Walks(x))$)_

**6. There is a person who writes computer class**

$$\exists x \, (Person(x) \land Writes(x, ComputerClass))$$

---

## **B) Using the resolution algorithm (Forward and backward) prove that: John likes peanuts.** 

1. John likes all kind of food.
    
2. Apple and vegetable are food
    
3. Anything anyone eats and not killed is food.
    
4. Anil eats peanuts and still alive
    
5. Harry eats everything that Anil eats.

### Sol

**1. Define Predicates and Translate to First-Order Logic:**

- $Food(x)$: $x$ is food
    
- $Likes(x, y)$: $x$ likes $y$
    
- $Eats(x, y)$: $x$ eats $y$
    
- $Killed(x)$: $x$ is killed (Note: "still alive" will be represented as $\neg Killed(x)$ to match the wording of premise 3).
    

_Premises:_

1. John likes all kind of food: $\forall x \, (Food(x) \rightarrow Likes(John, x))$
    
2. Apple and vegetable are food: $Food(Apple) \land Food(Vegetable)$
    
3. Anything anyone eats and not killed is food: $\forall x \, \forall y \, ((Eats(x, y) \land \neg Killed(x)) \rightarrow Food(y))$
    
4. Anil eats peanuts and still alive: $Eats(Anil, Peanuts) \land \neg Killed(Anil)$
    
5. Harry eats everything that Anil eats: $\forall x \, (Eats(Anil, x) \rightarrow Eats(Harry, x))$
    

_Goal:_ $Likes(John, Peanuts)$

**2. Convert to Conjunctive Normal Form (CNF) Clauses:**

- **C1:** $\neg Food(x) \lor Likes(John, x)$
    
- **C2a:** $Food(Apple)$
    
- **C2b:** $Food(Vegetable)$
    
- **C3:** $\neg Eats(x, y) \lor Killed(x) \lor Food(y)$
    
- **C4a:** $Eats(Anil, Peanuts)$
    
- **C4b:** $\neg Killed(Anil)$
    
- **C5:** $\neg Eats(Anil, x) \lor Eats(Harry, x)$
    

---

#### **Method 1: Forward Resolution (Fact-Driven)**

In forward resolution, we start with our known facts and continuously resolve them with implications to derive new facts until we reach the goal: $Likes(John, Peanuts)$.

1. Resolve **C3** ($\neg Eats(x, y) \lor Killed(x) \lor Food(y)$) with fact **C4b** ($\neg Killed(Anil)$) by substituting $x = Anil$:
    
    - _Result:_ $\neg Eats(Anil, y) \lor Food(y)$
        
2. Resolve the _Result_ ($\neg Eats(Anil, y) \lor Food(y)$) with fact **C4a** ($Eats(Anil, Peanuts)$) by substituting $y = Peanuts$:
    
    - _Result:_ $Food(Peanuts)$ _(We have now proven peanuts are food)_
        
3. Resolve **C1** ($\neg Food(x) \lor Likes(John, x)$) with the new fact $Food(Peanuts)$ by substituting $x = Peanuts$:
    
    - _Result:_ **$Likes(John, Peanuts)$**
        
        _Goal Derived!_
        

في اللاب مشروح ان في ال forward برضو ضيف ال negated goal to the facts and start from the facts فا الحل حرفيا هتضيف زيادة بس بعد ما توصل لستيب 3 فوق انك ناخد resolution with negated goal و توصل لل contradiction 

---

#### **Method 2: Backward Resolution (Goal-Driven / Refutation)**

In backward resolution, we negate the goal and resolve it against the clauses to find a logical contradiction (the empty clause, $\square$).

- **Negated Goal (C6):** $\neg Likes(John, Peanuts)$
    

1. Resolve **C6** ($\neg Likes(John, Peanuts)$) with **C1** ($\neg Food(x) \lor Likes(John, x)$) using $x = Peanuts$:
    
    - _Result:_ $\neg Food(Peanuts)$
        
2. Resolve $\neg Food(Peanuts)$ with **C3** ($\neg Eats(x, y) \lor Killed(x) \lor Food(y)$) using $y = Peanuts$:
    
    - _Result:_ $\neg Eats(x, Peanuts) \lor Killed(x)$
        
3. Resolve the _Result_ with **C4a** ($Eats(Anil, Peanuts)$) using $x = Anil$:
    
    - _Result:_ $Killed(Anil)$
        
4. Resolve $Killed(Anil)$ with fact **C4b** ($\neg Killed(Anil)$):
    
    - _Result:_ **$\square$ (Contradiction)**
        
        _Contradiction reached! Therefore, the original un-negated goal, "John likes peanuts," must be true._
    

---

# Q2


## **1) Establish by forward chaining that the pet is a dog.**

- Rule 1 IF the pet has hair THEN it is a mammal 
- Rule 2 IF the pet gives birth THEN it is a mammal 
- Rule 3 IF the pet has feathers THEN the pet is it is a bird 
- Rule 4 IF the pet flies AND the animal lays eggs THEN the pet is it is a bird 
- Rule 5 IF the pet is a mammal AND the pet eats meat THEN it is a carnivore 
- Rule 6 IF the pet is a mammal AND the pet has pointed teeth AND the pet has claws AND the pet's eyes point forward THEN it is a carnivore 
- Rule 7 IF the height is <10 cm THEN the size is small 
- Rule 8 IF the height is >10 cm AND the height is < 30 cm THEN the size is medium 
- Rule 9 IF the height is >50 cm THEN the size is big
- Rule 10 IF the pet is a carnivore AND the pet barks AND the pet has long legs AND the animal is big size THEN the pet is a dog. 
- R11 IF the pet is a carnivore AND the pet has soft hair AND the pet size is medium THEN the pet is a cat 
- R12 IF the pet is small AND the pet has short legs AND the pet has soft hair THEN the pet is a mouse 
- R13 IF the pet has light hair AND the pet size is medium AND the pet is not carnivore THEN then the pet is Guinea pig

### Sol

**Initial Facts in Working Memory:**

- gives birth
    
- long legs
    
- height is 70 cm (في الامتحان مكتوبة size فا اعتبرتها غلطة لان ال size في ال rules كلها categorical)
    
- eats meat
    
- barks
    

**Goal:**

- Prove: **the pet is a dog.**
    

**Iteration 1:**

- **Check Rule 2:** `IF the pet gives birth THEN it is a mammal`
    
    - Since we have the fact "gives birth", this rule fires.
        
    - _New Fact Derived:_ **mammal**
        
- **Check Rule 9:** `IF the height is > 50 cm THEN the size is big`
    
    - Since we have the fact "size is 70 cm" (which is > 50 cm), this rule fires.
        
    - _New Fact Derived:_ **size is big**
        
- _Current Working Memory:_ gives birth, long legs, height = 70 cm, eats meat, barks, mammal, size is big.
    

**Iteration 2:**

- **Check Rule 5:** `IF the pet is a mammal AND the pet eats meat THEN it is a carnivore`
    
    - We now have "mammal" and "eats meat" in our working memory. The rule fires.
        
    - _New Fact Derived:_ **carnivore**
        
- _Current Working Memory:_ `[gives birth, long legs, height = 70 cm, eats meat, barks, mammal, size is big, carnivore]`.
    

**Iteration 3:**

- **Check Rule 10:** `IF the pet is a carnivore AND the pet barks AND the pet has long legs AND the animal is big size THEN the pet is a dog.`
    
    - We check our working memory for the required conditions:
        
        1. carnivore (Yes)
            
        2. barks (Yes)
            
        3. long legs (Yes)
            
        4. big size (Yes)
            
    - Since all conditions are met, the rule fires.
        
    - _New Fact Derived:_ **the pet is a dog**
        

**Conclusion:** The goal that the pet is a dog has been successfully established using forward chaining.

---

### B) Explain the difference between forward chaining and backward chaining.

Support your answer with an example of giving a good result using forward chaining than backward chaining.

### Sol

#### **The Differences**

| **Feature**              | **Forward Chaining**                                                                                     | **Backward Chaining**                                                                                                              |
| ------------------------ | -------------------------------------------------------------------------------------------------------- | ---------------------------------------------------------------------------------------------------------------------------------- |
| **Fundamental Approach** | **Data-Driven:** It moves from the bottom up.                                                            | **Goal-Driven:** It moves from the top down.                                                                                       |
| **Starting Point**       | Starts with the **initial facts** (the data currently in the working memory).                            | Starts with a **hypothesis or goal** (what you want to prove).                                                                     |
| **Ending Point**         | Stops when a **goal is reached** or when no more rules can be fired.                                     | Stops when all **sub-goals are proven true** by the initial facts.                                                                 |
| **Rule Matching**        | Matches the current facts against the **IF part** (antecedent/premise) of the rules.                     | Matches the current goal/sub-goal against the **THEN part** (consequent/conclusion) of the rules.                                  |
| **Action Taken**         | If a rule fires, it extracts the conclusion and **adds it as a new fact** to the database.               | If a rule matches, it extracts the premises and **sets them as new sub-goals** to be proven.                                       |
| **Analogy**              | Like an investigator piecing together clues at a crime scene to figure out what happened and who did it. | Like a doctor starting with a suspected diagnosis (e.g., "flu") and checking if the patient has the specific symptoms to prove it. |

#### **When Forward Chaining is Better (The Example)**

Forward chaining excels in situations where you have a small number of initial facts but a massive number of possible conclusions, such as in **monitoring systems, event-driven systems, or synthesis/design tasks.**

**Example: A Smart Home Emergency System**

Imagine a smart home system with hundreds of rules determining what to do in various situations (turn on sprinklers, call police, lock doors, adjust AC, etc.).

- **The Facts:** The smoke detector in the kitchen triggers (a single new fact).
    
- **Using Forward Chaining:** The system takes this new fact ("smoke detected"), finds the rule `IF smoke detected THEN trigger alarm AND call fire department`, and executes it immediately. It is fast and responsive because it only processes the rules relevant to the incoming data.
    
- **Using Backward Chaining (Poor Result):** The system would have to start by guessing every possible goal. It would ask, _"Should I call the police? Let's check the sub-goals... no. Should I turn down the AC? Let's check the sub-goals... no. Should I call the fire department? Let's check the sub-goals... yes, there is smoke."_ In this scenario, backward chaining is terribly inefficient because it forces the system to evaluate countless irrelevant goals just to figure out what to do with one piece of data, whereas forward chaining handles it instantly.