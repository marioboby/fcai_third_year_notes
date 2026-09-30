---
{"dg-publish":true,"permalink":"/3rd-year/cognitive-science/expanded-explanations/lecture-4/","dg-note-properties":{}}
---


# Burglary Alarm 

## Global Semantics Formula

To understand how that formula is derived, we need to break down how probability builds from a single event into a massive sequence of events. Let’s tackle this step-by-step.
### 1. What is a "Complete Path"?

In the context of Bayesian Networks, a "Complete Path" (often called a "full joint assignment" or a "possible world") is a specific scenario where **every single variable in the network has an assigned, known value** (True or False).

There are no "hidden" or "unknown" variables in a complete path.

- In the Alarm network, there are 5 variables. Because each can be True or False, there are $2^5 = 32$ possible "complete paths" or unique universes.
<br>
- The sequence $j \cap m \cap a \cap \neg b \cap \neg e$ is one of those 32 complete paths. It describes the specific universe where: John calls ($j$) AND Mary calls ($m$) AND the alarm sounds ($a$) AND there is NO burglary ($\neg b$) AND there is NO earthquake ($\neg e$).

### 2. Deriving the Long Formula: The Chain Rule of Probability

The long formula you provided does not actually come from the Bayesian Network yet. It is a fundamental law of statistics called the **Chain Rule of Probability**. It is how you calculate the probability of _any_ sequence of events occurring together.

It starts with the basic definition of conditional probability for two events:

$P(X \cap Y) = P(X|Y) \times P(Y)$

_(The probability of X and Y happening = the probability of X happening given that Y happened, multiplied by the probability that Y happened in the first place)._

If we have three events, we just expand the chain:

$P(X \cap Y \cap Z) = P(X | Y \cap Z) \times P(Y \cap Z)$

Which further expands to:

$P(X \cap Y \cap Z) = P(X | Y \cap Z) \times P(Y | Z) \times P(Z)$

To get the formula in slides, we apply this exact same expanding chain to all 5 variables, peeling them off one by one from left to right:

1. **Start with the full sequence:** $P(j \cap m \cap a \cap \neg b \cap \neg e)$
<br>
2. **Peel off 'j':** $= P(j \mid m \cap a \cap \neg b \cap \neg e) \times P(m \cap a \cap \neg b \cap \neg e)$
<br>
3. **Peel off 'm':** $= P(j \mid m \cap a \cap \neg b \cap \neg e) \times P(m \mid a \cap \neg b \cap \neg e) \times P(a \cap \neg b \cap \neg e)$
<br>
4. **Peel off 'a':** $= P(j \mid m \cap a \cap \neg b \cap \neg e) \times P(m \mid a \cap \neg b \cap \neg e) \times P(a \mid \neg b \cap \neg e) \times P(\neg b \cap \neg e)$
<br>
5. **Peel off '¬b':** $= P(j \mid m \cap a \cap \neg b \cap \neg e) \times P(m \mid a \cap \neg b \cap \neg e) \times P(a \mid \neg b \cap \neg e) \times P(\neg b \mid \neg e) \times P(\neg e)$
<br>
That mathematical expansion is universally true for any probability distribution.

### 3. The Global Semantics Formula (The Bayesian Magic)

While the long formula above is mathematically correct, it is practically useless for computation. To calculate $P(j \mid m \cap a \cap \neg b \cap \neg e)$, you would need a massive data table tracking how often John calls under every possible combination of Mary, Alarms, Burglars, and Earthquakes.

This is where the **Global Semantics Formula** comes in.

The Global Semantics Formula defines the joint probability of a Bayesian Network by stating that **a node only cares about its direct parents**. The network's structure allows us to cross out the irrelevant variables in that long, ugly Chain Rule equation.

Mathematically, the Global Semantics states:

$P(X_1, ..., X_n) = \prod_{i=1}^{n} P(X_i \mid Parents(X_i))$

Let's apply the Global Semantics rule to simplify your long Chain Rule formula step-by-step:

