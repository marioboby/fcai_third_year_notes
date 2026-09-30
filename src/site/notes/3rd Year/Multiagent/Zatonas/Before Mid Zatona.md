---
{"dg-publish":true,"permalink":"/3rd-year/multiagent/zatonas/before-mid-zatona/","dg-note-properties":{}}
---


# Lecture 1

### **1. Fundamental Definitions**

- **Intelligence:** The ability of systems to perform tasks that typically require human or natural intelligence.
    
- **Agent:** A computer system situated in an environment that is capable of autonomous action within that environment to achieve delegated objectives.
    
- **Rational Agent:** An agent that acts to achieve the best outcome, or the best _expected_ outcome when uncertainty is involved. It uses knowledge representation and reasoning to reach good decisions.
    
- **Multiagent System (MAS):** A system consisting of several interacting agents that typically exchange messages through a computer network infrastructure.
    

### **2. The Four Categories of AI**

The definitions of AI are organized into four main approaches:

- **Thinking Humanly:** The automation of activities we associate with human thinking, such as learning, problem-solving, and decision-making.
    
- **Thinking Rationally:** The study of mental faculties using computational models, making it possible to perceive, reason, and act.
    
- **Acting Humanly:** The art of creating machines that perform functions requiring intelligence when performed by people (e.g., the Turing Test).
    
- **Acting Rationally:** The study of the design of intelligent agents and intelligent behavior in artifacts.
    

### **3. Strong AI vs. Weak AI (Comparison)**

- **Strong AI:**
    
    - _Definition:_ Machines that can genuinely reason, solve problems, and are conscious and self-aware.
        
    - _Classification Map:_ Maps to "Thinking rationally" and "Acting rationally".
        
    - _Current Status:_ Currently, no intelligent agent of this type has been created due to a bottleneck in brain science.
        
- **Weak AI:**
    
    - _Definition:_ Machines that only _appear_ intelligent but do not have real intelligence, self-awareness, or the ability to truly reason and solve problems.
        
    - _Classification Map:_ Maps to "Thinking like human beings" (e.g., Watson, AlphaGo) and "Acting like human beings" (e.g., humanoid robots like Atlas).
        

### **4. Key Characteristics of Agents (Properties)**

For an agent to be considered intelligent, it generally exhibits these four properties:

- **Autonomy:** The agent operates without direct human intervention.
    
- **Reactivity:** The agent perceives its environment and responds to changes occurring within it.
    
- **Proactiveness:** The agent exhibits ==goal-directed behavior==, taking the initiative to achieve its delegated objectives rather than just passively reacting to the environment.
    
- **Social Ability:** The agent is capable of interacting with other agents (and possibly humans) to satisfy its design objectives or common goals.
    

### **5. "Consists Of" & System Architecture**

- **An Agent's Interaction Loop consists of:**
    
    - **Sensors:** Used to receive _percepts_ from the environment.
        
    - **Effectors:** Used to execute _actions_ upon the environment.
        
- **A Multiagent System consists of:**
    
    - Several interacting agents, which can be software entities, robots, or humans.
        
    - _Key Idea:_ These components work together to solve problems that are beyond the capabilities of a single agent.

---
# Lecture 2
## **1. Fundamental Definitions**

- **Intelligent Agent:** Anything that can be viewed as perceiving its environment through sensors and acting upon that environment through actuators.
    
- **Percept:** The agent's perceptual inputs at any given instant.
    
- **Percept Sequence:** The complete history of everything the agent has perceived.
    
- **Agent Function:** A mathematical mapping from any given percept sequence to an action.
    
- **Task Environment:** Includes all relevant external factors and conditions that impact an agent's behavior and ability to perform.
    
- **Rational Agent:** For each possible percept sequence, it selects an action expected to maximize its performance measure, given the evidence provided by the percept sequence and built-in knowledge. Simply put: it does the right thing.
    
- **Performance Measure:** An objective criterion used to evaluate the success of an agent's behavior.
    

---

## **2. "Consists Of" & Core Architectures**

### **The Agent Architecture**

An agent is fundamentally composed of two parts:

- **Agent = Architecture + Program**.
    
    - _The Program:_ Runs on the physical architecture to produce the agent function.
        

### **The 4 Pillars of Rationality** Rationality is evaluated based on four components:

1. **Performance measure:** How to know the agent succeeded.
    
2. **Prior knowledge:** What the agent knows about the environment beforehand.
    
3. **Actions:** The capabilities the agent can perform.
    
