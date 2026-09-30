---
{"dg-publish":true,"permalink":"/3rd-year/multiagent/expanded-explanations/lecture-10/","dg-note-properties":{}}
---


# Alpha-Beta Pruning Steps

To understand **alpha-beta pruning**, you first have to understand the problem it solves: the bottleneck in the **minimax algorithm**.

In two-player games like Chess or Tic-Tac-Toe, the minimax algorithm tries to find the best possible move by looking ahead at all possible future moves. It assumes two players:

- **Maximizer (MAX):** Wants the highest possible score.
    
- **Minimizer (MIN):** Wants the lowest possible score.
    

Minimax builds a massive "game tree" of every possible move and counter-move. The problem? For complex games, this tree grows exponentially large, making it impossible to check every node in a reasonable amount of time.

This is where **alpha-beta pruning** comes in. It is an optimization technique for the minimax algorithm that drastically reduces the number of nodes evaluated by "pruning" (cutting off) branches that cannot possibly influence the final decision.

## The Core Concept: Alpha and Beta

Alpha-beta pruning keeps track of two values as it searches through the game tree:

- **$\alpha$ (Alpha):** The best (highest) value that the **Maximizer** can guarantee at that level or above. It starts at $-\infty$.
    
- **$\beta$ (Beta):** The best (lowest) value that the **Minimizer** can guarantee at that level or above. It starts at $+\infty$.
    

As the algorithm travels down and back up the tree, it updates these values.

## The Pruning Rule

The golden rule of alpha-beta pruning is simple: **If at any point $\alpha \ge \beta$, stop evaluating the current branch.**

Why? Because it means the current player already has a better move available somewhere else in the tree, or the opponent will never allow the game to reach this specific state anyway. There is no point in continuing to search a path that will never be chosen.

![Blog-8-5-2020-01-1536x897.jpg](/img/user/imgs/Blog-8-5-2020-01-1536x897.jpg)
### Step-by-Step Example (Based on the diagram above)

1. **Node D (MAX level):** Evaluates leaves 2 and 3. MAX chooses the highest: **3**.
    
2. **Node B (MIN level):** MIN looks at Node D and says, "The best MAX can force me into here is 3." So, MIN's $\beta$ (lowest guaranteed value so far) becomes **3**.
    
3. **Node E (MAX level):** MAX starts evaluating its leaves. It looks at the first leaf: **5**.
    
4. **The Prune:** At Node E, MAX now knows it can get at least a 5 ($\alpha = 5$). But look back at Node B (MIN). MIN already knows it has an option (Node D) that results in a 3. Since MIN wants the lowest number, MIN will _never_ choose Node E (which guarantees at least a 5).
    
5. **Result:** Because $\alpha$ (5) is greater than $\beta$ (3), the algorithm prunes the rest of Node E's children. It doesn't even bother looking at the leaf with the value **9**.
    

## Why It Matters

Alpha-beta pruning doesn't change the final decision of the minimax algorithm—it returns the exact same move. It just finds it much faster.

In the best-case scenario (where the best moves are always evaluated first), alpha-beta pruning can double the depth of the tree you can search in the same amount of time. If standard minimax evaluates $O(b^d)$ nodes (where $b$ is the branching factor and $d$ is depth), perfect alpha-beta pruning reduces this to $O(b^{d/2})$.

This optimization is what makes it computationally possible for AI to play complex games like Chess at a high level.

# الزتونة

### 1. When evaluating children of a MAX node

- **What you are doing:** Looking for the highest possible number to update your local $\alpha$.
    
- **The check:** After evaluating a child, you check if your current value ($\alpha$) is **greater than or equal to** $\beta$ ($\alpha \ge \beta$).
    
- **The logic:** If your value is $\ge \beta$, it means the MIN player above you already has a better or equal option elsewhere and will never let the game reach this node.
    
- **The action:** Prune the remaining children of this MAX node.
    

### 2. When evaluating children of a MIN node

- **What you are doing:** Looking for the lowest possible number to update your local $\beta$.
    
- **The check:** After evaluating a child, you check if your current value ($\beta$) is **less than or equal to** $\alpha$ ($\beta \le \alpha$).
    
- **The logic:** If your value is $\le \alpha$, it means the MAX player above you already has a better or equal option elsewhere and will never choose to go down this branch.
    
- **The action:** Prune the remaining children of this MIN node.
    

### The Golden Rule

Notice that both of those conditions mathematically mean the exact same thing. Whether you are at a MIN node or a MAX node, the universal trigger to cut off a branch is always:

**If $\alpha \ge \beta$, stop and prune.**