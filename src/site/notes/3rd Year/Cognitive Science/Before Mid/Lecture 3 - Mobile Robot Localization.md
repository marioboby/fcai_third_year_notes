---
{"dg-publish":true,"permalink":"/3rd-year/cognitive-science/before-mid/lecture-3-mobile-robot-localization/","dg-note-properties":{}}
---

This lecture shifts from abstract learning concepts into applied robotics, specifically how a robot figures out where it is in the world.

# Outline
[[3rd Year/Cognitive Science/Before Mid/Lecture 3 - Mobile Robot Localization#**Part 1 The Problem of Uncertainty**\|The Problem of Uncertainty]]
[[3rd Year/Cognitive Science/Before Mid/Lecture 3 - Mobile Robot Localization#**Part 2 Odometric Error Model** (مش علينا)\|Odometric Error Model]]
# **Part 1: The Problem of Uncertainty**

The core theme of the lecture is summarized by a quote from G.K. Chesterton: =="There is only one thing certain and that is that nothing is certain"==. A robot can never know its exact location because it constantly deals with two types of noise:

- **Sensor Noise:** Sensors are imperfect. For example, 
	- ultrasonic sensors suffer from acoustically reflective environments 
	- and multipath interference 
	- while color cameras can be confused by changes in illumination . 

- Furthermore, there is "**Sensor Aliasing**," meaning a single sensor reading is usually not unique enough to identify the robot's location on its own .

- **Effector Noise:** The robot's physical movement is also uncertain . Errors accumulate because the floor might be sloped, the wheels might slip, a human might push the robot, or the wheels might have slightly unequal diameters .

See more on the types of noises [[3rd Year/Cognitive Science/Expanded Explanations/Lecture 3#Sensor Uncertainty\|here]]

# **Part 2: Odometric Error Model** (مش علينا)

When a robot moves, it uses "odometry" to estimate its position using internal sensors (like measuring how many times its wheels have turned). The position is tracked using $x$ and $y$ coordinates, along with its orientation angle $\theta$.

However, because of the effector noise mentioned above, small errors in the rotation of the right wheel ($\Delta S_r$) and left wheel ($\Delta S_l$) cause the robot's calculated position to drift from its actual position over time. The mathematical error model calculates this drift by factoring in the distance traveled ($\Delta s$) and the angular error ($\theta_{error}$) .

# Part 3: Probabilistic Map-Based Localization

Because odometry errors accumulate endlessly, pure deduction fails. Instead, the robot uses probabilistic robotics: it calculates the _probability_ that it is in a given configuration. It combines data from:

- **Proprioceptive sensors:** Internal sensors (like odometry) that track movement.

- **Exteroceptive sensors:** External sensors (like cameras or ultrasonics) that look at the environment.

To process this, the lecture introduces the **Markov Technique**. This method allows the robot to track multiple, completely disparate possible locations simultaneously . 

To do this, the robot's continuous world must be chopped up into a discrete map. This is usually done using either a **Topological Graph** (nodes and connecting lines, like a subway map) or a **Geometric Grid** (a 2D array of square cells) . The tighter the discrete grid resolution, the more memory the robot requires.


# **Part 4: The Sense-Move Cycle & Numerical Examples**

The robot navigates by continuously looping a perception rule based on Bayes' Theorem: $P(B|A) = \frac{P(B)P(A|B)}{P(A)}$ . Every time the robot takes an action, the probabilities shift.

**The Colored Door Example:** Imagine a robot walking past a row of green and red doors. Its color sensor is imperfect: it guesses correctly 60% of the time, and guesses the wrong color 20% of the time .

1. If the robot senses "red", it updates its belief for every single door. It multiplies its prior belief by the likelihood of seeing red ($0.6$ for actual red doors, $0.2$ for green doors) .

2. It then normalizes the numbers (dividing by the total sum of all probabilities, e.g., $0.36$) so that the total probability across all doors equals exactly 1 (100%).

3. As the robot moves 1 or 2 cells to the right and takes new color readings, the math dynamically shifts the highest probability mass to the robot's true location .

**The 2D Grid Example:** This same logic applies to a 2D map. If a robot is dropped into a 16-cell (4x4) room, its initial state is totally unknown, meaning every single cell has a $1/16$ probability of containing the robot . As the robot moves and senses walls, it overlays probability matrices (Prediction, Correction, and Correlation steps) to filter out impossible cells until it finds itself .

See the numerical examples solution [[3rd Year/Cognitive Science/Expanded Explanations/Lecture 3#Numerical Examples\|here]]

# **Part 5: Algorithm Summary**

The entire localization process runs endlessly in a simple pseudo-code loop , summarized in four steps :

1. **Prediction Step:** The robot updates its belief state $P(x)$ using its internal proprioceptive sensors (guessing where it moved).

2. **Correction Step:** The robot takes a reading with its exteroceptive sensors to calculate a likelihood, like $p(x|red)$.

3. **Update Step:** The robot mathematically merges the Prediction and Correction data.

4. **Normalization Step:** The robot divides the raw numbers by the total sum so that the system remains a valid probability distribution.