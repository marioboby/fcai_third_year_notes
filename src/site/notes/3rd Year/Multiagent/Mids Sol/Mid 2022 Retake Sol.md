---
{"dg-publish":true,"permalink":"/3rd-year/multiagent/mids-sol/mid-2022-retake-sol/","dg-note-properties":{}}
---


For vacuum cleaner world: your agent is a vacuum cleaner robot, which is responsible for cleaning 4 rooms (A, B, C, and D). Each state is represented by the agent’s location and the dirt status at this location. The goal state is achieved when all rooms are clean. You are required to do the following tasks:

1. Create the state space for all possible states. 
	1. Comment on the state space type if it is cyclic or acyclic.

2. Consider the initial state is the corresponding figure a state (C’). Build a search tree with at least depth of 3 levels. 
	1. Comment on expected depth of complete tree.

3. Consider the use of one of the search techniques (such as depth-first, breadth first, A*,...etc). Explain and discuss the suitability of your choice for the given problem to achieve the goal state in terms of **completeness, optimality, time complexity, and space complexity.**

![Pasted image 20260406184304.png](/img/user/imgs/Pasted%20image%2020260406184304.png)

# Sol: 

### **1. State Space**

Based strictly on the rule ("Each state is represented by the agent’s location and the dirt status at this location"), the state space is defined by 8 distinct simplified states.

Let a letter represent a clean room and a prime (`'`) represent a dirty room:

- **State Space:** `{A, A', B, B', C, C', D, D'}`
    

