---
{"dg-publish":true,"permalink":"/3rd-year/cognitive-science/before-mid/lecture-6-local-probabilistic/","dg-note-properties":{}}
---

[[3rd Year/Cognitive Science/Before Mid/Lecture 6 - Local Probabilistic#Recording Breakdown\|#Recording Breakdown]]
[[3rd Year/Cognitive Science/Before Mid/Lecture 6 - Local Probabilistic#Slides Breakdown\|#Slides Breakdown]]
## Recording Breakdown

### Course Project Introduction and Group Formation  
- The project centers on **Cognitive Modeling**, specifically assessing individuals' human-level cognition according to their psychological states.  
- Students are asked to form groups of five to collaboratively build models applying course concepts.  
- The project is fundamental to understanding cognitive assessment and will involve practical applications of learned theories.  
- There is an additional **bonus project** related to medicine dosage evaluation, supervised by Dr. Wael, which is optional and separate from the core project.

### Bayesian Networks vs. Markovian Networks  
- **Bayesian Networks (BNs)** focus on **cause-effect relationships**, modeling parent-child node dependencies with directed edges.  
- BNs work through **conditional probabilities** by calculating from child nodes up to parent nodes until reaching the root.  
- Some relationships are not purely cause-effect, leading to the introduction of **Markovian Networks**:  
  - Nodes are at the same level with **undirected edges**, representing symmetric relations.  
  - Dependencies are captured through **potential functions** rather than direct probabilities.  
  - Markov models handle scenarios where multiple indirect paths exist between nodes.  
- The **Local Semantic** approach calculates probabilities along a single path (child to root), while **Global Markovian** involves more complex splitting calculations for the entire network.

### Computational Complexity in Probabilistic Networks  
- Calculating probabilities for large networks is computationally intensive; BNs require calculations from child to root, while Markovian networks need full network computations.  
- To manage complexity, **network simplification** is crucial, reducing computational power needs.  
- The process involves breaking down large nodes with many variables into **sub-nodes**, thereby reducing the size of **Conditional Probability Tables (CPTs)** exponentially.  
- Example: A node with 5 variables has a CPT size of $2^5 = 32$ records; decomposing it into 5 sub-nodes reduces this drastically, improving efficiency.  
- This approach is part of what is called **Local Probabilistic Models**, focusing on smaller sub-networks rather than the entire global network.

### Handling Large Nodes and Small Datasets  
- Large nodes with many variables cause exponential growth in CPT size, making calculations infeasible.  
- Decomposing such large nodes into sub-nodes (e.g., separating variables like window open/close, TV on/off, morning status, headphone use) creates a tree structure within the node, reducing complexity.  
- Small datasets pose challenges for learning accurate probabilities; solutions include **data augmentation** and **minimization of irrelevant variables**.  
- Example: Decision trees reduce complexity by prioritizing certain features (e.g., overcast weather) to avoid unnecessary calculations on less relevant branches.

### Local Probabilistic Models vs. Classical Decision Trees  
- Unlike classical decision trees that yield deterministic yes/no answers, local probabilistic models provide **probabilistic outputs** (e.g., probability of an event happening).  
- This probabilistic output reflects **uncertainty** and **incomplete knowledge**, introducing nuances absent in pure binary decisions.

### Techniques for Simplifying Complex Networks  
- Two key simplification techniques are introduced:  
  1. **Context-Specific Independencies (CSI):**  
     - By conditioning on known states (context), irrelevant paths or variables can be ignored, reducing computation.  
     - Example: When evaluating job applicants, if a connection (e.g., recommendation) is absent, certain branches of the network are pruned.  
  2. **Noisy-OR Models:**  
     - Models where multiple independent causes can produce an effect, each with its own probability; the overall effect is computed efficiently using an **OR operation** on probabilities.  
     - This avoids enumerating all combinations explicitly, reducing computational steps roughly by half.  
- Example of Noisy-OR: Coughing caused by cold, flu, or smoking, where each cause independently increases the chance of coughing.

### Noisy-OR and Noisy-MAX Models Explained  
- **Noisy-OR:** Used when multiple independent causes can singly trigger an effect.  
- Calculation example:  
  - Probability of coughing given cold = 40%, given smoking = 30%.  
  - Total probability is computed using:  
  $$P(\text{cough} | \text{cold or smoking}) = 1 - (1 - 0.4)(1 - 0.3) = 0.58$$
  
- **Noisy-MAX:** Extends Noisy-OR to multiple graded levels of effect rather than binary states.  
- Example in fire detection: sensors provide multi-level readings; the maximum reading determines the response (e.g., sprinkler activation).  
- These models help focus on the **most significant causes** or effects, simplifying probabilistic inference.

### Application of Noisy-MAX in Medical Dosage and Risk Assessment  
- In medical contexts, such as dosing for diseases like COVID-19 or cancer, Noisy-MAX helps prioritize treatment based on the **highest risk level** or severity.  
- Example: For chemotherapy dosing, focus is on the highest disease level rather than the lowest, to ensure effective treatment.  
- This approach balances **treatment risk** and **disease severity**, emphasizing the need for probabilistic modeling in uncertain environments.

### Combining Local Models and Context-Specific Simplifications  
- By combining **Local Probabilistic Models** with **Context-Specific Independencies** and **Noisy-OR/MAX**, computational load is significantly reduced.  
- Large networks are decomposed, irrelevant branches eliminated based on context or variable states, and independent causes combined efficiently.  
- This combined approach enhances scalability and interpretability of probabilistic models.

### Applying Local Probabilistic Concepts to Markovian Networks  
- The same simplification principles are extended to **Markovian Networks**, where large networks are decomposed into smaller sub-networks called **cliques**.  
- **Cliques** are fully connected subgraphs where nodes share many features and dependencies.  
- Decomposing a large network into cliques enables independent calculations within each sub-network, simplifying global computations.

### Cliques vs. Groups - Definitions and Differences  
| Term     | Definition                                                                                   | Key Characteristics                                      |
|----------|----------------------------------------------------------------------------------------------|----------------------------------------------------------|
| Group    | A set of individuals sharing some common features, but not necessarily highly similar ones. | May share a few features, loosely connected.             |
| Clique   | A fully connected subset where all nodes are closely related or highly similar.              | High similarity, strong interconnections, fully connected.|

- Cliques allow for higher similarity modeling and efficient factorization of probabilistic tables.

### Clique-Based Network Decomposition and Simplification  
- Large networks are split into multiple cliques based on connectivity.  
- Each clique can be processed independently, simplifying the overall computation.  
- Example: Dividing a network into two sub-networks connected via a common node enables separate processing and probability calculations.  
- This method reduces overall complexity and computational cost.

### Quantitative Example of Clique Decomposition  
- Consider a network with 6 nodes and associated probabilities.  
- The total number of records to process without decomposition is $2^6 = 64$.  
- By identifying cliques and assuming similarity within them, the number of distinct combinations reduces drastically to a few representative cases.  
- Calculations become manageable and scalable by:  
  - Multiplying probabilities of similar nodes and  
  - Using relative probabilities instead of absolute counts.  

### Calculating Normalization Constant ($Z$) in Markovian Networks  
- The normalization constant $Z$ sums over all possible states to normalize probabilities.  
- Clique decomposition helps approximate $Z$ by grouping similar states, reducing the need to enumerate all states.  
- This approximation is crucial for efficient probabilistic inference in large networks.

### Approximations and Relative Probabilities  
- Instead of computing all probabilities explicitly, relative probabilities between key states are used.  
- This **approximation** reduces computations while retaining useful probabilistic distinctions for classification or decision-making.  
- Example: Comparing relative likelihoods of different node configurations to identify the most probable class.

### Summary of Simplification Techniques  
| Technique                          | Purpose                               | Outcome                                  |
|----------------------------------|-------------------------------------|------------------------------------------|
| Local Probabilistic Models        | Decompose large nodes into sub-nodes| Reduced CPT size, simpler calculations   |
| Context-Specific Independencies  | Condition on known contexts to prune irrelevant branches | Reduced computations by ignoring irrelevant variables |
| Noisy-OR / Noisy-MAX Models      | Efficiently combine independent causes | Halved computational steps, accurate probabilities |
| Clique Decomposition             | Split large networks into fully connected subgraphs | Independent processing, scalable inference |
| Relative Probability Approximation | Approximate normalization constants and probabilities | Computational efficiency with acceptable accuracy |

### Course Project Details and Support  
- The main project focuses on **Cognitive Modeling for psychological state assessment** using real student data collected by Dr. Mai.  
- The dataset is realistic, collected from schools, and involves image data converted into parameter sets for modeling.  
- Teams are encouraged to divide work into two parts:  
  - Image processing and conversion to data parameters, and  
  - Data analysis and model implementation.  
- Dr. Mai, an assistant professor specializing in cognitive and psychological assessment, will provide ongoing support.  
- The bonus project on COVID-19 vaccine effects and immune response failure is optional and supervised by Dr. Wael.  
- Project deliverables may include app development (GUI/mobile) for result visualization, but initial phases focus on core modeling work.

### Administrative and Exam Information  
- Midterm exam covers material up to the current lecture, mainly numerical problems based on lectures.  
- Reference textbook and lecture materials are provided via links and are the primary study resources.  
- The exam includes both theoretical and practical questions aligned with lecture content.  
- Students are encouraged to review provided sheets and references ahead of the exam.

---

### **Key Insights and Conclusions**  
- **Probabilistic graphical models** such as Bayesian and Markovian networks are foundational for modeling uncertain cognitive and psychological states.  
- **Complexity management** is critical: decomposition into sub-nodes, exploiting context-specific independencies, Noisy-OR/MAX models, and clique decomposition dramatically improve computational feasibility.  
- **Local probabilistic models** provide a practical middle ground by focusing on parts of the network rather than global full computations.  
- **Approximate methods** leveraging relative probabilities and grouping similar nodes/cliques help scale inference to real-world sized problems.  
- The course project applies these concepts to **real data** for cognitive assessment, supported by domain experts and incorporating modern computational techniques.  
- The methodology emphasizes balancing **model accuracy**, **computational efficiency**, and **interpretability** in complex probabilistic systems.

---

### **Glossary of Key Terms**  
| Term                         | Definition                                                                                     |
|------------------------------|------------------------------------------------------------------------------------------------|
| Bayesian Network (BN)         | Directed acyclic graph representing cause-effect relationships with conditional probabilities.|
| Markovian Network             | Undirected graph representing symmetric dependencies between nodes on the same level.          |
| Conditional Probability Table (CPT) | Table detailing probabilities of node states given parent states in a BN.                  |
| Local Probabilistic Model     | Model focusing on smaller sub-networks or sub-nodes to simplify computations.                   |
| Context-Specific Independence (CSI) | Independence that holds under certain variable assignments, allowing branch pruning.       |
| Noisy-OR Model                | Probabilistic model where multiple independent causes can produce an effect via OR operation.  |
| Noisy-MAX Model               | Extension of Noisy-OR for multi-valued variables and graded effects.                           |
| Clique                       | Fully connected subgraph used for decomposing Markovian networks.                             |
| Normalization Constant ($Z$)  | Sum over all state probabilities used to normalize probability distributions.                  |

---

### **Frequently Asked Questions (FAQ)**  
**Q:** What is the difference between Bayesian and Markovian networks?  
**A:** Bayesian networks are directed and model cause-effect relations, while Markovian networks are undirected and model symmetric dependencies among nodes at the same level.  

**Q:** How do we handle large nodes with many variables?  
**A:** By decomposing large nodes into sub-nodes, creating trees inside nodes, and reducing CPT sizes exponentially to simplify calculations.  

more [[3rd Year/Cognitive Science/Expanded Explanations/Lecture 6#**Q ** How do we handle large nodes with many variables?\|here]]

**Q:** What is the role of Noisy-OR in probabilistic inference?  
**A:** Noisy-OR models efficiently combine multiple independent causes affecting a single effect, reducing computational steps needed for joint probability calculation.  

**Q:** How does clique decomposition improve performance?  
**A:** It breaks large networks into smaller fully connected subgraphs, allowing parallel and independent computation, thus reducing complexity.  

**Q:** How is the project structured and supported?  
**A:** Students form groups to work on cognitive modeling projects using real-world datasets, with support from domain experts focusing on psychological state assessment.

---
## Slides Breakdown

### Slide 2: The Representation Challenge

This slide outlines why standard Bayesian Networks struggle as they scale up.

- **The Core Problem**: Standard Bayesian Networks rely on Conditional Probability Tables (CPTs).
    <br>
- **Exponential Growth**: For a node $X$ with a set of parents $Pa_X = \{Y_1, ..., Y_k\}$, the size of the table grows exponentially, represented as $O(d^{k+1})$.
    <br>
- **The Curse of Dimensionality**: This exponential growth causes several major issues:
    <br>
    - It creates too many parameters.
        <br>
    - It is difficult to gather this much probabilities from domain experts.
        <br>
    - It is impossible to effectively learn the probabilities if you only have small datasets.
        <br>
    - It results in very high computational costs when running inference.
        <br>
- **Chapter Goal**: The main objective is to exploit "local" structures within the Conditional Probability Distribution (CPD) to drastically reduce the number of required parameters.
    
more on LCM [[3rd Year/Cognitive Science/Expanded Explanations/Lecture 6\|here]]

### Slide 3: Types of Local Structure

To fix the parameter explosion, the lecture identifies four primary types of local structures:

- **Deterministic Dependencies**: This occurs when specific values of parent nodes uniquely and absolutely determine the child node's value.
    <br>
- **Context-Specific Independence (CSI)**: This happens when a variable $X$ is independent of $Y$ given $Z$, but this independence only holds true for specific values of $Z$.
    <br>
- **Independence of Causal Influence (ICI)**: This applies when parent variables independently contribute to the probability of the child variable occurring (a common example is the Noisy-OR model).
    <br>
- **Continuous Variables**: This involves using continuous functional forms, such as Gaussian or Sigmoid functions, to define relationships.
    

### Slides 4, 5 & 6: Context-Specific Independence (CSI) & Tree-CPDs

These slides explain how to visualize and define CSI using tree structures.

- **Defining CSI**: A variable $X$ is conditionally independent of $Y$ given $Z$ in a specific context $c$ if $P(X|Y,Z,c) = P(X|Z,c)$. This is considered a "weaker" form of conditional independence because it only has to be true for that specific assignment $c$, not all possible values.
    <br>
- **Tree-CPD Representation**: Instead of a massive table, a Tree-CPD uses a rooted tree to represent $P(X|Y_1, ..., Y_k)$.
    <br>
    - Internal nodes represent the parent variables ($Y_j$), and the edges coming from them represent their possible values.
        <br>
    - The leaf nodes contain the final probability distribution $P(X)$.
        <br>
- **The Path Rule**: If you trace a path from the root of the tree to a leaf (representing context $c$) and a parent variable $Y_j$ is missing from that path, it means $X$ is independent of $Y_j$ in that specific context.
    <br>
- **Benefits**:
    <br>
    - **Efficiency**: It reduces the parameter count from $2^n$ in a full CPT to simply the number of leaves in your tree.
        <br>
    - **Inference**: It speeds up calculations (variable elimination) by allowing the system to ignore variables that are "irrelevant" in specific contexts.
        

### Slides 7 & 8: Independence of Causal Influence (ICI) & The Noisy-OR Model

These slides transition to ICI, focusing on the Noisy-OR model.

- **What is ICI?**: This occurs when multiple causes (parents) don't interact with each other in complex ways; instead, each cause has its own separate mechanism that contributes independently to triggering the effect (child). The total influence is calculated using a deterministic function like OR, SUM, or MAX.
    <br>
- **The Noisy-OR Model**: This is used when multiple independent causes ($X_1, ..., X_n$) can trigger an effect $Y$.
    <br>
- **Core Assumptions**:
    <br>
    - **Accountability**: The effect will not happen ($Y=0$) if all causes are absent ($X_i=0$).
        <br>
    - **Independent Inhibition**: Even if a cause is present ($X_i=1$), there is a specific probability ($q_i$) that it will "fail" to trigger the effect.
        <br>
- **Formulas**:
    <br>
    - Probability of effect not happening: $P(Y=0|X_1, ..., X_n) = \prod_{i:X_i=1} q_i$.
        <br>
    - Probability of effect happening: $P(Y=1|X_1, ..., X_n) = 1 - \prod_{i:X_i=1} q_i$.
        

### Slides 9 & 10: The Calculation Complexity and the Noisy-OR Trick

- **The Complexity Issue**: Calculating the standard OR probability—represented in the slide as $P(A \cup B) = P(A) \cdot P(B) \cdot P(A \cap B)$—is extremely difficult at scale. With just 2 causes, you have to sum 3 terms, but with $n$ causes, you would have to sum $2^n - 1$ distinct scenarios where the effect occurs.
    <br>
- **The "Complement" Trick**: To avoid this exponential math, Noisy-OR calculates the probability that _all_ active causes fail, and subtracts that from 1.
    <br>
    - **Formula logic**: $P(E=0|C) = \prod_{i:C_i=1} (1-q_i)$.
        <br>
    - **Example**: If a patient has a Cold (probability of triggering a cough is $0.4$) and Smokes (probability of triggering a cough is $0.3$).
        <br>
        - Probability Cold fails to trigger cough: $1 - 0.4 = 0.6$.
            <br>
        - Probability Smoking fails to trigger cough: $1 - 0.3 = 0.7$.
            <br>
        - Probability of No Cough: $0.6 \times 0.7 = 0.42$.
            <br>
        - Probability of Cough: $1 - 0.42 = 0.58$.
            

### Slides 11 & 12: Naming and Logic behind Noisy-OR

- **Noisy-OR vs. Noisy-AND**:
    <br>
    - **Noisy-OR** captures _Sufficiency_; either a Cold, Flu, or Smoking is sufficient on its own to cause a cough. It is the most common model.
        <br>
    - **Noisy-AND** captures _Necessity_; you need Fuel AND a Battery AND an Ignition to start a car. It is rarely used because it implies an effect might "accidentally" happen even if a strict requirement is missing, which is counter-intuitive.
        <br>
- **Why the Name?**:
    <br>
    - **"OR"**: Derived from Boolean logic; if Cause A OR Cause B is active, the effect happens.
        <br>
    - **"Noisy"**: In pure logic, causes trigger effects 100% of the time. In probabilistic graphical models (PGMs), "noise" acts as a mechanism that flips the signal from 1 to 0 with probability $(1-q_i)$, preventing the cause from triggering the effect every single time.
        

### Slide 13: Summary: CSI vs. ICI

This slide provides a comparative table to distinguish the two main concepts:

- **CSI (Context-Specific)**: Focuses on some parents being irrelevant in specific contexts. It uses Tree-CPDs, reduces parameters to the number of leaves, and captures logical exceptions.
    <br>
- **ICI (Causal Influence)**: Focuses on parents contributing independently without interacting. It uses Noisy-OR/MAX models, reduces parameters from $2^n$ to $n$, and captures accumulative effects.

more [[3rd Year/Cognitive Science/Expanded Explanations/Lecture 6#ICI Vs CSI\|here]]

### Slides 14, 15 & 16: Generalizing ICI & Noisy-MAX

- **Beyond Noisy-OR**: The ICI concept can be generalized beyond binary (0 or 1) outcomes using functions like Noisy-MAX or Noisy-ADD.
    <br>
- **What is Noisy-MAX?**: While Noisy-OR asks "Does it happen?", Noisy-MAX asks "To what degree does it happen?". It is used for multi-valued or ordinal variables.
    <br>
- **The Logic of Domination**: Each independent cause suggests a "level" for the effect, and the final outcome is simply the Maximum level triggered by any of the active causes.
    <br>
- **Numerical Example**:
    <br>
    - Sensor A predicts Alert Level $\le1$ with $0.4$ probability.
        <br>
    - Sensor B predicts Alert Level $\le1$ with $0.2$ probability.
        <br>
    - Probability that the final alert is at most Level 1: $P(E\le1) = 0.4 \times 0.2 = 0.08$.
        <br>
    - Probability of a High Alert (Level 2): $P(E=2) = 1 - 0.08 = 0.92$. This shows that having two high-risk sensors significantly drives up the final probability of a high alert.
        

### Slides 17, 18 & 19: The Need for Hybrid Models

- **The Problem**: Real-world variables act differently based on context. Some act as "switches" (e.g., Species: Human vs. Robot) while others act as "accumulative triggers" (e.g., Cold, Flu). A pure Tree-CPD gets too deep, and a pure Noisy-OR can't handle context-switching.
    <br>
- **The Solution (Nested Local Models)**: Use a Decision Tree (Tree-CPD) to determine which "rulebook" or context to use, and then place an ICI model (like Noisy-OR) at the leaf nodes of that tree to handle the accumulative causes.
    <br>
- **Why is this Powerful?**:
    <br>
    - **Symmetry Breaking**: It allows you to "turn off" the Noisy-OR logic entirely in certain contexts where the causes are irrelevant.
        <br>
    - **Efficiency**: Instead of a massive table of $2^{10}$ combinations, you only need 10 parameters ($q_i$) inside a single leaf.
        <br>
    - **Modularity**: You can assign completely different models (Noisy-MAX in one branch, Noisy-OR in another) depending on the context.
        

