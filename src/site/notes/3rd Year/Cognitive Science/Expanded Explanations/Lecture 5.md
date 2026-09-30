---
{"dg-publish":true,"permalink":"/3rd-year/cognitive-science/expanded-explanations/lecture-5/","dg-note-properties":{}}
---


## Analogies in Recording Vs Lecture Slides

### **1. The Foundation of Reasoning (0:15)**

- **Highlight:** Causality and Bayesian Networks.
    
- **Connection to Slides:** While the KR slides focus on similarity, the lecture begins by recalling Bayesian networks (from the previous lecture) to establish how we represent cause-and-effect. This sets the stage for understanding that _how_ we represent knowledge (whether as a directed causal graph or a feature set) dictates how a system learns.
    

### **2. Generalization and Structured Models (5:01 & 11:01)**

- **Highlights:** Defining friendship; Generalizing that all animals breathe after learning cats do.
    
- **Connection to Slides (Slides 15-17):** This perfectly illustrates **Structured Models** (Hierarchies and Trees). Instead of storing isolated facts (e.g., "Cat breathes," "Dog breathes"), humans group concepts into a structured hierarchy (Node: "Animal" -> Property: "Breathes").
    
- **The "Friendship" Example:** This shows how we use relational graphs rather than flat features to define complex social structures, much like the Kinship (Ego/Cousin) example in the slides.
    

### **3. Geometric Models and MDS (15:34, 29:28, & 37:31)**

- **Highlights:** Visualizing similarity using cities; The Birds example (pitch, duration); The concept of "Stress" in MDS.
    
- **Connection to Slides (Slides 3-5):** The recording walks through **Multidimensional Scaling (MDS)**.
    
    - The **Cities Example** shows how spatial distance maps to similarity.
        
    - The **Birds Example** demonstrates how abstract features (like harmonic complexity) can act as X, Y, and Z coordinates in a 3D acoustic space, allowing us to cluster similar birds (ravens, owls) together.
        
    - **Stress (37:31):** Dr. Shiple explains the "Stress" metric from Slide 4. Because human similarity judgments aren't always geometrically perfect (violating the triangle inequality), forcing them into a 2D or 3D space creates "stress" or a mismatch error.
        

### **4. Context Sensitivity and Feature-Based Models (52:18)**

- **Highlight:** Similarity Across Domains (forming student project teams based on GPA, skills, location).
    
- **Connection to Slides (Slides 10-14):** This highlights the **Contrast Model of Similarity**. Geometric distances are rigidly fixed, but human similarity is context-dependent. When forming a project team, the "research skill" feature might be heavily weighted ($\alpha$), making two otherwise different students highly "similar" in this specific context. This proves Tversky's point: we group objects based on matching discrete features rather than strict spatial distance.

---

## What is "Stress"

In the context of Knowledge Representation and Multidimensional Scaling (MDS), **"stress" is a mathematical measure of error or mismatch.** It represents how much the algorithm had to compromise or distort the original data to make it fit into a visual map.

To understand why stress happens, we have to look at what MDS is trying to do and why human cognition makes that job so difficult.

### What is MDS Trying to Do?

Imagine you have a spreadsheet filled with "dissimilarity scores" between different animals based on human judgments. The goal of MDS is to translate those scores into a 2D or 3D geometric map where the physical distance between the points perfectly matches the scores in your spreadsheet. If a dog and a cat have a high similarity score, they should be plotted very close together.

### Why Does "Stress" Happen?

Stress occurs because **it is often mathematically impossible to satisfy all the distance constraints at the same time.** The algorithm is forced to place points in a "best fit" location rather than a "perfect" location.

There are two main reasons why this happens:

