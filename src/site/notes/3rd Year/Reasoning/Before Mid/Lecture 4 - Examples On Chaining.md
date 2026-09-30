---
{"dg-publish":true,"permalink":"/3rd-year/reasoning/before-mid/lecture-4-examples-on-chaining/","dg-note-properties":{}}
---

# Example 

![Pasted image 20260520012732.png](/img/user/imgs/Pasted%20image%2020260520012732.png)

**Start with the first goal friend1(X):

 Matching friend1(X):

R1: IF h is true THEN Friend1(X) is true    (false)

R2: IF a is true AND b is true THEN Friend1(X) is true

                        Matching a :

                        R4: IF not(d) is true THEN a is true

                                        Matching d 

                                        R8: IF h1 is true  AND h2 is true THEN d is true

                                        h1 is true and h2 is true then d is true then R8 is true

                        Return to R4:

                        R4: IF not(d) is true THEN a is true

                       d is true then not(d) become false so a is false then R4 is false 

 Return to R2:

 a is false then Friend1(X) is **false.**

---

**The second goal friend2(X):

 Matching friend2(X):

R3: IF c(X) is true THEN Friend2(X) is true

                        Matching c(X) :

                        R7: IF not(e) is true THEN c(2) is true

                                        Matching e 

                                        R9: IF h2 is true  AND h3 is true THEN e is true

                                        h2 is true and h3 is false then e is false then R9 is false

                        Return to R7:

                        R7: IF not(e) is true THEN c(2) is true

                       e is false, then not(e) become true

                       So, c(2) is true then R7 is true 

Return to R3:

R3: IF c(X) is true THEN Friend2(X) is true

 c(X) is true for X=2 

then **Friend2(X) is true where X=2.**

---

| Feature            | Forward Chaining                                                                                                                | Backward Chaining                                                                                                                         |
| ------------------ | ------------------------------------------------------------------------------------------------------------------------------- | ----------------------------------------------------------------------------------------------------------------------------------------- |
| **When Suggested** | All or most data is given; large number of potential goals with few achievable; difficult to formulate a goal.                  | Goal or hypothesis is given or easily formulated; large number of rules but goal prunes search space; data must be acquired as necessary. |
| **Advantages**     | Simple.                                                                                                                         | Search is goal directed, only necessary rules are applied.                                                                                |
| **Disadvantages**  | Many applicable rules at each stage (conflict resolution needed); process not directed towards a goal (stop condition unknown). | A goal has to be known.                                                                                                                   |

---

## Conflict Resolution Strategies

Conflict resolution is the method for choosing which rule to fire when more than one rule (the conflict set) can be fired in a given cycle.

|Strategy|Description|
|---|---|
|**First Applicable**|Firing the first applicable rule if the rules are in a specified order.|
|**Most Specific**|Choosing the rule with the most conditions, based on the assumption that a specific rule processes more information.|
|**Least Recently Used**|Choosing the rule with the earliest time or step mark, which marks the last time it was used.|
|**Highest Priority**|Choosing the rule with the highest 'weight' assigned to it.|
|**Most Recently Entered**|Firing the rule whose antecedent uses the data most recently added to the database.|