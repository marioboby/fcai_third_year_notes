---
{"dg-publish":true,"permalink":"/3rd-year/cognitive-science/after-mid/lecture-7-template-based-representations/","dg-note-properties":{}}
---

### 1. Foundations of Temporal Models

The primary goal of temporal models is to represent how a system's state changes over discrete time slices ($\Delta, 2\Delta, ..., T\Delta$).

- **State Space:** The system's state at any given time $t$ is represented by a set of random variables, denoted as $X^{(t)} = \{X_{1}^{(t)},...,X_{n}^{(t)}\}$.
    
- **The Chain Rule:** To find the probability of a specific sequence of events (a trajectory) over time, you use the chain rule: $P(X^{(0:T)}) = P(X^{(0)}) \prod_{t}^{T-1} P(X^{(t+1)}|X^{(0:t)})$.
    
- **The Markov Assumption:** To simplify the math, these models assume that the future is independent of the past, given the present. This reduces the complex chain rule to $P(X^{(t+1)}|X^{(t)})$.
    
more on it [[3rd Year/Cognitive Science/Expanded Explanations/Lecture 7#Chain Rule and Markov Assumption\|here]]

### 2. Hidden Markov Models (HMMs)

An HMM is a temporal model where the true state of the system is hidden (latent), but we can measure observable evidence.

- **Structure:** It consists of Hidden Variables ($S^{(t)}$) and Observations ($O^{(t)}$), linked by an Observation Model $P(O^{(t)}|S^{(t)})$.
    
- **Example (The Robot & The Rain):** A robot indoors wants to know if it's raining ($R$), which is the hidden state. It can only observe if people are carrying umbrellas ($U$). By combining the transition model (how likely rain continues from yesterday) and the sensor model (how likely umbrellas mean rain), the robot uses Bayes' rule to update its belief about the weather. more on it [[3rd Year/Cognitive Science/Expanded Explanations/Lecture 7#HMM Numerical Example\|here]]
    

### 3. Dynamic Bayesian Networks (DBNs)

DBNs are a generalization of HMMs. While an HMM uses a single, atomic variable for the hidden state, a DBN uses a **factored representation**—meaning the hidden state is broken down into multiple interconnected variables.

- **Components:** A DBN requires an Initial Network defining the starting state ($B_0$) and a Transition Model defining how states evolve across time slices.
    
- **Flexibility:** Because they factor the state, DBNs scale better and can model overlapping causal influences compared to the rigid structure of HMMs.
    
- **Example:** Expanding the weather example, a DBN might split the hidden state into two variables: Rain ($R_t$) and Wind ($W_t$), both of which independently transition over time but jointly influence the observation of an Umbrella ($U_t$). more on it [[3rd Year/Cognitive Science/Expanded Explanations/Lecture 7#Dynamic BN Example\|here]]
    

### 4. Linear Dynamical Systems (LDS) & The Kalman Filter

While HMMs and DBNs generally deal with discrete probabilities, a Linear Dynamical System models continuous state evolution. It relies on linear algebra for transitions and assumes Gaussian (normal) noise.

- **The Kalman Filter:** This is the optimal mathematical algorithm used to estimate the hidden state in an LDS, heavily used in robotics for tracking and localization.
    
- **The Two-Step Loop:**
    
    1. **Prediction (Time Update):** The system projects the state forward mathematically based on movement and control inputs. Because of inherent process noise ($Q$), uncertainty _always_ increases during this step.
        
    2. **Correction (Measurement Update):** The system takes a noisy sensor reading and uses it to correct the prediction.
        
- **The Kalman Gain ($K$):** This is the crucial weighting factor used during the Correction phase. It determines what to trust more:
    
    - If sensors are highly accurate, $K \rightarrow 1$, and the filter heavily trusts the measurement.
        
    - If the prediction is highly accurate, $K \rightarrow 0$, and the filter heavily trusts the prediction.
        
- **Result:** By balancing the prediction and the measurement, the overall uncertainty (Error Covariance, $P$) drops significantly.
    

### Comparing the Models

The lecture provides a useful matrix to distinguish these approaches:

- **HMM:** Discrete state space, solved via Forward-Backward algorithms.
    
- **LDS:** Continuous state space, solved via the Kalman Filter.
    
- **DBN:** Mixed/Factored state space, often requiring complex Particle Filters to solve due to NP-Hard complexity.
    

