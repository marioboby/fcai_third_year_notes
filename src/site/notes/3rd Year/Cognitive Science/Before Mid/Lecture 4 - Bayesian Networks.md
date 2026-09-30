---
{"dg-publish":true,"permalink":"/3rd-year/cognitive-science/before-mid/lecture-4-bayesian-networks/","dg-note-properties":{}}
---

# 1 - Sensor Noise & Aliasing

In any physical system, perception is imperfect. **Sensor Noise** refers to the random, unpredictable variations in sensor readings, often modeled as Gaussian (normal) distributions. A distance sensor might read 1.0 meters, but the true distance could be 0.95 or 1.05 meters.

**Perceptual Aliasing** occurs when a system’s sensors map multiple, distinct states in the environment to the exact same sensor reading.

- Imagine a robot navigating a hallway. If it detects a wooden door, it might be outside Room 101 or Room 102. Because the inputs are identical, the robot cannot distinguish its true state based solely on that single observation.
<br>
- To resolve aliasing, systems must rely on a sequence of historical data (belief states) and probabilistic localization algorithms, rather than trusting a single snapshot in time.

# 2 - Actuator Effects (Uncertainty in Action)

Just as input is noisy, output is inconsistent. When a control command is sent to an actuator (like a motor turning a wheel), the physical execution will rarely be perfect.

- **Environmental factors:** Friction, uneven surfaces, or unexpected obstacles alter the trajectory.
    <br>
- **Internal factors:** A slight drop in battery voltage changes the motor's torque, or mechanical wear and tear causes a slight drift. If you command a system to move forward by exactly 1 meter, the resulting position is not a single deterministic point, but rather a probability distribution of possible end locations. Designing robust algorithms requires modeling this continuous uncertainty, often using Markov Decision Processes (MDPs) to plan for the highest-probability outcomes.

# 3 - AI Foundations

The explosive growth of modern AI rests on four critical pillars:

1. **Data:** The massive volume of digitized information required to train complex models.
<br>
2. **Computational Power:** The hardware infrastructure. Early AI research was severely bottlenecked by weak processors and memory limitations. Modern hardware architecture, heavily utilizing parallel processing, allows for the matrix multiplications required in deep learning.
<br>
3. **Algorithms:** The mathematical frameworks—from foundational search algorithms to backpropagation in neural networks—that allow systems to learn from data.
<br>
4. **Scenarios:** The real-world environments and use-cases (like autonomous driving or medical diagnosis) that provide the structural boundaries for the AI to solve.
# 4 - Bayesian Theory Vs Naive Bayes

Calculating the exact joint probability of many interacting variables requires exponential computational complexity ($O(2^n)$). The **Naïve Bayes classifier** drastically simplifies this by making a strong assumption: all features are conditionally independent of each other, given the class label.

- Mathematically, it changes the complex joint probability into a simple product of individual probabilities: $P(y | x_1, \dots, x_n) \propto P(y) \prod_{i=1}^n P(x_i | y)$
<br>
- While this assumption is "naïve" because real-world variables are almost always correlated, the algorithm is famously robust. It often achieves high accuracy in classification tasks (like spam filtering) with exceptionally low computational overhead.

# 5 - Bayesian Networks

When you cannot assume absolute independence (as in Naïve Bayes), you use a **Bayesian Network**. These are Directed Acyclic Graphs (DAGs) that explicitly map out the conditional dependencies between variables.

- **Nodes** represent random variables, and **directed edges** represent direct causal influence.
<br>
- Returning to the dental example: A `Cavity` directly influences the probability of a `Toothache` and a dental `Catch`.
<br>
- An isolated node, like `Weather`, has no edges connecting it to `Cavity`. Knowing it is raining does absolutely nothing to update the probability of having a cavity, successfully capturing absolute independence.

The network visually encodes two types of independence:

