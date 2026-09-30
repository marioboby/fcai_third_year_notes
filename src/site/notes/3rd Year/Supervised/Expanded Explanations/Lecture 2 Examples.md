---
{"dg-publish":true,"permalink":"/3rd-year/supervised/expanded-explanations/lecture-2-examples/","dg-note-properties":{}}
---


[[3rd Year/Supervised/Expanded Explanations/Lecture 2 Examples#1. Categorical Naïve Bayes (Play Tennis Dataset)\|#1. Categorical Naïve Bayes (Play Tennis Dataset)]]
[[3rd Year/Supervised/Expanded Explanations/Lecture 2 Examples#2. Gaussian Naïve Bayes (Numerical Data)\|#2. Gaussian Naïve Bayes (Numerical Data)]]
[[3rd Year/Supervised/Expanded Explanations/Lecture 2 Examples#3. Principal Component Analysis (PCA)\|#3. Principal Component Analysis (PCA)]]
[[3rd Year/Supervised/Expanded Explanations/Lecture 2 Examples#More On Naive Bayes\|#More On Naive Bayes]]
### 1. Categorical Naïve Bayes (Play Tennis Dataset)

**The Scenario:**

We have historical data on whether a tennis game was played (`OK_Play` = 9 days, `NO_Play` = 5 days, Total = 14 days).

![Pasted image 20260330221457.png](/img/user/imgs/Pasted%20image%2020260330221457.png)

We need to classify a new, unseen day ($Z$) with the following conditions:

- **Outlook:** Sunny
    
- **Temperature:** Cool
    
- **Humidity:** High
    
- **Wind:** Strong
    

**Step 1: Calculate the Probabilities for `OK_Play`**

We multiply the likelihood of each feature occurring when the game _was_ played by the prior probability of playing at all.

- $P(Sunny|OK\_Play) = 2/9$
    
- $P(Cool|OK\_Play) = 3/9$
    
- $P(High|OK\_Play) = 3/9$
    
- $P(Strong|OK\_Play) = 3/9$
    
- **Prior:** $P(OK\_Play) = 9/14$
    

![Pasted image 20260330221512.png](/img/user/imgs/Pasted%20image%2020260330221512.png)

**Equation:**

$$P(OK\_Play|Z) \propto \left(\frac{2}{9} \times \frac{3}{9} \times \frac{3}{9} \times \frac{3}{9}\right) \times \frac{9}{14} = \mathbf{0.0053}$$

**Step 2: Calculate the Probabilities for `NO_Play`**

We do the same for when the game was _not_ played.

- $P(Sunny|NO\_Play) = 3/5$
    
- $P(Cool|NO\_Play) = 1/5$
    
- $P(High|NO\_Play) = 4/5$
    
- $P(Strong|NO\_Play) = 3/5$
    
- **Prior:** $P(NO\_Play) = 5/14$
    

**Equation:**

$$P(NO\_Play|Z) \propto \left(\frac{3}{5} \times \frac{1}{5} \times \frac{4}{5} \times \frac{3}{5}\right) \times \frac{5}{14} = \mathbf{0.0206}$$

**Result:** Since 0.0206 is greater than 0.0053, the model classifies this new day as **NO_Play**.

---

### 2. Gaussian Naïve Bayes (Numerical Data)

**The Scenario:**

![Pasted image 20260330222240.png](/img/user/imgs/Pasted%20image%2020260330222240.png)

**Recall**: the marginalization rule "total probability", is when you calculate the probability of some variable, by summing over the joint distribution of all possible values of another variable

$$P(a_i) = \sum_{j=1}^{N_{\text{b-classes}}} P(a_i, b_j)$$


Instead of counts, we are dealing with continuous numbers (PSA level and Age) to classify a patient as `Healthy` or `Cancer`. Because we assume the features follow a normal distribution, we plug our values into the Gaussian Probability Density Function:

$$f(x) = \frac{1}{\sqrt{2\pi\sigma^2}}e^{-\frac{(x-\mu)^2}{2\sigma^2}}$$

- **Test Patient:** PSA = 2.6, Age = 70
    
- **Priors:** $P(Healthy) = 0.5$, $P(Cancer) = 0.5$
    

**Step 1: Calculate Likelihoods for `Healthy`**

Using the mean ($\mu$) and standard deviation ($\sigma$) from the `Healthy` training data:

- **PSA Likelihood:** $\mu = 1.5, \sigma = 0.61 \rightarrow P(PSA=2.6|Healthy) = 0.13$
    
- **Age Likelihood:** $\mu = 66, \sigma = 4.58 \rightarrow P(Age=70|Healthy) = 0.059$
    

**Posterior Numerator (Healthy):**

$$P(Healthy) \times P(PSA|Healthy) \times P(Age|Healthy)$$

$$0.5 \times 0.13 \times 0.059 = \mathbf{0.004}$$

**Step 2: Calculate Likelihoods for `Cancer`**

Using the mean and standard deviation from the `Cancer` training data:

- **PSA Likelihood:** $\mu = 2.8, \sigma = 0.82 \rightarrow P(PSA=2.6|Cancer) = 0.47$
    
- **Age Likelihood:** $\mu = 67, \sigma = 6.45 \rightarrow P(Age=70|Cancer) = 0.055$
    

**Posterior Numerator (Cancer):**

$$P(Cancer) \times P(PSA|Cancer) \times P(Age|Cancer)$$

$$0.5 \times 0.47 \times 0.055 = \mathbf{0.013}$$

**Result:** Since 0.013 is greater than 0.004, the model classifies the patient as having **Cancer**.

---

### 3. Principal Component Analysis (PCA)

**The Scenario:**

We want to reduce the dimensionality of a 2D dataset while preserving the most important variance.

**Data Points:** (2, 1), (3, 5), (4, 3), (5, 6), (6, 7), (7, 8)

**Step 1: Calculate the Mean Vector ($\mu$)**

Find the average of all $X$ values and all $Y$ values.

$$\mu = \begin{bmatrix} (2+3+4+5+6+7)/6 \\ (1+5+3+6+7+8)/6 \end{bmatrix} = \begin{bmatrix} 4.5 \\ 5 \end{bmatrix}$$

**Step 2: Subtract the Mean**

Center the data around the origin by subtracting the mean from every point.

- $P_1: (2 - 4.5, 1 - 5) = (-2.5, -4)$
    
- _...repeat for all points..._
    
- $P_6: (7 - 4.5, 8 - 5) = (2.5, 3)$
    

**Step 3: Calculate the Covariance Matrix**

Compute $(x_i - \mu)(x_i - \mu)^T$ for each point, sum them up, and divide by the number of points ($n=6$) to find how the variables vary together.

$$Covariance Matrix = \begin{bmatrix} 2.92 & 3.67 \\ 3.67 & 5.67 \end{bmatrix}$$

**Step 4: Find Eigenvalues and Eigenvectors**

Solve the characteristic equation $|M - \lambda I| = 0$ (where $M$ is the covariance matrix) to find the eigenvalues ($\lambda$).

$$\lambda^2 - 8.59\lambda + 3.09 = 0$$

- **Eigenvalues:** $\lambda_1 = 8.22$ (Principal), $\lambda_2 = 0.38$ (We drop this small one to reduce dimensions).
    

Next, we solve $MX = \lambda X$ using the dominant eigenvalue ($\lambda_1 = 8.22$) to get the principal Eigenvector.

$$Principal Component = \begin{bmatrix} 2.55 \\ 3.67 \end{bmatrix}$$

**Step 5: Project the Data**

Finally, we recast the original data points onto this new 1D principal component line by taking the dot product.

- **Projecting Point 1 (2, 1):** $(2 \times 2.55) + (1 \times 3.67) = \mathbf{8.77}$
    
- **Projecting Point 2 (3, 5):** $(3 \times 2.55) + (5 \times 3.67) = \mathbf{26.00}$
    
- _(This creates your new, 1-dimensional reduced dataset!)_
  
---
### More On Naive Bayes

#### Why Naive Bayes

Its main strength is requiring less training data, while its primary weakness is the "naive" assumption that all features are independent, which rarely holds true in real-world scenarios. 

**Pros of Naive Bayes:**

- **Fast and Scalable:** Highly efficient in training and prediction, suitable for real-time applications and large datasets.
- **Performance with Less Data:** Performs well with small training sets and high-dimensional data (e.g., text categorization).
- **Simplicity:** Easy to implement and understand.
- **Handles Multi-class:** Effective for multi-class prediction tasks.
- **Robust to Irrelevant Features:** Not sensitive to irrelevant features in the dataset. 

**Cons of Naive Bayes:**

- **Independence Assumption:** Assumes features are independent, which rarely holds true. If features are correlated, performance can degrade.
- **Zero-Frequency Problem:** If a categorical variable has a category in the test set that was not observed in the training set, the model will assign a zero probability.
- **Not Ideal for Complex Relationships:** Struggles to capture complex relationships or dependencies between features.
- **Data Scarcity:** Requires sufficient data for each class to accurately estimate probabilities.

**Common Use Cases:**

- Spam filtering
- Sentiment analysis
- Document/Article classification
- Real-time prediction systems

---

$$A \equiv \quad y=C_i$$

$$B \equiv \quad x=x_i$$

$$P(y=C_i|x=x_j) = \frac{P(x=x_j|y=C_i) \; P(y=C_i)}{P(x=x_j)}$$
$$\hat{y}_j = C_{\text{argmax}_i \; P(y=C_i|x=x_j)}$$

$$P(y=C_i|x_{1j}, x_{2j}, x_{3j}, \dots, x_{dj}) = \frac{P(x_{1j}, x_{2j}, x_{3j}, \dots, x_{dj}|y=C_i) \; P(y=C_i)}{P(x_{1j}, x_{2j}, x_{3j}, \dots, x_{dj})}$$

- Naive Assumption: We assume that all features are mutually exclusive / uncorrelated
- **Recall**: the marginalization rule "total probability", is when you calculate the probability of some variable, by summing over the joint distribution of all possible values of another variable

$$P(a_i) = \sum_{j=1}^{N_{\text{b-classes}}} P(a_i, b_j)$$


$$P(A|B) = P(A)$$
Therefore:

$$P(y=C_i|x_{1j}, x_{2j}, x_{3j}, \dots, x_{dj}) = \frac{P(x_{1j}|y=C_i)\, P(x_{2j}|y=C_i) \, P(x_{3j}|y=C_i) \dots \, P(x_{dj}|y=C_i) \; P(y=C_i)}{P(x_{1j})\, P(x_{2j}) \, P(x_{3j}) \dots \, P(x_{dj})}$$
#### The "Naivety" 

Calculating the exact joint probability of many interacting variables " for calculating $P(B)$ , and $P(B|A)$ " requires exponential computational complexity ($O(2^n)$). The **Naïve Bayes classifier** drastically simplifies this by making a strong assumption: all features are conditionally independent of each other, given the class label.

- Mathematically, it changes the complex joint probability into a simple product of individual probabilities: $P(y | x_1, \dots, x_n) \propto P(y) \prod_{i=1}^n P(x_i | y)$
    
- While this assumption is "naïve" because real-world variables are almost always correlated, the algorithm is famously robust. It often achieves high accuracy in classification tasks (like spam filtering) with exceptionally low computational overhead.

When making a decision between two classes ($w_1$ and $w_2$) given an observation $x$, the rule is to choose $w_1$ if $P(w_1|x) > P(w_2|x)$.

#### Normalization

The **Marginalization Rule** (often called the Law of Total Probability) is the mathematical engine behind the denominator in Bayes' Theorem. In the context of machine learning, it is what allows us to convert raw, abstract scores into clean, readable percentages that sum up to **100%** (or **1.0**).

![Bayes theorem with marginal probability, AI generated](https://encrypted-tbn1.gstatic.com/licensed-image?q=tbn:ANd9GcQnqNL2rK_GMxPiJpA_rBC2JVhNc-O3Tc2MFVj_ETW1-JUXOUnDUywBVkwqHNsrlCmHx9YwhinB8YwOWHDb4dRByyw7YV5lLOW2vgFdYjJ8yS1C_oA)

Here is Bayes' formula:

$$P(Class|Features) = \frac{P(Features|Class) \times P(Class)}{P(Features)}$$

The denominator, $P(Features)$, is the marginal probability. It represents the total probability of seeing this specific exact combination of features across _all_ possible classes in your dataset.

### How to Calculate the Marginal Probability with Multiple Features

When you have multiple features (like Temperature, Humidity, Wind, or Age, PSA), calculating the exact probability of that specific combination occurring in the wild is extremely difficult.

Instead, the Marginalization Rule lets us calculate it by summing up the "numerators" of all our possible classes.

The formula for the marginal probability becomes:

$$P(Features) = \sum_{k=1}^{n} P(Features|Class_k) \times P(Class_k)$$

### The Normalization Trick (Step-by-Step)

Because the marginal probability (the denominator) is the exact same for every class you are evaluating, you do not actually need to compute it right away. You can just calculate the numerators, sum them up, and use that sum to normalize your results.

Let's use the numerical **Healthy vs. Cancer** example from the lecture slides to see this in action!

**1. Calculate the Numerator for Class A (Healthy)**

We multiply the Prior by the Likelihoods of the features (PSA = 2.6):

- Numerator (Healthy) = $P(Healthy) \times P(PSA=2.6|Healthy)$
    
- Numerator (Healthy) = $0.5 \times 0.13 = \mathbf{0.065}$
    

**2. Calculate the Numerator for Class B (Cancer)**

- Numerator (Cancer) = $P(Cancer) \times P(PSA=2.6|Cancer)$
    
- Numerator (Cancer) = $0.5 \times 0.47 = \mathbf{0.235}$
    

**3. Apply the Marginalization Rule (Find the Denominator)**

To find the total marginal probability of someone having a PSA of 2.6, we simply add the numerators together:

- Marginal Probability = $0.065 + 0.235 = \mathbf{0.300}$
    

**4. Normalize to get the Final Probabilities**

Now, divide each class's numerator by the marginal probability to get the true, normalized posterior probabilities:

- **Final Probability (Healthy):** $0.065 / 0.300 = \mathbf{0.21}$ (or **21%**)
    
- **Final Probability (Cancer):** $0.235 / 0.300 = \mathbf{0.783}$ (rounded to **79%** in the slides)
    

By dividing by the marginalized sum, you force the probabilities to scale proportionally so that $0.21 + 0.79 = \mathbf{1.0}$. This tells your model not just _which_ class is more likely, but exactly _how confident_ it should be in that prediction!

---

To use that exact marginalization rule for multiple features, we have to rely on the "Naïve" part of the Naïve Bayes algorithm: **Conditional Independence**.

If you have a dataset with multiple features (like `PSA` and `Age`, or `Outlook`, `Temperature`, `Humidity`, and `Wind`), trying to find the true, real-world probability of that exact combination happening all at once—$P(Features)$—is nearly impossible. You would need a massive dataset to find enough people who are exactly 70 years old _and_ have a PSA of exactly 2.6 just to count them.

To get around this, Naïve Bayes assumes that every feature is completely independent of the others, _as long as you already know the class_.

Here is how that theoretical breakdown works mathematically.

### 1. Breaking Down the Likelihood

Because we assume the features are independent, the joint probability of all features given a specific class—$P(Features|Class_k)$—simply becomes the product of their individual probabilities:

$$P(Features|Class_k) = P(Feature_1|Class_k) \times P(Feature_2|Class_k) \times \dots \times P(Feature_m|Class_k)$$

### 2. Expanding the Marginalization Rule

Now, we take that expanded, multiplied likelihood and plug it back into the formula you provided. To find the total marginal probability of seeing this specific combination of features across your entire dataset, you calculate this product for _every single class_ and add them all together:

$$P(Features) = \sum_{k=1}^{n} \Big( [P(Feature_1|Class_k) \times \dots \times P(Feature_m|Class_k)] \times P(Class_k) \Big)$$

### 3. A Theoretical Example

Let's apply this directly to the two-feature numerical example from your lecture (`PSA = 2.6` and `Age = 70`) to see the full equation in action.

We have two classes ($n=2$): `Healthy` and `Cancer`.

**Part A: Calculate the inner term for Class 1 (Healthy)**

$$\text{Term}_1 = P(PSA=2.6|Healthy) \times P(Age=70|Healthy) \times P(Healthy)$$

**Part B: Calculate the inner term for Class 2 (Cancer)**

$$\text{Term}_2 = P(PSA=2.6|Cancer) \times P(Age=70|Cancer) \times P(Cancer)$$

**Part C: Sum them up for the Marginal Probability**

$$P(PSA=2.6 \text{ and } Age=70) = \text{Term}_1 + \text{Term}_2$$

Once you have that final summed number, you have successfully calculated your denominator! You then divide $\text{Term}_1$ by this denominator to get the final normalized percentage for `Healthy`, and divide $\text{Term}_2$ by this denominator to get the final normalized percentage for `Cancer`.

بالبلدي لو عايز زتونة الحل, لو طلب منك تعمل normalization, اجمع البسط بتاع كل الاحتمالات اللي حسبتها "ال likelihoods في ال priors بتاعت كل كلاس, سواء بقي كانت one or more than one feature"
