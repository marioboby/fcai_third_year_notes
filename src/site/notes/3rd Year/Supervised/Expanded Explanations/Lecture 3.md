---
{"dg-publish":true,"permalink":"/3rd-year/supervised/expanded-explanations/lecture-3/","dg-note-properties":{}}
---


## Why Use Bagging

### The Problem: The "Know-It-All" Single Model

Imagine you are training a complex model, like a deep Decision Tree, on your entire dataset. Decision trees are incredibly eager learners. If you feed them all your data, they will memorize _everything_—including the noise, the outliers, and the random quirks specific to that exact dataset.

Because it memorized the training data so perfectly, it performs poorly on new, unseen data. Furthermore, if you were to change even just 5% of your training data, the resulting tree would look completely different. This extreme sensitivity to the training data is called **high variance**.

### The Solution: Bagging's "Wisdom of the Crowd"

Bagging solves this by relying on a committee of models rather than one dictator.

Here is exactly how it improves upon the single-model approach:

**1. Creating Diversity through Bootstrapping**

Instead of giving the whole dataset to one model, Bagging creates multiple new datasets (bootstrap samples) of the same size as the original. It does this by randomly picking data points from the original set **with replacement**.

- _With replacement_ means a single data point might get picked three times for one sample, while another data point gets left out entirely.
    
- Because each model is trained on a slightly different variation of the data, you get a diverse set of models. Some will overfit to certain outliers, while others will never even see those outliers.
    

**2. Canceling Out the Noise through Aggregation**

When it is time to make a prediction, Bagging asks all of its models to vote (or averages their continuous predictions).

- Because the models are diverse, their individual errors and the specific noise they memorized are uncoordinated.
    
- When you average them out, the random noise cancels itself out, but the underlying true pattern—which all the models managed to learn—remains strong.
    

### Summary of Improvements

By shifting from one model trained on all data to many models trained on bootstrapped data, you gain:

- **Massive Variance Reduction:** The final ensemble prediction is much more stable. A slight change in the original training data won't drastically alter the final output.
    
- **Built-in Validation (OOB):** Because each bootstrap sample leaves out about 36.8% of the original data (due to sampling with replacement), you can test each model on the exact data it was blinded to. This is called the Out-Of-Bag (OOB) score, allowing you to evaluate the model without needing a separate validation set.
    
- **Less Overfitting:** The ensemble naturally smoothens out the harsh, overfitted decision boundaries that a single model would create.

---

## Comparison of Ensemble Learning Techniques

While all three are **Ensemble Learning** techniques designed to combine multiple "base models" to create one highly accurate "super model," they take fundamentally different approaches to how they train and aggregate those models.

### High-Level Comparison Table

|**Feature**|**Bagging**|**Random Forest**|**Boosting**|**Stacking**|
|---|---|---|---|---|
|**Primary Goal**|Reduce variance (fix overfitting)|Reduce variance (even better than Bagging)|Reduce bias (improve predictive accuracy)|Combine strengths of entirely different algorithms|
|**Training Style**|Parallel (Independent)|Parallel (Independent)|Sequential (Iterative)|Layered (Parallel base models, then a sequential meta-model)|
|**Base Learners**|Homogeneous (Usually deep Decision Trees)|Homogeneous (Deep Decision Trees)|Homogeneous (Usually shallow, "weak" Decision Trees)|**Heterogeneous** (Mix of SVM, KNN, Trees, Neural Networks, etc.)|
|**Data Sampling**|Bootstrap (Random with replacement)|Bootstrap (Random with replacement)|Weighted (Focuses on previously misclassified data)|Original dataset (usually relies on cross-validation to generate training data for the meta-model)|
|**Final Output**|Simple Majority Vote / Average|Simple Majority Vote / Average|Weighted Vote (Better models get more say)|**Meta-Model Prediction** (A final algorithm decides the output)|

---

### 1. Bagging (Bootstrap Aggregating)

Bagging is all about creating a "wisdom of the crowd" effect to stabilize eager algorithms that tend to overfit.