1. **Absolute Independence:** If a node like "Weather" has no edges connecting it to the rest of the graph, it is entirely isolated. Knowing the weather provides absolutely zero information to update your belief about whether someone has a cavity.
<br>
2. **Conditional Independence:** "Toothache" and "Catch" are independent _only if_ the state of "Cavity" is known. Mathematically, this is written as $P(T|Catch,C)=P(T|C)$.

### Dependency Shifts (Conditional Independence)

The flow of information in a network changes dynamically based on what you _know_ (your observed evidence). This dictates whether variables are dependent or independent.

- **Common Cause (Fork Structure):** $X \leftarrow Y \rightarrow Z$.
    
    If you don't know $Y$, then $X$ and $Z$ are dependent. However, if you _observe_ the parent $Y$, the path is blocked. $X$ and $Z$ become **conditionally independent**. (e.g., If you know a cavity is present, a toothache doesn't give you any _new_ information about the likelihood of a dental catch).
    <br>
- **Common Effect (Collider Structure):** $X \rightarrow Y \leftarrow Z$.
    
    Here, the parent variables $X$ and $Z$ are initially independent. But if you observe their shared child $Y$ (or any descendant of $Y$), the parents suddenly become **dependent**. This is known as "explaining away." If an alarm sounds, an earthquake and a burglary are suddenly competing explanations; confirming an earthquake drastically reduces the probability that a burglary is simultaneously happening.
## Joint Probability Distribution & Efficiency

For the mathematical power of the network. Instead of calculating a massive, unmanageable table for every possible combination of events, the network's structure allows you to factor the joint probability into smaller, manageable pieces.

- This is done using the **Chain Rule**: $P(x_{1},...,x_{n})=\prod_{i=1}^{n}P(x_{i}|parents(x_{i}))$.
<br>
- **Why use this?** It drastically reduces computational complexity. For a system with $n$ variables where each has at most $k$ parents, a Bayesian Network requires $O(n\cdot2^{k})$ parameters, which is linear growth.
<br>
- Without the network's independence assumptions, you would need $2^{n}-1$ parameters, resulting in exponential growth that becomes computationally catastrophic.

## Why Use Bayesian Networks?

1. **Uncertainty**: Real-world data is noisy and incomplete. 
2. **Causality**: Helps visualize ”cause and effect” relationships. 
3. **Efficiency**: Allows for compact representation of joint probability (as mentions above)
4. **Inference**: Allows us to update our beliefs as new evidence arrives.

# 6 - The Alarm Scenario & Global Semantics

The lecture introduces Judea Pearl's famous "Burglary Alarm" network, which consists of five variables: Burglary, Earthquake, Alarm, JohnCalls, and MaryCalls.

- The alarm is very likely to trigger during a burglary, but occasionally triggers during an earthquake.
<br>
- John and Mary have different reliabilities when calling to report the alarm.

Using the Global Semantics Formula, you can calculate the exact probability of a specific "complete path" of events. For example, the probability that John and Mary both call, the alarm sounds, but there is _no_ burglary and _no_ earthquake is calculated by multiplying the respective probabilities from the CPTs, resulting in approximately $0.00063$.

