---
{"dg-publish":true,"permalink":"/3rd-year/cognitive-science/sheets/sheet-1/","dg-note-properties":{}}
---


## **Mapping the Sheet Problem to the Colored Doors**

**1. Initial State $\rightarrow$ The Prior Belief**

- **Doors Example:** The robot woke up, didn't know where it was, and assigned $1/5$ (0.2) to every door.
    <br>
- **Sheet Problem:** The exact same. The robot starts completely lost in a 5-cell corridor: $P(X_0) = [0.2, 0.2, 0.2, 0.2, 0.2]$.
    

**2. Step A: Prediction $\rightarrow$ The Move Step (Odometry Model)**

- **Doors Example:** When the robot moved right, the probabilities physically "shifted" over. We discussed how noisy motors cause the probabilities to "smear" based on a slip rate (e.g., undershoot/overshoot).
    <br>
- **Sheet Problem:** This is that "smearing" in action. The math calculates the **Law of Total Probability (Convolution)**. For every single cell, it asks: _"What are all the possible ways the robot could have landed here?"_
    <br>
    - **The Boundary Difference:** In our earlier door examples, we assumed the hallway looped endlessly. In this sheet problem, there is a physical wall at $x_5$. If the robot is at $x_4$ and tries to move 2 steps (overshoot), or is at $x_5$ and tries to move 1 step, it just crashes into the wall and stays at $x_5$. This is why the probability heavily piles up at $P(x_5) = 0.38$.
        

**3. Step B: Correction $\rightarrow$ The Sense Step (Measurement Model)**

- **Doors Example:** The robot sensed "Red", so we multiplied the shifted belief by the likelihood of seeing Red at each specific door ($0.6$ or $0.2$).
    <br>
- **Sheet Problem:** The robot's sensor detects a "Landmark". We take the smeared array from Step A and multiply it by the likelihood of detecting that landmark from each cell. Because the landmark is physically at $x_3$, the likelihood is highest there ($0.8$), decays for adjacent cells ($0.4$), and is lowest far away ($0.1$).
    

**4. Step C: Normalization $\rightarrow$ Normalization**

- **Doors Example:** We divided the raw decimals by the total sum to ensure the array equaled exactly $100\%$ ($1.0$).
    <br>
- **Sheet Problem:** The exact same. The raw numbers (like $0.160$) added up to $0.354$. Dividing by $0.354$ yields the final percentages, showing a massive probability spike ($45.2\%$) at $x_3$.
    

---
## Solution to the homework exercise for the $A - B - C$ chain network.

### 1. Construct the full unnormalized joint distribution table $\tilde{P}(A, B, C)$

First, we determine the values for the new factor $\phi_2(B,C)$ based on the given rule:

- $\phi_2(B, C) = 10$ if $B = C$
    
- $\phi_2(B, C) = 2$ if $B \neq C$
    

The unnormalized joint probability for each state is calculated as $\tilde{P}(A, B, C) = \phi_1(A, B) \cdot \phi_2(B, C)$.

|**A**|**B**|**C**|**ϕ1​(A,B)**|**ϕ2​(B,C)**|**P~(A,B,C)=ϕ1​(A,B)⋅ϕ2​(B,C)**|
|---|---|---|---|---|---|
|0|0|0|30|10|$30 \cdot 10 = \mathbf{300}$|
|0|0|1|30|2|$30 \cdot 2 = \mathbf{60}$|
|0|1|0|5|2|$5 \cdot 2 = \mathbf{10}$|
|0|1|1|5|10|$5 \cdot 10 = \mathbf{50}$|
|1|0|0|1|10|$1 \cdot 10 = \mathbf{10}$|
|1|0|1|1|2|$1 \cdot 2 = \mathbf{2}$|
|1|1|0|10|2|$10 \cdot 2 = \mathbf{20}$|
|1|1|1|10|10|$10 \cdot 10 = \mathbf{100}$|

---

### 2. Calculate the new Partition Function $Z_{new}$

The partition function is the sum of the unnormalized weights for all possible assignments in the state space:

$$Z_{new} = \sum_{a,b,c \in \{0,1\}} \tilde{P}(a, b, c)$$