- **$P(j \mid m \cap a \cap \neg b \cap \neg e)$**: Look at the network. John's calling ($j$) is only directly influenced by the Alarm ($a$). Whether Mary called, or a burglar entered, does not directly change John's behavior; only the alarm does. So, this simplifies to just **$P(j|a)$**.
<br>
- **$P(m \mid a \cap \neg b \cap \neg e)$**: Similarly, Mary only reacts to the Alarm. This simplifies to **$P(m|a)$**.
<br>
- **$P(a \mid \neg b \cap \neg e)$**: The Alarm is directly influenced by both Burglary and Earthquake. Since both are its parents, this term **cannot be simplified**. It remains $P(a \mid \neg b, \neg e)$.
<br>
- **$P(\neg b \mid \neg e)$**: Burglary has no parents in this network. Furthermore, Burglary and Earthquake are completely independent of each other. Knowing there is no earthquake tells you nothing about a burglary. This simplifies to just **$P(\neg b)$**.
<br>
- **$P(\neg e)$**: Earthquake has no parents. It remains **$P(\neg e)$**.
<br>

**The Final Result:**

Thanks to the Global Semantics of the Bayesian Network, that massive, computationally heavy Chain Rule formula is transformed into a highly efficient product of simple conditional probabilities:

**$P(j, m, a, \neg b, \neg e) = P(j|a) \times P(m|a) \times P(a|\neg b, \neg e) \times P(\neg b) \times P(\neg e)$**

Instead of needing a giant table of 32 probabilities, you can just look up these 5 simple numbers in your network's Conditional Probability Tables (CPTs) and multiply them together.

---

## Problem 2

In Problem 2, the lecture addresses a significantly more complex challenge than calculating a simple joint probability. The goal is to calculate the conditional probability **$P(j, \neg m | b)$** — the probability that John calls and Mary does not call, given that we _know_ a burglary has occurred.

The core difficulty here is that the network contains 5 variables, but our query only mentions 3 ($J$, $M$, and $B$). The remaining two variables—**Alarm ($A$)** and **Earthquake ($E$)**—are neither given as evidence nor part of the final answer. These are called **Hidden Variables**.

To get the correct probability, we cannot just ignore them. We must account for every possible state they could be in. This mathematical process is called **Marginalization** (or "summing out").

### 1. Setting Up the Equation

Because we know $B$ is true, we need to calculate the probability of our target events $(j, \neg m)$ across all possible combinations of the hidden variables $A$ and $E$, while keeping $B$ as true.

Using the Global Semantics of the network, the general term inside our sum looks like this:

$P(j | a) \times P(\neg m | a) \times P(a | b, e) \times P(e)$

Notice that $P(b)$ is omitted from this product. Because $B$ is a _given condition_ (we already know it happened), its probability is effectively 1 in this isolated context.

To marginalize, we sum this product over the four possible "worlds" (combinations of $A$ and $E$):

$P(j, \neg m | b) = \sum_{a \in \{a, \neg a\}} \sum_{e \in \{e, \neg e\}} P(j | a) P(\neg m | a) P(a | b, e) P(e)$

### 2. The Four "Worlds"

We must calculate the product for these four distinct scenarios and then add the results together:

**World 1: The Alarm sounds, and an Earthquake happens $(a, e)$**

$P(j | a) \times P(\neg m | a) \times P(a | b, e) \times P(e)$

$= 0.90 \times 0.30 \times 0.95 \times 0.002 = \mathbf{0.000513}$
<br>

**World 2: The Alarm sounds, but NO Earthquake happens $(a, \neg e)$**

$P(j | a) \times P(\neg m | a) \times P(a | b, \neg e) \times P(\neg e)$

$= 0.90 \times 0.30 \times 0.94 \times 0.998 = \mathbf{0.2532924}$
<br>
**World 3: The Alarm is silent, but an Earthquake happens $(\neg a, e)$**

$P(j | \neg a) \times P(\neg m | \neg a) \times P(\neg a | b, e) \times P(e)$