4. **Percept sequence:** What the agent has perceived to date.
    

### **The PEAS Framework (Specifying the Task Environment)** Used to define the relevant external factors impacting an agent:

- **P - Performance:** The goal context or objective criteria (e.g., safe, fast, distance traveled, battery life).
    
- **E - Environment:** The physical or virtual surroundings the agent interacts with (e.g., roads, traffic, pedestrians).
    
- **A - Actuators:** The mechanisms used for interaction/action (e.g., steering wheel, motors, cleaning mechanism).
    
- **S - Sensors:** How the agent perceives the world (e.g., cameras, GPS, keyboard).
    

---

## **3. Key Comparisons**

### **Table 1: Types of Physical Agents** 

| Agent Type        | Sensors                             | Actuators                                 |
| :---------------- | :---------------------------------- | :---------------------------------------- |
| **Human Agent**   | Eyes, ears, and other organs.       | Hands, legs, mouth, and other body parts. |
| **Robotic Agent** | Cameras and infrared range finders. | Various motors.                           |

### **Table 2: Rationality Distinctions** 

| Concept         | Characteristics                                                                                                                              |
| :-------------- | :------------------------------------------------------------------------------------------------------------------------------------------- |
| **Rationality** | Maximizes the _expected_ outcome based on what it has perceived. Involves learning from perceived information to avoid repeating mistakes.   |
| **Omniscience** | Having total actual wisdom. Rational agents are _not_ omniscient because percepts rarely supply all relevant information.                    |
| **Perfection**  | Maximizes the _actual_ outcome. Rationality is about doing the best with what you have, not necessarily achieving a flawless actual outcome. |

### **Table 3: Task Environment Characteristics** 

| Characteristic Focus | Type 1                                                                                               | Type 2 (The Contrast)                                                                                                                                                        |
| :------------------- | :--------------------------------------------------------------------------------------------------- | :--------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **Observability**    | **Fully Observable:** Sensors detect all aspects required to choose an action (perfect information). | **Partially Observable:** Parts of the environment are inaccessible; agent must make informed guesses.                                                                       |
| **Certainty**        | **Deterministic:** The next state depends _only_ on the current state and the agent's action.        | **Stochastic:** Non-deterministic; aspects are beyond the agent's control.                                                                                                   |
| **Time/Planning**    | **Episodic:** Current action choice does _not_ depend on previous actions (sporadic).                | **Sequential:** Current choice affects future actions; requires planning ahead.                                                                                              |
| **Stability**        | **Static:** The environment doesn't change while the agent deliberates.                              | **Dynamic:** The environment changes during deliberation. _(Note: **Semi-dynamic** means the environment itself doesn't change, but the performance score drops over time)_. |
| **State Space**      | **Discrete:** A limited, distinct, clearly defined number of percepts and actions.                   | **Continuous:** Features a range of values.                                                                                                                                  |
| **Population**       | **Single Agent:** Operating by itself in an environment.                                             | **Multiagent:** Many agents working together.                                                                                                                                |

---

# Lecture 3
## **1. Fundamental Definitions & Concepts**

- **Agent Function (with State):** An agent program can implement an agent function by maintaining an internal state, mapping from percept histories to actions: $f: P^* \rightarrow A$. When including the history of actions taken, it maps as $f: P^*, A^* \rightarrow S \rightarrow A$, where $S$ is the set of states.
    
- **Markovian State Space:** A state space where each internal state includes all information relevant to decision-making.
    
- **Perfect Recall:** A state space where each state includes the information about the percepts and actions that led to it.
    
- **Perfect Information:** A condition that requires Perfect Recall, Full Observability, and Deterministic Actions.
    
- **Utility Function:** A function that maps a state onto a real number, describing the associated degree of "happiness", "goodness", or "success".
    

---

## **2. "Consists Of" & Core Architectures**

**A Model-Based Agent consists of:**

- An internal representation of the world, often called a "model".
    
- Knowledge of "How the world evolves" (e.g., an overtaking car gets closer from behind).
    
- Knowledge of "What my actions do" (e.g., turning the wheel clockwise takes you right).
    

**A Learning Agent consists of:**

- **Performance element:** What was previously considered the whole agent; it takes sensor input and outputs actions.
    
- **Critic:** Evaluates how the agent is doing against a fixed performance standard.
    
- **Learning element:** Modifies the performance element for the future based on feedback.
    
- **Problem generator:** Suggests exploring new actions and tries to solve problems differently instead of just optimizing.
    

---