$$Z_{new} = 300 + 60 + 10 + 50 + 10 + 2 + 20 + 100$$

$$Z_{new} = \mathbf{552}$$

---

### 3. Calculate the probability $P(A=0, B=0, C=1)$

To find the normalized joint probability for this specific state, divide its unnormalized weight by the partition function:

$$P(A=0, B=0, C=1) = \frac{\tilde{P}(A=0, B=0, C=1)}{Z_{new}}$$

$$P(A=0, B=0, C=1) = \frac{60}{552}$$

$$P(A=0, B=0, C=1) = \frac{5}{46} \approx \mathbf{0.1087}$$

---

### 4. Bonus: Find the marginal probability $P(C=0)$

To find the marginal probability for $C=0$, we marginalize out variables $A$ and $B$. This means we sum the unnormalized probabilities of all states where $C=0$ and then divide by the partition function.

First, isolate the unnormalized weights where $C=0$:

- $\tilde{P}(A=0, B=0, C=0) = 300$
    
- $\tilde{P}(A=0, B=1, C=0) = 10$
    
- $\tilde{P}(A=1, B=0, C=0) = 10$
    
- $\tilde{P}(A=1, B=1, C=0) = 20$
    

Sum these unnormalized probabilities:

$$\sum_{a,b} \tilde{P}(a, b, C=0) = 300 + 10 + 10 + 20 = 340$$

Finally, normalize by dividing by $Z_{new}$:

$$P(C=0) = \frac{340}{552}$$

$$P(C=0) = \frac{85}{138} \approx \mathbf{0.6159}$$

---
# Q3 Explanation

### 1. The "Separation" Concept (The What)

Imagine a Markov Network as a group of people spreading a rumor.

Let's look at a simple chain: **Alice — Bob — Charles**.

- Alice only talks to Bob.
    
- Bob talks to Alice and Charles.
    
- Charles only talks to Bob.
    

Alice and Charles do not talk directly.

**The Concept:** Does knowing Alice's state tell you anything about Charles's state?

- **Unobserved (No Separation):** If you don't know what Bob is doing, then yes! If Alice heard the rumor, she probably told Bob, who probably told Charles. Information flows through the path. Alice and Charles are **dependent**.
    
- **Observed (Separation):** Now, imagine you have Bob's phone tapped. You know _exactly_ what Bob knows. If you already know Bob's state, finding out Alice's state gives you _zero new information_ about Charles. Bob's known state acts as a wall, blocking the flow of probability.
    

In network terms, we say **Bob separates Alice and Charles**. When a path is blocked by an observed node, the nodes on either side become **conditionally independent**.

### 2. Variable Elimination (The How)

So, separation tells us when nodes influence each other. But what if we _want_ to know the probability of Charles knowing the rumor, but we **can't** observe Bob?

Since Bob is unobserved, information flows from Alice to Charles through him. To calculate the math for Alice and Charles, we have to deal with Bob.

**Variable Elimination is the mathematical process of removing the middleman.**

Since we don't know Bob's exact state, we must consider _every possible state Bob could be in_.

1. We calculate the probability of the rumor passing if Bob is in State 0.
    
2. We calculate the probability of the rumor passing if Bob is in State 1.
    
3. We add those probabilities together.
    

By summing out all of Bob's possible states, we perfectly summarize his influence. We can then erase Bob from our graph entirely and draw a brand new, direct line connecting Alice and Charles. That new line is the "message" or "factor" we calculated. We "eliminated" the variable by doing the math for it.

Let’s bring the rumor analogy to the sheet problem!

We have four nodes locked in a cycle: **Node 2, Node 4, Node 6, and Node 5**.

### The Problem with Loops (The "Echo Chamber")

Imagine Node 2, 4, 5, and 6 are friends standing in a circle. Node 2 starts a rumor.

- Node 2 whispers to Node 4 and Node 5.
    
- Node 4 hears it and whispers to Node 6.
    
- Node 5 hears it and _also_ whispers to Node 6.
    
- Now Node 6 has heard it twice, gets excited, and whispers it _back_ to Node 4 and Node 5.
    

