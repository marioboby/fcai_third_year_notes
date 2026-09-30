---
{"dg-publish":true,"permalink":"/3rd-year/multiagent/finals-sol/final-2025-sol-ai/","dg-note-properties":{}}
---



> أي سؤال يخص Algorithms لحل ال Constraint Satisfaction Problems "CSP" مش علينا في منهج المادة، الامتحان جايبه من درايف مادة AI اللي بياخدوها كل الاقسام التانية ما عدا احنا و IT "المادة حرفيا عبارة عن الاتونوموس + ريسونينج" 
# Q1 - T/F

![Pasted image 20260602174056.png](/img/user/imgs/Pasted%20image%2020260602174056.png)

1. **False** (not included in course)
    
    _Logic knowledge representation requires both syntax (symbols) and semantics (rules of inference/meaning) to reason about the real world._
    
2. **False**
    
    _Simple reflex agents act **only** on the current percept and completely ignore the percept history._
    
3. **True**
    
    _Iterative Deepening Search is complete (assuming a finite branching factor) and operates by applying a depth limit that incrementally increases._
    
4. **False**
    
    _Hill climbing does not always find the optimal solution; it can easily get stuck in local maxima, ridges, or plateaus._
    
5. **False**
    
    _Depth-first search (DFS) is not optimal. It will return the first solution path it finds, which may be much deeper or costlier than the optimal path._
    
6. **False**
    
    _A search problem can have multiple goal states. A "goal test" determines if any given state is a goal, rather than requiring exactly one named state._
    
7. **False**
    
    _A* search expands the node with the lowest total estimated cost, represented by the function $f(n) = g(n) + h(n)$. Greedy Best-First Search is the algorithm that expands the node closest to the goal based only on the heuristic $h(n)$._
    
8. **False**
    
    _A local maximum is a peak that is **higher** than each of its neighboring states, but lower than the global maximum._
    
9. **False**
    
    _Simulated annealing is specifically designed to avoid getting stuck by allowing "bad" moves with a certain probability. Standard hill climbing is the algorithm that frequently gets stuck when no progress can be made._
    
10. **False**
    
    _The minimax algorithm explores the game tree using a depth-first search mechanism, not breadth-first._
    
11. **True**
    
    _Local search algorithms operate using a single current state and generally do not retain paths. They are ideal for optimization problems where only the final state (the goal) matters, not how you got there._
    
12. **False**
    
    _The fitness function is a specific, non-random objective function defined by the creator to evaluate how good a state is. It is the **initial population** that is generated randomly._
    
13. **True**
    
    _AND-OR trees are used to model non-deterministic environments. "OR" nodes represent the agent's choices, and "AND" nodes represent the different possible outcomes produced by the environment._
    
14. **False**
    
    _In classic state-space search, states are treated as black boxes. In a Constraint Satisfaction Problem (CSP), states use a **factored representation** (variables and values), meaning their internal structure is visible and utilized._
    
15. **False** (not included in course)
    
    _Forward checking only removes values from unassigned variables if those values **conflict** with the most recently assigned variable. It does not remove all values._
    
16. **False** (not included in course)
    
    _The preferences themselves are called soft constraints. Finding the assignment that maximizes these preferences is the process of solving the optimization CSP, not the definition of the term itself._

---

# Q2 - MCQ

![Pasted image 20260602174706.png](/img/user/imgs/Pasted%20image%2020260602174706.png)

---

# Q3 - Min - Max

![Pasted image 20260602175658.png](/img/user/imgs/Pasted%20image%2020260602175658.png)

We will traverse the tree from left to right, keeping track of $\alpha$ (the best alternative for MAX along the path to the root) and $\beta$ (the best alternative for MIN along the path).

- Initial values at the Root (MAX): $\alpha = -\infty$, $\beta = +\infty$.
    

### **1. Evaluating Branch A (MIN)**

- The Root passes $[\alpha = -\infty, \beta = +\infty]$ to Node A.
    
- **Node max (1,1):** Evaluates its only leaf **{6}**. Returns `6`.
    
    - Node A (MIN) updates its current value to `6`. Therefore, $\beta = 6$.
        
