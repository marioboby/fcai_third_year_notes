---
{"dg-publish":true,"permalink":"/3rd-year/cognitive-science/before-mid/lecture-4-2-markovian-networks-markov-random-field/","dg-note-properties":{}}
---

[Message Passing in Markov Networks](https://knowledgespace.asser.md/concepts/message-passing-in-markov-networks)
By: Asser Ahmed

### **Motivation: Why Markov Networks?**

- While Bayesian Networks (Directed Acyclic Graphs, or DAGs) are powerful, they are restricted because they cannot contain cycles, which are vital for modeling spatial dependencies.
    <br>
- **The Directed Graph Challenge:** Using the example of analyzing a patch of grass, a directed network fails because influence can only flow forward. A single pixel cannot logically state both "My neighbors are grass, so I should be grass" and "Because I am grass, my neighbors should remain grass" simultaneously due to the restrictive flow of a DAG.
    <br>
- **The Undirected Solution:** Markov Networks solve this by using undirected edges to represent bidirectional dependencies and mutual influence, allowing nodes to affect each other symmetrically.
    <br>
- This symmetry is necessary because many physical interactions (like image pixels) lack a causal direction, and we are often more interested in modeling constraint satisfaction or "affinity" rather than strict cause and effect.
    <br>
- Additionally, DAGs cannot represent certain dependency structures—like a diamond shape with specific constraints—without unnecessarily adding extra edges.
    

### **Key Differences: Bayesian vs. Markov**

This slide provides a direct comparison of the two network types:

|**Feature**|**Bayesian Networks**|**Markov Networks**|
|---|---|---|
|**Edge Type & Graph Type**|Directed (Arrows); DAG (No cycles)|Undirected (Lines); General Graph (Cycles allowed)|
|**Factorization**|Conditional Probability Distributions (CPDs)|Potential Functions (Factors / Cliques)|
|**Normalization**|Locally normalized ($\sum P = 1$)|Globally normalized by Partition Function (Z)|
|**Interpretation**|Causal Influence|Mutual Interaction / Correlation|

### **Independence, Normalization, and Semantics**

- **Independence:** Bayesian Networks rely on complex d-separation rules due to V-structures, whereas Markov Networks use straightforward graph separation. In an MRF, if all paths between $X$ and $Y$ are blocked by $Z$, then $X$ is conditionally independent of $Y$ given $Z$ ($X \perp Y | Z$).
    <br>
- **Normalization Cost:** Because MRFs use potentials ($\phi$) that are not actual probabilities, global normalization is required.
    <br>
- The probability is defined by the **Gibbs Distribution**:
    
    $$P(X) = \frac{1}{Z} \prod_{c \in \mathcal{C}} \phi_c(x_c)$$
    
    Here, $Z = \sum_X \prod_c \phi_c(x_c)$ serves as the Partition Function.
    <br>
- The network's syntax consists of an undirected graph, and its semantics rely on these potential functions distributed over maximal cliques.
    

### **The "Misconception" Example Calculation**

- **The Setup:** The lecture introduces a model with four individuals—Alice, Charles, Bob, and Debbie—arranged in a diamond structure.
    <br>
- **Step 1 (Identify Structure):** The joint distribution is the product of four local factors: $\tilde{P}(a,b,c,d) = \phi_1(a,b) \cdot \phi_2(b,c) \cdot \phi_3(c,d) \cdot \phi_4(d,a)$. For a specific state where all values are zero, the values given are 30, 100, 1, and 100, respectively.
    <br>
- **Step 2 (Unnormalized Weight):** The unnormalized weight ($\tilde{P}$) for this state is found by multiplying the factors: 30 × 100 × 1 × 100 = 300,000. This is a relative weight, not a probability.
    <br>
- **Step 3 (Partition Function Z):** To find $Z$, you must sum the weights of all 16 possible combinations of assignments for the four nodes. The total sum $Z$ for this network equals 7,201,000.
    <br>
- **Step 4 (Normalize):** Dividing the unnormalized weight by $Z$ gives the final probability. For the previous state, $300,000 / 7,201,000$ yields approximately 0.04166, meaning there is roughly a 4.17% chance of this specific configuration occurring.
    

### **The Complexity of Factors and Estimation**

- **Nature of Factors:** Factors ($\phi(C)$) do not represent probabilities, meaning a value of "100" does not mean 100%. They simply measure affinity, which makes them much harder for humans to intuitively estimate compared to standard probabilities.
    <br>
- **Global vs. Local:** In Bayesian Networks, CPD tables sum to 1 locally and have independent meaning. In Markov Networks, a single factor means nothing on its own; it depends on the whole network via the global $Z$.
    <br>
- **Estimation Difficulty:** Changing one parameter forces a recalculation of the entire global $Z$. Furthermore, computing $Z$ requires summing over an exponential number of states, and parameters cannot be optimized independently during learning because they are tightly coupled. This computational cost is the price paid for the network's structural flexibility.
    

### **Dependencies and Local Intuition Fallacies**

- **Dependencies:** Variables sharing a factor (like A and B) have direct dependencies and an edge in the graph. Variables without a direct factor (like Alice and Debbie) have no direct edge and only influence each other indirectly through the chain. Adding a factor between them would add a diagonal edge to the graph, increasing the number of factors in the weight calculation from four to five.
    <br>
- **The Fallacy:** It is a misconception to view a local factor $\phi(A,B)$ as the marginal probability of A and B. The full network relies on the collective "votes" of all factors.
    <br>
- **Peer Pressure:** In the example, local factor $\phi_1(A,B)$ strongly prefers that Alice and Bob agree. However, the global reality dictates that they are most likely to disagree. This is due to "peer pressure" from the rest of the network: the other three factors collectively force Alice and Bob into disagreement to satisfy the stronger overarching constraints.
    

### **Reduced Networks and Inference Methods**

- **Reduced Networks:** When evidence is observed, the nodes representing that evidence are completely removed from the graph, along with any edges attached to them.
    <br>
- **Proposition 4.1:** Reducing factors down to an observed context $u$ is mathematically equivalent to conditioning the distribution. The required renormalization for this conditioning is automatically handled by the standard normalization of the newly reduced Gibbs distribution.
    <br>
- **Approximate Inference:** Because exact computation is often too complex for dense graphs, approximate methods are required. These include:
    
    - **Sampling (MCMC):** Methods like Gibbs Sampling and Metropolis-Hastings generate samples from the posterior.
        <br>
    - **Variational Inference:** Minimizes KL-Divergence to turn inference into an optimization problem.
        <br>
    - **Loopy Belief Propagation:** Applies message-passing techniques to networks containing cycles.