If we just let them talk, the rumor bounces around the circle forever in an infinite feedback loop. The math completely breaks down because the probabilities keep multiplying endlessly.

**To solve the math, we have to break the circle.** We do this by systematically silencing people (eliminating them) while perfectly preserving what they _would_ have said.

### Step-by-Step Elimination

**1. Eliminating Node 6 (Breaking the Cycle)**

Node 6 only talks to Node 4 and Node 5. We take Node 6 aside and say, "We are going to remove you from the circle, but first, write down exactly how you would react to every possible rumor Node 4 and Node 5 could tell you."

Node 6 does the math (the $2 \times 2 + 1 \times 1 = 5$ stuff we did earlier) and writes a "Manual of Node 6."

We hand this manual directly to Node 4 and Node 5.

- **The Result:** Node 6 goes home. Node 4 and Node 5 no longer need him; they just read his manual. By doing this, we established a brand new, direct link between 4 and 5. **The circle is broken!** It is now a triangle.
    

**2. Eliminating Node 4 (Folding the Triangle)**

Now Node 4 is holding Node 6's manual, and is connected to Node 2 and Node 5.

We take Node 4 aside and say, "Your turn. Combine your own thoughts with Node 6's manual, and write down how you would react to Node 2 and Node 5."

Node 4 writes a new, thicker "Combined Manual" and hands it to Node 2 and Node 5.

- **The Result:** Node 4 goes home. Node 2 and Node 5 are now directly connected by this thick manual. The triangle is now just a single straight line.
    

**3. Eliminating Node 5 (The Final Answer)**

Only Node 2 and Node 5 are left. Node 5 takes the thick manual from Node 4, looks at how it feels about Node 2, does one final mathematical summary, and hands the ultimate result to Node 2.

- **The Result:** Node 5 goes home. Node 2 is left holding a single set of numbers that contains the perfect "echo" of the entire group. No feedback loop required.

---
# Q: why didn't we do the same for the 1-2-3 triangle 

When a group of nodes is completely interconnected like that (everyone is directly connected to everyone else), it is called a **Clique**. Eliminating a node from a clique is actually the cleanest and easiest part of the algorithm!

Let's look at why eliminating Node 3 from the 1-2-3 triangle is different from breaking the 4-node cycle we talked about earlier.

### The Rumor Analogy: The Tight-Knit Group

Imagine Nodes 1, 2, and 3 are a tight-knit friend group.

- Node 1 talks to Node 2.
    
- Node 2 talks to Node 3.
    
- Node 1 talks to Node 3.
    

We need to eliminate Node 3.

Just like before, Node 3 writes down a "manual" summarizing how it reacts to rumors from Node 1 and Node 2. Node 3 hands this manual to 1 and 2, and then goes home.

**Here is the big difference:**

When we eliminated Node 6 from the 4-person loop, Node 4 and Node 5 _didn't_ have a direct line to each other. Node 6's manual forced them to build a brand new phone line.

In our 1-2-3 triangle, Node 1 and Node 2 **already have a direct phone line** (their original edge $\psi_{12}$). They don't need to build a new connection. They just take Node 3's manual and tape it to their existing phone line.

### The Math: Updating Instead of Creating

Mathematically, this means instead of creating a brand new factor, we just multiply Node 3's message into the factor that already exists between 1 and 2.

1. **Calculate the Message:** Node 3 sums out its states based on its connections to 1 and 2.
    
    $$m_3(X_1, X_2) = \sum_{x_3 \in \{0,1\}} \psi_{13}(X_1, x_3) \cdot \psi_{23}(X_2, x_3)$$
    
2. **Update the Edge:** We take that message and multiply it directly into the existing connection between 1 and 2.
    
    $$\psi_{12}^{new}(X_1, X_2) = \psi_{12}^{old}(X_1, X_2) \cdot m_3(X_1, X_2)$$
    

### Why Triangles are Perfect

In graph theory, triangles are mathematically beautiful because they collapse without creating any structural mess. When you eliminate a node from a triangle, it cleanly folds down into a single straight line.

In fact, for really complex networks, computer scientists will purposely add "fake" edges to the graph before solving it just to turn all the messy loops into a bunch of triangles (a process called _Triangulation_).
