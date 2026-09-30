---
{"dg-publish":true,"permalink":"/3rd-year/multiagent/expanded-explanations/lecture-1/","dg-note-properties":{}}
---


## Four Types of AI

### The Two Dimensions

1. **Top vs. Bottom (Thinking vs. Acting):**
    
    - **Thinking (Internal):** Focuses on the reasoning process, the "mind," and how inferences are made.
        <br>
    - **Acting (External):** Focuses on behavior, output, and the results of actions, regardless of how they were computed.
        
2. **Left vs. Right (Humanly vs. Rationally):**
    
    - **Humanly (Empirical):** The standard of success is "people." Does it do it like we do? (Even if humans make mistakes).
        <br>
    - **Rationally (Mathematical):** The standard of success is "optimality." Does it do the _correct_ thing to achieve the best outcome?
        

---

### 1. Acting Humanly (The Turing Test Approach)

- **Goal:** To create a system that acts so much like a person that you cannot tell the difference.
    <br>
- **The Test:** The famous **Turing Test** (1950). A human interrogator types questions to a computer and a human. If the interrogator cannot determine which is which, the computer has passed.
    <br>
- **Required Capabilities:** To pass, the computer doesn't need to be "conscious," but it needs specific skills:
    
    - _Natural Language Processing_ (to communicate).
        
    - _Knowledge Representation_ (to store what it knows).
        
    - _Automated Reasoning_ (to answer questions and draw new conclusions).
        
    - _Machine Learning_ (to adapt to new circumstances).
        <br>
- **Slide Example:** The slides classify "Acting like human beings" as **Weak AI**. Examples given are humanoid robots (like **Atlas** from Boston Dynamics) or **iRobot** vacuums—machines that mimic human motion or function.
    
### 2. Thinking Humanly (The Cognitive Modeling Approach)

- **Goal:** To understand and mimic the actual workings of the human mind. It is not enough to just get the right answer; the _steps_ taken to get there must match the steps a human brain takes.
    <br>
- **Method:** This requires looking inside the human mind through **introspection** (catching our own thoughts) or **psychological experiments**.
    <br>
- **Connection to Science:** This is the bridge between AI and **Cognitive Science**.
    <br>
- **Slide Example:** The slides classify this as **Weak AI**. Examples include **Watson** and **AlphaGo**. Even though they solve complex problems, they are "weak" in this specific definition because they don't actually _think_ or experience consciousness the way a human does; they simulate the result of thinking.
    
### 3. Thinking Rationally (The "Laws of Thought" Approach)

- **Goal:** To codify "right thinking" using logic.
    <br>
- **Basis:** This is based on **Aristotle’s syllogisms** (e.g., "Socrates is a man; all men are mortal; therefore, Socrates is mortal").
    <br>
- **The Problem:**
    
    1. It is hard to take informal, fuzzy knowledge ("It looks like it might rain") and turn it into strict logical notation.
        <br>
    2. There is a difference between solving a problem "in principle" and doing it in practice (computational exhaustion).
        
    <br>
- **Slide Status:** The slides classify this as **Strong AI**, noting that _no intelligent agent of this type currently exists_ due to bottlenecks in brain science. We cannot yet perfectly model the brain's logical processes physically.
    
### 4. Acting Rationally (The Rational Agent Approach)

- **Goal:** To act in a way that achieves the best expected outcome.
    <br>
- **Why this is the focus of the course:**
    <br>
    - It is more general than "Thinking Rationally." (Example: Pulling your hand away from a hot stove is a reflex action—it is "rational" because it saves your hand, but it doesn't involve "thinking" or logic).
        <br>
    - It is scientifically easier to test than "Human" approaches. We can mathematically measure if an agent achieved a goal (Rationality), but it is subjective to measure if it "thought like a human."
        <br>
- **Slide Status:** This is the definition of **Computational Intelligence** used in the course. The agent simply uses its sensors and effectors to maximize its performance measure.

|                               | **Humanly (Like People)**                                    | **Rationally (Ideally/Logically)**                                   |
| ----------------------------- | ------------------------------------------------------------ | -------------------------------------------------------------------- |
| **Thinking**<br><br>(Process) | **Cognitive Science**<br><br>_"Does it think like a brain?"_ | **Laws of Thought**<br><br>_"Does it follow strict logic?"_          |
| **Acting**<br><br>(Behavior)  | **Turing Test**<br><br>_"Can it fool a person?"_             | **Rational Agent (This Course)**<br><br>_"Did it achieve the goal?"_ |
