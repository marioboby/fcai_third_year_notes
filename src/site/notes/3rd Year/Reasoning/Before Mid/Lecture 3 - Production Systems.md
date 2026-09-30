---
{"dg-publish":true,"permalink":"/3rd-year/reasoning/before-mid/lecture-3-production-systems/","dg-note-properties":{}}
---

## Production Systems

A production system, which is also called a rule-based system, is a framework that operates using logical "IF... THEN..." rules. It is built on three fundamental components:

- **The Production Rules:** These are the core pieces of problem-solving knowledge. They are structured as condition-action pairs (e.g., "IF _condition_ THEN _action_"). The "condition" part checks if the rule matches the current problem, while the "action" part tells the system what step to execute next.
    
- **The Working Memory:** Think of this as the system's active scratchpad. It holds the description of the current state of the problem being solved.
    
- **The Control Structure:** Also known as an "interpreter." This is the engine of the system. It uses a "recognize-act cycle" to search through the production rules, deciding which ones to apply to the working memory in order to move closer to a goal.
    

### Production System Types

Production systems are broadly divided into two main categories based on how they process information:

- **Forward Chaining Systems (Data-Driven):** This approach starts with the initial, known facts. The system continuously applies its rules to these facts to draw new conclusions or trigger actions. It works from the data forward to an eventual conclusion.
    
- **Backward Chaining Systems (Goal-Driven):** This approach works in reverse. It starts with a specific hypothesis or goal that it wants to prove. The system then searches backward for rules that would allow it to conclude that hypothesis, often generating new "subgoals" that must be proven along the way.

## 1. Introduction to Rule-Based Systems

A rule-based system is a type of production system that models the knowledge of a human expert.

- **Core Concept:** The system consists of a set of IF-THEN rules, a collection of facts, and an interpreter that controls how the rules are applied to those facts.
    
- **Goal:** When the system is exposed to the same data as a human expert, it is expected to perform in a similar manner. 

-  **Applicability:** These systems are simple and adaptable to many problems, provided the domain knowledge can actually be expressed as "if-then" rules. However, the problem area shouldn't be too vast, as an excessive number of rules can make the expert system inefficient.

## 2. Elements of a Rule-Based System

There are three fundamental components to any rule-based system:

- **Facts:** These are assertions that describe the beginning state of the system or relevant data.

	- Facts can be seen as a collection of data and conditions. Data associates the value of characteristics with a thing and conditions perform tests of the values of characteristics to determine if something is of interest, perhaps the correct classification of something or whether an event has taken place.
	<br>
	- For instance, if we have the fact: temperature < 0
		- then temperature is the data and the condition is < 0.
		- Rules do not interact directly with data, but only with conditions either singly or multiple (joined by logical operators). 
<br>
- **Rules:** These specify the actions to be taken; they relate the facts in the "IF" part to an action or conclusion in the "THEN" part. To maintain efficiency, the system must only contain relevant rules.
<br>
- **Termination Criterion:** This is a specific condition that dictates when a solution has been found or if no solution exists, which is critical for preventing the system from getting stuck in infinite loops.


## 3. Understanding Rules

Rules are the logic engines of the system, constructed in two parts:

- **The Antecedent (IF part):** Also known as the premise or condition. It typically consists of an object (a linguistic object) and a value, linked by an operator (like "is", "are", or mathematical operators like ">").
    
- **The Consequent (THEN part):** Also known as the conclusion or action.
    

**How they work together:** A simple rule looks like `IF antecedent THEN consequent` (e.g., IF the season is winter, THEN it is cold). The system evaluates the logical expression in the premise; if it is true, it asserts the consequent as a new fact.

- _Complexity:_ Rules can have multiple antecedents joined by logical operators like `AND` or `OR`. Similarly, the consequent can also trigger multiple clauses at once.

**Parts of an Antecedent (IF part):** The condition itself is made up of two specific components:

1. An **object** (often called a linguistic object), such as "the season".
<br>
2. The **value** of that object, such as "winter".
---

## 4. Forward Chaining Systems (Data-Driven)

Forward chaining is a "data-driven" approach. It starts with the known facts and applies rules to generate _new_ facts until a specific goal is reached. **The 4-Step Process:**

1. Match the IF part of all rules against the facts currently in the working memory.
    
2. If multiple rules match (can "fire"), use _conflict resolution_ to select just one.
    
3. Apply the selected rule and add any newly obtained facts to the working memory.
    
4. Stop if the desired conclusion is reached or if a rule specifies to end; otherwise, loop back to step 1.
    

**Example Walkthrough:**

- **Database (Facts):** A, B, C, D, E.
    
- **Rules:** 1-5 .

![Pasted image 20260331212923.png](/img/user/imgs/Pasted%20image%2020260331212923.png)
    