- **Node max (1,2):** Receives $[\alpha = -\infty, \beta = 6]$.
    
    - Evaluates **{4}**: Current max is 4.
        
    - Evaluates **{8}**: Current max is 8.
        
    - _Pruning check:_ The current value (8) is $\ge \beta$ (6). Node A will never choose a value greater than 6, so we can stop searching this node.
        
    - **PRUNED:** Leaves **{3, 2}** are pruned. Returns `8`.
        
    - Node A (MIN) compares its current `6` with `8`. It keeps `6`. $\beta$ remains `6`.
        
- **Node max (1,3):** Receives $[\alpha = -\infty, \beta = 6]$.
    
    - Evaluates **{2}**: Current max is 2.
        
    - Evaluates **{4}**: Current max is 4.
        
    - Evaluates **{7}**: Current max is 7.
        
    - _Pruning check:_ The current value (7) is $\ge \beta$ (6).
        
    - **PRUNED:** Leaf **{9}** is pruned. Returns `7`.
        
    - Node A (MIN) compares `6` and `7`. It keeps `6`.
        
- **Node A returns `6` to the Root.**
    
- **Root (MAX)** updates its $\alpha$ to $\max(-\infty, 6) = 6$.
    

### **2. Evaluating Branch B (MIN)**

- The Root passes $[\alpha = 6, \beta = +\infty]$ down to Node B.
    
- **Node max (2,1):** Evaluates its only leaf **{8}**. Returns `8`.
    
    - Node B (MIN) updates its current value to `8`. Therefore, $\beta = 8$.
        
- **Node max (2,2):** Receives $[\alpha = 6, \beta = 8]$.
    
    - Evaluates **{4}**: Current max is 4.
        
    - Evaluates **{2}**: Current max remains 4.
        
    - Evaluates **{3}**: Current max remains 4.
        
    - Evaluates **{4}**: Current max remains 4. Returns `4`.
        
    - Node B (MIN) compares its current `8` with `4`. It updates its value to `4`.
        
- _Pruning check:_ Node B's current value (4) is $\le \alpha$ (6). The Root (MAX) already has a guaranteed option of 6 from Branch A, so it will never choose Branch B which evaluates to 4 (or potentially worse). We can stop searching Branch B entirely.
    
- **PRUNED:** The entire **max (2,3)** node and its leaves **{1, 5, 9, 8}** are pruned.
    
- **Node B returns `4` to the Root.**
    
- **Root (MAX)** compares `6` and `4`. It keeps `6`. $\alpha$ remains `6`.
    

### **3. Evaluating Branch C (MIN)**

- The Root passes $[\alpha = 6, \beta = +\infty]$ down to Node C.
    
- **Node max (3,1):** Evaluates its only leaf **{3}**. Returns `3`.
    
    - Node C (MIN) updates its current value to `3`.
        
- _Pruning check:_ Node C's current value (3) is $\le \alpha$ (6). The Root (MAX) will never choose this branch since it already has a guaranteed 6 from Branch A. We can stop searching Branch C entirely.
    
- **PRUNED:** The entire **max (3,2)** and **max (3,3)** nodes and their leaves **{9, 4, 3, 2}** and **{1, 9, 6, 2}** are pruned.
    
- **Node C returns `3` to the Root.**
    
- **Root (MAX)** compares `6` and `3`. It keeps `6`.
    

### **Final Result & Summary**

- **Final Optimal Value of the Tree:** `6` (Choosing path to Node A, then max (1,1))
    
- **List of all Pruned Leaves/Nodes:**
    
    - In max (1,2): leaves **3, 2**
        
    - In max (1,3): leaf **9**
        
    - Node **max (2,3)** entirely (leaves 1, 5, 9, 8)
        
    - Node **max (3,2)** entirely (leaves 9, 4, 3, 2)
        
    - Node **max (3,3)** entirely (leaves 1, 9, 6, 2)
      
  ![Pasted image 20260602180329.png](/img/user/imgs/Pasted%20image%2020260602180329.png)

## Suppose we invert the levels to Min-Max-Min, what is the best move for the Min Player using Min-Max procedure  

![Pasted image 20260602180734.png](/img/user/imgs/Pasted%20image%2020260602180734.png)

we choose **C**

---

# Q4 - Match

![Pasted image 20260602181237.png](/img/user/imgs/Pasted%20image%2020260602181237.png)

![Pasted image 20260602181252.png](/img/user/imgs/Pasted%20image%2020260602181252.png)

---

# Q5 - Complete 

