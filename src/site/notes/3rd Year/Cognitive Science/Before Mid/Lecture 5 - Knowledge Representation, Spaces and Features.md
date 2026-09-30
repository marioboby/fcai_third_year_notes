---
{"dg-publish":true,"permalink":"/3rd-year/cognitive-science/before-mid/lecture-5-knowledge-representation-spaces-and-features/","dg-note-properties":{}}
---


### **Geometric Models and Multidimensional Scaling**

- **Core Concept:** In geometric models, concepts are represented as individual points situated within a multidimensional coordinate space.

- Similarity between concepts is inversely proportional to the spatial distance between them; meaning, the closer two points are to one another, the more similar they are considered to be.
    <br>
- **Multidimensional Scaling (MDS):** This statistical tool is used to visualize these similarity levels by processing a proximity matrix (dissimilarity scores) and outputting a spatial map.
    
- The primary goal of MDS is to uncover the hidden dimensions that shape how humans judge similarity.
    <br>
- The lecture illustrates this with two examples: plotting European cities based on geographic distances, and mapping bird vocalizations (like the Canary or Raven) in a 3D acoustic space defined by features like average pitch, duration, and complexity.
    
- MDS uses a "Stress" value to measure the mismatch between the original dissimilarity data and the generated spatial distances.

More on "Stress" [[3rd Year/Cognitive Science/Expanded Explanations/Lecture 5#What is "Stress"\|here]]

### **Distance Metrics and Spatial Recovery**

- **Minkowski Metrics:** Geometric similarity is frequently calculated using the Minkowski metric: $d(x,y)=(\sum_{i=1}^{n}|x_{i}-y_{i}|^{r})^{1/r}$.
    <br>
- When $r=2$, this represents the standard Euclidean distance (straight-line distance), and when $r=1$, it represents the City-block or Manhattan distance.
    <br>
- **Geometric Axioms:** For these models to hold true, they must satisfy three mathematical rules: 
  - Minimality ($d(a,b)\ge d(a,a)=0$), 
  - Symmetry ($d(a,b)=d(b,a)$), 
  - and the Triangle Inequality ($d(a,c)\le d(a,b)+d(b,c)$).
    <br>
- An example of spatial recovery is shown by positioning a Dog at $(0,0)$ and a Cat at $(2,0)$, and using distance equations like $x^{2}+y^{2}=5^{2}$ to deduce that a Fish belongs at $(3.25, 3.8)$, placing it further away from the mammal cluster.
    

### **The Flaws of Geometric Models**

- Cognitive psychologist Amos Tversky pointed out that human perception frequently violates the strict rules of geometric axioms.
        <br>
- **[[3rd Year/Cognitive Science/Expanded Explanations/Lecture 5#Violation of Minimality\|Violation 1 - Minimality]]:** Humans tend to rate complex objects as being more similar to themselves compared to simpler objects, which violates the geometric assumption that all identical objects have a distance of exactly zero.
        <br>
- **Violation 2 - Asymmetry (The Prototype Effect):** Human similarity is directional, breaking the symmetry rule. We often focus on a prototype; for example, saying "North Korea is like China" feels more natural than the reverse, making $S(a,b)\ne S(b,a)$.
        <br>
- **Violation 3 - Triangle Inequality:** Human logic allows for relational chains that don't translate to geometry. We might agree that Jamaica is similar to Cuba, and Cuba is similar to Russia, without concluding that Jamaica is similar to Russia.
        <br>
- **Context Sensitivity:** Geometric distances are fixed, but human cognitive similarity fluctuates based on the context or comparison set. For instance, the "Hungary Effect" shows that Austria and Germany seem very similar until Hungary is introduced to the comparison set, which suddenly highlights specific geopolitical differences.
    

### **The Contrast Model of Similarity**

- Introduced by Tversky in 1977, this feature-based model rejects the idea of spatial distance and instead defines objects purely by their sets of discrete features.
        <br>
- Similarity is calculated by weighing the features that objects share against the features that are distinct to each.
        <br>
- The mathematical representation is: $S(A,B)=\theta f(A\cap B)-\alpha f(A-B)-\beta f(B-A)$.
        <br>
- In this equation, $f(A\cap B)$ represents the shared features, while $f(A-B)$ and $f(B-A)$ represent the features unique to subjects A and B, respectively.
        <br>
- This model perfectly explains cognitive asymmetry: if the weights $\alpha$ and $\beta$ are not equal, then the similarity judgments will shift depending on which object is the subject and which is the referent.
    

### **Structured Models**

- Both vectors (spaces) and lists (features) are considered "flat" and struggle to capture deep relations like "part-of" or "caused-by".
        <br>
- **Hierarchies and Trees:** These organize data into levels of abstraction where lower nodes inherit properties from the nodes above them, heavily used in biological taxonomies.
        <br>
- **Graphs and Networks:** These map knowledge using nodes for entities and edges for relations, allowing for complex semantic networks like defining that a "Robin" is a "Bird" that can "Fly".
        <br>
- These structural constraints actually enable learning by acting as a roadmap. If an agent learns a dog has a spleen, the hierarchical structure implies other mammals likely do too, drastically reducing the data needed to understand biology.
    

### **Case Study: Kinship Systems**

- Kinship, studied extensively by Claude Lévi-Strauss, relies entirely on structured paths rather than visual features.
        <br>
- A "Grandparent" relation is a composition defined as a Parent of a Parent ($P\circ P$), and an "Uncle/Aunt" is the Sibling of a Parent ($S\circ P$).
        <br>
- To logically define a "Cousin," one must traverse the relational graph starting from the "Ego" (self) node, move up to the "Grandparent" node, and trace back down a completely different branch.
    

### **Summary and Conclusion**

- **Geometric Models:** Define similarity as distance and excel with perceptual data.
        <br>
- **Feature-Based Models:** Rely on set-theory to explain cognitive asymmetries.
        <br>
- **Structured Models:** Are fundamentally relational and are required for reasoning within complex domains.
        <br>
- Ultimately, the specific knowledge representation chosen directly dictates how powerful a learning algorithm can be.