$= 0.05 \times 0.99 \times 0.05 \times 0.002 = \mathbf{0.00000495}$
<br>
**World 4: The Alarm is silent, and NO Earthquake happens $(\neg a, \neg e)$**

$P(j | \neg a) \times P(\neg m | \neg a) \times P(\neg a | b, \neg e) \times P(\neg e)$

$= 0.05 \times 0.99 \times 0.06 \times 0.998 = \mathbf{0.00296406}$

### 3. The Final Sum

> Because this involves summing multiple small decimal products, it is easy to make a transcription error. Storing each of these four intermediate results into the memory variables (`A`, `B`, `C`, `D`) on an fx-991ES Plus calculator allows you to compute each scenario separately and sum them at the end without losing floating-point precision.

Adding the four worlds together:

$0.000513 + 0.2532924 + 0.00000495 + 0.00296406 = \mathbf{0.25677441}$

So, the exact conditional probability $P(j, \neg m | b)$ is approximately **$0.257$**.

### 4. The Intuition: Why does it equal 0.257?

The lecture emphasizes a powerful shortcut in probabilistic reasoning: **High-probability paths dominate the calculation.**

Look closely at the four worlds above. World 2 ($0.253$) contributes almost the entirely of the final answer. Why?

1. Earthquakes are extremely rare ($0.2\%$). Therefore, Worlds 1 and 3 are so mathematically tiny they barely affect the outcome.
<br>
2. If a burglary happens, the alarm is almost guaranteed to sound ($94\%$). Therefore, World 4 (Burglary happens but the alarm is silent) is also highly unlikely.


Because we know a burglary happened, we can logically assume the alarm probably sounded, and an earthquake probably didn't happen. If we _only_ calculate the highest probability scenario (assuming $A$ is true):

$P(j|a) \times P(\neg m|a) = 0.90 \times 0.30 = \mathbf{0.27}$

The exact answer ($0.257$) is incredibly close to our simplified logical guess ($0.27$). This demonstrates that in large Bayesian Networks, you can often estimate outcomes by simply following the single most likely path of events and ignoring the extremely rare "edge case" universes.

---
## Problem 3

Problem 3 demonstrates **Diagnostic Inference**, which is a "bottom-up" approach where we reason backward from an observed effect to a hidden cause.

Specifically, the goal is to calculate the probability that an earthquake occurred ($e$), given the observed evidence that John has called ($j$).

To solve this, we cannot use the standard top-down chain rule. Instead, we must use **Bayes' Rule**:

$$P(e|j) = \frac{P(j|e)P(e)}{P(j)}$$

Here is the detailed, step-by-step breakdown of how the lecture solves this equation.

### Step 1: Identify the Prior Probability

The easiest piece of the puzzle is $P(e)$, which is the baseline prior probability of an earthquake occurring.

- From the network's tables, this is a given constant: $P(e) = 0.002$.
  
### Step 2: Calculate $P(j|e)$ via Marginalization

We need to figure out the probability of John calling given that an earthquake is happening. However, John's calling is not directly connected to the earthquake; it is mediated by the Alarm ($A$) and influenced by the possibility of a Burglary ($B$).

Because Alarm and Burglary are hidden variables in this specific query, we must marginalize (sum) over all four of their possible states. The formula for this is:

$$P(j|e) = \sum_{b,a} P(j|a)P(a|b,e)P(b)$$

Using the given constants $P(b) = 0.001$ and $P(\neg b) = 0.999$, we calculate the four possible scenarios:

- **Burglary and Alarm ($b, a$):** $0.90 \times 0.95 \times 0.001 = 0.000855$
    
- **Burglary and No Alarm ($b, \neg a$):** $0.05 \times 0.05 \times 0.001 = 0.0000025$
    
- **No Burglary, but Alarm rings ($\neg b, a$):** $0.90 \times 0.29 \times 0.999 = 0.260739$
    
- **No Burglary and No Alarm ($\neg b, \neg a$):** $0.05 \times 0.71 \times 0.999 = 0.0354645$
    

