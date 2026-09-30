---
{"dg-publish":true,"permalink":"/3rd-year/cognitive-science/expanded-explanations/lecture-8/","dg-note-properties":{}}
---


The Kalman Filter can look like an intimidating wall of linear algebra at first glance, but beneath the matrix multiplication, the core concept is incredibly elegant and intuitive. It is arguably the most famous algorithm for tracking and predicting the state of a moving system over time.

This is a breakdown of how it works, the intuition behind its math, and exactly how the simple 1D scalar version scales up into the full matrix version.

### 1. The General Intuition: Fusing Two Imperfect Guesses

Imagine you are building a computer vision system to track vehicles moving on a highway. You want to know a car's exact position, but you face two major problems:

1. **Your Sensors are Noisy (Measurement):** The object detector (like YOLO) drawing the bounding box isn't perfect. Glare, distance, and camera framerates mean the box jitters around slightly. We call this **Measurement Noise**.
    
2. **Your Physics Model is Flawed (Prediction):** You know the car was moving 100 km/h a fraction of a second ago, so you can predict where it _should_ be now. However, physics equations don't account for wind gusts, sudden braking, or potholes. We call this **Process Noise**.
    

The Kalman Filter's brilliance is that it refuses to fully trust either one. Instead, it computes a **weighted average** of your _physics prediction_ and your _sensor measurement_.

If the camera is temporarily blinded by the sun (high measurement noise), the filter relies more on the physics prediction to keep the box moving smoothly. If the car suddenly slams on the brakes (violating the physics model), the filter shifts its trust back to the camera measurements to catch the sudden change.

### 2. The Intuition Behind the Rules (The Cycle)

The Kalman filter runs in a continuous, infinite loop of two distinct phases: **Predict** and **Update**.

#### Phase A: Predict (The Blind Guess)

In this step, you close your eyes and push your previous knowledge one step forward into the future using your math model.

- **State Prediction:** "Based on where the car was and how fast it was going, it should be exactly _here_ now."
    
- **Uncertainty Prediction:** "Because I am blindly guessing into the future without looking at my sensors, my confidence drops. My uncertainty (variance) **increases**."
    

#### Phase B: Update (The Reality Check)

Now, you open your eyes and take a measurement from your sensors.

- **The Residual (Error):** You calculate the difference between where you _predicted_ the car would be and where the _sensor_ actually says it is.
    
- **The Kalman Gain ($K$):** This is the magic number. It acts as a sliding scale between **0 and 1** (or a matrix acting similarly).
    
    - If $K \approx 1$: You trust the sensor completely.
        
    - If $K \approx 0$: You trust your physics prediction completely.
        
- **State Update:** You adjust your prediction slightly toward the sensor measurement, dictated by the Kalman Gain. This becomes your new official "State."
    
- **Uncertainty Update:** Because you just took a fresh measurement and fused the data, you are now more confident. Your uncertainty **decreases**.
    

### 3. Scalar vs. Matrix Kalman Filter Comparison

The **Scalar** Kalman filter is used when you only care about a single 1D variable (e.g., just the position $x$ on a straight line). The **Matrix** Kalman filter is used when you are tracking multiple correlated variables simultaneously (e.g., a car's 2D position $x, y$ and its velocities $v_x, v_y$).

The logic is perfectly identical; the matrix version just uses linear algebra to handle multiple dimensions and their covariances (how changing $x$ impacts $v_x$).

| **Step**    | **Concept**                              | **Scalar Form (1D)**                            | **Matrix Form (N-D)**                                                                                 |
| ----------- | ---------------------------------------- | ----------------------------------------------- | ----------------------------------------------------------------------------------------------------- |
| **Predict** | **State Estimate**<br>Where is it?       | $x_{p} = a \cdot x + b \cdot u$                 | $\mathbf{x}_{p} = \mathbf{F} \mathbf{x} + \mathbf{B} \mathbf{u}$                                      |
| **Predict** | **Uncertainty**<br>How unsure am I?      | $p_{p} = a^2 \cdot p + q$                       | $\mathbf{P}_{p} = \mathbf{F} \mathbf{P} \mathbf{F}^T + \mathbf{Q}$                                    |
| **Update**  | **Kalman Gain**<br>Who do I trust?       | $k = \frac{p_{p} \cdot h}{h^2 \cdot p_{p} + r}$ | $\mathbf{K} = \mathbf{P}_{p} \mathbf{H}^T (\mathbf{H} \mathbf{P}_{p} \mathbf{H}^T + \mathbf{R})^{-1}$ |
| **Update**  | **New State**<br>Adjusted belief.        | $x = x_{p} + k(z - h \cdot x_{p})$              | $\mathbf{x} = \mathbf{x}_{p} + \mathbf{K}(\mathbf{z} - \mathbf{H} \mathbf{x}_{p})$                    |
| **Update**  | **New Uncertainty**<br>Confidence rises. | $p = (1 - k \cdot h)p_{p}$                      | $\mathbf{P} = (\mathbf{I} - \mathbf{K} \mathbf{H})\mathbf{P}_{p}$                                     |

**Key to Matrix Variables:**

- $\mathbf{x}$: The State vector (e.g., position and velocity).
    
- $\mathbf{F}$: The State Transition matrix (the physics model pushing state forward).
    
- $\mathbf{P}$: The Covariance matrix (your uncertainty).
    
- $\mathbf{Q}$: Process Noise covariance (how much you distrust your physics model).
    
- $\mathbf{z}$: The Measurement vector (what the sensor reads).
    
- $\mathbf{H}$: The Observation matrix (maps your internal state format to your sensor's format).
    
- $\mathbf{R}$: Measurement Noise covariance (how much you distrust your sensors).
    