- **Goal:** Prove Z.
    
- **Conflict Resolution Strategy:** Always pick the first available rule that hasn't fired yet.
    
- **Cycle 1:** Rules 1, 2, and 3 match the facts . Rule 1 is selected. Firing Rule 1 yields "B", but "B" is already in the database, so no new facts are added. Goal Z is not reached.
    
- **Cycle 2:** Rules 1, 2, and 3 still match. Rule 1 already fired, so Rule 2 is selected. Rule 2 fires, yielding "F". "F" is added to the database.
    
- **Cycle 3:** Rule 3 is selected. It fires and yields "X", which is added to the database.
    
- **Cycle 4:** Now that "X" is a fact, Rule 4 can match. Rule 4 fires and adds "Y" to the database.
    
- **Cycle 5:** Now that "Y" is a fact, Rule 5 matches. Rule 5 fires and adds "Z" to the database. "Z" is the goal, so the process successfully stops.
    

```python
    def forward_chaining(self, goal: str) -> None:

        print("\n--- Starting Forward Chaining ---")
        self.reset_facts()
        cycle = 1

        print(f"Initial Facts: {self.current_facts}")
        if goal not in self.current_facts and all(goal != rule.consequent for rule in self.rules):

            print(f"Goal '{goal}' cannot be achieved with the current knowledge base.")
            return

        new_fact_added = True

        while goal not in self.current_facts and new_fact_added:
            new_fact_added = False
            for rule in self.rules: # First fit conflict resolution strategy
                if rule.consequent not in self.current_facts:
                    if all(self.evaluate_condition(antecedent) for antecedent in rule.antecedents):
                        print(f"Applying Rule: {rule}")
                        self.current_facts.add(rule.consequent)
                        new_fact_added = True

            print(f"Cycle {cycle}: Current Facts = {self.current_facts}")
            cycle += 1

        if goal in self.current_facts:
            print(f"Success! Goal '{goal}' reached.")
        else:
            print(f"Failed. Goal '{goal}' could not be reached.")
```

---

## 5. Backward Chaining Systems (Goal-Driven)

Backward chaining is the exact opposite. It starts with a hypothesized goal and works _backwards_ to see if there is enough evidence in the initial facts to prove it. **The 3-Step Process:**

1. Select rules that have conclusions matching your current goal.
    
2. Replace the current goal with that rule's premises. These premises become your new "sub-goals".
    
3. Work backwards recursively until all sub-goals are proven true (either because they are already facts in memory, or the user inputs them). If the evidence doesn't match, you start over with a new hypothesis.
    

**Example Walkthrough (Same database and rules as above):**

- **Goal:** Prove Z.
    
- **Step 1:** Rule 5 is the only rule where the conclusion is Z.
    
- **Step 2:** Rule 5's premises are D and Y. "D" is already a known fact, but "Y" is not. Therefore, "Y" becomes the new sub-goal.
    
- **Step 3 (Backchaining for Y):** Rule 4's conclusion is Y. Its premises are A, B, and X. "A" and "B" are facts, but "X" is not. "X" becomes the new sub-goal.
    
- **Step 4 (Backchaining for X):** Rule 3's conclusion is X. Its premises are C, D, and E. Because C, D, and E are _all_ already in our database of facts, we have proven the sub-goal X.
    
- **Resolving:** Because X is proven, Rule 4 can fire, proving Y. Because Y is proven, Rule 5 can fire, successfully proving the original goal Z.

| **Feature**              | **Forward Chaining**                                                                                     | **Backward Chaining**                                                                                                              |
| ------------------------ | -------------------------------------------------------------------------------------------------------- | ---------------------------------------------------------------------------------------------------------------------------------- |
| **Fundamental Approach** | **Data-Driven:** It moves from the bottom up.                                                            | **Goal-Driven:** It moves from the top down.                                                                                       |
| **Starting Point**       | Starts with the **initial facts** (the data currently in the working memory).                            | Starts with a **hypothesis or goal** (what you want to prove).                                                                     |
| **Ending Point**         | Stops when a **goal is reached** or when no more rules can be fired.                                     | Stops when all **sub-goals are proven true** by the initial facts.                                                                 |
| **Rule Matching**        | Matches the current facts against the **IF part** (antecedent/premise) of the rules.                     | Matches the current goal/sub-goal against the **THEN part** (consequent/conclusion) of the rules.                                  |
| **Action Taken**         | If a rule fires, it extracts the conclusion and **adds it as a new fact** to the database.               | If a rule matches, it extracts the premises and **sets them as new sub-goals** to be proven.                                       |
| **Analogy**              | Like an investigator piecing together clues at a crime scene to figure out what happened and who did it. | Like a doctor starting with a suspected diagnosis (e.g., "flu") and checking if the patient has the specific symptoms to prove it. |