to see more, [[3rd Year/Cognitive Science/Expanded Explanations/Lecture 4#Global Semantics Formula\|here]]

## Problem 1 - Joint Probability

This slide walks through a specific joint probability calculation: finding $P(j,\neg m,\neg a,b,e)$.

- This translates to: John calls, Mary does not, the alarm is silent, a burglar enters, and an earthquake happens.
<br>
- By tracing the chain rule, you multiply: $P(j|\neg a)P(\neg m|\neg a)P(\neg a|b,e)P(b)P(e)$.
<br>
- Plugging in the numbers from the CPT yields a vanishingly small probability of $0.00000000495$.
    

## Problem 2 - Conditional Probability

Here, the lecture tackles a harder problem: calculating $P(j,\neg m|b)$. Because the states of the Alarm (A) and Earthquake (E) are not specified, they are considered "hidden variables" that must be marginalized (summed over).

- This requires analyzing the 4 possible "worlds" or scenarios for the hidden variables: (a, e), (¬a, e), (a, ¬e), and (¬a, ¬e).
  <br>
- Summing the probabilities of all four scenarios gives the exact result of roughly $0.257$.
   <br>
- **The Intuition:** Because earthquakes are incredibly rare (0.2%), scenarios involving them can essentially be ignored. Furthermore, if a burglary happens, the alarm is highly likely to sound. Therefore, the calculation is dominated by the highest-probability path, simplifying to roughly $P(j|a)P(\neg m|a) \approx 0.27$, which is very close to the exact calculated result.

to see more on this problem, [[3rd Year/Cognitive Science/Expanded Explanations/Lecture 4#Problem 2\|here]]
  
## Problem 3 - Diagnostic Inference

This problem demonstrates "Bottom-Up" inference using Bayes' Rule: calculating the probability of a cause given an effect. The goal is to find the probability of an earthquake given that John called: $P(e|j)$.

- The formula is $P(e|j)=\frac{P(j|e)P(e)}{P(j)}$.
<br>
- After calculating the required marginalizations, the final probability is $0.0113$.
<br>
- **Interpretation:** The baseline probability of an earthquake is tiny (0.002). Knowing that John called increases that probability to about 1.1%. However, the network correctly deduces that a burglary remains the much more logical and likely explanation for the alarm.
  
to see more on this problem, [[3rd Year/Cognitive Science/Expanded Explanations/Lecture 4#Problem 3\|here]]
# 7 - Local Semantics & Markov Blanket

These slides define the structural rules that allow nodes to be isolated for calculations.

- **Local Semantics:** A node is conditionally independent of its non-descendants as long as its parents are known.
<br>
- **Markov Blanket:** This is a specific grouping that completely isolates a node from the rest of the network. A node's Markov Blanket consists of its parents, its children, and the co-parents of its children.


## The Rain-Sprinkler-Grass Example

The lecture shifts to a new network to demonstrate the Markov Blanket.

- The causal chain is $R\rightarrow S\rightarrow W$ (Cloudy → affects Sprinkler and Rain → affects Wet Grass).
<br>
- Applying Local Semantics: "Sprinkler" is independent of "Rain" given "Cloudy" ($S\perp R|C$).
<br>
- Applying the Markov Blanket: To completely isolate the "Sprinkler" (S) node, you need its parent (Cloudy), its child (Wet Grass), and its child's co-parent (Rain). Therefore, $MB(S)=\{C,W,R\}$.
## Solving an Example Inference

This slide runs a standard joint probability calculation on the new network, finding $P(c,\neg s,r,w)$.

- Using the Chain Rule: $P(c)\cdot P(\neg s|c)\cdot P(r|c)\cdot P(w|\neg s,r)$.
<br>
- The result is $0.5 \times 0.9 \times 0.8 \times 0.9 = 0.324$, or 32.4%.

## Numerical Inference & "Explaining Away"

This section uses the Markov Blanket to perform inference on the "Sprinkler" node.

- **Scenario:** You observe that it is Cloudy, Raining, and the Grass is Wet. What is the probability that the Sprinkler is currently on?
<br>
- By calculating the scores for the Sprinkler being ON ($0.099$) versus OFF ($0.810$) and normalizing them, the probability that the sprinkler is on is only $10.9\%$.
<br>
- **Interpretation:** Because you know it is raining, the rain "explains away" the wet grass, making the sprinkler highly unlikely to be the cause.

# Local Semantics vs. Markov Blanket

The final instructional slide contrasts the two main properties.

1. **Prediction (Local Semantics):** Works top-down. If you know a parent state (e.g., it is raining), you ignore ancestors because the parents "shield" the node.
 <br>
2. **Inference (Markov Blanket):** Works bottom-up. When working backward from an effect (e.g., an employee is late), discovering one cause (an accident) will significantly decrease the probability of other competing causes, effectively "explaining away" the effect.