Summing these four scenarios gives us our marginalized probability: $P(j|e) \approx 0.29706$.

### Step 3: Calculate the Normalizer $P(j)$

The denominator in Bayes' rule, $P(j)$, represents the absolute total probability that John calls under _any_ circumstance.

To find this, we must sum the probability of John calling when there _is_ an earthquake, and the probability of him calling when there is _not_ an earthquake:

$$P(j) = P(j|e)P(e) + P(j|\neg e)P(\neg e)$$

- We already know $P(j|e) \approx 0.297$ and $P(e) = 0.002$.
<br>
- We know $P(\neg e) = 0.998$.
<br>
- The lecture provides $P(j|\neg e) \approx 0.0521$, which is derived from a similar marginalization process.
    

Plugging these in:

$P(j) = (0.297 \times 0.002) + (0.0521 \times 0.998)$ $P(j) \approx 0.000594 + 0.05199 = 0.05258$

### Step 4: Final Computation and Interpretation

Now we have all three pieces for Bayes' Rule:

$$P(e|j) = \frac{0.29706 \times 0.002}{0.05258}$$

$$P(e|j) = \frac{0.000594}{0.05258} \approx 0.0113$$

**The Conceptual Takeaway:** This result perfectly illustrates how evidence shifts our beliefs. The baseline probability of an earthquake occurring on any given day is an incredibly low **0.2%** ($0.002$). However, the moment we receive the evidence that John called, our mathematical belief that an earthquake is happening jumps to about **1.1%**.

Despite this increase, the network correctly models reality: a burglary is mathematically a much more likely cause for John's call than a rare earthquake.

---

# Local Semantics Vs Markov Blanket 

Here is a detailed breakdown of the concepts of **Local Semantics** and the **Markov Blanket**, which are crucial for understanding how information flows (and gets blocked) in a Bayesian Network.

## 1. Local Semantics (The "Top-Down" Shield)

Local Semantics define the most basic rule of independence in a Bayesian Network: **A node is conditionally independent of all its non-descendants, given its parents.**

- **What this means:** If you know the exact state of a node's parents, you do not need to look at its grandparents or any other parallel ancestors to predict its behavior. The parents act as an informational "shield."
    
- **Use Case:** This is primarily used for **Prediction** (reasoning forward from causes to effects).
<br>
- **Example:** Imagine a chain: `Storm` $\rightarrow$ `Traffic Jam` $\rightarrow$ `Late for Work`.
    
    If you _know_ there is a Traffic Jam (the parent), knowing whether there was a Storm (the grandparent) gives you absolutely no new or useful information about whether you will be Late for Work (the child). The parent shields the child from the ancestor.
    
In Bayesian Networks, the difference between local and global semantics comes down to perspective: **Local semantics** focus on the rules for an individual node and its immediate neighbors to construct the network, while **global semantics** describe the properties of the entire network as a whole, allowing you to answer any arbitrary query.

Here is the exact breakdown of how they differ mathematically and conceptually.

### 1. Local Semantics (The Building Blocks)

Local semantics define the network from the perspective of a single, individual node. They establish the minimal set of independence assumptions required to build the graph.

- **The Core Rule:** A node $X$ is conditionally independent of all its non-descendants, given its parents.
    
- **Notation:** $X \perp\!\!\!\perp \text{NonDescendants}(X) \mid \text{Parents}(X)$
    
- **Purpose:** This is how you _construct_ the network. When you are drawing the directed acyclic graph (DAG) and defining the Conditional Probability Tables (CPTs), you rely on local semantics. You only need to calculate the probability of a node based on its direct parents, ignoring the rest of the graph.
    

If you look at the Sprinkler example from earlier, the local semantics state that the **Sprinkler** is conditionally independent of **Rain** (its non-descendant), given its parent, **Cloudy**.

### 2. Global Semantics (The Big Picture)

Global semantics define what the entire, completed network represents. They describe the full mathematical system that emerges from linking all those local rules together.

