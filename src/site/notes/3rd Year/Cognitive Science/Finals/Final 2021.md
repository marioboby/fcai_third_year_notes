---
{"dg-publish":true,"permalink":"/3rd-year/cognitive-science/finals/final-2021/","dg-note-properties":{}}
---


# Q1 

![Pasted image 20260605190030.png](/img/user/imgs/Pasted%20image%2020260605190030.png)
## **1. Given $P(A|B) = 2/3$**

**Answer:** not enough info

**Reasoning:** To compute $P(B|A)$, we need to use Bayes' Theorem:

$$P(B|A) = \frac{P(A|B) \cdot P(B)}{P(A)}$$

We only have $P(A|B)$. We are missing the prior probability of $B$, $P(B)$, and the total probability of $A$, $P(A)$.

**2. Given $P(A|B) = 2/3$ and $P(A|\neg B) = 1/3$**

**Answer:** not enough info

**Reasoning:** Using the expanded form of Bayes' Theorem (incorporating the Law of Total Probability for the denominator):

$$P(B|A) = \frac{P(A|B) \cdot P(B)}{P(A|B) \cdot P(B) + P(A|\neg B) \cdot P(\neg B)}$$

While we now have both conditional probabilities ($P(A|B)$ and $P(A|\neg B)$), we still do not know the base probability of $B$ occurring ($P(B)$) or not occurring ($P(\neg B)$).

**3. Given $P(A|B) = 2/3$, $P(A|\neg B) = 1/3$, and $P(B) = 1/3$**

**Answer:** Yes, we have enough information.

**Computation:**

First, we can determine $P(\neg B)$ since the probabilities must sum to 1:

$$P(\neg B) = 1 - P(B) = 1 - \frac{1}{3} = \frac{2}{3}$$

Next, we calculate the total probability of $A$, $P(A)$, using the Law of Total Probability:

$$P(A) = P(A|B) \cdot P(B) + P(A|\neg B) \cdot P(\neg B)$$

$$P(A) = \left(\frac{2}{3} \cdot \frac{1}{3}\right) + \left(\frac{1}{3} \cdot \frac{2}{3}\right)$$

$$P(A) = \frac{2}{9} + \frac{2}{9} = \frac{4}{9}$$

Finally, we use Bayes' Theorem to compute $P(B|A)$:

$$P(B|A) = \frac{P(A|B) \cdot P(B)}{P(A)}$$

$$P(B|A) = \frac{\frac{2}{9}}{\frac{4}{9}}$$

$$P(B|A) = \frac{2}{4} = \frac{1}{2}$$

**Final Value:** $P(B|A) = \frac{1}{2}$

---

# Q2

![Pasted image 20260605190432.png](/img/user/imgs/Pasted%20image%2020260605190432.png)

### **a) $P(A|B)$**

To find $P(A|B)$, we use Bayes' Theorem. First, we need to calculate the marginal probability of B, $P(B)$, using the Law of Total Probability:

$$P(B) = P(B|A)P(A) + P(B|\sim A)P(\sim A)$$

Given from the diagram:

- $P(A) = 0.3$, which means $P(\sim A) = 1 - 0.3 = 0.7$
    
- $P(B|A) = 0.7$
    
- $P(B|\sim A) = 0.5$
    

Calculate $P(B)$:

$$P(B) = (0.7 \cdot 0.3) + (0.5 \cdot 0.7)$$

$$P(B) = 0.21 + 0.35 = 0.56$$

Now, apply Bayes' Theorem:

$$P(A|B) = \frac{P(B|A)P(A)}{P(B)}$$

$$P(A|B) = \frac{0.7 \cdot 0.3}{0.56} = \frac{0.21}{0.56} = 0.375$$

**Answer:** $P(A|B) = 0.375$ (or $\frac{3}{8}$)

### **b) $P(B|D)$**

This is where the hint comes into play. Look at the probabilities for Node D:

- $P(D|C) = 0.7$
    
- $P(D|\sim C) = 0.7$
    

Because the probability of D is exactly the same regardless of whether C occurs or not, **D is independent of C**. Furthermore, since the only path from the rest of the network to D is through C, D is independent of the entire network.

Because D and B are completely independent events, knowing D tells us nothing new about B. Therefore, the conditional probability $P(B|D)$ is simply equal to the prior probability $P(B)$.

$$P(B|D) = P(B)$$

We already calculated $P(B)$ in part (a).

**Answer:** $P(B|D) = 0.56$

### **c) $P(C|B)$**

و النقطة اللي بعدها اللي هو جايبلك

P(C|B) 

فيها فكرة جامدة ان المفروض دول children بس ليهم نفس ال parent و احنا هنا منعرفش حالته عاملة ازاي ف انت هتضطر تربطهم ببعض بالشكل ده 

$$P(C|B) = P(C|A, B) × P(A|B) + P(C|not A, B) × P(not A|B)$$

وبعدين هتلاقي طالما A, B ظهروا المفروض ان C هتحتاج A فقط وبالتالي هتخليها

$$P(C|B) = P(C|A) × P(A|B) + P(C|not A) × P(not A|B)$$

