---
{"dg-publish":true,"permalink":"/3rd-year/cognitive-science/expanded-explanations/markov-localization-steps/","dg-note-properties":{}}
---


### **The General Blueprint for Markov Localization Problems**

Whether it is a 1D hallway, a 2D grid, colored doors, or landmarks, every single problem of this type loops through these four exact steps.

**Step 1: Initialization**

- Determine the starting state. If the robot is lost, divide $1.0$ by the total number of cells ($N$) for a uniform array. If the robot knows exactly where it is starting, that cell is $1.0$ and all others are $0.0$.
    

**Step 2: Prediction (The Move Step)**

- **Goal:** Update the belief based on the movement command.
    <br>
- **Formula:** $\overline{Bel}(x_i) = \sum \Big( P(x_i | x_{start}, action) \times Bel(x_{start}) \Big)$
    <br>
- **Execution:** For every cell $x_i$, add up the probabilities of arriving there from any other cell based on the odometry percentages (stalling, moving perfect, overshooting).
    <br>
- **Crucial Check:** Always check the boundary rules! Does the environment wrap around (like a circle), or does it have walls where probabilities pile up at the last cell?
    

**Step 3: Correction (The Sense Step)**

- **Goal:** Factor in the new sensor data.
    <br>
- **Formula:** $Raw(x_i) = P(SensorReading | x_i) \times \overline{Bel}(x_i)$
    <br>
- **Execution:** Multiply the new value of each cell (from Step 2) by the likelihood of getting that specific sensor reading if the robot were actually standing in that cell.
    

**Step 4: Normalization**

- **Goal:** Convert the raw numbers back into a valid probability distribution.
    <br>
- **Formula:** $Bel(x_i) = \frac{Raw(x_i)}{\sum Raw}$
    <br>
- **Execution:** Add up all the values calculated in Step 3 to find the total sum. Divide each individual value by that sum. The new array must add up to $1.0$.
    

This completes one full cycle. If the problem asks you to calculate a second movement or a second sensor reading, you simply take the final normalized array from Step 4, treat it as your new Step 1, and run the exact same loop again.