- **The Core Rule (Joint Distribution):** The network defines a complete joint probability distribution over all variables, which is exactly the product of all the local conditional probabilities.
    
- **Notation:**
    
    $$P(X_1, X_2, \dots, X_n) = \prod_{i=1}^n P(X_i \mid \text{Parents}(X_i))$$
    
- **d-Separation:** Global semantics also dictate the conditional independence between _any_ arbitrary sets of nodes $A$ and $B$, given some evidence $E$. This is evaluated using the algorithm known as **d-separation** (directed separation).
    
- **Purpose:** This is how you _query_ the network. While local semantics tell you about a node and its parents, global semantics allow an algorithm to figure out if two nodes on completely opposite sides of a massive network influence each other given the current evidence.
    

### The Key Difference Summarized

|**Feature**|**Local Semantics**|**Global Semantics**|
|---|---|---|
|**Scope**|Node-by-node (Microscopic)|The entire graph (Macroscopic)|
|**What it defines**|Independence from non-descendants|The full Joint Probability Distribution & d-separation|
|**Use Case**|**Building** the model (Defining CPTs)|**Querying/Inference** (Calculating probabilities across the network)|

Ultimately, they are mathematically consistent with one another. If you construct a graph where every node obeys the **local semantics**, mathematical proofs guarantee that the entire graph will correctly satisfy the **global semantics**. Local semantics are the inputs; global semantics are the result.
## 2. The Markov Blanket (The "Complete Isolation" Zone)

While Local Semantics only shields a node from its ancestors, the **Markov Blanket** is a stronger concept. It is the minimal set of surrounding nodes required to make a target node conditionally independent of **every other node in the entire network**.

To completely isolate a target node, its Markov Blanket must include exactly three things:

1. **Its Parents** (to shield it from ancestors).
<br>
2. **Its Children** (to shield it from descendants).
<br>
3. **The Co-parents of its children** (often called "spouses").
    

**Why do we need the Co-parents?**

This is due to the "explaining away" effect (the collider structure we discussed earlier). If you observe a child node, its parents suddenly become dependent on each other. Therefore, to fully understand the probability of a target node, you must know what its "rival" co-parents are doing, because they might be providing an alternative explanation for the child's behavior.

## 3. The Rain-Sprinkler-Grass Example

To make this concrete, the lecture introduces a new 4-node network:

- **Cloudy (C)** is the starting parent.
<br>
- Cloudy directly influences whether the **Sprinkler (S)** is turned on, and whether it will **Rain (R)**.
<br>
- Both the Sprinkler and the Rain directly influence whether the **Wet Grass (W)** is true.
    

Let's look at the **Sprinkler (S)** node using both concepts:

**Applying Local Semantics to the Sprinkler:**

- What are the Sprinkler's non-descendants? Rain.
<br>
- What is the Sprinkler's parent? Cloudy.
<br>
- **Rule:** Sprinkler is independent of Rain _given_ Cloudy. If you know it is cloudy, knowing it is raining doesn't change the probability of the sprinkler being on.
    

**Applying the Markov Blanket to the Sprinkler:**

To completely isolate the Sprinkler from the entire universe, we must gather its blanket:

- Its Parent: **Cloudy (C)**
<br>
- Its Child: **Wet Grass (W)**
<br>
- Its Child's Co-parent: **Rain (R)**
<br>
- **Result:** $MB(S) = \{C, W, R\}$. If you know the state of those three specific variables, the Sprinkler is mathematically sealed off. No other variable in a larger network could possibly affect its probability.
    

## 4. The Final Contrast (Slide 26 Summary)

The lecture concludes this section by contrasting how these two concepts are used in practice:

- **Local Semantics = Prediction (Top-Down).** You use this when you are working forward. If you know the causes (parents), you can safely ignore the rest of the historical chain to predict the effect.
<br>
- **Markov Blanket = Inference (Bottom-Up and Lateral).** You use this when you are working backward from an effect to deduce a cause. Because of the "explaining away" phenomenon, inference requires you to look sideways at co-parents, not just straight up and down the causal chain.


