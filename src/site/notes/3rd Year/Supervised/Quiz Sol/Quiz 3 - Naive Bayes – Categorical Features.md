---
{"dg-publish":true,"permalink":"/3rd-year/supervised/quiz-sol/quiz-3-naive-bayes-categorical-features/","dg-note-properties":{}}
---


# Using Naive Bayes, classify a new instance X = (Outlook=Sunny, Humidity=High) as Play=Yes or Play=No.

Training Data (8 samples):

| #   | Outlook  | Humidity | Play |
| --- | -------- | -------- | ---- |
| 1   | Sunny    | High     | No   |
| 2   | Sunny    | Normal   | Yes  |
| 3   | Overcast | High     | Yes  |
| 4   | Rain     | High     | Yes  |
| 5   | Rain     | Normal   | Yes  |
| 6   | Sunny    | High     | No   |
| 7   | Overcast | Normal   | Yes  |
| 8   | Sunny    | Normal   | Yes  |

Compute:
1. P(Yes), P(No)
2. P(Sunny|Yes), P(Sunny|No), P(High|Yes), P(High|No)
3. P(Yes|Sunny,High) vs P(No|Sunny,High) using Bayes rule
4. The final classification.

## Sol

To classify the new instance $X = (\text{Outlook=Sunny}, \text{Humidity=High})$ using the Naive Bayes algorithm, we need to calculate the probabilities step-by-step based on the provided training data of $8$ samples.

### 1. Prior Probabilities

First, we determine the overall probability of each class ("Yes" and "No") occurring in the entire dataset.

- Total samples = $8$
    
- Total "Yes" samples = $6$ (Rows 2, 3, 4, 5, 7, 8)
    
- Total "No" samples = $2$ (Rows 1, 6)
    

$$P(\text{Yes}) = \frac{6}{8} = 0.75$$

$$P(\text{No}) = \frac{2}{8} = 0.25$$

### 2. Conditional Probabilities

Next, we calculate the probability of the specific features ($\text{Sunny}$ and $\text{High}$) given each class.

**For Class = Yes ($6$ total samples):**

- Samples where Outlook is Sunny AND Play is Yes = $2$ (Rows 2, 8)
    
- Samples where Humidity is High AND Play is Yes = $2$ (Rows 3, 4)
    

$$P(\text{Sunny}|\text{Yes}) = \frac{2}{6} \approx 0.333$$

$$P(\text{High}|\text{Yes}) = \frac{2}{6} \approx 0.333$$

**For Class = No ($2$ total samples):**

- Samples where Outlook is Sunny AND Play is No = $2$ (Rows 1, 6)
    
- Samples where Humidity is High AND Play is No = $2$ (Rows 1, 6)
    

$$P(\text{Sunny}|\text{No}) = \frac{2}{2} = 1.0$$

$$P(\text{High}|\text{No}) = \frac{2}{2} = 1.0$$

### 3. Compute Posteriors (Using Bayes' Rule)

In Naive Bayes, we calculate the proportional posterior probability (the numerator of Bayes' theorem) for each class because the denominator—the probability of the evidence $P(\text{Sunny}, \text{High})$—is constant and can be ignored for comparison.

**For Play = Yes:**

$$P(\text{Yes}|\text{Sunny}, \text{High}) \propto P(\text{Yes}) \times P(\text{Sunny}|\text{Yes}) \times P(\text{High}|\text{Yes})$$

$$P(\text{Yes}|\text{Sunny}, \text{High}) \propto 0.75 \times 0.333 \times 0.333 = 0.0833$$

_(Or as fractions: $\frac{3}{4} \times \frac{1}{3} \times \frac{1}{3} = \frac{1}{12}$)_

**For Play = No:**

$$P(\text{No}|\text{Sunny}, \text{High}) \propto P(\text{No}) \times P(\text{Sunny}|\text{No}) \times P(\text{High}|\text{No})$$

$$P(\text{No}|\text{Sunny}, \text{High}) \propto 0.25 \times 1.0 \times 1.0 = 0.25$$

_(Or as fractions: $\frac{1}{4} \times 1 \times 1 = \frac{1}{4}$)_

### 4. Final Classification

To classify the new instance, we compare the final computed proportional probabilities:

- $P(\text{No}|\text{Sunny}, \text{High}) \propto 0.25$
    
- $P(\text{Yes}|\text{Sunny}, \text{High}) \propto 0.0833$
    

Since $0.25 > 0.0833$, the probability of "No" is strictly higher than "Yes".

Therefore, the final classification for $X = (\text{Outlook=Sunny}, \text{Humidity=High})$ is **Play = No**.