كريديت ابراهيم رضا

To find $P(C|B)$, we can marginalize over A. Since B and C share A as a common parent (a "fork" structure), they are conditionally independent given A. This allows us to calculate $P(C|B)$ using the probabilities of A given B:

$$P(C|B) = P(C|A)P(A|B) + P(C|\sim A)P(\sim A|B)$$

From part (a), we know:

- $P(A|B) = 0.375$
    
- $P(\sim A|B) = 1 - 0.375 = 0.625$
    

From the diagram, we are given:

- $P(C|A) = 0.2$
    
- $P(C|\sim A) = 0.6$
    

Plug these values into the formula:

$$P(C|B) = (0.2 \cdot 0.375) + (0.6 \cdot 0.625)$$

$$P(C|B) = 0.075 + 0.375 = 0.45$$

**Answer:** $P(C|B) = 0.45$ (or $\frac{9}{20}$)

## Proof of rules (مش مهم الجزئية دي اوي طالما فهمت اللي فوق)

### Unpacking Part (c): Where did $P(A|B)$ come from?

We know that $B$ and $C$ are conditionally independent given $A$. You also know Rule 2: to find a probability, you can marginalize (sum) over the hidden variables.

Let's say we want to find $P(C|B)$. By the standard definition of conditional probability:

$$P(C|B) = \frac{P(B, C)}{P(B)}$$

How do we find the joint probability $P(B, C)$? We use your **Rule 2** and marginalize over the parent node $A$:

$$P(B, C) = \sum_{A} P(A, B, C)$$

$$P(B, C) = P(A, B, C) + P(\sim A, B, C)$$

How do we find $P(A, B, C)$? We use **Rule 1** (the global semantics of the BN):

$$P(A, B, C) = P(A) \cdot P(B|A) \cdot P(C|A)$$

Now, let's plug that back into our equation for $P(C|B)$:

$$P(C|B) = \frac{P(A) \cdot P(B|A) \cdot P(C|A) + P(\sim A) \cdot P(B|\sim A) \cdot P(C|\sim A)}{P(B)}$$

This looks like a mess, but watch what happens if we rearrange the terms. We can split the fraction into two parts:

$$P(C|B) = \left[ \frac{P(A) \cdot P(B|A)}{P(B)} \cdot P(C|A) \right] + \left[ \frac{P(\sim A) \cdot P(B|\sim A)}{P(B)} \cdot P(C|\sim A) \right]$$

Take a close look at that fraction: $\frac{P(A) \cdot P(B|A)}{P(B)}$.

**That is the exact formula for Bayes' Theorem!** (Rule 3).

$$\frac{P(A) \cdot P(B|A)}{P(B)} = P(A|B)$$

So, if we substitute $P(A|B)$ back into the equation, we get the exact shortcut formula I used:

$$P(C|B) = P(A|B)P(C|A) + P(\sim A|B)P(C|\sim A)$$

I didn't use $P(A|B)$ because of a special new rule. I used it because when you do the full marginalization math using Rule 1 and Rule 2, the Bayes' Theorem equation naturally appears in the middle of the algebra. Using $P(A|B)$ is just a faster way of doing the full marginalization.

### Unpacking Part (b): The reason behind the shortcut

Let's use the rules to prove exactly why $P(B|D) = P(B)$ in this specific problem.

Again, start with the definition:

$$P(B|D) = \frac{P(B, D)}{P(D)}$$

To find $P(B, D)$, we use **Rule 2** and marginalize over everything else in the graph ($A$ and $C$):

$$P(B, D) = \sum_{A} \sum_{C} P(A, B, C, D)$$

Using **Rule 1**, we expand the joint probability:

$$P(B, D) = \sum_{A} \sum_{C} P(A) \cdot P(B|A) \cdot P(C|A) \cdot P(D|C)$$

Here is the trick for part (b): Look at $P(D|C)$. The problem states that $P(D|C) = 0.7$ and $P(D|\sim C) = 0.7$. Because this value is exactly $0.7$ no matter what $C$ is, it is a constant. In algebra, you can pull a constant outside of a summation!

$$P(B, D) = 0.7 \cdot \sum_{A} \sum_{C} P(A) \cdot P(B|A) \cdot P(C|A)$$

What is $\sum_{A} \sum_{C} P(A)P(B|A)P(C|A)$? It is just the full marginalization to find $P(B)$. 
(since $\sum_{C} P(C|A)$ is just = 1, basically Variable Elimination)

So, we can simplify that massive equation down to:

$$P(B, D) = 0.7 \cdot P(B)$$

We also know that the total probability of $P(D)$ is $0.7$.

So, plug it all back into the very first equation:

$$P(B|D) = \frac{P(B, D)}{P(D)} = \frac{0.7 \cdot P(B)}{0.7} = P(B)$$

**The Reason:** Structurally, if you want to find $P(B|D)$, you _must_ use Rule 1 and Rule 2 to calculate the joint probability of all variables and marginalize out $A$ and $C$. However, because Dr Shiple made $P(D|C)$ a constant $0.7$, all that complex marginalization math instantly cancels itself out.