By defining these boundaries, Bayesian Networks allow AI to make incredibly complex calculations efficiently, only looking at the localized "blanket" of variables that actually matter, rather than the entire universe of data at once.

## Calculating Inference Using the Markov Blanket (Slide 25)

Slide 25 provides a concrete mathematical example of how to use a Markov Blanket to determine the state of a specific node. The scenario asks us to figure out the probability that the Sprinkler is ON ($S=t$), given a specific set of observations about its Markov Blanket.

- We observe that it is Cloudy ($C=t$).
<br>
- We observe that it is Raining ($R=t$).
<br>
- We observe that the Grass is Wet ($W=t$).
    

To find the probability of the sprinkler being on given this Markov Blanket—written mathematically as $P(S=t|MB(S))$—we must calculate a "score" for both possible states of the sprinkler (ON and OFF).

$$P(S | MB(S)) = \alpha \cdot P(S | Parents(S)) \prod P(Children(S) | Parents(Children(S)))$$

**1. Calculating the Score for Sprinkler ON ($S=t$)** We multiply the probability of the sprinkler turning on when it is cloudy by the probability of the grass being wet when both the sprinkler is on and it is raining.

- Score formula: $P(S=t|C=t) \times P(W=t|S=t, R=t)$.
<br>
- Using the network's tables: $0.10 \times 0.99 = 0.099$.


**2. Calculating the Score for Sprinkler OFF ($S=f$)** We multiply the probability of the sprinkler staying off when it is cloudy by the probability of the grass being wet when the sprinkler is off but it is raining.

- Score formula: $P(S=f|C=t) \times P(W=t|S=f, R=t)$.
<br>
- Using the network's tables: $0.90 \times 0.90 = 0.810$.

> Dependence on the Spouse "Raining given cloudy", is missing, we need the whole blanket.
<br>

**3. Normalization** These raw scores ($0.099$ and $0.810$) do not add up to 1. To convert them into true probabilities, we must normalize them by dividing the target score by the sum of all possible scores.

- The sum of the scores is represented by $\alpha = 0.099 + 0.810 = 0.909$.
<br>
- We divide the "ON" score by this total: $\frac{0.099}{0.909} \approx 10.9\%$.
<br>

**The Takeaway** Even though the grass is wet, the final probability that the sprinkler is running is only 10.9%. Because we know it is raining, the rain provides a sufficient explanation for the wet grass, making the sprinkler highly unlikely to be the cause.

---
### Local Semantics vs. Markov Blanket (Slide 26)

Slide 26 shifts to a conceptual summary, contrasting the two primary ways information flows through a Bayesian Network using a new hypothetical example involving an employee's Lateness ($L$), an Accident ($A$), and another variable ($X$, such as rain or a specific route).

**1. Prediction via Local Semantics** Local Semantics is used for top-down prediction.

- This principle states that a node's parents completely shield it from its ancestors.
<br>
- If we know it is raining ($R=t$), we can ignore other variables to predict the outcome of $X$.
<br>
- The math simplifies to $P(X|R=t, A=f, S=t) = P(X|R=t) = 0.8$.

**2. Inference via the Markov Blanket** The Markov Blanket is used for bottom-up inference, which requires completely isolating a node.

- In the new scenario, we observe the effect: the employee is late ($L=t$).
<br>
- We must evaluate the target variable ($X$) within its blanket, which includes the lateness ($L$) and the co-parent accident variable ($A$).
<br>
- The formula for this calculation is $P(X|Blanket) = \frac{P(X|R)P(L|X,A)}{\sum_{x} P(x|R)P(L|x,A)}$.

**The "Explaining Away" Phenomenon** This slide formally defines the "explaining away" effect we saw with the sprinkler.

- If we discover that an accident actually happened ($A=true$), the probability of $X$ being the cause of the lateness decreases, because the accident has successfully "explained away" why the employee is late.
<br>
- Conversely, if we verify that no accident occurred ($A=false$), the probability of $X$ being the cause increases significantly.