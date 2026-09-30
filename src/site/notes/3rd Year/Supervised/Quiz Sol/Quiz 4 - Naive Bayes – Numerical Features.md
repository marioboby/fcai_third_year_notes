---
{"dg-publish":true,"permalink":"/3rd-year/supervised/quiz-sol/quiz-4-naive-bayes-numerical-features/","dg-note-properties":{}}
---


# Using Gaussian Naive Bayes, classify a new point x = 5.5 into Class A or Class B.

Training Data:
- Class A values: {2, 3, 4, 5}
- Class B values: {6, 7, 8, 9}
(Equal priors: P(A) = P(B) = 0.5)

Steps:
1. Compute mean and variance for each class.
2. Use the Gaussian PDF
3. Compute P(A|x) vs P(B|x) and classify.
   
Show all calculations.

## Sol

To classify the new point $x = 5.5$ using Gaussian Naive Bayes, we model the continuous numerical features of each class using a Gaussian (Normal) distribution.

### Step 1: Compute Mean and Variance for Each Class

We need the mean ($\mu$) and variance ($\sigma^2$) for both Class A and Class B. We will use the population variance (Maximum Likelihood Estimate, dividing by $n$), which is standard for machine learning implementations of Naive Bayes.

**For Class A $\{2, 3, 4, 5\}$:**

- **Mean ($\mu_A$):**
    
    $$\mu_A = \frac{2 + 3 + 4 + 5}{4} = \frac{14}{4} = 3.5$$
    
- **Variance ($\sigma_A^2$):**
    
    $$\sigma_A^2 = \frac{(2-3.5)^2 + (3-3.5)^2 + (4-3.5)^2 + (5-3.5)^2}{4}$$
    
    $$\sigma_A^2 = \frac{(-1.5)^2 + (-0.5)^2 + (0.5)^2 + (1.5)^2}{4}$$
    
    $$\sigma_A^2 = \frac{2.25 + 0.25 + 0.25 + 2.25}{4} = \frac{5}{4} = 1.25$$
    

**For Class B $\{6, 7, 8, 9\}$:**

- **Mean ($\mu_B$):**
    
    $$\mu_B = \frac{6 + 7 + 8 + 9}{4} = \frac{30}{4} = 7.5$$
    
- **Variance ($\sigma_B^2$):**
    
    $$\sigma_B^2 = \frac{(6-7.5)^2 + (7-7.5)^2 + (8-7.5)^2 + (9-7.5)^2}{4}$$
    
    $$\sigma_B^2 = \frac{(-1.5)^2 + (-0.5)^2 + (0.5)^2 + (1.5)^2}{4} = \frac{5}{4} = 1.25$$
    

### Step 2: Use the Gaussian PDF

The Probability Density Function (PDF) for a Gaussian distribution is:

$$P(x | C) = \frac{1}{\sqrt{2\pi\sigma_C^2}} \exp\left(-\frac{(x - \mu_C)^2}{2\sigma_C^2}\right)$$

Now, let's calculate the likelihood of $x = 5.5$ for both classes.

**Likelihood for Class A $P(x=5.5 | A)$:**

- $x - \mu_A = 5.5 - 3.5 = 2.0$
    
- Exponent part: $-\frac{(2.0)^2}{2 \times 1.25} = -\frac{4}{2.5} = -1.6$
    
- $P(x=5.5 | A) = \frac{1}{\sqrt{2 \pi (1.25)}} e^{-1.6}$
    
- $P(x=5.5 | A) \approx \frac{1}{\sqrt{7.854}} \times 0.2019 \approx 0.3568 \times 0.2019 \approx 0.0720$
    

**Likelihood for Class B $P(x=5.5 | B)$:**

- $x - \mu_B = 5.5 - 7.5 = -2.0$
    
- Exponent part: $-\frac{(-2.0)^2}{2 \times 1.25} = -\frac{4}{2.5} = -1.6$
    
- $P(x=5.5 | B) = \frac{1}{\sqrt{2 \pi (1.25)}} e^{-1.6}$
    
- $P(x=5.5 | B) \approx 0.3568 \times 0.2019 \approx 0.0720$
    

### Step 3: Compute Posteriors and Classify

Using Bayes' Theorem, the posterior probability is proportional to the Prior $\times$ Likelihood. We are given equal priors: $P(A) = 0.5$ and $P(B) = 0.5$.

**Posterior for Class A:**

$$P(A | x=5.5) \propto P(A) \times P(x=5.5 | A)$$

$$P(A | x=5.5) \propto 0.5 \times 0.0720 = 0.0360$$

**Posterior for Class B:**

$$P(B | x=5.5) \propto P(B) \times P(x=5.5 | B)$$

$$P(B | x=5.5) \propto 0.5 \times 0.0720 = 0.0360$$

### Final Classification

Because $P(A | x=5.5) = P(B | x=5.5)$, the probabilities are exactly tied ($50\%$ chance for each class).

> Conceptually, this makes perfect sense: the point $x = 5.5$ is perfectly equidistant from the centers of both Class A ($\mu = 3.5$) and Class B ($\mu = 7.5$), and both classes have the exact same spread (variance = $1.25$). Therefore, $x = 5.5$ lies exactly on the **decision boundary**, and a definitive classification cannot be made without a tie-breaking rule.