## **3. Key Comparisons**

### **Agents vs. Objects** 

| Feature      | Agents                                                                                                                    | Objects                                                                             |
| :----------- | :------------------------------------------------------------------------------------------------------------------------ | :---------------------------------------------------------------------------------- |
| **Autonomy** | Embody a stronger notion of autonomy; they decide for themselves whether to perform an action requested by another agent. | Possess a weaker notion of autonomy compared to agents.                             |
| **Behavior** | Capable of flexible, reactive, proactive, and social behavior.                                                            | The standard object model has nothing to say about such flexible types of behavior. |
| **Control**  | A multiagent system is inherently multi-threaded, meaning each agent is assumed to have at least one thread of control.   | Typically lack independent, multi-threaded control in standard models.              |

### **Four Basic Agent Types (Pros & Cons)** 

| Agent Type             | Core Characteristics                                                                                                                  | Pros                                                                                                                                            | Cons                                                                                                                                                         |
| :--------------------- | :------------------------------------------------------------------------------------------------------------------------------------ | :---------------------------------------------------------------------------------------------------------------------------------------------- | :----------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **Simple Reflex**      | Direct mapping from perceptions to actions using condition-action rules. Has no memory; action depends only on the current percept.   | The simplest agent. Fast reaction times, making it well-suited for dynamic environments.                                                        | Limited intelligence; ignores percept history; fails in partially observable environments; prone to infinite loops, cannot learn or adapt to new situations. |
| **Goal-Based**         | Uses knowledge about a goal to guide actions through search and planning. Needs the current state and a goal state to make decisions. | Goal-oriented behavior. Can solve complex problems requiring planning and can flexibly adapt by replanning.                                     | Computationally expensive. Defining usable goals is challenging, and it struggles with incomplete information. Cannot learn or adapt to new situations.      |
| **Utility-Based**      | Uses a utility function to evaluate goals based on factors like speed and safety. Focuses on degrees of happiness or success.         | Makes rational decisions that maximize expected utility. Can handle uncertainty via probabilities and manage complex, time-variant preferences. | Defining an accurate utility function is challenging. Calculating expected utility is computationally expensive. Utility functions can be subjective.        |
| **Model-based Reflex** | Maintains an internal state (model) to track parts of the world not currently visible.                                                | Uses transition and sensor models to predict how the world evolves and how actions affect it.                                                   | More complex than simple reflex; requires constant updating of the internal world state.                                                                     |
| **Learning**           | Can be applied to any architecture to improve performance over time.                                                                  | Includes a learning element, critic (evaluator), and problem generator for exploration.                                                         | Exploration can be costly in the short term (e.g., fewer tips for a taxi driver while experimenting).                                                        |
### **Alternative Architectural Perspectives**

| Architecture Category          | Characteristics                                                                                                               |
| :----------------------------- | :---------------------------------------------------------------------------------------------------------------------------- |
| **Reactive**                   | Behavior-based architectures, such as simple reflex agents.                                                                   |
| **Deliberative (Intentional)** | Involves thoughtful and planned action, reasoning before acting, and relying on internal knowledge-based models of the world. |
| **Hybrid**                     | Combines elements of both reactive and deliberative architectures.                                                            |
| **Learning**                   | Utilizes systems like Reinforcement Learning and Deep Learning.                                                               |

---

# Lecture 4

### **1. Fundamental Definitions**

- **Graph:** A mathematical structure used to model pairwise relations between distinctive entities, made up of vertices (nodes) connected by edges (links/arcs).
    
- **Tree:** A directed, connected graph without cycles (acyclic) where any two vertices are connected by exactly one path, and nodes have a single parent "In Graph Theory, it's ***Undirected***".
    
- **State Space Graph:** A mathematical representation of a search problem where nodes are abstracted world configurations and arcs represent action results.
    
- **Search Tree:** A "what if" tree of plans and their outcomes where the root is the start state, children are successors, and nodes represent the _plans_ that achieve specific states.
    
- **Solution:** A sequence of actions (a plan) that transforms the start state into a goal state.
    

---

### **2. "Consists Of" & Core Frameworks**

**A Formal Problem consists of 5 components:**

1. **Initial state:** The starting configuration.
    
2. **Possible set of actions:** What the agent can do.
    
3. **Transition model:** A description of what each action does and the resulting consequence.
    
4. **Goal test:** Determines whether a given state is a goal state (or belongs to a set of possible goal states).
    
5. **Path cost:** The numeric cost associated with the sequence of actions.
    

