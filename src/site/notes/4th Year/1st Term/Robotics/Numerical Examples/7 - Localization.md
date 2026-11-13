---
{"dg-publish":true,"permalink":"/4th-year/1st-term/robotics/numerical-examples/7-localization/","dg-note-properties":{}}
---


# An error model for odometric position estimation


An initial robot pose, wheel parameters, and encoder measurements illustrate how these kinematic and error equations update the robot's state.

  

### 1. Setup Parameters & Initial State

- **Wheelbase ($b$):** $0.5\text{ m}$ (distance between the two contact wheels)
    
      
    
- **Initial Pose ($p$):**
    
      
    
    $$p = \begin{bmatrix} x \\ y \\ \theta \end{bmatrix} = \begin{bmatrix} 2.00\text{ m} \\ 1.00\text{ m} \\ 0\text{ rad} \end{bmatrix}$$
    
    _(The robot starts facing along the positive $X_I$ axis)_
    
      
    

Over a single sample interval $\Delta t$, the wheel encoders report the following linear displacements:

  

- **Right wheel travel ($\Delta S_r$):** $0.22\text{ m}$
    
      
    
- **Left wheel travel ($\Delta S_l$):** $0.18\text{ m}$
    
      
    

### 2. Forward Kinematics (Dead Reckoning Step)

Using the equations from the slide:

  

#### Step A: Average Linear Displacement ($\Delta S$)

$$\Delta S = \frac{\Delta S_r + \Delta S_l}{2} = \frac{0.22 + 0.18}{2} = 0.20\text{ m}$$

  

#### Step B: Change in Orientation ($\Delta \theta$)

$$\Delta \theta = \frac{\Delta S_r - \Delta S_l}{b} = \frac{0.22 - 0.18}{0.50} = \frac{0.04}{0.50} = +0.08\text{ rad} \quad (\approx 4.58^\circ)$$

  

#### Step C: Average Heading During the Step

The trajectory is curved; using the mid-point angle $\theta + \frac{\Delta \theta}{2}$ improves positional accuracy over simple first-order Euler integration:

  

$$\theta_{\text{mid}} = \theta + \frac{\Delta \theta}{2} = 0 + \frac{0.08}{2} = 0.04\text{ rad}$$

  

Evaluating the trigonometric components:

  

$$\cos(0.04) \approx 0.9992$$

$$\sin(0.04) \approx 0.03999$$

#### Step D: Position Increment ($\Delta x, \Delta y$)

$$\Delta x = \Delta S \cos\left(\theta + \frac{\Delta \theta}{2}\right) = 0.20 \times 0.9992 \approx 0.1998\text{ m}$$

  

$$\Delta y = \Delta S \sin\left(\theta + \frac{\Delta \theta}{2}\right) = 0.20 \times 0.03999 \approx 0.0080\text{ m}$$

  

#### Step E: Updated Pose ($p'$)

$$p' = \begin{bmatrix} x' \\ y' \\ \theta' \end{bmatrix} = \begin{bmatrix} x \\ y \\ \theta \end{bmatrix} + \begin{bmatrix} \Delta x \\ \Delta y \\ \Delta \theta \end{bmatrix} = \begin{bmatrix} 2.00 \\ 1.00 \\ 0 \end{bmatrix} + \begin{bmatrix} 0.1998 \\ 0.0080 \\ 0.08 \end{bmatrix} = \begin{bmatrix} 2.1998\text{ m} \\ 1.0080\text{ m} \\ 0.08\text{ rad} \end{bmatrix}$$

  

### 3. Interpreting the Error Model ($\theta_{\text{goal}} + \frac{\theta_{\text{error}}}{2}$)

The top equations model a path following execution where the robot commands a heading change towards a goal $\theta_{\text{goal}}$, but incurs an angular tracking error $\theta_{\text{error}}$ (due to wheel slippage, encoder noise, or unequal wheel diameters):

  

$$\Delta x = \Delta s \cos\left(\theta_{goal} + \frac{\theta_{error}}{2}\right), \quad \Delta y = \Delta s \sin\left(\theta_{goal} + \frac{\theta_{error}}{2}\right)$$

  

Suppose:

  

- **Target orientation increment:** $\theta_{goal} = 0.08\text{ rad}$
    
      
    
- **Slippage error:** One wheel slips, introducing $\theta_{error} = -0.02\text{ rad}$
    
      
    
