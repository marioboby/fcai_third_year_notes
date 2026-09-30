---
{"dg-publish":true,"permalink":"/3rd-year/cognitive-science/expanded-explanations/lecture-7/","dg-note-properties":{}}
---

# Chain Rule and Markov Assumption

### 1. The General Chain Rule (The Exhaustive Way)

The first equation on the slide is the standard chain rule for probabilities:

$$P(X^{(0:T)}) = P(X^{(0)}) \prod_{t=0}^{T-1} P(\mathbf{X}^{(t+1)} \mid \mathbf{X}^{(0:t)})$$

This rule states that the probability of an entire sequence of events happening exactly in a specific order is the probability of the first event, multiplied by the probability of the second event _given the first_, multiplied by the probability of the third event _given the first two_, and so on.

As the sequence gets longer, the conditional probabilities become massively complex because you have to account for the entire history up to time $t$ ($\mathbf{X}^{(0:t)}$). If you were tracking daily server statuses over a year, predicting day 365 would require knowing the exact permutation of the previous 364 days.

### 2. The Markov Assumption (The "Amnesia" Shortcut)

The second section introduces the **Markov Property**:

$$P(\mathbf{X}^{(t+1)} \mid \mathbf{X}^{(0:t)}) = P(\mathbf{X}^{(t+1)} \mid \mathbf{X}^{(t)})$$

This is a simplifying assumption that states: **the future is independent of the past, given the present.** Conceptually, this is very similar to how the final output of an RNN sequence depends solely on that final hidden state, rather than needing to recalculate the entire history of preceding hidden states for a single decision. All the relevant information from the past is assumed to be encoded in the _current_ state $\mathbf{X}^{(t)}$.

### 3. The Simplified Chain Rule

By substituting the Markov Assumption into the original chain rule, we get the third equation:

$$P(X^{(0:T)}) = P(X^{(0)}) \prod_{t=0}^{T-1} P(\mathbf{X}^{(t+1)} \mid \mathbf{X}^{(t)})$$

Now, instead of conditioning on the entire history, we only condition on the immediately preceding time step.

### The Numerical Example: Weather Prediction

Let's model the weather over 3 days (Day 0, Day 1, Day 2). The weather can either be **Sunny (S)** or **Rainy (R)**.

First, we define our **Transition Probabilities** (the probability of tomorrow's weather given today's):

- If it is Sunny today, tomorrow is: 80% Sunny ($0.8$), 20% Rainy ($0.2$).
    
- If it is Rainy today, tomorrow is: 40% Sunny ($0.4$), 60% Rainy ($0.6$).
    

Let's say we want to find the probability of this specific trajectory: **Day 0 is Sunny $\rightarrow$ Day 1 is Rainy $\rightarrow$ Day 2 is Rainy.**

**Without the Markov Assumption:**

You would need to know the historical probability $P(\text{Day 2 Rainy} \mid \text{Day 1 Rainy, Day 0 Sunny})$. You would need a massive lookup table for every possible historical sequence.

**With the Markov Assumption:**

We use the simplified chain rule.

$$P(S^{(0)}, R^{(1)}, R^{(2)}) = P(S^{(0)}) \cdot P(R^{(1)} \mid S^{(0)}) \cdot P(R^{(2)} \mid R^{(1)})$$

Assuming we _know_ Day 0 is Sunny (so $P(S^{(0)}) = 1.0$), we just plug in our transition probabilities:

- $P(S^{(0)}) = 1.0$ (It is Sunny today)
    
- $P(R^{(1)} \mid S^{(0)}) = 0.2$ (Chance of rain tomorrow given sunny today)
    
- $P(R^{(2)} \mid R^{(1)}) = 0.6$ (Chance of rain the next day given rainy the day prior)
    

$$Probability = 1.0 \times 0.2 \times 0.6 = \mathbf{0.12}$$

There is a **12% chance** of that exact 3-day trajectory occurring. By making the Markov assumption, we turned a complex historical query into simple, step-by-step matrix multiplication.

# HMM Numerical Example 
### The Scenario Setup

Imagine a robot that is stationed inside a building without any windows. Its goal is to determine the hidden state of the world: **Is it Raining ($R$) outside?**. Because the robot cannot observe the weather directly, the true weather state is considered a "hidden variable".