- **How it works:** It creates multiple subsets of the original dataset by sampling _with replacement_ (Bootstrapping). It then trains a separate, independent model on each subset at the exact same time (in parallel).
    
- **The Voting:** When making a prediction, every model gets an equal 1-vote say. For classification, it takes the majority; for regression, it takes the average.
    
- **Biggest Strength:** It drastically reduces variance. If your base model is highly sensitive to noise in the training data, bagging smooths that noise out.
    
- **The Catch:** If the underlying dataset has one or two incredibly dominant features, every single bagged tree will use those same features at the top of their splits. This results in a bunch of trees that look highly correlated (too similar to each other).
    

### 2. Random Forest

Random Forest is essentially "Bagging Version 2.0." The lecture defines it perfectly: **Bagging with trees + random feature subsets.**

- **How it works:** It does the exact same parallel bootstrap sampling as Bagging, but it introduces a second layer of randomness. When building the individual decision trees, it is not allowed to look at all the features to find the best split. It is forced to choose from a small, random subset of features at each node.
    
- **The Voting:** Exactly like Bagging—an equal majority vote or average.
    
- **Biggest Strength:** By forcing the trees to use different features, it "decorrelates" the trees. Even if there is a dominant feature in the dataset, some trees won't be allowed to see it, forcing them to find hidden patterns in the weaker features. This makes the ensemble much more robust and diverse than standard Bagging.
    

### 3. Boosting (e.g., AdaBoost, Gradient Boosting)

Boosting takes a completely different philosophical approach. Instead of building independent models in parallel, it builds them **sequentially**, with each new model trying to fix the mistakes of the previous one.

- **How it works:** It starts by training a "weak learner" (a model that is only slightly better than random guessing, like a tree with only one split). It checks where that model made errors. It then heavily increases the "weight" (importance) of those misclassified data points. The _next_ weak learner is forced to focus specifically on getting those hard examples right. This chain continues, with each model correcting the predecessor.
    
- **The Voting:** Unlike Bagging and Random Forest, Boosting does not give every model an equal vote. Models that performed well historically get a higher "Amount of Say" (weighted voting) in the final prediction.
    
- **Biggest Strength:** It reduces bias. It can take a highly inaccurate base model and turn it into a state-of-the-art predictor.
    
- **The Catch:** Because it hyper-focuses on errors, Boosting is highly sensitive to outliers and noisy data. It can easily overfit if you do not control the learning rate or the number of models. Furthermore, because it is sequential, it takes much longer to train than parallel methods.


### 4. Stacking (Stacked Generalization)

Stacking completely discards the idea of simple voting or averaging. Instead, it assumes that different types of algorithms (e.g., an SVM vs. a Decision Tree) look at the data in fundamentally different ways. It uses a machine learning model to learn exactly _how_ to combine the outputs of other machine learning models.

- **How it works:**
    
    1. **Level 1 (Base Models):** You train several completely different algorithms (like a KNN, a Random Forest, and a Logistic Regression) on your original dataset.
        
    2. **Generate Meta-Features:** You have these Level 1 models make predictions on the dataset.
        
    3. **Level 2 (Meta-Model/Combiner):** You take those _predictions_ (not the original features) and use them as the input data to train a brand new, final model (often a simple Logistic Regression).
        
- **The Combination:** There is no hard-coded voting. The Level 2 meta-model actively learns which base models to trust. For example, it might learn, "When the KNN and the SVM disagree, the SVM is usually right, so I will weight the SVM's prediction higher."
    
- **Biggest Strength:** It is incredibly powerful for squeezing out the absolute maximum performance from a dataset, which is why Stacking is almost always used in winning Kaggle competition solutions. It capitalizes on the diverse mathematical approaches of completely different algorithms.
    
- **The Catch:** It is highly complex, computationally expensive, and notoriously prone to overfitting. If you train your Level 1 models and your Level 2 model on the exact same training data without careful cross-validation, the meta-model will just memorize the training set and fail completely on new data.