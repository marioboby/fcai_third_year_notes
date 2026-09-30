---
{"dg-publish":true,"permalink":"/3rd-year/multiagent/before-mid/lecture-5-example-state-space-graph-for-the-two-cell-vacuum-world/","dg-note-properties":{}}
---


To solve a problem, an agent needs a formalized model. The slides break down this specific vacuum environment into several key components:

### 1. The State Space

The state space represents every possible configuration the environment can possibly be in. For the two-cell vacuum world, a state is defined by three variables:

- **Robot Location:** Is it in the Left room (0) or the Right room (1)?
    
- **Dirt in Left Room:** Is it clean (0) or dirty (1)?
    
- **Dirt in Right Room:** Is it clean (0) or dirty (1)?
    

Because there are three distinct variables that each have two possible conditions, the total number of states in this mathematical model is calculated as $2^n = 2^3 = 8$. The slides show a table outlining all 8 of these specific combinations (e.g., Robot is Left, Left is dirty, Right is clean).

### 2. The Action Space

This defines what the agent is actually capable of doing in any given state. In this environment, the agent has three available actions:

- **Move Left (L)**
    
- **Move Right (R)**
    
- **Suck dirt (S)**
    
![Pasted image 20260327001045.png](/img/user/imgs/Pasted%20image%2020260327001045.png)
### 3. The Transition Model (Successor Function)

The transition model (represented by the arrows in the state-space graph) describes the exact effect of each action. It defines what the "next state" will be after an action is taken.

- For example, if the agent is in a state where there is dirt in its current room, applying the "Suck" action transitions the environment to a new state where that room is now clean.
    
- If the agent is in the right room and applies the "Move Left" action, it transitions to a state where the robot's location is now the left room.
    

### 4. Goal States and Action Costs

To evaluate how well the agent is doing, two final components are needed:

- **Goal States:** The specific state (or states) the agent is trying to achieve. In the vacuum world, this is typically the state where both rooms have 0 dirt.
    
- **Action Cost:** A numerical penalty applied for taking actions, which could represent distance, power consumption, or time.

![Pasted image 20260327001122.png](/img/user/imgs/Pasted%20image%2020260327001122.png)

---
## Example of three-cell vacuum world

To find the total number of possible states in this vacuum-cleaner world, we need to consider two independent variables: the **location of the vacuum cleaner** and the **status of the rooms**.

### 1. Vacuum Cleaner Location

The vacuum cleaner can be in any one of the three rooms at a given time.

- **Locations:** Room A, Room B, or Room C.
    
- **Total possibilities:** 3
    

### 2. Room Status (Clean/Dirty)

Each of the three rooms has two possible conditions: it is either **Dirty** or **Clean**. Since the status of one room does not depend on the others, we calculate the combinations for the rooms using $2^n$, where $n$ is the number of rooms.

- **Status per room:** 2 (Dirty or Clean)
    
- **Number of rooms:** 3
    
- **Total status combinations:** $2^3 = 8$
    

---

### 3. Total State Space

To find the total number of distinct states, we multiply the number of possible vacuum locations by the number of possible room configurations:

$$\text{Total States} = (\text{Locations}) \times (\text{Room Configurations})$$

$$\text{Total States} = 3 \times 8 = 24$$

There are **24 unique states** in this environment.

### Break Down of the 8 Room Configurations

If we labels the rooms 1, 2, and 3 (where D = Dirty and C = Clean), the 8 possible "world" configurations are:

1. D, D, D
    
2. D, D, C
    
3. D, C, D
    
4. D, C, C
    
5. C, D, D
    
6. C, D, C
    
7. C, C, D
    
8. C, C, C
    

Each of these 8 configurations can exist while the vacuum is in Room 1, Room 2, or Room 3, resulting in the $8 \times 3$ calculation.

---

To map out the transition model, we define the **Actions** the agent can take and how those actions move the agent from one state to another. In a standard vacuum-world problem, the agent typically has three primary actions: **Left**, **Right**, and **Suck**.

### 1. The Transition Rules

A transition is defined as $(s, a) \rightarrow s'$, where $s$ is the current state, $a$ is the action, and $s'$ is the resulting state.

