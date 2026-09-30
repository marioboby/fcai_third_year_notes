---
{"dg-publish":true,"permalink":"/3rd-year/cognitive-science/after-mid/lecture-9-exact-inference-variable-elimination/","dg-note-properties":{}}
---


### Overview of Variable Elimination

Variable Elimination (VE) is an exact inference algorithm used to calculate marginal probability distributions, such as $P(Y|E=e)$, within a Bayesian Network. Instead of calculating the entire joint probability distribution up front, VE systematically [[3rd Year/Cognitive Science/Expanded Explanations/Lecture 9#Meaning of "pushing summations inward"\|"pushes" summations inward over the products of factors to isolate the variable you want to calculate.]]

The process relies on two core operations:

1. **Product:** Multiplying all factors that involve the specific variable you are about to eliminate.
    
2. **Sum-out (Marginalization):** Summing over the variable to be eliminated to generate a new, smaller factor.
    

### Why Variable Elimination Beats Brute Force

The traditional "Brute Force" method computes the full joint distribution before marginalizing. This approach quickly hits an "Exponential Wall". For example, calculating the full joint probability for a chain of 50 binary variables requires an impossibly large table of $2^{50} \approx 10^{15}$ entries. Memory growth for brute force is exponential.

Variable Elimination solves this by only working with smaller, intermediate tables at each step. The computational complexity is determined by the maximum factor size created during the process, which is directly tied to the elimination order and the graph's "induced width".

### Graph Transformations: Moralization & Elimination

To utilize VE, the directed Bayesian Network must be converted into an undirected graph.

- **Moralization:** This involves "marrying the parents" (drawing a connection between all parents of the same child) and converting all directed arrows into undirected edges. This step ensures that a Conditional Probability Table like $P(X|Pa_X)$ forms a complete clique containing all of its related variables.
    
- **The Elimination Graph:** When a variable $X$ is eliminated, a new factor is created from the union of the scopes of all factors involving $X$. In the graph, this corresponds to making all current neighbors of $X$ a clique (creating "Fill-in Edges"), and then removing $X$ and its incident edges.
    

### Step-by-Step Example: Student Performance Network

The lecture walks through a 5-node network calculating a student's performance. The variables are $D$ (Difficulty), $I$ (Intelligence), $S$ (SAT/Score), $G$ (Grade), and $L$ (Letter).

[[3rd Year/Cognitive Science/Expanded Explanations/Lecture 9#"Student Performance" example\|The goal is to query the probability of getting a good letter]], $P(L=1)$. To do this, the algorithm marginalizes out the other variables in a specific order: $S, I, D, G$.

1. **Eliminating S (The Barren Node):** $S$ is an unobserved leaf child. According to the "Barren Node Rule", summing out an unobserved leaf node always produces a factor of 1s. This proves $S$ has no impact on $L$ unless a specific value for $S$ is observed.
    
2. **Eliminating I:** The algorithm takes the product of all factors involving $I$ ($\phi_I(I)$, $\tau_1(I)$, and $\phi_G(G,D,I)$) and sums out $I$. This results in a new 2-variable factor, $\tau_2(G,D)$.
    
3. **Eliminating D:** It multiplies the factors involving $D$ ($\phi_D(D)$ and $\tau_2(G,D)$) and sums out $D$. This leaves a new factor, $\tau_3(G)$.
    
4. **Eliminating G:** Finally, the remaining factors $\tau_3(G)$ and $\phi_L(L,G)$ are multiplied together, and $G$ is summed out.
    
5. **Result:** The calculation concludes that there is a $\approx 50.6\%$ chance ($0.5056$) of the student receiving a good recommendation letter.
    

### Key Takeaway

The order in which you eliminate variables is incredibly important. By strategically eliminating variables (such as eliminating the leaf node $S$ first), the algorithm in the example never had to construct a full 32-row joint distribution table ($2^5=32$). Instead, the maximum table size it ever had to handle was just 8 rows ($2^3=8$) during the calculation of $I$.