1. In Constraint Satisfaction Problems (CSPs), the **forward checking** algorithm is used to improve performance by crossing off values that violate a constraint when added to the existing assignment. **(not included in course)**
    
    _(Note: "Constraint propagation" or "Arc consistency" are also closely related, but forward checking specifically describes the action of looking ahead to immediately connected unassigned variables)._
    
2. In Game, the **utility** (or payoff) function is defined for each terminal state, while in informed search the **heuristic** (or evaluation) function is used to evaluate the value of each node.
    
3. Alpha-beta search is equal to **minimax** search but eliminates the branches that can't influence the final decision in the game.
    
4. **Backtracking** search is the basic uninformed algorithm for solving CSPs. **(not included in course)**
    
5. In search, the **state space** (or state space graph) is the configuration of the possible states and how they connect to each other.
    
6. The search strategy that minimizes the cost of the path from the start to the current node is called **Uniform-cost search**.
    
    _(This uses the path cost function $g(n)$)._
    
7. The uninformed search strategy which is incomplete is called **Depth-first search**.
    
    _(DFS is incomplete because it can get stuck going down an infinite path or caught in a cycle)._

---

# Q6 - Search Algorithms 

![Pasted image 20260602183814.png](/img/user/imgs/Pasted%20image%2020260602183814.png)

S is the start state, G is the only goal, the cost of each path is shown, and the heuristic value of each node is considered, using the tree search version of these 5 search algorithms: BFS, DFS, UCS, Greedy, and A*

1. What are the explored states using BFS, A*, and UCS in order
2. Which algorithm will return the shortest path, and write that path? 

### 1. The Tree Version of the Graph

In a **Tree Search**, we explore paths without keeping track of a "visited" list, meaning we expand branches downwards. Based on the connections and costs, here is the tree structure:


```Plaintext
          S
       /  |  \
     B    C    D
    /      \
   E        G
  / \
 F   G
 |
 G
```

### 2. Explored States (Expansion Order)

We will trace Breadth-First Search (BFS), Uniform Cost Search (UCS), and A* Search.

- **Goal Test Rule:** In standard search, the goal test is applied when a node is **popped** from the frontier/queue, not when it is added.
    
- **Tie-breaking:** When nodes have the same priority or level, we will expand them in **alphabetical order**.
    

#### **Breadth-First Search (BFS)**

BFS uses a standard FIFO Queue, exploring level by level.

1. **Pop S**: Expand to B, C, D. Queue: `[B, C, D]`
    
2. **Pop B**: Expand to E. Queue: `[C, D, E]`
    
3. **Pop C**: Expand to G. Queue: `[D, E, G]`
    
4. **Pop D**: No children. Queue: `[E, G]`
    
5. **Pop E**: Expand to F, G. Queue: `[G, F, G]`
    
6. **Pop G**: Goal found! (This is the G from path S $\rightarrow$ C $\rightarrow$ G with cost 1 + 15 = 16)
    

- **Explored order:** **S, B, C, D, E, G**
    
    _(Note: BFS returns path S $\rightarrow$ C $\rightarrow$ G, which is the shallowest path, but not the cheapest)._
    

#### **Depth-First Search (DFS)**

DFS uses a **LIFO Stack** (Last-In, First-Out), meaning it explores the deepest unexpanded node first.

1. **Pop S**: Expand to B, C, D. Stack: `[B, C, D]` _(B is at the top/front)_
    
2. **Pop B**: Expand to E. Stack: `[E, C, D]`
    
3. **Pop E**: Expand to F, G. Stack: `[F, G, C, D]` _(F is at the top)_
    
