---
{"dg-publish":true,"permalink":"/3rd-year/supervised/before-mid/lecture-2-naive-bayes-feature-reduction/","dg-note-properties":{}}
---


### 1. Gradient Descent Recap

The lecture briefly re-introduces Gradient Descent as a method to find the local optima of a differentiable function. The algorithm takes steps in the direction of the negative gradient (the direction of the greatest decrease) to reduce the function's value. The update rule is defined as:

$$x_{t+1} = x_t - \gamma_t \nabla f(x_t)$$

Here, $\gamma_t$ represents the step size, which is also commonly known as the learning rate.

---

### 2. The Naïve Bayes Classifier

Naïve Bayes is a classification algorithm rooted in probability, specifically Bayes' Theorem.

The core formula is:

$$P(A|B) = \frac{P(B|A) \cdot P(A)}{P(B)}$$

- **Posterior ($P(A|B)$):** The probability of $A$ being true given that $B$ is true.
    
- **Likelihood ($P(B|A)$):** The probability of observing $B$ given that $A$ is true.
    
- **Prior ($P(A)$):** Our initial knowledge or the base probability of $A$ being true.
    
- **Marginalization ($P(B)$):** The overall probability of $B$ being true.

more on it [[3rd Year/Supervised/Expanded Explanations/Lecture 2 Examples#More On Naive Bayes\|here]]
#### Naïve Bayes on Categorical Data

When dealing with categorical features (like weather conditions), the algorithm calculates probabilities based on the frequency of occurrences in the training data.

- **Example:** The lecture uses a "Play Tennis" dataset where weather conditions (Outlook, Temperature, Humidity, Wind) dictate whether a game is played.
    
- To predict a new day, the algorithm multiplies the individual likelihoods of each feature occurring for a specific class (e.g., `OK_Play`) by the prior probability of that class, then compares it to the alternative (`NO_Play`).

#### Naïve Bayes on Numerical Data

When your features are continuous numbers (like Age or test scores) rather than categories, you cannot simply count frequencies. Instead, Naïve Bayes assumes the data follows a Normal (Gaussian) distribution.

![Gaussian Normal probability distribution curve, AI generated](https://encrypted-tbn0.gstatic.com/licensed-image?q=tbn:ANd9GcROs-PBOMVheg1IGkD0LR6g_R7COcSk-shNjyX_6IewYfNWDtflp_xfZVkeVGE8zOftsUlmeVnFWB2sKTPsbAnLCuN9JCc_zQfq7ZtTEErcUEVual8)


The probability density function used is:

$$f(x) = \frac{1}{\sqrt{2\pi\sigma^2}}e^{-\frac{(x-\mu)^2}{2\sigma^2}}$$

- **Example:** The lecture uses numerical PSA levels and Age to classify a patient as "Cancer" or "Healthy". The algorithm calculates the mean ($\mu$) and standard deviation ($\sigma$) for each class's features, plugs the new patient's values into the formula to get the likelihoods, and multiplies them by the prior probabilities to find the posterior.

Numerical Examples on Naive Bayes and More, [[3rd Year/Supervised/Expanded Explanations/Lecture 2 Examples\|here]]

---

### 3. Feature Selection vs. Feature Reduction

As datasets grow, using every single feature becomes inefficient. The lecture highlights two ways to handle this:

- **Feature Selection:** You keep a subset of the original features and discard the rest. Techniques include Supervised methods (Filters, Wrappers, Embedded) and Unsupervised methods.
    
- **Feature Reduction:** All original features are used, but they are transformed into entirely new features that are linear combinations of the originals.
    

**Why reduce dimensionality?**

- Reduces complexity and storage space.
    
- Decreases computation time and speeds up algorithm training.
    
- Improves model accuracy by removing noise, redundant features, and misleading data.
    
- Helps visualize data faster.
    

---

### 4. Principal Component Analysis (PCA)

PCA is a powerful feature reduction technique. It condenses data while retaining as much variance as possible through the following steps:

1. **Standardize** the continuous initial variables.
    
2. **Compute the covariance matrix** to identify how variables correlate with one another.
    
3. **Compute the eigenvectors and eigenvalues** of the covariance matrix. The eigenvector corresponding to the highest eigenvalue is the "Principal Component".
    
4. **Create a feature vector** to decide which components to keep (usually dropping those with very small eigenvalues).
    
5. **Recast the data** by projecting the original points onto these new principal component axes.