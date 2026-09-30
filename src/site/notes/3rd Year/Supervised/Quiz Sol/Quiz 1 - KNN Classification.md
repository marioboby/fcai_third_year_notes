---
{"dg-publish":true,"permalink":"/3rd-year/supervised/quiz-sol/quiz-1-knn-classification/","dg-note-properties":{}}
---

# Given the following 2-D training data and a new point P = (3, 4), use KNN with k = 3 and Euclidean distance to classify P.

Training Data:
1. Point A = (1, 2) → Class 0
2. Point B = (2, 3) → Class 0
3. Point C = (5, 5) → Class 1
4. Point D = (4, 4) → Class 1
5. Point E = (3, 1) → Class 0
6. Point F = (6, 6) → Class 1
   
Show all distance calculations and the final classification.

## Sol

To classify the new point $P = (3, 4)$ using the K-Nearest Neighbors (KNN) algorithm, we first need to calculate the Euclidean distance from $P$ to every point in the training dataset.

The formula for Euclidean distance between two points $(x_1, y_1)$ and $(x_2, y_2)$ is:

$$d = \sqrt{(x_2 - x_1)^2 + (y_2 - y_1)^2}$$

### Step 1: Distance Calculations

Let's calculate the distance from $P(3, 4)$ to each training point:

- **Distance to Point A (1, 2) → Class 0:**
    
    $$d(P, A) = \sqrt{(1 - 3)^2 + (2 - 4)^2} = \sqrt{(-2)^2 + (-2)^2} = \sqrt{4 + 4} = \sqrt{8} \approx 2.828$$
    
- **Distance to Point B (2, 3) → Class 0:**
    
    $$d(P, B) = \sqrt{(2 - 3)^2 + (3 - 4)^2} = \sqrt{(-1)^2 + (-1)^2} = \sqrt{1 + 1} = \sqrt{2} \approx 1.414$$
    
- **Distance to Point C (5, 5) → Class 1:**
    
    $$d(P, C) = \sqrt{(5 - 3)^2 + (5 - 4)^2} = \sqrt{2^2 + 1^2} = \sqrt{4 + 1} = \sqrt{5} \approx 2.236$$
    
- **Distance to Point D (4, 4) → Class 1:**
    
    $$d(P, D) = \sqrt{(4 - 3)^2 + (4 - 4)^2} = \sqrt{1^2 + 0^2} = \sqrt{1 + 0} = \sqrt{1} = 1.000$$
    
- **Distance to Point E (3, 1) → Class 0:**
    
    $$d(P, E) = \sqrt{(3 - 3)^2 + (1 - 4)^2} = \sqrt{0^2 + (-3)^2} = \sqrt{0 + 9} = \sqrt{9} = 3.000$$
    
- **Distance to Point F (6, 6) → Class 1:**
    
    $$d(P, F) = \sqrt{(6 - 3)^2 + (6 - 4)^2} = \sqrt{3^2 + 2^2} = \sqrt{9 + 4} = \sqrt{13} \approx 3.606$$
    

### Step 2: Sort the Distances

Now, we sort the calculated distances in ascending order to find the nearest neighbors:

1. **Point D:** $\sqrt{1} = 1.000$ _(Class 1)_
    
2. **Point B:** $\sqrt{2} \approx 1.414$ _(Class 0)_
    
3. **Point C:** $\sqrt{5} \approx 2.236$ _(Class 1)_
    
4. **Point A:** $\sqrt{8} \approx 2.828$ _(Class 0)_
    
5. **Point E:** $\sqrt{9} = 3.000$ _(Class 0)_
    
6. **Point F:** $\sqrt{13} \approx 3.606$ _(Class 1)_
    

### Step 3: Select the $k$ Nearest Neighbors

Since $k = 3$, we select the top 3 closest points to $P$:

1. **Point D** (Distance: 1.000) → **Class 1**
    
2. **Point B** (Distance: 1.414) → **Class 0**
    
3. **Point C** (Distance: 2.236) → **Class 1**
    

### Step 4: Majority Vote (Final Classification)

Among the $3$ nearest neighbors:

- **Class 1** appears $2$ times (Points D and C)
    
- **Class 0** appears $1$ time (Point B)
    

Because Class 1 has the majority vote, the new point **$P(3, 4)$ is classified as Class 1**.