---
{"dg-publish":true,"permalink":"/3rd-year/cognitive-science/after-mid/lecture-8-kalman-filter/","dg-note-properties":{}}
---


### The Kalman Filter: Continuous State Tracking

Unlike Hidden Markov Models (HMMs) and Dynamic Bayesian Networks (DBNs) which often deal with discrete probability states (e.g., "Raining" vs "Not Raining"), a Linear Dynamical System using a Kalman Filter tracks continuous variables (e.g., position, velocity, temperature).

The Kalman Filter represents uncertainty using Gaussian (normal) distributions, defined by a **mean** (the most likely estimate, $x$) and a **variance/covariance** (the uncertainty, $P$).

The filter operates in a continuous two-step loop:

**1. The Predict Step (Time Update)**

The filter uses the laws of physics or a known mathematical model to guess where the system will be next. Because no model is perfect, this step injects **Process Noise ($Q$)**, which always _increases_ our uncertainty ($P$).

- **Predicted State:** $x_{pred} = x_{t-1} + \text{motion}$
    
- **Predicted Uncertainty:** $P_{pred} = P_{t-1} + Q$
    

**2. The Update Step (Measurement Update)**

The system takes a reading from a sensor. Sensors are inherently flawed, so this measurement has its own **Measurement Noise ($R$)**. The filter calculates the **Kalman Gain ($K$)** to figure out how much to trust the sensor versus its own prediction.

- **Kalman Gain:** $K = \frac{P_{pred}}{P_{pred} + R}$
    
- **New State Estimate:** $x_t = x_{pred} + K(\text{Measurement} - x_{pred})$
    
- **New Uncertainty:** $P_t = (1 - K)P_{pred}$
    

_Note on Tuning: As mentioned in the lecture notes, if $Q$ is high, the filter trusts the sensors more and becomes "jumpy." If $R$ is high, it trusts its own predictions more and becomes "sluggish."_

---

### A 1D Numerical Example

Imagine a robot moving in a straight line. We want to track its position along an x-axis.

- It starts at position 0, moving at 1 meter per second.
    
- **Initial Estimate ($x_0$):** 0
    
- **Initial Uncertainty ($P_0$):** 1.0 (We are somewhat unsure exactly where the center of the robot is)
    
- **Process Noise ($Q$):** 0.1 (We trust our motors and physics model quite a bit)
    
- **Measurement Noise ($R$):** 0.5 (Our GPS/sensor is somewhat noisy)
    

**Time Step 1 ($t=1$):**

**Step 1: Predict**

The robot was at 0 and moves at 1 m/s, so we predict it is now at 1.0. We add the process noise to our uncertainty.

- $x_{pred} = 0 + 1.0 = 1.0$
    
- $P_{pred} = 1.0 + 0.1 = 1.1$
    

**Step 2: Measure**

The robot's GPS sensor takes a reading. Let's say it reads a slightly inaccurate value of **1.2**.

**Step 3: Update (Calculate Kalman Gain)**

We calculate $K$ to weigh our prediction (1.0) against the measurement (1.2).

- $K = \frac{1.1}{1.1 + 0.5} = \frac{1.1}{1.6} \approx 0.6875$
    
    _(Because $K$ is closer to 1, it means we are leaning slightly more toward trusting the measurement than our prediction, since our prediction uncertainty 1.1 is higher than the sensor noise 0.5)._
    

**Step 4: Update (Final State and Uncertainty)**

Now we calculate the final, optimized position estimate and update our confidence.

- **New Position ($x_1$):** $1.0 + 0.6875 \times (1.2 - 1.0) = 1.0 + (0.6875 \times 0.2) = \mathbf{1.1375}$
    
- **New Uncertainty ($P_1$):** $(1 - 0.6875) \times 1.1 = \mathbf{0.34375}$
    

**The Result:** The filter cleverly blended the prediction (1.0) and the sensor reading (1.2) to arrive at 1.1375. Crucially, as highlighted in the lecture snippet, observe how the uncertainty $P$ dropped dramatically from 1.0 down to 0.34. Even though moving forward added process noise, blending the two independent sources of information significantly increased the filter's overall confidence.