- **Actual path displacement:** $\Delta s = 0.20\text{ m}$
    
      
    

Effective propagation heading:

  

$$\theta_{\text{eff}} = \theta_{goal} + \frac{\theta_{error}}{2} = 0.08 + \frac{-0.02}{2} = 0.07\text{ rad}$$

  

Resulting actual movement:

  

- $\Delta x = 0.20 \cos(0.07) \approx 0.20 \times 0.9976 = 0.1995\text{ m}$
    
      
    
- $\Delta y = 0.20 \sin(0.07) \approx 0.20 \times 0.0699 = 0.0140\text{ m}$
    
      
    

Comparing this against the intended path ($\theta_{error} = 0 \implies \Delta y = 0.20 \sin(0.08) \approx 0.0160\text{ m}$) shows how the angular error $\theta_{error}$ directly perturbs the orthogonal drift component $\Delta y$ over distance $\Delta s$.

---

The difference comes down to **what the equations are used for** and **which perspective of the robot's motion they describe**:

  

### 1. Point 2: Forward Kinematics (Dead Reckoning) — _The Observer / Tracker_

- **Perspective:** The robot looking backward at what already happened.
    
      
    
- **Input:** Physical sensor readings from the wheel encoders ($\Delta S_r, \Delta S_l$).
    
      
    
- **Purpose:** Integration over time. The robot reads its tick counters, calculates the incremental arc displacement ($\Delta S$) and angular rotation ($\Delta \theta$):
    
      
    
    $$\Delta \theta = \frac{\Delta S_r - \Delta S_l}{b}$$
    
    Then it estimates where it has moved in the global coordinate frame:
    
      
    
    $$\begin{bmatrix} x' \\ y' \\ \theta' \end{bmatrix} = \begin{bmatrix} x \\ y \\ \theta \end{bmatrix} + \begin{bmatrix} \Delta S \cos(\theta + \frac{\Delta \theta}{2}) \\ \Delta S \sin(\theta + \frac{\Delta \theta}{2}) \\ \Delta \theta \end{bmatrix}$$
    
      
    
- **Key Idea:** There is no concept of a "target" or an "error" here; this is purely mathematical bookkeeping of actual wheel rotations under the assumption of pure rolling (no slip).
    
      
    

### 2. Point 3: Error Model — _The Controller / Planner_

- **Perspective:** The planner or controller comparing what _should_ have happened versus what _actually_ occurred.
    
      
    
- **Input:** A commanded trajectory/target angle ($\theta_{goal}$) and a disturbance/drift perturbation ($\theta_{error}$).
    
      
    
- **Purpose:** Modeling real-world physical imperfections (wheel slip, uneven terrain, imperfect motor speed synchronization).
    
      
    - If a controller commands the robot to steer towards a heading change $\theta_{goal}$, but physical factors cause it to deviate by an offset $\theta_{error}$, the actual orientation update experienced during that arc is:
        
          
        
        $$\Delta \theta = \theta_{goal} + \theta_{error}$$
        
    - Substituting this into the midpoint kinematic equation yields:
        
          
        
        $$\theta_{\text{mid}} = \theta + \frac{\Delta \theta}{2} \quad \longrightarrow \quad \theta_{goal} + \frac{\theta_{error}}{2}$$
        
        (assuming the base coordinate is aligned with the reference segment).
        
          
        
- **Key Idea:** This is used in **simulation, Kalman filter uncertainty modeling, and path tracking control** to analyze how angular tracking deviations cause Cartesian position drift $(\Delta x, \Delta y)$ over traveled distance $\Delta s$.
    
      
    

### Summary Comparison

| **Aspect**              | **Point 2: Kinematic Pose Update**                                      | **Point 3: Odometric Error Model**                                                            |
| ----------------------- | ----------------------------------------------------------------------- | --------------------------------------------------------------------------------------------- |
| **Equation**            | $\Delta x = \Delta S \cos\left(\theta + \frac{\Delta \theta}{2}\right)$ | $\Delta x = \Delta s \cos\left(\theta_{goal} + \frac{\theta_{error}}{2}\right)$               |
| **Origin of Variables** | Measured directly from wheel encoder ticks.                             | Split into _intended action_ ($\theta_{goal}$) and _stochastic deviation_ ($\theta_{error}$). |
| **Core Role**           | Odometry integration (calculating current $(x, y, \theta)$).            | Propagation of uncertainty and modeling dead-reckoning drift.                                 |