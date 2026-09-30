---
{"dg-publish":true,"permalink":"/3rd-year/supervised/quiz-sol/quiz-2-knn-regression/","dg-note-properties":{}}
---

# Using KNN Regression with k = 3 and Euclidean distance, predict the target value for a new point P = (4, 2).

Training Data:
1. Point A = (1, 1) → y = 10
2. Point B = (3, 3) → y = 20
3. Point C = (5, 1) → y = 15
4. Point D = (6, 4) → y = 30
5. Point E = (3, 2) → y = 18
   
## Sol

To predict the target value for the new point $P = (4, 2)$ using K-Nearest Neighbors (KNN) Regression, we need to calculate the Euclidean distance from $P$ to each point in the training data, find the $k=3$ closest points, and average their target ($y$) values.

The Euclidean distance formula is:

$$d = \sqrt{(x_2 - x_1)^2 + (y_2 - y_1)^2}$$

### Step 1: Distance Calculations

Let's calculate the distance from $P(4, 2)$ to each training point:

- **Distance to Point A (1, 1) → y = 10:**
    
    $$d(P, A) = \sqrt{(1 - 4)^2 + (1 - 2)^2} = \sqrt{(-3)^2 + (-1)^2} = \sqrt{9 + 1} = \sqrt{10} \approx 3.162$$
    
- **Distance to Point B (3, 3) → y = 20:**
    
    $$d(P, B) = \sqrt{(3 - 4)^2 + (3 - 2)^2} = \sqrt{(-1)^2 + 1^2} = \sqrt{1 + 1} = \sqrt{2} \approx 1.414$$
    
- **Distance to Point C (5, 1) → y = 15:**
    
    $$d(P, C) = \sqrt{(5 - 4)^2 + (1 - 2)^2} = \sqrt{1^2 + (-1)^2} = \sqrt{1 + 1} = \sqrt{2} \approx 1.414$$
    
- **Distance to Point D (6, 4) → y = 30:**
    
    $$d(P, D) = \sqrt{(6 - 4)^2 + (4 - 2)^2} = \sqrt{2^2 + 2^2} = \sqrt{4 + 4} = \sqrt{8} \approx 2.828$$
    
- **Distance to Point E (3, 2) → y = 18:**
    
    $$d(P, E) = \sqrt{(3 - 4)^2 + (2 - 2)^2} = \sqrt{(-1)^2 + 0^2} = \sqrt{1 + 0} = \sqrt{1} = 1.000$$
    

### Step 2: Sort the Distances

Arranging the calculated distances in ascending order to find the closest neighbors:

1. **Point E:** $\sqrt{1} = 1.000$ _(y = 18)_
    
2. **Point B:** $\sqrt{2} \approx 1.414$ _(y = 20)_
    
3. **Point C:** $\sqrt{2} \approx 1.414$ _(y = 15)_
    
4. **Point D:** $\sqrt{8} \approx 2.828$ _(y = 30)_
    
5. **Point A:** $\sqrt{10} \approx 3.162$ _(y = 10)_
    

### Step 3: Select the $k$ Nearest Neighbors

For $k = 3$, we take the top 3 closest points to $P$:

1. **Point E** (y = 18)
    
2. **Point B** (y = 20)
    
3. **Point C** (y = 15)
    

### Step 4: Calculate the Predicted Value

In KNN Regression, the final prediction is the average (mean) of the target values of the $k$ nearest neighbors:

$$\hat{y} = \frac{y_E + y_B + y_C}{3}$$

$$\hat{y} = \frac{18 + 20 + 15}{3}$$

$$\hat{y} = \frac{53}{3} \approx 17.67$$

The predicted target value for point $P(4, 2)$ is **$17.67$**.