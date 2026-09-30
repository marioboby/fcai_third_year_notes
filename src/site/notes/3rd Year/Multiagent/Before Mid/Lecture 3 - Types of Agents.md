---
{"dg-publish":true,"permalink":"/3rd-year/multiagent/before-mid/lecture-3-types-of-agents/","dg-note-properties":{}}
---



## Recap of Lecture 2
### **The Four Pillars of Rationality**

Rationality is defined by four core components that determine an agent's success:

- **Performance Measures**: The criteria used to determine if an agent has successfully completed its task.
- **Prior Knowledge**: What the agent knows about its environment beforehand.
- **Actions**: The specific set of actions the agent is capable of performing.
- **Percept Sequence**: The complete history of everything the agent has perceived to date.

### **Environment Properties**

Environments are categorized by several key characteristics that influence agent design:

- **Observability**: Whether the environment is fully or partially observable.
- **Certainty**: Whether outcomes are deterministic or stochastic.
- **Episodicity**: Whether the environment is episodic or sequential.
- **Dynamics**: Whether the environment is static or dynamic.
- **Continuity**: Whether the environment is discrete or continuous.
- **Complexity**: Whether there is a single agent or multiple agents involved.

---
### Agents Vs Objects

- **Autonomy**: Agents embody a stronger sense of autonomy than objects. Specifically, agents decide for themselves whether to perform an action requested by another agent.
    <br>
- **Behavioral Flexibility**: Agents are capable of flexible behaviors, which include being reactive, proactive, and social. The standard object model does not address these types of behaviors.
    <br>
- **Control Threads**: A multiagent system is inherently multi-threaded. Each agent is assumed to possess at least one of its own threads of control
---
### **Agent Architectures**

The lecture details four basic agent types, ordered by increasing complexity and generality:

#### **1. Simple Reflex Agents**

- **Core Principle**: These agents use a direct mapping from current perceptions to actions based on "condition-action" rules, ignoring percept history "Memoryless".
<br>
- **Pros**: They are simple to design, fast, and responsive in dynamic environments.
  <br>
- **Cons**: 
	- They have limited intelligence, 
	- cannot handle partially observable environments, 
	- cannot learn or adapt to new situations, 
	- the dynamics of the interactions between the different behaviors become too complex to understand. 
	- short term (local environment ) reactive system, 
	- and are prone to infinite loops if they lack memory.
  
A simple reflex agent can fall into an **infinite loop** when it operates in a **partially observable environment**.

A specific case presented in the lecture involves a **vacuum cleaner agent** that does not observe its current location:

- **The Scenario**: Suppose the vacuum cleaner can only perceive if the current square is "Dirty" or "Clean" but cannot see if it is in "Location A" or "Location B".
    
- **The Problem**: If the current location is "Clean," the agent must decide whether to move Left or Right.
    
- **The Loop**: If the agent's rule is to move Right when at Location A and Left when at Location B, but it cannot see its location, it might perpetually move back and forth between the two squares if the rules are not carefully defined for the limited percepts.
    
A possible solution to break such infinite loops in simple reflex agents is to **randomize the action**.

---
#### **2. Model-based Reflex Agents**

- **Core Principle**: These agents maintain an internal state (a "model") to keep track of the parts of the world they cannot see currently.
- **Functionality**: They use a transition model to predict how the world evolves and a sensor model to understand how the world state is reflected in their percepts.

---

The main difference between a **Simple Reflex Agent** and a **Model-based Reflex Agent** lies in how they represent and interact with their environment:

- **Reliance on Current Percepts vs. Internal State**:
    - **Simple Reflex Agents** select actions based only on the current percept, completely ignoring any history of past perceptions. They are considered memoryless systems.
       <br>
    - **Model-based Reflex Agents** maintain an internal state (or "model") that tracks parts of the environment not currently visible. This state is updated based on the history of percepts and actions.
      <br>
- **Environmental Understanding**:
    - **Simple Reflex Agents** use a direct mapping from perceptions to actions via condition-action rules. They lack an internal world model and cannot reason about how the world evolves.
       <br>
    - **Model-based Reflex Agents** incorporate two types of knowledge to reason about the environment: a **transition model**, which describes how the world changes over time and how the agent's actions affect it, and a **sensor model**, which describes how the state of the world is reflected in its percepts.
       <br>