However, the robot can observe "evidence." In this case, the robot can see if people walking into the building are carrying **Umbrellas ($U$)**.

To solve this, the HMM uses two distinct probability models:

- **The Transition Model ($P(R_t | R_{t-1})$):** How weather behaves over time.
    
    - If it rained yesterday, there is a 70% chance it rains today: $P(R_t | R_{t-1}) = 0.7$.
        
    - If it did _not_ rain yesterday, there is a 30% chance it starts raining today: $P(R_t | \neg R_{t-1}) = 0.3$.
        
- **The Sensor/Observation Model ($P(U_t | R_t)$):** How the weather influences people's behavior.
    
    - If it is raining, there is a 90% chance people carry umbrellas: $P(U | R) = 0.9$.
        
    - If it is _not_ raining, there is still a 20% chance people carry umbrellas (perhaps to block the sun): $P(U | \neg R) = 0.2$.
        

**The Initial Belief:** The robot starts with a prior belief that there was a 50% chance of rain yesterday, meaning $P(R_{t-1}) = 0.5$. Today, the robot observes someone carrying an umbrella ($U_t = \text{true}$).

Here is how the robot calculates the updated probability that it is currently raining, denoted mathematically as $P(R_t | U_t)$.

---

### Step 1: Prediction (The Time Update)

Before the robot even looks at the umbrellas, it must forecast today's weather based purely on yesterday's belief and how weather naturally transitions. It does this using the Law of Total Probability to sum up all possible ways it could rain today.

$$P(R_t) = \sum_{r_{t-1}} P(R_t | r_{t-1})P(R_{t-1})$$

Expanded, this means the probability of rain today is the sum of two scenarios: (1) it rained yesterday and continued, OR (2) it didn't rain yesterday but started today.

$$P(R_t) = (P(R_t | R_{t-1}) \times P(R_{t-1})) + (P(R_t | \neg R_{t-1}) \times P(\neg R_{t-1}))$$

Plugging in the numbers from the models:

$$P(R_t) = (0.7 \times 0.5) + (0.3 \times 0.5)$$

$$P(R_t) = 0.35 + 0.15 = 0.5$$

So, based purely on time passing, the robot believes there is a 50% chance of rain today. This also inherently means there is a 50% chance of _no_ rain today ($P(\neg R_t) = 0.5$).

---

### Step 2: The Update (Using the Observation)

Now, the robot looks at the door and observes an umbrella ($U_t$). It uses Bayes' Rule to update its 50% belief by factoring in how likely it is to see an umbrella under different weather conditions.

The formula introduces an unnormalized constant, $\alpha$.

$$P(R_t | U_t) = \alpha P(U_t | R_t)P(R_t)$$

The robot calculates the "unnormalized" likelihoods for both possible realities (Rain vs. No Rain):

**Reality 1: It is raining.**

$$P(R_t | U_t) = \alpha (0.9 \times 0.5) = 0.45\alpha$$

**Reality 2: It is NOT raining.**

$$P(\neg R_t | U_t) = \alpha P(U_t | \neg R_t)P(\neg R_t)$$

$$P(\neg R_t | U_t) = \alpha (0.2 \times 0.5) = 0.1\alpha$$

---

### Step 3: Normalization

In probability, the total chance of all possible states must equal exactly 1 (or 100%). Currently, our unnormalized values ($0.45$ and $0.1$) do not sum to 1. The constant $\alpha$ is used to force them to sum to 1.

We find $\alpha$ by dividing 1 by the sum of our unnormalized values:

$$\alpha = \frac{1}{0.45 + 0.1} = \frac{1}{0.55}$$

Finally, we apply this normalization factor to our "Rain" calculation to get the true percentage:

$$P(R_t | U_t) = \frac{0.45}{0.55} \approx 0.818$$

**The Conclusion:** Before seeing the umbrella, the robot thought there was a 50% chance of rain. After seeing the umbrella—because the sensor model tells the robot that umbrellas are a very strong indicator of rain—its confidence skyrocketed to approximately 81.8%.

---

# Dynamic BN Example 

![Pasted image 20260416002553.png](/img/user/imgs/Pasted%20image%2020260416002553.png)

### The Scenario Setup

