---
{"dg-publish":true,"permalink":"/3rd-year/multiagent/after-mid/lecture-10-multi-agent-systems-and-game-theory/","dg-note-properties":{}}
---


This final lecture brings everything together. Up until now, the algorithms have been operating in a vacuum—solving puzzles or navigating maps where the environment is static and the agent is the only entity making decisions.

Lecture 10 introduces the **Multiagent Environment**, where multiple decision-making agents interact in a shared space to achieve common or conflicting goals.


---

### 1. The Multiagent System (MAS)

A multiagent system is a core area of modern AI. Because multiple agents exist in the same environment, they must interact.

- **Characteristics:** Agents possess autonomy (they are partially independent), they only have local views (no single agent possesses a full global view of a complex system), and the system is decentralized (no single agent controls the others).
    
- **Communication:** To function, agents must coordinate, cooperate, negotiate, share information, and allocate tasks . They do this through various "speech acts," such as Assertives (stating facts), Directives (requesting actions), Commissives (promising actions), Expressives (sharing states), and Declarations .
    
- **Architectures:** The environment can be organized in three ways :
    
    - _Centralized:_ The environment dictates communication and decisions.
        
    - _Hierarchical:_ Agents have different levels of responsibility.
        
    - _Distributed:_ All agents are equal, make independent decisions, and communicate directly.
        

### 2. Game Theory and Adversarial Search

When agents have conflicting goals in a multiagent environment, the scenario becomes a competitive environment, which AI treats as a "game" using Game Theory.

In AI research, the most common focus is on games that are **deterministic, turn-taking, two-player, zero-sum games of perfect information** (like Chess or Tic-Tac-Toe) . "Zero-sum" means the utility values at the end are equal and opposite; if one player wins (+1), the other loses (-1).

A game is defined by :

- $S_0$: The initial state.
    
- $PLAYER(s)$: Whose turn it is.
    
- $ACTIONS(s)$: Legal moves.
    
- $RESULT(s, a)$: The transition model (what happens after a move).
    
- **Terminal Test:** Checks if the game is over.
    
- $UTILITY(s, p)$: The final numeric payoff for a player at a terminal state.
    

### 3. The MINIMAX Algorithm

Because the opponent is actively trying to make you lose, you cannot just plan a path to victory. You must find a _contingent strategy_ that accounts for every possible move the opponent might make.

- **MAX:** You want to maximize your utility score.
    
- **MIN:** The opponent wants to minimize your utility score.
    

The algorithm explores the game tree all the way to the terminal states, then backs up the values. If it is MAX's turn, MAX chooses the branch with the highest value. If it is MIN's turn, MIN chooses the branch with the lowest value .

### 4. Overcoming Complexity: Alpha-Beta Pruning

Games are incredibly complex. Chess has an average branching factor of 35, resulting in search trees with roughly $10^{154}$ nodes. Calculating the entire tree is physically impossible.

**Alpha-Beta Pruning** solves this by ignoring (pruning) branches of the search tree that mathematically cannot influence the final decision.

- $\alpha$ (Alpha): The value of the best (highest) choice found so far for MAX.
    
- $\beta$ (Beta): The value of the best (lowest) choice found so far for MIN.
    

If a node is deemed worse than the current $\alpha$ or $\beta$, the algorithm stops exploring its children entirely.

To truly understand Alpha-Beta pruning, we have to look at it from the perspective of the two players: **MAX** (you) and **MIN** (the opponent).

The goal of the algorithm is to compute the exact same optimal move as standard Minimax, but without looking at every single node. It achieves this by maintaining two values during the search:

- **$\alpha$ (Alpha):** The value of the best (highest) guaranteed option found so far for **MAX** along the current path.
    
- **$\beta$ (Beta):** The value of the best (lowest) guaranteed option found so far for **MIN** along the current path.
    

### The Golden Rule of Pruning

The search can be abandoned (pruned) at a node whenever **$\alpha \ge \beta$**.

In plain English: _If MAX already knows a path that guarantees a score of 5, and begins evaluating a new path where MIN can force a score of 3, MAX will never choose this new path. Therefore, there is no need to look at the rest of the options on that new path._

---

### A Step-by-Step Numerical Example

Let's trace a game tree with **3 branches** (choices for MAX). The opponent (MIN) will then have **2 choices** in each branch.

- **Branch A leaves:** 3, 5
    
- **Branch B leaves:** 2, 7
    
- **Branch C leaves:** 6, 4
    

Here is exactly how the algorithm processes this from left to right:

**1. Evaluating Branch A**

- The algorithm dives down to the first leaf: **3**.
    
- It looks at the second leaf: **5**.
    
- **MIN** is choosing here, so MIN picks the lowest: **3**.
    
- **MAX** now knows that if it goes down Branch A, it is guaranteed a score of at least 3.
    
- _Update:_ **$\alpha = 3$**.
    

**2. Evaluating Branch B (The Prune)**

- The algorithm dives down to the first leaf of Branch B: **2**.
    
- **MIN** evaluates this and says, "I can force a score of 2 (or possibly lower if the next leaf is worse)."
    
- **MAX** compares this to its $\alpha$ value. MAX thinks: _"Why would I ever choose Branch B, where my opponent can limit me to a 2, when I already have Branch A which guarantees me a 3?"_
    
- Because $2 < 3$, **Branch B is instantly pruned.** The algorithm completely ignores the **7**. It doesn't matter if that hidden number was a 7, a 100, or a 1,000—MIN would never let MAX have it anyway.
    

**3. Evaluating Branch C**

- The algorithm dives down to the first leaf: **6**.
    
- It looks at the second leaf: **4**.
    
- **MIN** chooses the lowest: **4**.
    
- **MAX** compares this new option (Branch C yields 4) to its previous best option (Branch A yields 3).
    
- Because $4 > 3$, MAX updates its best choice.
    
- _Update:_ **$\alpha = 4$**.
    

**The Result:** MAX chooses Branch C. By using Alpha-Beta pruning, the algorithm skipped evaluating the '7', saving computation time while arriving at the exact same mathematically perfect decision.

---

### Interactive Pruning Simulator

The best way to solidify this is to step through it visually. Use the widget below to explore the exact numerical example we just traced.

![Pasted image 20260516195333.png](/img/user/imgs/Pasted%20image%2020260516195333.png)

more on it [[3rd Year/Multiagent/Expanded Explanations/Lecture 10#Alpha-Beta Pruning Steps\|here]]
### 5. Real-World Constraints

Even with Alpha-Beta pruning, searching to the absolute end of a game like chess is often too slow. The lecture highlights several ways AI adapts to reality:

- **Imperfect Real-Time Decisions:** Programs cut off the search early and apply a **heuristic evaluation function (EVAL)** to estimate the utility of the current board state without playing the game to completion .
    
- **Stochastic Games:** Games involving luck (like dice rolls in Backgammon) introduce "Chance" nodes into the tree, requiring the algorithm to calculate probabilities alongside MIN and MAX choices .
    
- **The RoboCup vs. Chess Paradigm:** The course concludes by contrasting a rigid game like Chess (static, central control, complete info) with real-world robotics like RoboCup soccer, which forces the multiagent system to operate dynamically, in real-time, with distributed control and incomplete information .