- **Left / Right:** These actions change the **Location** variable.
    
    - If the agent is in Room 1 and moves Left, it typically stays in Room 1 (hitting a "wall").
        
    - If it is in Room 1 and moves Right, it transitions to Room 2.
        
- **Suck:** This action changes the **Status** variable of the _current_ room only.
    
    - If the current room is Dirty, it becomes Clean.
        
    - If the current room is already Clean, the state remains unchanged.
        

---

### 2. State-Space Graph

A state-space graph visualizes these 24 states as nodes and the actions as edges (arrows) connecting them.

### 3. Example Transition Path

Let's look at a subset of the transitions starting from a completely dirty world:

|**Current State (Loc,R1,R2,R3)**|**Action**|**Resulting State (Loc,R1,R2,R3)**|
|---|---|---|
|$(1, \text{Dirty, Dirty, Dirty})$|**Suck**|$(1, \text{Clean, Dirty, Dirty})$|
|$(1, \text{Clean, Dirty, Dirty})$|**Right**|$(2, \text{Clean, Dirty, Dirty})$|
|$(2, \text{Clean, Dirty, Dirty})$|**Suck**|$(2, \text{Clean, Clean, Dirty})$|
|$(2, \text{Clean, Clean, Dirty})$|**Right**|$(3, \text{Clean, Clean, Dirty})$|
|$(3, \text{Clean, Clean, Dirty})$|**Suck**|$(3, \text{Clean, Clean, Clean})$|

---

### 4. Complexity and Search

In AI terms, the **Goal Test** for this agent is to reach any state where all rooms are "Clean," regardless of where the vacuum ends up. Since there are 3 possible locations for the vacuum in a clean world, there are **3 Goal States** out of the 24 total states.

---
## Rest of Lecture
### 1. Evaluating Search Algorithms

Once a search problem is converted from a state-space graph into a search tree, the algorithm systematically explores the "fringe" (the unexpanded nodes). To determine if a specific search algorithm is good, we evaluate it based on four performance metrics:

- **Completeness:** Is the algorithm mathematically guaranteed to find a solution if one actually exists? Will it correctly report failure if no solution exists?
    
- **Optimality:** Does it find the _best_ solution? (i.e., the one with the lowest path cost among all possible solutions).
    
- **Time Complexity:** How long does it take to find the solution? In AI, this isn't just measured in seconds, but abstractly by the number of states and actions the algorithm has to consider.
    
- **Space Complexity:** How much memory is required to keep track of the search tree during the process?
    

To calculate these time and space complexities, the lecture introduces three key mathematical parameters of the tree:

- $b$: The maximum **branching factor** (the maximum number of successors any given node can have).
    
- $d$: The **depth** of the shallowest optimal solution.
    
- $m$: The **maximum depth** of the entire state space (which could be infinite).
    

### 2. The Node Data Structure

To actually program a search algorithm, the agent needs a specific data structure (the "infrastructure") to build and keep track of the search tree in memory.

For every node $n$ in the tree, the agent must store four specific components:

1. **STATE:** The specific configuration of the environment this node represents.
    
2. **PARENT:** A pointer to the previous node in the search tree that generated this current node. This is crucial for backtracking to find the final sequence of actions once the goal is reached.
    
3. **ACTION:** The specific move that was applied to the PARENT to generate this node.
    
4. **PATH-COST:** Traditionally denoted as $g(n)$, this is the total cumulative cost of the path from the initial starting state all the way down to this specific node.
    

### 3. Midterm Exam Revision


**Topics to Review:**

- Intelligent agents and Environment types.
    
- Rationality and PEAS (Performance measure, Environment, Actuators, Sensors).
    
- Agent types (Simple reflex, model-based, goal-based, utility-based).
    
- Internal representations (Atomic, Factored, Structured).
    
- Graphs vs. Trees and problem solving using search strategies.
    

**Exam Format:**

The exam will consist of True/False questions, Multiple Choice Questions (MCQs), and an applied case study. For the case study, you will be expected to analyze a scenario using the **PEAS** framework and choose/justify the most appropriate agent type for that specific problem.