**1. The Reduction of Dimensions** Real-world concepts and human judgments are based on dozens or hundreds of hidden features (habitat, size, diet, sound, color). Trying to squash all of those dimensions into a flat 2D map or a 3D space is like trying to flatten the peel of an orange onto a table—it will inevitably tear or stretch. This stretching and compressing is measured as stress.
<br>
**2. Human Logic Violates Geometric Rules** As Amos Tversky pointed out, human similarity judgments do not obey the strict laws of geometry (the geometric axioms). Because MDS uses a geometric space, forcing human logic into it creates friction:
<br>
- **The Triangle Inequality Problem:** In geometry, the shortest distance between two points is a straight line. If point A is 2 inches from B, and B is 2 inches from C, A and C cannot be 100 inches apart. But human logic does this all the time. For example, a person might say Jamaica is highly similar to Cuba, and Cuba is highly similar to Russia (historically/politically), but Jamaica is completely dissimilar to Russia. If you try to draw this on a piece of paper, the dots for Jamaica, Cuba, and Russia simply cannot be placed at the exact distances that reflect those judgments. The algorithm has to pull them out of position, creating stress.
    <br>
- **Asymmetry:** On a map, the distance from New York to London is the exact same as London to New York. But cognitively, we might judge "a toy train is similar to a real train" as highly accurate, while "a real train is similar to a toy train" feels wrong. A spatial map cannot represent two different distances between the same two points, so the MDS algorithm has to calculate an average, compromising both values and adding to the stress metric.
    

### The Overlapping Circles Analogy

In the lecture recording, this is demonstrated using the analogy of overlapping circles. If you know exactly how far Point A should be from Point B, you can draw a line. But when you add Point C, Point D, and Point E, and you have specific distances they must all be from each other, drawing circles to find where they intersect perfectly becomes impossible. They will slightly miss each other.

The algorithm places the point in the center of that "miss," and the distance between where the point _actually_ is on the map and where it _technically should be_ according to the raw data is the **stress**.

Ultimately, stress is the accepted trade-off for geometric models. We accept a certain level of error (stress) in exchange for a map that gives us a highly useful, intuitive visualization of complex relationships.

___

## Violation of Minimality 

This concept comes from Amos Tversky’s critique of geometric models and specifically breaks the mathematical rule of **Minimality**.

In strict geometry, the distance from any point to itself is always exactly zero ($d(a,a) = 0$). Therefore, geometrically, _every_ object is equally identical to itself. A simple square is just as identical to a simple square as a complex 100-sided polygon is to a 100-sided polygon. The "similarity score" is exactly the same: 100% match.

However, human psychology doesn't work like geometry. When humans judge similarity, we don't just look at whether things match perfectly; we subconsciously count **the sheer number of overlapping features**.

### An Exact Translation

When the slides say humans rate complex objects as "more similar to themselves" than simple objects, it means: **If you show a person two identical complex things, and two identical simple things, they will feel that the complex pair has a "stronger" or "deeper" similarity than the simple pair.**

### The "Twin" Analogy

Imagine comparing two pairs of identical twins:

- **Pair 1 (Simple):** Two identical, blank, white pieces of standard printer paper.
    
- **Pair 2 (Complex):** Two identical, highly detailed replica paintings of the Mona Lisa.
    

If you ask someone, "Which pair is more similar?", logically, the answer should be "They are both exact duplicates, so the similarity is equal."

But cognitively, humans will rate the two Mona Lisa paintings as _more similar_. Why? Because with the blank paper, you only match a few basic features (color, size, shape). With the paintings, your brain registers hundreds of matching features—the exact shade of the sky, the curvature of the smile, the brush strokes on the hands.

### Why this matters for the Lecture (The Feature-Based Model)

Because humans tally up shared features, a purely spatial geometric map fails to represent human thought.

This is exactly why Tversky introduced the **Contrast Model of Similarity** (using the equation $S(A,B)=\theta f(A\cap B)-\dots$).

- For the simple blank papers, the shared features $f(A\cap B)$ is a very small number (e.g., 3 shared traits).
    
- For the complex paintings, the shared features $f(A\cap B)$ is a massive number (e.g., 500 shared traits).
    

Because the complex objects share a much larger volume of features, the mathematical output for their similarity is much higher, perfectly explaining this quirk of human psychology!