_(Note: While these 8 codes represent the agent's immediate local state, the environment must implicitly track the dirt in all four rooms behind the scenes to know when the "all rooms are clean" goal is achieved. A fully observable global state space would contain $4 \times 2^4 = 64$ states, every combination of "Cleanness", for each of the 4 rooms)._

**State Space Type:**

The state space is **Cyclic**. The agent can move back and forth between rooms indefinitely without performing a "Suck" action (e.g., moving $C' \rightarrow A' \rightarrow C' \rightarrow A'$), creating infinite loops in a search tree if visited states are not tracked.

---

### **2. Search Tree (Depth of 3 Levels)**

**Environment Layout:**

- **Top Row:** A (Dirty), C (Dirty)
    
- **Bottom Row:** B (Clean), D (Dirty)
    
- _Possible Moves:_ Up, Down, Left, Right (bounded by walls), Suck.
    

**Initial State:** `C'` (Agent in C, C is dirty. Uncleaned rooms: A, C, D)

Here is the search tree expanded to 3 levels. To make the tree accurate to the goal, I have tracked the hidden global dirt states to determine what code the agent encounters when moving.

- **Level 0**
    
    - **C'** 
      
* **Level 1** _(Actions from C')_
        
    - ├── **Suck** $\rightarrow$ **C** _(Rooms A, D remain dirty)_
        
    - ├── **Left** $\rightarrow$ **A'** _(Rooms A, C, D remain dirty)_
        
    - └── **Down** $\rightarrow$ **D'** _(Rooms A, C, D remain dirty)_
        
- **Level 2** _(Actions from Level 1)_
    
    - _From C:_
        
        - ├── **Left** $\rightarrow$ **A'** _(Rooms A, D remain dirty)_
            
        - └── **Down** $\rightarrow$ **D'** _(Rooms A, D remain dirty)_
            
    - _From A':_
        
        - ├── **Suck** $\rightarrow$ **A** _(Rooms C, D remain dirty)_
            
        - ├── **Right** $\rightarrow$ **C'** _(Cycle)_
            
        - └── **Down** $\rightarrow$ **B** _(Rooms A, C, D remain dirty)_
            
    - _From D':_
        
        - ├── **Suck** $\rightarrow$ **D** _(Rooms A, C remain dirty)_
            
        - ├── **Up** $\rightarrow$ **C'** _(Cycle)_
            
        - └── **Left** $\rightarrow$ **B** _(Rooms A, C, D remain dirty)_
            
- **Level 3** _(Actions from a selection of Level 2 nodes)_
    
    - _From C $\rightarrow$ A':_
        
        - ├── **Suck** $\rightarrow$ **A** _(Only D remains dirty)_
            
        - ├── **Right** $\rightarrow$ **C** _(Cycle)_
            
        - └── **Down** $\rightarrow$ **B** _(Rooms A, D remain dirty)_
            
    - _From C $\rightarrow$ D':_
        
        - ├── **Suck** $\rightarrow$ **D** _(Only A remains dirty)_
            
        - ├── **Up** $\rightarrow$ **C** _(Cycle)_
            
        - └── **Left** $\rightarrow$ **B** _(Rooms A, D remain dirty)_
            
    - _From A' $\rightarrow$ A:_
        
        - ├── **Right** $\rightarrow$ **C'** _(Rooms C, D remain dirty)_
            
        - └── **Down** $\rightarrow$ **B** _(Rooms C, D remain dirty)_
            

![Pasted image 20260406190737.png](/img/user/imgs/Pasted%20image%2020260406190737.png)

---

### **Comment on the Expected Depth of the Complete Tree**

- **Optimal Path Depth:** The expected depth to reach the goal state (all rooms clean) along the most efficient path is **6 levels**.
    
    - _Optimal Sequence:_ Suck at C (1) $\rightarrow$ Move Left to A (2) $\rightarrow$ Suck at A (3) $\rightarrow$ Move Down to B (4) $\rightarrow$ Move Right to D (5) $\rightarrow$ Suck at D (6).
        <br>
- Complete Path Depth: Because the state space is cyclic, the complete unpruned search tree has an **infinite depth**. The agent can wander forever without cleaning. but in case of cycle detection:  
### The Reachable State Space

Because the agent can never _add_ dirt, it can never enter a state where Room B is dirty. Room B is clean in the initial state, so it will remain clean forever. This immediately cuts the reachable state space in half.

From your initial state, there are only 3 possible rooms that can change from dirty to clean (A, C, and D).

- The number of possible dirt combinations left is $2^3 = 8$ combinations.
    
- The agent can still be in any of the 4 rooms.
    
- **Total Reachable States:** $8 \times 4 = 32$ states.
    

The complete search tree for this specific problem is capped at exploring those 32 reachable states.

### The True "Complete Tree" Depth for Your Problem

If we force the algorithm to generate the absolute longest branch from your initial state without repeating any global state, the maximum depth of your complete tree is **16 nodes (or 15 actions)**.

Here is how that exact worst-case branch unfolds from your starting position:

**Phase 1: 3 Rooms Dirty (A, C, D are dirty)**

1. **(C, 3 dirty)** - _Your Initial State_
    
2. **Move Down $\rightarrow$ (D, 3 dirty)**
    
3. **Move Left $\rightarrow$ (B, 3 dirty)**
    
4. **Move Up $\rightarrow$ (A, 3 dirty)**
    
    _(If it moves anywhere else, it cycles. It must Suck)._
    

**Phase 2: 2 Rooms Dirty (Suck at A; C and D remain dirty)**

5. **Suck $\rightarrow$ (A, 2 dirty)**

6. **Move Down $\rightarrow$ (B, 2 dirty)**

7. **Move Right $\rightarrow$ (D, 2 dirty)**

8. **Move Up $\rightarrow$ (C, 2 dirty)**

_(Must Suck again)._

**Phase 3: 1 Room Dirty (Suck at C; only D remains dirty)**

9. **Suck $\rightarrow$ (C, 1 dirty)**

10. **Move Left $\rightarrow$ (A, 1 dirty)**

11. **Move Down $\rightarrow$ (B, 1 dirty)**

12. **Move Right $\rightarrow$ (D, 1 dirty)**

_(Must Suck again)._

**Phase 4: 0 Rooms Dirty (The Goal State)**

13. **Suck $\rightarrow$ (D, all clean)** 
    
14. **Move Left $\rightarrow$ (B, all clean)**

15. **Move Up $\rightarrow$ (A, all clean)**

16. **Move Right $\rightarrow$ (C, all clean)**

At step 17, no matter what action the agent takes, it will hit a state it has already visited in "Phase 4." The cycle-checker terminates the branch.

---

# 3. Best Suitable search algorithm 

For the vacuum cleaner world, **Breadth-First Graph Search (BFS)** is highly suitable and arguably the best uninformed search choice for this specific problem.

It is crucial to specify **Graph Search** rather than _Tree Search_. Because the state space is cyclic (the agent can move back and forth indefinitely), the algorithm must keep a "closed list" (an explored set) to remember previously visited states and avoid infinite loops.

### 1. Completeness

- **Status:** **Complete**
    
- **Discussion:** Since the branching factor is finite "each state has at most 3 valid moves + suck", we are sure that BFS will find a solution in the shallowest depth.
    

### 2. Optimality

- **Status:** **Optimal**
    
- **Discussion:** An algorithm is optimal if it finds the solution with the lowest path cost. In this vacuum world, every action (moving Up, Down, Left, Right, or Sucking) has a uniform step cost of 1. BFS inherently explores all nodes at depth $d$ before moving to depth $d+1$. Therefore, the first time it encounters the goal state, it is guaranteed to be along the shortest possible path (minimum number of actions).
    

### 3. Time Complexity

- **Status:** $O(b^d)$
    
- **Discussion:** In the worst-case scenario for a standard tree, BFS time complexity is $O(b^d)$, where $b$ is the maximum branching factor (up to 4 actions: 3 movements + 1 clean) and $d$ is the depth of the shallowest solution (which we established is 6).

### 4. Space Complexity

- **Status:** $O(b^d)$
    
- **Discussion:** Space complexity is typically the main drawback of BFS, as it must keep all nodes of the current depth in memory (the frontier), yielding $O(b^d)$ in a tree search.
    

### Why not other algorithms?

- **Depth-First Search (DFS):** If implemented as a Tree Search, DFS would fail completely by getting stuck in an infinite loop (e.g., Left, Right, Left, Right). Even as a Graph Search, DFS is **not optimal**; it might find a incredibly long, winding path to clean the rooms instead of the shortest one (basically the leftmost solution).
    
- **A* Search:** A* is an excellent, optimal, and complete algorithm, but it requires a heuristic (e.g., counting the number of dirty rooms left). While A* would technically explore fewer nodes than BFS, the overhead of calculating the heuristic is unnecessary for a state space this small. BFS is simpler to implement and just as effective here.