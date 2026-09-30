---
{"dg-publish":true,"permalink":"/3rd-year/multiagent/before-mid/lecture-1-intro/","dg-note-properties":{}}
---


## 1. Defining Artificial Intelligence (AI)

The concept of AI is traditionally categorized into four distinct approaches based on whether the system aims to mimic human behavior or act ideally (rationally):

- **Thinking Humanly**: The automation of activities associated with human thought, such as learning, problem-solving, and decision-making.
<br>
- **Thinking Rationally**: The study of mental faculties and computations that enable systems to perceive, reason, and act using computational models.
<br>
- **Acting Humanly**: The art of building machines that perform functions requiring human intelligence (e.g., passing the Turing Test).
<br>
- **Acting Rationally**: The study of computational intelligence and the design of intelligent agents.

To see more, [[3rd Year/Multiagent/Expanded Explanations/Lecture 1#Four Types of AI\|here]]

## 2. The Rational Agent Approach

An agent is essentially a computer system situated in an environment that is capable of taking autonomous action to achieve delegated objectives.

- **Mechanism**: Agents use **sensors** to receive percepts from their environment and **effectors** to perform actions back onto that environment.
<br>
- **Rationality**: A rational agent acts to achieve the best possible outcome. In situations with uncertainty, it acts to achieve the best _expected_ outcome. For instance, when engineering the decision-making logic for the state class in a Python chess game, the system evaluates board states to rationally pursue the optimal next move—a prime example of this kind of rational agency.
<br>
- Agents rely on knowledge representation and reasoning to reach these good decisions. Computer agents are specifically expected to operate autonomously, perceive their surroundings, persist over time, adapt to change, and actively pursue goals.


## 3. Key Characteristics of Intelligent Agents

For an agent to be considered "intelligent," it typically exhibits the following properties:

- **Autonomy**: The ability to operate without direct human intervention.
- **Proactiveness**: Goal-directed behavior where the agent takes the initiative to achieve its objectives.
- **Reactivity**: The ability to perceive the environment and respond to changes in a timely fashion.
- **Social Ability**: The capacity to interact with other agents or humans to satisfy common goals or delegated objectives.


## 4. Multiagent Systems (MAS)

When problems become too complex for a single agent, Multiagent Systems are utilized.

- **Definition**: A system consisting of several agents (software entities, robots, or humans) that interact with each other.
<br>
- **Interaction**: This interaction typically occurs by exchanging messages across a computer network infrastructure.

## 5. Types of AI: Strong vs. Weak

AI systems are fundamentally divided by their true capacity for understanding and self-awareness:

### Strong AI

- **Concept**: Machines that are conscious, self-aware, and can genuinely reason to independently work out optimal solutions.
    <br>
- **Current Status**: Classifies as "Thinking rationally" or "Acting rationally". Currently, no such agents exist due to bottlenecks in brain science.
    

### Weak AI

- **Concept**: Machines that _appear_ intelligent and can automate complex tasks (like natural language processing or image recognition), but lack real intelligence, reasoning, or self-awareness, where intelligent machines cannot really reason and solve problems..
    <br>
- **Current Status**: Classifies as "Thinking like human beings" (e.g., IBM Watson, AlphaGo) or "Acting like human beings" (e.g., Boston Dynamics' Atlas, humanoid robots).