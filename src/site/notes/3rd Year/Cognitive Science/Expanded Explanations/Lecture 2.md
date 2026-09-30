---
{"dg-publish":true,"permalink":"/3rd-year/cognitive-science/expanded-explanations/lecture-2/","dg-note-properties":{}}
---

# Hume's Problem meaning

To really understand Hume’s Problem of Induction, it helps to look at it through the lens of formal logic and proofs.
{ #ac021c}


In propositional or first-order logic, you rely on **deduction**. If your premises are true, your conclusion is mathematically guaranteed to be true. For example, if $A \rightarrow B$ and $A$ is true, then $B$ is absolutely true (Modus Ponens). There is no uncertainty.

**Induction** works in the exact opposite direction. It takes specific, limited observations and tries to write a universal rule.

Here is the core of Hume’s problem: **How do you mathematically or logically prove that induction is a valid way to find the truth?**

If you try to write a proof to justify induction, your argument will naturally look something like this:

1. "In the past, when I observed patterns, those patterns continued into the future."
2. "Therefore, observing patterns now means they will continue into the future."

Hume pointed out a fatal logical flaw here: **You are using induction to prove induction.** You are assuming that because induction worked in the past, it will work in the future. In the realm of formal proofs, this is a circular argument. It’s like trying to define a word by using the same word in the definition. Because of this, Hume concluded that induction has absolutely no rational or logical justification. We just use it because it’s a biological habit.

### Why This Matters for Machine Learning

If you are building a neural network or training a machine learning model, you are essentially building an "induction machine." The model looks at a limited training dataset $\mathcal{D}$ (the past) and tries to formulate a generalized hypothesis $h$ to predict unseen test data (the future).

Hume’s problem proves that **data alone is never enough to find the correct rule.** No matter how large your training set is, there are always an infinite number of completely different hypotheses that could perfectly fit the data you have seen so far, but wildly diverge on the very next data point.

Because we cannot rely purely on the data (due to Hume's problem), we are forced to "cheat" a little bit to make learning possible. We have to inject our own external assumptions into the algorithm before it even looks at the data.

This is why Bayesian theory is so crucial here. In Bayesian learning, you don't just let the data speak for itself. You introduce a **Prior** $P(h)$—a built-in bias that tells the algorithm, "Before you even look at the training data, I want you to favor simpler rules over highly complex, convoluted ones." Without that prior assumption (Inductive Bias), the algorithm would be paralyzed by Hume's problem, unable to choose between an infinite number of rules that all fit the training data.


# why are simpler hypotheses assigned a higher Prior probability

Assigning a higher prior probability to simpler hypotheses—a concept often referred to as an **Inductive Bias** toward simplicity—is driven by a mix of philosophy, practical machine learning, and probability theory.

Here is why cognitive systems and machine learning algorithms are designed this way:

### 1. The Philosophical Foundation: Occam’s Razor

Occam's Razor is a historical problem-solving principle that suggests: "Entities should not be multiplied beyond necessity." In practical terms, if you have two rules that both explain the observations equally well, the simpler one is statistically more likely to be the true underlying mechanism. In Bayesian inference, we encode this centuries-old intuition directly into the math. By giving simpler rules a higher Prior $P(h)$, we give them a mathematical "head start."

### 2. Preventing Overfitting (The Machine Learning Reason)

Real-world data is rarely perfect; it almost always contains "noise" (random errors, measurement mistakes, or natural fluctuations).

If a hypothesis is incredibly complex—like a neural network with billions of parameters or a 50th-degree polynomial equation—it has the flexibility to zig-zag and contort itself to pass perfectly through every single data point in your training set, including the random noise. This is called **overfitting**.

![underfitting vs overfitting graph, AI generated](https://encrypted-tbn1.gstatic.com/licensed-image?q=tbn:ANd9GcQfQnM2h5NSYVZtkvCvcc8ok5FMOdWqWvJvy1C8SCeCA8Qsd429nvJ4L9mIIcV1EltukwNpEQGlQbQlQ4teVNMnCpoq_YFoJsP_vsaKYTjpfgWSfUs)


By assigning a low prior to complex hypotheses, we build skepticism into the algorithm. We essentially tell the model: _"You can only use a highly complex rule if the data proves it is absolutely necessary."_ The evidence $P(d|h)$ must be overwhelmingly strong to overcome a low prior. This constraint forces the model to learn the actual underlying _signal_ (the true pattern) rather than just memorizing the _noise_.

### 3. The Mathematics of the Hypothesis Space

Think about the sheer size of the Hypothesis Space (the pool of all possible rules). For any given dataset, there is only a tiny handful of simple hypotheses (e.g., a straight line or a simple curve). However, there is an _infinite_ number of wildly erratic, highly complex hypotheses that could also fit the data.

In probability theory, the sum of all prior probabilities across the entire Hypothesis Space must equal exactly 1 (100%). If you tried to assign an equal probability to every possible hypothesis, the probability of any single rule would be infinitely close to zero, and the model couldn't learn anything. To make the math function, we have to concentrate the bulk of the probability mass on the smaller pool of simple hypotheses, leaving the remaining fractions of a percent spread out across the infinite sea of complex ones.

### An Intuitive Example

Imagine you plot five points on a graph that generally trend upwards, but they don't form a perfectly straight line.

- **Hypothesis A (Simple):** A simple straight line $y = mx + c$. It doesn't hit every point perfectly, but it captures the obvious upward trend.

- **Hypothesis B (Complex):** A massive equation like $y = 3x^5 - 12x^4 + 2x^3 ...$ that violently snakes up and down just to hit all five points perfectly.

If we relied purely on the data without an inductive bias, the model would choose Hypothesis B, because its training accuracy is 100%. But because Bayesian learning assigns a much higher Prior to Hypothesis A, the final mathematical calculation (the Posterior) correctly identifies the simple straight line as the much more rational, generalizable rule for predicting future points.