**Solving a Problem formally consists of 4 phases:**

1. **Goal formulation:** Defining the objective.
    
2. **Problem formulation:** Defining the states and actions.
    
3. **Search:** Finding a solution sequence.
    
4. **Execution:** Carrying out the planned actions.
    

**A Planning Agent consists of/relies on:**

- Asking "what if" to consider how the world would be.
    
- Making decisions based on the hypothesized consequences of its actions.
    
- A model of how the world evolves in response to those actions.
    
- Formulating a specific goal test.
    

---

### **3. Key Comparisons**

**Table 1: Levels of Agent Representation**

|**Representation Level**|**Description**|**Characteristics & Usage**|
|---|---|---|
|**Atomic**|A state is a "black box" with no internal structure.|Used in search, game-playing, and Hidden Markov Models (HMM). Standard for problem-solving agents.|
|**Factored**|States are represented as a vector of attribute values (variables or properties).|Example: State = [Location: Room A, SoC: 80%]. Frequently used by planning agents.|
|**Structured**|States include complex relationships between objects.|Actions can affect specific variables or relationships. Frequently used by planning agents.|

**Table 2: Graph vs. Tree**

|**Feature**|**Graph**|**Tree**|
|---|---|---|
|**Parenting**|Nodes can have multiple parents.|Nodes have a single parent.|
|**Cycles**|Can contain loops or cyclic paths.|Acyclic; strictly without cycles.|
|**Relationship**|A graph is not necessarily a tree.|Any tree is a graph.|
|**Conversion**|Can be turned into a tree by replacing undirected links with two directed links and avoiding loops.|The resulting structure after removing cycles from a graph search.|

**Table 3: State Space Graph vs. Search Tree**

|**Concept**|**Characteristics**|
|---|---|
|**State Space Graph**|In this graph, each state occurs only _once_. It is rarely built fully in memory because it is usually too large.|
|**Search Tree**|Built on demand. A single node in a search tree represents an _entire path_ (plan) mapped out in the state space graph. Because different paths can lead to the same state, there is lots of repeated structure in the tree.|

---

# Lecture 5

### **1. Fundamental Definitions**

- **Search Space:** The set of objects among which we search for the solution.
    
- **Goal Condition:** Characteristics of the object we want to find in the search space.
    
- **Solution:** A sequence of actions to move from the start state to the goal state.
    
- **Optimal Solution:** The solution that has the lowest path cost among all possible solutions.
    
- **Completeness:** Whether an algorithm is guaranteed to find a solution when one exists and correctly report failure when there is none.
    
- **Optimality:** Whether a strategy finds the optimal solution (lowest path cost).
    
- **Time Complexity:** How long it takes to find a solution (measured in seconds, states, or actions).
    
- **Space Complexity:** How much memory is needed to perform the search.
    

---

### **2. "Consists Of" & Core Frameworks**

**A Search Problem consists of:**

1. **A state space:** Configurations of the world.
    
2. **A successor function:** World dynamics (with actions and costs).
    
3. **A start state**
    
4. **A goal test:** Characteristics to find.
    

**A Tree Node consists of (Infrastructure for Search Algorithms):**

- **STATE:** The state in the state space to which the node corresponds.
    
- **PARENT:** The node in the search tree that generated this node.
    
- **ACTION:** The action that was applied to the parent to generate the node.
    
- **PATH-COST:** The cost ($g(n)$) of the path from the initial state to the node, as indicated by the parent line.
    

**Tree Complexity Parameters consist of:**

- **b:** Maximum branching factor.
    
- **d:** Depth of the optimal solution.
    
- **m:** Maximum depth of the state space.
    

---

### **3. Key Comparisons**

**Table 1: State Space Graph vs. Search Tree (Revisited for this Lecture)**

| Feature                 | State Space Graph                                               | Search Tree                                   |
| :---------------------- | :-------------------------------------------------------------- | :-------------------------------------------- |
| **Usage**               | Can be used directly by developing a suitable search technique. | Often easier to build first before searching. |
| **Nodes represent**     | Abstracted world configurations (states).                       | Plans for reaching states.                    |
| **Cost representation** | Arcs represent actions and costs.                               | Plans have costs (the sum of action costs).   |

**Table 2: Search Strategy Types**

| Strategy Type | Characteristics |
| :--- | :--- |
| **Uninformed (Blind)** | Uses only the information available in the problem definition. |
| **Informed** | Uses domain-specific knowledge beyond the problem definition (heuristics, metaheuristics). |