4. **Pop F**: Expand to G. Stack: `[G, G, C, D]` _(F's child G is at the top)_
    
5. **Pop G**: Goal found!
    

- **Explored order:** **S, B, E, F, G**
    
- **Path returned:** **S $\rightarrow$ B $\rightarrow$ E $\rightarrow$ F $\rightarrow$ G**
    
- **Total Cost:** **13** $(2 + 7 + 1 + 3)$
    
    _(As expected, DFS did not find the optimal path. It just dove straight down the left-most branch until it hit the goal)._


#### **Uniform Cost Search (UCS)**

UCS uses a Priority Queue based strictly on the path cost from the start node: $g(n)$.

1. **Pop S** $g=0$: Expand to C(1), B(2), D(10). PQ: `[C(1), B(2), D(10)]`
    
2. **Pop C** $g=1$: Expand to G(16). PQ: `[B(2), D(10), G(16)]`
    
3. **Pop B** $g=2$: Expand to E(9). PQ: `[E(9), D(10), G(16)]`
    
4. **Pop E** $g=9$: Expand to F(10), G(11). PQ: `[D(10), F(10), G(11), G(16)]` _(Alphabetical tie-break for D and F)_
    
5. **Pop D** $g=10$: No children. PQ: `[F(10), G(11), G(16)]`
    
6. **Pop F** $g=10$: Expand to G(13). PQ: `[G(11), G(13), G(16)]`
    
7. **Pop G** $g=11$: Goal found! (Path S $\rightarrow$ B $\rightarrow$ E $\rightarrow$ G) with cost 2 + 7 + 2 
    

- **Explored order:** **S, C, B, E, D, F, G**
    

#### **Greedy Best-First Search**

Greedy uses a **Priority Queue** based strictly on the heuristic value: $f(n) = h(n)$. It ignores path cost entirely and always expands the node that _appears_ closest to the goal.

- _Recall the heuristics:_ $S=9, B=7, C=10, D=7, E=1, F=1, G=0$
    

1. **Pop S** $h=9$: Expand to B, C, D.
    
    - B: $h=7$
        
    - C: $h=10$
        
    - D: $h=7$
        
    - PQ: `[B(7), D(7), C(10)]` _(Alphabetical tie-break puts B before D)_
        
2. **Pop B** $h=7$: Expand to E.
    
    - E: $h=1$
        
    - PQ: `[E(1), D(7), C(10)]`
        
3. **Pop E** $h=1$: Expand to F, G.
    
    - F: $h=1$
        
    - G: $h=0$
        
    - PQ: `[G(0), F(1), D(7), C(10)]`
        
4. **Pop G** $h=0$: Goal found!
    

- **Explored order:** **S, B, E, G**
    
- **Path returned:** **S $\rightarrow$ B $\rightarrow$ E $\rightarrow$ G**
    
- **Total Cost:** **11** $(2 + 7 + 2)$
    

_(Fun fact: Even though Greedy is not an optimal algorithm by design, in this specific graph, it ironically stumbled right into the optimal path because the heuristic values perfectly aligned with the best route!)_

#### **A* Search**

A* uses a Priority Queue based on the total estimated cost: $f(n) = g(n) + h(n)$.

1. **Pop S** $f=9$ $(0+9)$: Expand S $\rightarrow$ B, C, D.
    
    - B: $f = 2 + 7 = 9$
        
    - C: $f = 1 + 10 = 11$
        
    - D: $f = 10 + 7 = 17$
        
    - PQ: `[B(9), C(11), D(17)]`
        
2. **Pop B** $f=9$: Expand to E.
    
    - E: $f = 9 + 1 = 10$ _(path cost 9 + heuristic 1)_
        
    - PQ: `[E(10), C(11), D(17)]`
        
3. **Pop E** $f=10$: Expand to F, G.
    
    - F: $f = 10 + 1 = 11$
        
    - G: $f = 11 + 0 = 11$
        
    - PQ: `[C(11), F(11), G(11), D(17)]` _(Alphabetical tie-break for C, F, G)_
        
4. **Pop C** $f=11$: Expand to G.
    
    - G: $f = 16 + 0 = 16$
        
    - PQ: `[F(11), G(11), G(16), D(17)]`
        
5. **Pop F** $f=11$: Expand to G.
    
    - G: $f = 13 + 0 = 13$
        
    - PQ: `[G(11), G(13), G(16), D(17)]`
        
6. **Pop G** $f=11$: Goal found!
    

- **Explored order:** **S, B, E, C, F, G**
    
    _(Note: If the criteria breaks ties by prioritizing the node with the lowest heuristic $h(n)$ rather than alphabetical, G would be popped right after E, making the order S, B, E, G)._
    

### 3. Which Algorithm Returns the Shortest Path?

Both **Uniform Cost Search (UCS)** and **A* Search** guarantee finding the optimal (shortest) path, as they both evaluate edge costs and the given heuristics are admissible (they never overestimate the true cost to the goal). Even though Greedy is not an optimal algorithm by design, in this specific graph, it ironically stumbled right into the optimal path because the heuristic values perfectly aligned with the best route! 

- **Shortest Path:** **S $\rightarrow$ B $\rightarrow$ E $\rightarrow$ G**
    
- **Total Cost:** **11** ($2 + 7 + 2$)