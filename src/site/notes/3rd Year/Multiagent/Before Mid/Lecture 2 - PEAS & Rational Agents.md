---
{"dg-publish":true,"permalink":"/3rd-year/multiagent/before-mid/lecture-2-peas-and-rational-agents/","dg-note-properties":{}}
---

### **1. Agents and Environments**

- **Definition of an Agent:** An agent is anything that perceives its environment through **sensors** and acts upon that environment through **actuators**.
  <br>
- **Key Components:**
    - **Percept:** The agent's perceptual inputs at any specific instant.
    - **Percept Sequence:** The complete history of everything the agent has ever perceived.
    - **Agent Function:** A mathematical mapping that links any given percept sequence to an action ($f: P^* \rightarrow A$).
    - **Agent Program:** The internal software that runs on the physical **architecture** (hardware) to produce the agent function.
    - agent = architecture + program
<br>
- **Examples of Agents:**
    - **Human Agent:** Uses eyes and ears as sensors; uses hands, legs, and mouth as actuators.
    - **Robotic Agent:** Uses cameras and infrared range finders as sensors; uses motors as actuators.

### **2. Rationality**

- **Definition:** A rational agent is one that acts to achieve the best outcome (or best expected outcome under uncertainty). It is described as "doing the right thing".
  <br>
- **Factors of Rationality:** Rationality depends on four "pillars":
    1. The **performance measure** defining success.
    2. The agent's **prior knowledge** of the environment.
    3. The **actions** the agent can perform.
    4. The agent's **percept sequence** to date.
<br>
- **Distinctions:**
    - **Rationality vs. Omniscience:** Rationality is based on what is perceived, not necessarily all possible information (which may be unavailable).
      <br>
    - **Rationality vs. Perfection:** Rationality maximizes the _expected_ outcome, while perfection maximizes the _actual_ outcome.
      <br>
- **Autonomy:** An agent is autonomous if it can learn from its perceptions and act without human assistance. An ideal rational agent should have some autonomy and increase it through experience.

- **Need to learn**: To be rational agent, it is not only to gather information but also to learn as much as possible from what it perceives.

### **3. PEAS: Specifying the Task Environment**

To design an agent, one must specify its "PEAS" description:

- **P - Performance Measure:** The objective criterion for the agent's success (e.g., safety, speed, or profit).
  <br>
- **E - Environment:** The external world or context the agent operates in (e.g., roads, traffic, or a set of students).
  <br>
- **A - Actuators:** The tools the agent uses to change the environment (e.g., steering wheel, screen display, or robotic arm).
  <br>
- **S - Sensors:** The tools the agent uses to perceive the environment (e.g., cameras, GPS, or keyboard).

### **4. Task Environment Characteristics**

Task environments are classified by several dimensions:

- **Observability:** **Fully observable** (sensors see everything needed to choose an action) vs. **partially observable**.
  <br>
- **Certainty:** **Deterministic** (next state is determined only by the current state and action) vs. **stochastic** (uncertainty exists).
  <br>
- **Episodic vs. Sequential:** In **episodic** environments, the current action does not affect future actions; in **sequential** environments, agents must plan ahead.
  <br>
- **Static vs. Dynamic:** **Static** environments do not change while the agent is "thinking"; **dynamic** environments change over time.
  <br>
- **Discrete vs. Continuous:** **Discrete** environments have a limited number of distinct percepts and actions (like Chess); **continuous** environments use a range of values (like Taxi driving).
  <br>
- **Single agent vs. Multiagent:** Whether the agent works alone or interacts with others.
---
