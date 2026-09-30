---
{"dg-publish":true,"permalink":"/3rd-year/cognitive-science/before-mid/lecture-1-intro-from-behaviorism-to-marr-s-levels-of-analysis/","dg-note-properties":{}}
---

# Outline
[[3rd Year/Cognitive Science/Before Mid/Lecture 1 - Intro From Behaviorism to Marr’s Levels of Analysis#A - History The Great Shift (1950s)\|History]]
[[3rd Year/Cognitive Science/Before Mid/Lecture 1 - Intro From Behaviorism to Marr’s Levels of Analysis#B - The Pioneers\|The Pioneers]]
[[3rd Year/Cognitive Science/Before Mid/Lecture 1 - Intro From Behaviorism to Marr’s Levels of Analysis#C - Famous Debates\|Famous Debates]]
[[3rd Year/Cognitive Science/Before Mid/Lecture 1 - Intro From Behaviorism to Marr’s Levels of Analysis#D - Bayesian Reconciliation\|Bayesian Reconciliation]]
[[3rd Year/Cognitive Science/Before Mid/Lecture 1 - Intro From Behaviorism to Marr’s Levels of Analysis#**Part 2 Marr’s Tri-Level Hypothesis**\|Part 2 : Marr's Tri-Level Hypothesis]]

# **Part 1: The Cognitive Revolution & Fundamental Debates**

## A - History: The Great Shift (1950s)

The Cognitive Revolution in the 1950s was a direct rebellion against "Behaviorism". Before the 1950s, Behaviorists treated the mind as an impenetrable "Black Box," focusing strictly on observable ==**stimulus**== and **==response==**. 

Everything changed at the 1956 MIT Symposium on Information Theory. Researchers introduced the "Computer Metaphor," realizing that human minds and machines both manipulate symbols and internal representations. [[3rd Year/Cognitive Science/Expanded Explanations/Lecture 1#^0cdac1\|The fundamental shift was moving away from merely observing "what we do" to analyzing "how we process" information computationally.]]

## B - The Pioneers

1. **George Miller**: Proved there are strict limits to human mental processing capacity, famously encapsulated in his paper on the "Magical Number Seven".

2. **Noam Chomsky**: Completely rejected the Behaviorist idea that language is just a learned habit. Instead, he argued that language is an innate, computational system governed by strict, pre-existing rules.

3. **David Marr**: Created the definitive framework for analyzing cognitive systems via three distinct levels: Computational, Algorithmic, and Implementational.

4. **Herbert Simon and Allen Newell** were the first to build "Artificial Intelligence" programs capable of solving logic theorems. They made early, bold predictions about ==machines playing chess==. When programming game state classes or move generation rules in a modern chess engine, you are essentially building upon the symbolic logic foundation they championed. Interestingly, when they submitted their groundbreaking "Logic Theorist" program to an academic journal, it was rejected purely because ==the editors had no existing policy for publishing a paper authored by software==.

## C - Famous Debates 

- **Chomsky vs. Skinner:** Debated whether language is innate or learned through reinforcement, effectively ending Behaviorism's dominance.

- **Symbols vs. Neurons (Connectionism):** Debated whether the mind operates strictly like a logic machine (Symbolic AI) or as a statistical network (Neural Networks).

- **Nature vs. Nurture:** Questioned how much of our mental "hardware" is pre-programmed versus learned dynamically from environmental data. Today, modern cognitive science uses Bayesian Models to reconcile these views by combining structured innate knowledge with statistical learning.

#### 1 - Chomsky vs. Skinner
##### The Chomskyan Approach - Pure Rule-Based Logic
This approach relies entirely on innate, universal structures and ==hard-coded rules==. For instance, if you program a self-driving car with a strict rule stating "Any foreign object is an obstacle," the car will suddenly brake for a harmless plastic bag floating in the wind. While this ensures high safety, it makes the system incredibly brittle and inefficient in the real world.

##### The Skinnerian Approach - Pure Reinforcement Learning
This approach relies entirely on ==reinforcement learning and trial-and-error==. A self-driving car built purely on this philosophy would learn strictly through rewards and punishments. It would practically have to hit pedestrians multiple times to finally learn that "Collision is a negative reinforcement". While highly adaptive, it is ethically and practically impossible to deploy.

| Approach   | Philosophy                                        | Result                                                                |
| ---------- | ------------------------------------------------- | --------------------------------------------------------------------- |
| Chomskyan  | innate, universal structures and hard-coded rules | High safety, but extremely low efficiency and ”brittle” behavior.     |
| Skinnerian | reinforcement learning and trial-and-error        | Adaptive, but ethically and practically impossible in the real world. |

#### 2 - Nature vs. Nurture

##### Nature/Innatism
Representing Chomsky's view, "Nature" dictates that humans are born with a biological blueprint—==the mind is not a blank slate, but rather comes with "pre-installed software" and basic, non-negotiable physical constraints.== This concept is supported by verses emphasizing innate knowledge, specifically Surah Al-Baqarah (2:31) and Surah Ar-Room (30:30).

##### Nurture/Behaviorism
Representing Skinner's view, =="Nurture" treats the mind as a "Tabula Rasa" (blank slate) where all intelligence is formed via stimulus, response, and reinforcement.== In modern AI, this philosophy is the foundation for Large Language Models (LLMs) that develop intelligence purely by training on massive datasets. This idea is paired with Surah An-Nahl (16:78), and the core takeaway is summarized as: "Rules provide safety; Data provides intelligence".

## D - Bayesian Reconciliation

Modern science bridges the *Nature* vs. *Nurture* gap using **Bayes' theorem**: $Knowledge=\underline{Prior}+\underline{Evidence}$. 

- "Priors" represent Nature (innate expectations of the world), while 
- "Evidence" represents Nurture (new environmental data). 
- The mind continuously processes new evidence to update its innate priors.

##### The Hybrid Future
- Intelligence requires both innate structure and flexible learning.
- The future of AI is "Neuro-Symbolic," merging logical rules with the power of statistical neural networks. 
- Ultimately, the ==Cognitive Revolution established the mind as an information-processing system==, solidifying the metaphor that mental processes are software, while the physical brain is the hardware.
- We are born with the ”rules” to learn, but the ”content” comes from the world.

# **Part 2: Marr’s Tri-Level Hypothesis**

David Marr argued that you cannot understand complex systems (like human vision) just by staring at neurons. You have to analyze the system across ***three distinct, coupled levels***.


1. **Level 1: The Computational Level:** This level answers =="What"== the system is doing and =="Why"==. It defines the ==specific problem being solved and the logical constraints governing the solution.== For example, the goal of a cash register is addition, which is bound by mathematical logic like associativity and commutativity

2. **Level 2: The Algorithmic Level:** This level answers =="How"== the theory is executed. It defines the exact data structures and representations used (e.g., using Arabic numerals instead of Roman numerals) and the step-by-step procedures used to transform inputs into outputs. For addition, this could be the specific algorithm of "carrying" digits.

3. **Level 3: The Implementational Level):** This level looks at the ==physical "Hardware"==. It answers how the algorithm is physically realized. For humans, the substrate involves neurotransmitters and synaptic weights. For computers, analyzing how logic gates, CMOS architecture, and silicon transistors physically process voltages falls entirely under this implementational tier.

The levels operate under ***"Multiple Realizability"***—meaning the exact same computational goal (addition) can be solved using totally different algorithms, and run on entirely different physical hardware (brain tissue vs. silicon chips). Marr pushed for a top-down approach: researchers must understand the computational goal first before trying to map the physical circuitry. 

| Level            | Key Question | Main Concern                 |
| ---------------- | ------------ | ---------------------------- |
| Computational    | What / Why?  | Task Goals & Logic           |
| Algorithmic      | How?         | Data Structures & Procedures |
| Implementational | Where?       | Physical Hardware / Biology  |