The DBN example builds on the HMM scenario but introduces a **factored representation**. Instead of a single hidden variable, the true state of the weather is split into two independent variables: **Rain ($R_t$)** and **Wind ($W_t$)**.

The robot still observes **Umbrellas ($U_t$)**, but the likelihood of seeing an umbrella is now jointly influenced by both rain and wind.

**The Initial Belief ($t=0$):**

The robot starts with absolute certainty that it is neither raining nor windy.

- $P(r_0) = 0$.
    
- $P(w_0) = 0$.
    

**The Transition Models:** Rain and wind evolve over time completely independently of one another.

- **Rain:** If it did not rain yesterday, there is a 30% chance it starts today ($P(R_{t+1}|\neg R_t) = 0.3$).
    
- **Wind:** If it was not windy yesterday, there is a 40% chance it gets windy today ($P(W_{t+1}|\neg W_t) = 0.4$).
    

**The Observation Model ($P(U_t | R_t, W_t)$):** The chance of seeing an umbrella changes based on the combination of weather:

- Rain + Wind: $0.8$ (Wind might break umbrellas, slightly lowering usage).
    
- Rain + No Wind: $0.9$.
    
- No Rain + Wind: $0.1$.
    
- No Rain + No Wind: $0.2$.
    

---

### Step 1: Prediction ($t=0 \rightarrow t=1$)

Because the robot knew with 100% certainty that yesterday was clear and calm ($P(\neg r_0, \neg w_0) = 1.0$), predicting today's weather prior to observing anything relies entirely on the transition probabilities from a "False" state.

First, calculate the individual probabilities for today:

- $P(r_1) = 0.3$.
    
- $P(w_1) = 0.4$.
    

Next, because the transitions are independent, you multiply them to find the "Joint Prior" representing the four possible realities for today's weather:

- **Rain & Wind ($r_1, w_1$):** $0.3 \times 0.4 = 0.12$.
    
- **Rain & No Wind ($r_1, \neg w_1$):** $0.3 \times (1 - 0.4) = 0.18$.
    
- **No Rain & Wind ($\neg r_1, w_1$):** $(1 - 0.3) \times 0.4 = 0.28$.
    
- **No Rain & No Wind ($\neg r_1, \neg w_1$):** $0.7 \times 0.6 = 0.42$.
    

---

### Step 2: The Update (Using the Observation)

The robot now observes an Umbrella ($u_1$). To update its beliefs, it uses Bayes' Rule to multiply the prior probability of each reality by the likelihood of seeing an umbrella in that specific reality.

This creates the **unnormalized posterior** ($P'$):

- **Reality 1 ($r_1, w_1$):** $0.8 \times 0.12 = 0.096$.
    
- **Reality 2 ($r_1, \neg w_1$):** $0.9 \times 0.18 = 0.162$.
    
- **Reality 3 ($\neg r_1, w_1$):** $0.1 \times 0.28 = 0.028$.
    
- **Reality 4 ($\neg r_1, \neg w_1$):** $0.2 \times 0.42 = 0.084$.
    

---

### Step 3: Normalization & Marginalization

To convert these unnormalized values into true probabilities, they must be scaled so they sum to 1.

First, sum the unnormalized values:

$$0.096 + 0.162 + 0.028 + 0.084 = 0.37$$

.

Next, divide each unnormalized value by the sum ($0.37$) to get the **normalized posterior**:

- $P(r_1, w_1 | u_1) \approx 0.259$.
    
- $P(r_1, \neg w_1 | u_1) \approx 0.438$.
    
- $P(\neg r_1, w_1 | u_1) \approx 0.076$.
    
- $P(\neg r_1, \neg w_1 | u_1) \approx 0.227$.
    

Finally, the robot wants to answer its core question: **Is it raining?** To find the overall probability of rain, it performs **marginalization**. It sums the probabilities of the two possible realities where it is raining, effectively factoring the wind variable out of the final equation:

$$P(r_1 | u_1) = 0.259 + 0.438 = 0.697$$

### The Conclusion

At time $t=0$, the robot was certain it was not raining (0%). After the natural passage of time and observing an umbrella, the robot's belief in rain shifted to **69.7%**. A key takeaway from this DBN is that even though rain and wind naturally evolve completely independently of each other, the single observation of the umbrella immediately "couples" them together in the posterior calculation.