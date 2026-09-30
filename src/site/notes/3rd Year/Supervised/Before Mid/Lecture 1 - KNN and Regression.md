---
{"dg-publish":true,"permalink":"/3rd-year/supervised/before-mid/lecture-1-knn-and-regression/","dg-note-properties":{}}
---

# Intro to ML and Supervised Learning



### 1. Introduction to Supervised Learning

Supervised learning requires a "teacher"—meaning the algorithm is trained on a dataset that already contains the desired output labels. The model processes input data, generates classifications, and corrects its errors based on the known labels until it learns the underlying pattern.

![Supervised Machine Learning process, AI generated](https://encrypted-tbn2.gstatic.com/licensed-image?q=tbn:ANd9GcQRJ5nB3bXnFwkkue3ivKSevkPplVK-ZyMRo2Ec9rM_1lk87eug0SDZq4ccZ98iVmd-R7aJn9RlAh1QTbDUofwtGG2R7kHfrdZeOJnjqY-B96AgNcA)


The slides break machine learning down into two primary supervised tasks:

- **Classification:** Used when the output is a discrete category (e.g., categorizing an image as a dog or a cat). Binary classification deals with two possible outcomes, while multiclass classification handles more.
    
- **Regression:** Used when predicting a continuous numerical value (e.g., predicting weight based on length). It can be univariate (one input feature) or multivariate (multiple input features).
    

### 2. Data Types & Processing

Before feeding data into an algorithm, you must understand its structure:

- **Structured Data:** Data neatly organized in rows and columns, such as relational databases and transaction data. This is generally easier to work with.
    
- **Unstructured Data:** Complex data like text, images, audio, and video. For example, when processing text for categorization, the raw text must first undergo **tokenization**, which splits sentences into individual words or tokens so the algorithm can process them.
    

### 3. Similarity Distances

Many algorithms rely on measuring how "close" or similar two data points are. The lecture highlights two fundamental distance metrics:

- **Euclidean Distance:** The straight-line distance between two points, calculated as the square root of the sum of squared differences: $\sqrt{\sum_{i=1}^{k}(x_{i}-y_{i})^{2}}$.
    
- **Manhattan Distance:** The sum of the absolute differences across all dimensions: $\sum_{i=1}^{k}|x_{i}-y_{i}|$.
    

### 4. Parameters vs. Hyperparameters

A critical distinction when designing machine learning models is knowing what the model learns versus what the programmer configures.

|**Concept**|**Description**|**Examples**|
|---|---|---|
|**Parameters**|Internal variables automatically learned and updated by the model during training.|Weights ($w_0, w_1$) in regression.|
|**Hyperparameters**|External configurations manually set by the developer before training begins.|$K$ in KNN, learning rate ($\eta$), max depth.|

To find the best hyperparameters, developers use tuning strategies:

- **Grid Search:** Exhaustively tests every combination (optimal but computationally expensive).
    
- **Random Search:** Randomly samples combinations (faster, often yielding near-optimal results).
    
- **Bayesian Optimization:** Iteratively predicts optimal configurations using probabilistic models.
    
- **Hyperband:** Combines random search with an early-stopping mechanism.

more on it later    

---
# KNN

KNN is a **non-parametric**, instance-based learning algorithm. Rather than learning an explicit mathematical mapping function during training, it simply stores the training data and performs computations only at test time (often called "lazy learning").

**How it works:**

1. Calculate the distance between the new test point and all existing training points.
    
2. Sort these distances in ascending order to find the $K$ closest neighbors.
    
3. **For Classification:** Apply the majority voting rule (assign the class most common among the neighbors).
    
4. **For Regression:** Calculate the mean average of the $K$ neighbors' values.
    
to see example, [[3rd Year/Supervised/Expanded Explanations/Lecture 1 Examples\|here]]

https://caramel-grey-424.notion.site/k-NN-simplified-318cd062d0ff80719a01e209069d6787?source=copy_link

السلام عليكم ورحمة الله وبركاته 🫡🩵، بصوا يا شباب، عارف إن اللهم بارك جزء كبير منكم مغطي الحاجات دي بالفعل 😅، وهي مش concept صعب، ولكن ده شرح لل k-NN بتفاصيل شوية للي حابب يفهم الموضوع بشكل أعمق، ال resources كلها هتلاقوها موجوده بإذن الله في الآخر خالص، ولو الكلام برضو مش واضح ممكن بإذن الله أبقا أسجل فيديو بسيط ولا حاجه بوضح فيه لو حاجه صعبه ❤️

By: Mohammed Ehab

---
# Logistic and Linear Regression

Linear regression models the relationship between inputs ($X$) and an output ($Y$) by fitting a line, a 2D plane, or a multi-dimensional hyperplane. The standard model equation is:

$$y = w_1x_1 + w_2x_2 + \dots + w_dx_d + b$$

Here, $w$ represents the weights and $b$ is the bias. In matrix notation, this simplifies to $Y = XW$.

The lecture outlines two primary mathematical approaches for finding the optimal weights ($W$) to minimize error:

#### Approach A: The Normal Equation (Analytical Solution)

You can directly calculate the optimal weights using linear algebra if you structure your inputs into an augmented matrix $X$. By setting the derivative of the loss function to zero, you get the closed-form solution:

$$\underline{W} = (X^T X)^{-1} X^T \underline{Y}$$

This method requires calculating the inverse of $X^T X$, which can be computationally heavy for massive datasets.

#### Approach B: Gradient Descent (Iterative Solution)

Instead of calculating the exact answer at once, gradient descent starts with random weights and takes iterative steps down the "error curve" to find the minimum cost.

1. **Initialize Parameters:** Start with random weights (e.g., slope $m=0$, intercept $b=0$).
    
2. **Calculate Cost:** Use a cost function like Mean Squared Error (MSE): $J = \frac{1}{n}\sum(y_i - \hat{y}_i)^2$.
    
3. **Compute Gradients:** Calculate partial derivatives to find the slope of the error curve ($\frac{\partial J}{\partial m}$ and $\frac{\partial J}{\partial b}$).
    
4. **Update Parameters:** Adjust the weights in the opposite direction of the gradient, scaled by a learning rate ($\alpha$ or $\eta$):
    
    $$m = m - \alpha \frac{\partial J}{\partial m}$$
    
5. **Repeat:** Iterate until the algorithm converges (the error stops significantly decreasing).
    

The slides distinguish between three gradient descent variations:

- **Batch Gradient Descent (BGD):** Uses the entire dataset to calculate the gradient for one update.
    
- **Stochastic Gradient Descent (SGD):** Randomly selects a single sample to calculate the gradient, resulting in faster but noisier updates.
    
- **Mini-Batch Gradient Descent (MBGD):** A middle ground that uses a small subset of $n$ samples per update.

example on it [[3rd Year/Supervised/Expanded Explanations/Lecture 1 Examples#Part 2 Linear Regression\|here]]