---
{"dg-publish":true,"permalink":"/3rd-year/reasoning/before-mid/lecture-5-introduction-to-fuzzy-logic/","dg-note-properties":{}}
---

# **Fuzzy Logic: Introduction**

- **Core Concept:** Fuzzy logic is a way to represent variation or imprecision in logic, enabling the use of natural language in approximate reasoning (e.g., "If it is sunny and warm today, I will drive fast").
  <br>
- **Linguistic Variables Examples:**
    - Temp: {freezing, cool, warm, hot}
    - Cloud Cover: {overcast, partly cloudy, sunny}
    - Speed: {slow, fast}

## **Crisp (Traditional) Variables**

- **Definition:** Crisp variables represent precise, binary quantities.
- **Characteristics:**
    - Variables have exact numerical values (e.g., x = 3.1415296).
    - Propositions are strictly True or False (A ∈{0,1}).
    - A statement like "Richard is greedy" is either completely true (1) or completely false (0).
      
      شغل أبيض و أسود بس

## **Fuzzy Sets**

- **Definition:** Fuzzy Sets represent the _degree_ to which a quality is possessed.
- **Characteristics:**
    - Values are in the range of [0, 1].
    - Example: Greedy(Richard) = 0.7 (meaning Richard is 70% greedy).

## **Fuzzy Linguistic Variables**

- **Definition:** These variables use words (linguistic terms) to represent qualities spanning a spectrum.
  <br>
- **Example:** Temp: {Freezing, Cool, Warm, Hot}.

## **Membership Functions**

- **Concept:** A membership function determines the "Degree of Truth" or "Membership" of a crisp input value within a fuzzy set.
- **Example Calculation:** A temperature of 36 F° might be determined to be 70% Freezing (0.7) and 30% Cool (0.3).

## **Fuzzy Logic Connectives**

- **Purpose:** To use fuzzy membership functions in predicate logic.
- **Connectives:** Fuzzy Conjunction ($\land$, like AND) and Fuzzy Disjunction ($\lor$, like OR) operate on degrees of membership in fuzzy sets.

**Fuzzy Disjunction ($\lor$)**

- **Rule:** $A \lor B = \max(A, B)$.
- **Interpretation:** The resulting quality C (the disjunction of A and B) is equal to the maximum membership value of A or B.

**Fuzzy Conjunction ($\land$)**

- **Rule:** $A \land B = \min(A, B)$.
- **Interpretation:** The resulting quality C (the conjunction of A and B) is equal to the minimum membership value of A or B.

**Example: Fuzzy Conjunction**

- **Problem:** Calculate $A \land B$ given that A is .4 and B is 20.
- **Process:**
    1. Determine degrees of membership for A and B (e.g., A = 0.7, B = 0.9).
    2. Apply Fuzzy AND: $A \land B = \min(A, B) = \min(0.7, 0.9) = 0.7$.

**Fuzzy Control**

- **Definition:** Combines fuzzy linguistic variables with fuzzy logic to create a control system.
- **Example:** Speed Control (Driving speed depends on the weather).

**Fuzzy Control Variables**

- **Inputs:** Temperature ({Freezing, Cool, Warm, Hot}) and Cloud Cover ({Sunny, Partly, Overcast}).
- **Output:** Speed ({Slow, Fast}).

**Fuzzy Control Rules**

- **Structure:** IF-THEN rules using linguistic variables.
- **Examples:**
    - IF it's Sunny AND Warm, drive Fast (Sunny(Cover) $\land$ Warm(Temp) $\Rightarrow$ Fast(Speed)).
    - IF it's Cloudy AND Cool, drive Slow (Cloudy(Cover) $\land$ Cool(Temp) $\Rightarrow$ Slow(Speed)).

![Pasted image 20260331214730.png](/img/user/imgs/Pasted%20image%2020260331214730.png)
## **Fuzzification and Rule Evaluation Example**

- **Scenario:** Input is 65 F° and 25% Cloud Cover.
- **Fuzzification (Input Membership):**
    - 65 F° $\Rightarrow$ Cool = 0.4, Warm = 0.7.
    - 25% Cover $\Rightarrow$ Sunny = 0.8, Cloudy = 0.2.
- **Rule Evaluation (Applying min for AND):**
    - Sunny $\land$ Warm: $\min(0.8, 0.7) = 0.7 \Rightarrow$ Fast = 0.7.
    - Cloudy $\land$ Cool: $\min(0.2, 0.4) = 0.2 \Rightarrow$ Slow = 0.2.

## **Defuzzification**

- **Goal:** Convert the fuzzy output (Speed is 20% Slow and 70% Fast) back into a crisp, non-fuzzy output (a specific speed).
  <br>
- **Method:** The final speed is calculated using a **weighted mean** (based on centroids/locations where membership is 100%).
  <br>
- **Result:** Speed $= (2 \times 25 + 7 \times 75) / (2+7) = 63.8 \text{ mph}$.

## **Notes: Follow-up Points**

- Fuzzy Logic Control allows for **smooth interpolation** between variable centroids with few rules.
- It provides a natural way to **model human expertise** in a computer program, which is not possible with crisp (traditional Boolean) logic.

**Notes: Drawbacks to Fuzzy logic**

- It **requires tuning** of membership functions.
- It may not **scale well** to large or complex problems.

## **Fuzzy Logic in Knowledge Bases (Benefits)**

- **Handles Uncertainty:** Ideal for imprecise or uncertain real-world information.
  <br>
- **Closer to Human Reasoning:** Mimics how humans make decisions using approximate data.
<br>
- **Enhances Knowledge Representation:** Supports vague concepts like "high temperature" or "low risk."
  <br>
- **Flexibility:** Combines qualitative (linguistic) and quantitative (numerical) knowledge.
  <br>
- **Robustness:** Supports incomplete or noisy data.
  <br>
- **Transparency:** Facilitates rule-based systems using transparent IF-THEN rules.

## **Application: Fuzzy Logic in Medical Diagnosis System**

- **Objective:** To diagnose illness severity based on fuzzy symptoms (Temperature, Headache, Fatigue).
  <br>
- **Rules Example:** IF temperature is High AND headache is Severe AND fatigue is High THEN illness severity is High.
  <br>
- **Why it helps:** Medical symptoms are often vague, fuzzy sets allow for **overlapping values** (e.g., a temperature being both "Normal" and "High" to some degree), and it provides a **soft decision** (e.g., "Severity leaning towards High") rather than a binary diagnosis.