---
### 1. The Intuition: Overlapping Certainty

The lecture begins by visually explaining _why_ the Kalman Filter works using Probability Density Functions (Gaussian curves).

- A system starts with an initial state estimate that has a specific variance (uncertainty).
    
- When the system predicts its next state through movement, the variance widens, meaning uncertainty increases.
    
- A sensor provides a measurement, which has its own independent curve and variance.
    
- **The Magic:** By multiplying these two probability curves together, the filter produces an "Optimal state estimate". Crucially, the resulting curve is narrower and taller than both the prediction and the measurement curves, proving that combining two uncertain sources results in a higher overall certainty.
    

### 2. The Scalar Kalman Filter & The Gain ($K$)

The presentation mathematically dissects the 1D (scalar) version of the filter to explain the behavior of the Kalman Gain ($KG$ or $K$).

The formula for the gain is defined as the ratio of the Estimate Error ($E_{st}$) to the total error (Estimate Error + Measurement Error, $E_{mes}$):

$$K = \frac{E_{st}}{E_{st} + E_{mes}} = \frac{1}{1 + \frac{E_{mes}}{E_{st}}}$$

This creates two important extreme bounds for how the filter behaves:

- **Trusting the Sensor ($K \rightarrow 1$):** If the measurement error is effectively zero ($E_{mes} = 0$), the sensors are perfectly accurate. The gain becomes 1, and the filter updates its current state to match the measurement exactly ($st_t = MES$).
    
- **Trusting the Prediction ($K \rightarrow 0$):** If the measurement error is incredibly high (sensors are inaccurate), the denominator approaches infinity, driving the gain toward 0. The filter ignores the faulty sensor data and relies entirely on its previous prediction ($st_t = st_{t-1}$).
    

### 3. The Matrix Kalman Filter

Real-world robotics rarely operate in 1D. A robot needs to track position and velocity across X and Y axes simultaneously. To do this, the scalar equations are upgraded to matrices.

**The Prediction Step:**

- **State ($x_k$):** $x_k = Ax_{k-1} + Bu_k + w_k$. The state matrix ($x$) is updated by multiplying the previous state by the transition matrix ($A$), adding any control inputs ($u$) modified by the control matrix ($B$), and accounting for process noise ($w$).
    
- **Uncertainty ($P_k$):** $p_k = Ap_{k-1}A^T + Q_k$. The covariance matrix ($p$) is projected forward using $A$ and its transpose $A^T$, while adding the process noise covariance ($Q$).
    

**The Update Step:**

- **Kalman Gain ($K$):** $K = \frac{p_k H}{H p_k H^T + R}$. This uses the observation matrix ($H$) to map the state space to the measurement space, factoring in sensor noise ($R$).
    
- **Final State ($x_k$):** $x_k = x_k + K[Y - H x_k]$. The filter calculates the residual (the difference between the actual measurement $Y$ and the predicted measurement $H x_k$) and scales it by $K$ to correct the state.
    
- **Final Uncertainty ($P_k$):** $p_k = (I - KH)p_k$. The uncertainty is reduced based on how much new information was gained.
    

### 4. Designing a Filter using Kinematics

The lecture concludes by showing how to build the $A$ and $B$ matrices using standard Newtonian physics.

If you are tracking 1D distance and velocity, the state matrix is $x = \begin{bmatrix} x \\ \dot{x} \end{bmatrix}$. Using the kinematic equation $x = x_0 + vt + \frac{1}{2}at^2$, the matrices are derived as follows:

- **The Transition Matrix ($A$):** Models how position and velocity naturally evolve over a time step ($\Delta t$).
    
    $$A = \begin{bmatrix} 1 & \Delta t \\ 0 & 1 \end{bmatrix}$$
    
- **The Control Matrix ($B$):** Models how external acceleration ($a$) impacts both position and velocity.
    
    $$B = \begin{bmatrix} 0.5 \Delta t^2 \\ \Delta t \end{bmatrix}$$
    

By plugging these physically derived matrices into the Kalman equations, the algorithm can accurately track a moving object over time.

---
