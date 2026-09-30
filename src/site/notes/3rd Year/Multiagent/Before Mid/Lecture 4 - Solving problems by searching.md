---
{"dg-publish":true,"permalink":"/3rd-year/multiagent/before-mid/lecture-4-solving-problems-by-searching/","dg-note-properties":{}}
---


### . Types of Agents and Planning

The lecture begins by categorizing agents and detailing how advanced agents think.

- **Four Basic Agent Types:** Simple reflex agents, reflex agents with state/model, goal-based agents, and utility-based agents "[[3rd Year/Multiagent/Before Mid/Lecture 3 - Types of Agents\|refer to prev lecture]]".
    <br>
- **Planning Agents:** These are advanced agents that ask "what if" before acting.
    <br>
    - They require a model of how the world evolves in response to their actions.
        <br>
    - They base their decisions on the hypothesized consequences of their actions and must formulate a goal to test against.
        <br>
- **Problem-Solving Agents:** These are a specific type of goal-based agent that consider future actions and the desirability of their outcomes.
    

---

### 2. Formulating the Problem

Before an agent can search for a solution, the problem must be strictly defined.

- **The Four Phases of Problem Solving:** Goal formulation, Problem formulation, Search, and Execution.
    <br>
- **The Five Components of a Formal Problem:**
    
    - **Initial state:** Where the agent starts.
        
    - **Possible set of actions:** What the agent can do.
        
    - **Transition model:** A description of what each action does and its resulting consequence.
        
    - **Goal test:** A check to determine whether a given state qualifies as a goal state.
        
    - **Path cost:** The measurable cost of taking a specific sequence of actions.
        

---

### 3. Representing the Environment

When designing agents, you have to decide how complex the agent's internal representation of the world needs to be. There are three levels of abstraction:

- **Atomic:** The state is treated as a simple "black box" with no internal structure. This is typically used in basic search algorithms, game-playing, and Hidden Markov Models. Problem-solving agents use this representation.
    <br>
- **Factored:** A more complex abstraction where states are represented by a set of variables, properties, or a vector of attribute values (e.g., Location: Room A, Battery: 80%). Planning agents often use this.
    <br>
- **Structured:** The most complex representation, where states include explicit relationships between different objects, and actions can affect specific variables or relationships. Planning agents also frequently use this.
    

---

### 4. Graphs vs. Trees

To understand search algorithms, you need to understand the underlying mathematical structures used to model them .

- **Graphs:** Mathematical structures made of vertices (nodes) connected by edges (arcs).
    <br>
    - Nodes represent discrete states of a problem (like a game board configuration).
        <br>
    - Arcs represent transitions or legal moves between those states.
        <br>
- **Trees:** A specific type of graph. It is a connected, acyclic (no loops) undirected graph where nodes have a single parent. All trees are graphs, but not all graphs are trees.
    <br>
- **Conversion:** You can turn a graph search problem into a tree search problem by avoiding loops in your path or keeping track of globally visited nodes.
    

---
### 4.2 Search Problem

A search problem is a formal way to define a problem so that an intelligent agent can solve it by searching for a solution.

### Components of a Search Problem

A search problem consists of the following elements:

- **State Space**: A set of all possible states the environment can be in.
- **Successor Function**: This includes the possible actions available and their associated costs.
- **Start State**: The initial state where the agent begins.
- **Goal Test**: A condition used to determine if a given state is a goal state.

### Solutions to Search Problems

- **The Plan**: A solution is defined as a sequence of actions (a plan) that transforms the start state into a goal state.
- **Representations**: Search problems often use an **atomic representation**, where each state is treated as a "black box" without internal structure.
- **Mathematical Models**: These problems are often represented mathematically using **state space graphs**, where nodes represent states and arcs represent actions or transitions. They can also be visualized as **search trees**, which map out "what if" scenarios and potential future outcomes.
---
### 5. State Space Graphs vs. Search Trees

This is a critical distinction in the lecture regarding how search is actually executed.

- **State Space Graphs:** This represents the actual layout of the problem.
    <br>
    - Nodes are abstracted world configurations.
        <br>
    - _Crucially, in a state space graph, each state occurs only once_.
        <br>
    - It is rarely built fully in memory because it is usually too large.
        <br>
- **Search Trees:** This represents the agent's _process_ of finding a solution.
    <br>
    - It is a "what if" tree of potential plans and outcomes.
        <br>
    - The root node is the start state, and children are successors.
        <br>
    - _Crucially, each node in a search tree represents an entire path from the start state, not just a single physical location_. This means a single state from the State Space Graph can appear multiple times in a Search Tree if there are multiple ways to get there.
        

---

### 6. The Tree Search Process

The lecture concludes by outlining the general strategy for a tree search algorithm.

- **The Fringe:** As the agent explores potential plans (expanding tree nodes), it maintains a "fringe" of partial plans currently under consideration.
    <br>
- **The Loop:** The algorithm loops by choosing a leaf node from the fringe for expansion based on a specific strategy.
    <br>
    - If it's the goal, it returns the solution.
        <br>
    - If not, it expands the node, adding the resulting new nodes back to the search tree's fringe.
        <br>
- **Efficiency:** The main goal is to expand as few tree nodes as possible while deciding which fringe nodes to explore.