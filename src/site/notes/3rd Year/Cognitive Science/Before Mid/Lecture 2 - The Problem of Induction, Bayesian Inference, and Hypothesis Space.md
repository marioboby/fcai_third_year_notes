---
{"dg-publish":true,"permalink":"/3rd-year/cognitive-science/before-mid/lecture-2-the-problem-of-induction-bayesian-inference-and-hypothesis-space/","dg-note-properties":{}}
---

# Outline
[[3rd Year/Cognitive Science/Before Mid/Lecture 2 - The Problem of Induction, Bayesian Inference, and Hypothesis Space#Part 1 The Problem of Induction\|The Problem of Induction]]
[[3rd Year/Cognitive Science/Before Mid/Lecture 2 - The Problem of Induction, Bayesian Inference, and Hypothesis Space#Part 2 Bayesian Inference\|Bayesian Inference]]
[[3rd Year/Cognitive Science/Before Mid/Lecture 2 - The Problem of Induction, Bayesian Inference, and Hypothesis Space#Part 3 Hypothesis Space\|Hypothesis Space]]

# Part 1: The Problem of Induction

### **What is Inductive Learning?**
Inductive learning is the ==process of moving from specific observations to general rules.==

- The goal is to predict outputs for unseen data based on a mathematical training set defined as $D=\{(x_{1},y_{1}),...,(x_{n},y_{n})\}$.
- The core assumption is that the future will behave exactly like the past.
- However, there is a massive logical gap: unlike pure deduction, induction is never logically guaranteed. Just because you observe 1,000 white swans, it does not prove the 1,001st swan won't be black.

### **Hume's Challenge**
The philosopher David Hume argued that induction cannot be rationally justified.

- [[3rd Year/Cognitive Science/Expanded Explanations/Lecture 2#^ac021c\|When we try to justify it, we say "it worked in the past," which relies on induction to justify induction—a circular argument.]]
- In machine learning, this means data alone is never enough; we must introduce a "Prior" (initial assumptions) to narrow down the infinite possible rules that could fit the training data.

# Part 2: Bayesian Inference

Bayesian inference provides the mathematical framework to update our beliefs as we see new data.

- The core formula is Bayes' Theorem: 
  $$P(h|d)=\frac{P(d|h)P(h)}{P(d)}$$
- **Prior $P(h)$:** Your initial belief in a hypothesis before seeing data.
- **Likelihood $P(d|h)$:** The probability of seeing this specific data if your hypothesis were true.
- **Evidence $P(d)$:** The total probability of seeing the data under all possible hypotheses.
- **Posterior $P(h|d)$:** Your updated belief after observing the data

$$P(h|d) ∝ P(d|h)P(h)$$
## Example: The ”Fair vs. Biased” Coin

- We have a bag with two coins:
	- $h_{fair}$ : 50% chance of Heads.
	- $h_{biased}$: 90% chance of Heads.
- Prior: We pick one at random. $P(h_{fair}) = 0.5$, $P(h_{biased}) = 0.5$.
- Data (d): We flip the coin once and get Heads. Likelihoods: $P(Heads|h_{fair}) = 0.5$ $P(Heads|h_{biased}) = 0.9$

1. Calculate Total Probability (Evidence) P(d): 
   $$P(d) = P(d|hf)P(hf) + P(d|hb)P(hb)$$
   
   $$P(d) = (0.5×0.5)+(0.9×0.5) = 0.25+0.45 = 0.70$$
1. Calculate Posterior for $h_{biased}$: 
   $$P(hb|d) = \frac{Likelihood × Prior Total}  {Probability (Evidence)} = \frac{0.9 × 0.5}{ 0.70} = \frac{0.45} {0.70} ≈ 0.643$$
2. After seeing one Head, our belief that the coin is biased increased from 50% to 64.3%
3. Repeat the same 3 steps after getting a second head using 0.643 as the new prior, the prior will increase further more to 76.4%

When repeating the same steps, taking our 76.4% belief as the New Prior and observe 3 more flips, 

- New Data (d): Two Heads and one Tail (H,H,T)
- Likelihoods: 
	- Biased: 0.9 × 0.9 × 0.1 = 0.081 
	- Fair: 0.5 × 0.5 × 0.5 = 0.125
- New Posterior: 
  $$P(hbias|d) = \frac{0.081 × 0.764} {(0.081 × 0.764) + (0.125 × 0.236)} ≈ 0.677$$
The single ”Tail” significantly weakened our hypothesis that the coin is biased.
# Part 3: Hypothesis Space

## **Underfitting vs. Overfitting** 

**The "Hypothesis Space" (H)** represents the pool of possible models the algorithm can choose from.

- **Underfitting:** Happens when H is too weak or simple. For example, trying to fit a straight line to complex curved data.
- **Overfitting:** Happens when H is too complex. For example, using a 20th-degree polynomial for simple linear data. The model memorizes the noise instead of learning the actual signal.

### **The Matching Principle** 

- A small dataset with a highly complex hypothesis leads to overfitting.
- A huge dataset with a simple hypothesis leads to underfitting.
- Successful learning requires matching the complexity of the Hypothesis Space to the volume and nature of the dataset.

### **Inductive Bias**
Because of Hume's Problem, algorithms must use an "Inductive Bias" to prefer certain hypotheses over others.

- A common bias is **Occam's Razor**, which states we should prefer the simplest hypothesis that successfully explains the data. In Bayesian terms, simpler hypotheses are assigned a higher Prior probability $P(h)$.

# Summary

| Concept          | Role in Learning                                                      |
| ---------------- | --------------------------------------------------------------------- |
| Induction        | Generalizing from samples to populations.                             |
| Hume’s Problem   | Pure induction is logically impossible.                               |
| Bayes’ Theorem   | Provides a mathematical way to update beliefs.                        |
| Hypothesis Space | The ”search area” for the learning algorithm.                         |
| Priors           | The ”initial guess” that solves Hume’s problem.                       |
| Occam's Razor    | Prefering the simplest hypothesis that successfully explains the data |


