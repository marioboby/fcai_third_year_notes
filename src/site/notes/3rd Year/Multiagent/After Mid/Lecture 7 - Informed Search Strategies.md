---
{"dg-publish":true,"permalink":"/3rd-year/multiagent/after-mid/lecture-7-informed-search-strategies/","dg-note-properties":{}}
---




While blind search techniques like Breadth-First and Depth-First Search rely purely on basic queue structures and problem definitions to traverse the search space, they lack a "sense of direction." Informed search introduces intelligence by utilizing expert knowledge or mathematical rules of thumb—known as heuristics—to rapidly identify and explore the most promising paths.

---

### 1. Evaluation and Heuristic Functions

Informed search is driven by "best-first search" strategies. These strategies order the nodes in the frontier (the priority queue) based on an **Evaluation Function**, denoted as $f(p)$.

- **Purpose:** $f(p)$ estimates the "desirability" of a path. The algorithm will always expand the path that ends in the most desirable state first.
    
- **The Heuristic ($h(n)$):** A critical component of the evaluation function is the heuristic function, which assigns a estimated value to state $n$. Specifically, $h(n)$ estimates the remaining distance or cost from node $n$ to the goal.
    
- **Example:** On a map problem like navigating to Bucharest, a common heuristic ($h_{SLD}$) is the "straight-line distance" between the current city and the destination.
    

---

### 2. Greedy Best-First Search

Greedy search relies entirely on the heuristic to make its decisions.

- **The Formula:** $f(p) = h(n)$.
    
- **Behavior:** It ignores the cost of the path taken so far and simply expands the node that _appears_ to be closest to the goal based on the heuristic.
    
- **Properties & Flaws:**
    
    - **Completeness & Optimality:** Greedy search is **not optimal** and **not complete**. Because it is single-minded, it can easily make poor long-term decisions or get stuck in infinite loops (e.g., bouncing between the same cities).
        
    - **Complexity:** In the worst-case scenario, its time and space complexities are $O(b^m)$ (where $b$ is the branching factor and $m$ is the maximum depth), meaning it might have to process the entire tree. However, a highly accurate heuristic can dramatically improve its practical speed.
        

---

### 3. A* Search Algorithm

A* (A-Star) is widely used because it fixes the flaws of Greedy search by keeping track of the actual costs incurred, avoiding paths that have already become too expensive.

- **The Formula:** $f(p) = g(p) + h(n)$.
        <br>
    - $g(p)$ = The actual cost taken so far to reach node $n$.
            <br>
    - $h(n)$ = The estimated heuristic cost from $n$ to the goal.
            <br>
    - $f(p)$ = The estimated _total_ cost of the path through $n$ to the goal.
        
    <br>
- **Behavior:** A* behaves like Uniform-Cost Search, but uses $g+h$ instead of just $g$. It explores the search space by expanding nodes in increasing order of their $f$-value, gradually adding outward "f-contours" (similar to concentric topographical lines).
        <br>
- **The Stopping Condition:** The algorithm only terminates when the node with the absolute lowest $f$-value is confirmed to be a goal state.
    

#### Properties of A*

A* is considered optimally efficient, but it requires massive memory resources.

- **Complete:** Yes (guaranteed to find a solution if one exists).
    
- **Optimal:** Yes (guaranteed to find the absolute shortest path).
    
- **Time & Space Complexity:** Exponential. Like Breadth-First Search, A* must keep all generated nodes in memory, which is its primary bottleneck.
    

#### Example With Code "8 - Puzzle Problem"

```python
def a_star_search(start_state, goal_state, heuristic='h1'):

    fringe = []

    counter = 0 # Tie-breaker for Priority Queue

    if heuristic == 'h1':
        h_start = calculate_h1(start_state, goal_state)
    else:
        h_start = calculate_h2(start_state, goal_state)

    heapq.heappush(fringe, (h_start, counter, start_state, []))

    # Track visited states and the minimum g_score (depth) it took to reach them
    explored = {start_state: 0}
    nodes_expanded = 0

    while fringe:
        f_score, _, current_state, path = heapq.heappop(fringe)

        if current_state == goal_state:
            return path, nodes_expanded

        nodes_expanded += 1
        current_g_score = len(path)

        for next_state, action in get_successors(current_state):
            new_g_score = current_g_score + 1

            # If we found a shorter path to a previously visited state, or it's a                new state
            if next_state not in explored or new_g_score < explored[next_state]:
                explored[next_state] = new_g_score

                if heuristic == 'h1':
                    h_score = calculate_h1(next_state, goal_state)
                else:
                    h_score = calculate_h2(next_state, goal_state)
                f_score = new_g_score + h_score

                counter += 1
                heapq.heappush(fringe, (f_score, counter, next_state, path + [action]))

    return None, nodes_expanded
```

---

### 4. Requirements for A* Optimality

A* is only mathematically guaranteed to be optimal if its heuristic ($h(n)$) follows strict rules:

1. **Admissibility (For Tree Search):** The heuristic must be optimistic. This means $h(n) \le h^*(n)$, where $h^*$ is the true, exact cost to reach the goal. An admissible heuristic must _never_ overestimate the cost. For example, straight-line distance is always mathematically shorter than or equal to actual winding road distances, making it perfectly admissible.
    <br>
2. **Consistency / Monotonicity (For Graph Search):** A stronger condition is required if the algorithm tracks visited states (Graph Search). A heuristic is consistent if $h(n) \le c(n, a, n') + h(n')$. This means the estimated cost from node $n$ to the goal can never be greater than the actual step cost to its neighbor ($n'$) plus the neighbor's estimated cost to the goal.
    
more on it [[3rd Year/Multiagent/Expanded Explanations/Lecture 7\|here]]

---