---
{"dg-publish":true,"permalink":"/3rd-year/reasoning/before-mid/lecture-1-intro/","dg-note-properties":{}}
---


### 1. The Knowledge Progression (From Data to Wisdom)

This section explains how raw facts are transformed into deep understanding.

- **Data:** This is the starting point, defined as a collection of disconnected facts with limited utility.
 
    - _Example:_ "It is raining".
    <br>
- **Information:** When data is organized and analyzed, relationships among facts are established, giving the data meaning. Information answers the questions "who", "what", "where", and "when".

    - _Example:_ "The temperature dropped 15 degrees and then it started raining".
    <br>
- **Knowledge:** This emerges from the interpretation and evaluation of information, uncovering relationships among patterns. It provides answers to "how".

    - _Example:_ "If the humidity is very high and the temperature drops, then atmospheres is unlikely to hold the moisture, so it rains".
    <br>
- **Wisdom:** The highest level of understanding, which grasps the underlying principles of the knowledge. It answers "why" things happen.

    - _Example:_ Understanding all the complex interactions between evaporation, air currents, temperature changes, and rain.

![Pasted image 20260226234453.png](/img/user/imgs/Pasted%20image%2020260226234453.png)

---
### 2. Categories and Types of Knowledge

Knowledge is broadly categorized into two major types: **Tacit** and **Explicit**.

- **Explicit Knowledge** "مكتسبة"
    - This is considered **formal** knowledge.
    - It emerges from the interpretation or evaluation of information.
    - This knowledge provides answers as to "**how**" relationships among patterns are identified and understood (e.g., "If the humidity is very high and the temperature drops, then atmosphere is unlikely to hold the moisture, so it rains.").
    - It is the stage that follows information, which is established relationships among disconnected facts (answering "who," "what," "where," and "when").
  <br>
- **Tacit Knowledge** "فطرية"
    - This is categorized as **informal** or **implicit** knowledge.
    - It relates to the highest level of understanding, which is **Wisdom**.
    - Wisdom is the "pinnacle of understanding" that uncovers the underlying principles and encompassing understanding of all interactions, providing answers as to "**why**".

![Pasted image 20260331195634.png](/img/user/imgs/Pasted%20image%2020260331195634.png)

Furthermore, cognitive psychologists classify knowledge into specific types:

- **Declarative Knowledge:** Explicit knowledge that has been articulated. It is knowing "that something is true or false" (e.g., "A car has four tyres"). It deals with representing facts, concepts, objects, semantic nets, and descriptive models.
<br>
- **Procedural Knowledge:** Knowing "how to do something" (e.g., finding someone's age to see who is older). It focuses on the tasks required to reach a goal, such as procedures, rules, and models. Experts debate whether it is closer to tacit knowledge (manifested in doing but hard to explain) or declarative knowledge (describing a method).
<br>
- **Strategic Knowledge:** This is generally considered a subset of declarative knowledge.
---
### 3. Knowledge-Based Systems (Cognitive Systems)

A Knowledge-Based System (KBS), also referred to as a Cognitive System, ==These systems must represent objects, properties, categories, and relationships.== The core components and processes of a cognitive system include:

- **Metacognition:** This means "cognition about cognition" or "knowing about knowing" (the root "meta" means "beyond"). It involves higher-order thinking, such as knowing when and how to apply specific learning or problem-solving strategies.
<br>
- **Reasoning:** A fundamental process associated with thinking and making sense of things. It involves establishing facts, applying logic, making decisions, and understanding or generating natural language.
<br>
- **Learning:** Another core process, which involves making useful changes in the system's "mind" so it can perform tasks more effectively in the future. Simply put, it involves getting a right (or wrong) answer and storing it for future use.
---
### 4. Course Overview and Goals

Your course aims to cover the core methods of knowledge-based AI.

- **Key Methods:** Structured knowledge representation, memory organization, reasoning, and learning.

- **Key Tasks:** Classification, understanding, planning, explanation, diagnosis, and design.

- **Topics Covered:** Logic, Semantic Networks, Frames, Production Rules, various Machine Learning models (Nearest Neighbor, Decision Trees, SVMs), and Reasoning Methods (Predicate Logic, Bayesian, Rule-Based, and Fuzzy Theory) .

- **Grading:** Based on a project, assignments, quizzes, and presentation/discussion .
---
### 5. Introduction to Logic

Logic is defined as a language with concrete rules that allows for unambiguous communication and processing, without the ambiguity found in natural languages like English.

Logic consists of two main parts:

- **Syntax:** The rules for constructing legal sentences, detailing which symbols can be used and how they can be combined.
<br>
- **Semantics:** How we interpret and assign meaning to those sentences. A sentence can be syntactically valid and have meaning, even if it isn't factually true (e.g., "All lecturers are seven foot tall").
---
### 6. Propositional Calculus

This is a formal language used to represent and reason about the world. Its primitives include symbols, sentences, and semantics.

- **Symbols:** Denotes statements that are true or false (e.g., $P, Q, R$), alongside truth symbols (true, false) and connectives ($\land, \lor, \neg, \implies, \equiv$).
<br>
- **Well Formed Formulas (WFFs):** Valid sentences formed using specific composition rules. For example, the sentence $((P \land Q) \implies R) \equiv \neg P \lor \neg Q \lor R$ includes conjunctions, implications, negations, disjunctions, and equivalence .
---
### 7. Inference Rules

An inference rule is considered "sound" if its conclusion is always true when its premises are true. Key inference rules include:

- **Modus Ponens:** Affirming the premise. From $\alpha \implies \beta$ and $\alpha$, you infer $\beta$. (e.g., If A is true then B is true. A is true. Therefore B is true) .
  <br>
- **Modus Tollens:** Denying the premise. From $\alpha \implies \beta$ and $\neg \beta$, you infer $\neg \alpha$. (e.g., If P then Q. Not Q. Therefore not P) .
   <br>
- **And-Introduction:** If you have $\alpha_1$ through $\alpha_n$, you can infer $\alpha_1 \land \dots \land \alpha_n$.
   <br>
- **Double Negation:** From $\neg \neg \alpha$, you infer $\alpha$.
   <br>
- **Resolution:** From $A \lor B$ and $\neg B \lor C$, you infer $A \lor C$. If you have $l_1 \lor l_2$ and $\neg l_2 \lor l_3$, the truth value of $l_2$ will dictate whether $l_3$ or $l_1$ must be true.
   <br>
- **Equivalence Rules:** Two sentences are logically equivalent ($\alpha \equiv \beta$) if they are true in the exact same models, and these rules can also be used for inference.