- **Handling Partial Observability**:
    - **Simple Reflex Agents** cannot handle partially observable environments because they only react to what they can see at the present moment. This can lead to infinite loops if the current percept does not provide enough information to decide on a unique action.
       <br>
    - **Model-based Reflex Agents** are designed to "keep track of the world," allowing them to function more effectively in partially observable environments by using their internal model to fill in missing information.
       <br>
- **Architecture and Complexity**:
    - **Simple Reflex Agents** are the simplest kind of agent to design and implement, offering fast reaction times but limited intelligence.
       <br>
    - **Model-based Reflex Agents** are more complex as they must constantly update their internal representation of the world as new percepts arrive.

---
#### **3. Goal-based Agents**

- **Core Principle**: These agents act to achieve specific, explicit goals.
- **Functionality**: They use reasoning, search, and planning to choose actions that lead to a desired goal state.
- **Cons**: They can be computationally expensive due to the need for planning and may struggle with incomplete information.
---
#### **4. Utility-based Agents**

- **Core Principle**: These agents use a utility function to map states to a real number representing "happiness" or "success".
- **Pros**: They allow for more rational decision-making by maximizing expected utility and can handle trade-offs between conflicting goals (e.g., speed vs. safety).

### **Learning Agents**

All the above architectures can be transformed into learning agents. A learning agent consists of four main components:

- **Learning Element**: Responsible for making improvements to the agent.
- **Performance Element**: The part that selects external actions (previously the entire agent).
- **Critic**: Evaluates how the agent is doing against a fixed performance standard.
- **Problem Generator**: Suggests exploratory actions that will lead to new experiences.

---
The following table compares the different agent architectures presented in the lecture, ordered by their increasing level of generality and complexity.

| Agent Type             | Core Principle                                                                         | Key Characteristics & Pros                                                                                            | Cons & Limitations                                                                                                                                           |
| ---------------------- | -------------------------------------------------------------------------------------- | --------------------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| **Simple Reflex**      | Maps current percepts directly to actions using condition-action rules.                | Simple to design; fast reaction times; memoryless.                                                                    | Limited intelligence; ignores percept history; fails in partially observable environments; prone to infinite loops, cannot learn or adapt to new situations. |
| **Model-based Reflex** | Maintains an internal state (model) to track parts of the world not currently visible. | Uses transition and sensor models to predict how the world evolves and how actions affect it.                         | More complex than simple reflex; requires constant updating of the internal world state.                                                                     |
| **Goal-based**         | Acts to achieve specific, explicit goals using search and planning.                    | Goal-oriented; flexible behavior; can solve complex problems by predicting consequences.                              | Planning is computationally expensive; struggles with incomplete information; cannot adapt to new situations without learning.                               |
| **Utility-based**      | Uses a utility function to measure the "happiness" or "success" of a state.            | Makes rational decisions by maximizing expected utility; handles trade-offs (e.g., speed vs. safety) and uncertainty. | Challenging to define accurate utility functions; high computational cost for calculating expected utility.                                                  |
| **Learning**           | Can be applied to any architecture to improve performance over time.                   | Includes a learning element, critic (evaluator), and problem generator for exploration.                               | Exploration can be costly in the short term (e.g., fewer tips for a taxi driver while experimenting).                                                        |

---

### Another Perspective

- **Reactive Architectures (Behavior-Based Architectures)**: These are identified as corresponding to **simple reflex** agents.
  <br>
- **Deliberative (Intentional) Architectures**: These agents engage in thoughtful and planned action, using careful consideration and planning before acting. They rely on:
  <br>
    - **Reasoning and Planning**: This involves analyzing the situation, predicting consequences, and evaluating options based on goals.
      <br>
    - **Internal Models**: They use internal models to represent and reason about their environment.
      <br>
    - **Knowledge-Based**: They often utilize knowledge representation and logical inference for decision-making.
      <br>
- **Hybrid Architectures**: Architectures that combine different approaches.
  <br>
- **Learning Architectures**: These include approaches such as **Reinforcement Learning** and **Deep Learning**.