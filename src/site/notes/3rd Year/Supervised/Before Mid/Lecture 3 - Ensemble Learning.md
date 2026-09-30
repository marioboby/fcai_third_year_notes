---
{"dg-publish":true,"permalink":"/3rd-year/supervised/before-mid/lecture-3-ensemble-learning/","dg-note-properties":{}}
---


### 1. Introduction to Ensemble Learning

The core idea behind ensemble learning is simple: one classifier is rarely perfect. However, if you combine multiple classifiers (called "base models"), their complementary strengths can compensate for individual weaknesses. Think of it like a weather forecast: if you look at five different weather models and four of them predict rain, you can be fairly confident it will rain.

The two main steps in ensemble learning are:

1. **Generate multiple machine learning models** (using the same or different algorithms).
    
2. **Combine their predictions** to produce a final output.
    

### 2. Majority Voting (and Averaging)

This is the simplest way to combine models.

- **For Classification (Voting):** If you have five models predicting an image, and three say "Cat" while two say "Dog", the ensemble outputs "Cat".
    
- **For Regression (Averaging):** If five models predict a house price, the ensemble takes the mathematical average of all five predictions.
    

**"Soft" Voting:** Instead of just taking the final hard labels (e.g., Cat or Dog), soft voting averages the _probabilities_ output by each classifier.

- _Example:_ If Model 1 predicts Class 0 with 90% confidence, and Model 2 predicts Class 0 with 80% confidence, the ensemble averages these probabilities to make a more nuanced final decision. The slides define this mathematically as finding the class $j$ that maximizes $\sum w_i p_{i,j}$.
  
```python
import numpy as np
import warnings
from sklearn import datasets
from sklearn import model_selection
from sklearn.ensemble import RandomForestClassifier
from sklearn.ensemble import VotingClassifier
from sklearn.linear_model import LogisticRegression
from sklearn.naive_bayes import GaussianNB 
from mlxtend.plotting import plot_decision_regions
import matplotlib.pyplot as plt

# Load the iris dataset from sklearn datasets
iris = datasets.load_iris()
X, y = iris.data[:, 1:3], iris.target

# Create three different classifiers
clf1 = LogisticRegression(random_state=1)
clf2 = RandomForestClassifier(random_state=1)
clf3 = GaussianNB()

# Print a header for the cross-validation results
print('5-fold cross validation:\n')

# Create a list of labels for the classifiers
labels = ['Logistic Regression', 'Random Forest', 'Naive Bayes']

# Loop through the classifiers and perform 5-fold cross-validation for each
for clf, label in zip([clf1, clf2, clf3], labels):

    # Use cross_val_score to compute accuracy scores using 5-fold cross-validation
    scores = model_selection.cross_val_score(clf, X, y, 
                                              cv=5, 
                                              scoring='accuracy')
    
    # Print the mean accuracy and standard deviation of accuracy for the current classifier
    print("Accuracy: %0.2f (+/- %0.2f) [%s]"
          % (scores.mean(), scores.std(), label))


```


### 3. Bagging (Bootstrap Aggregating)

Bagging is designed to reduce the variance "overfitting" of a single model (like a Decision Tree) by training multiple versions of it on slightly different data.

more on advantage of Bagging [[3rd Year/Supervised/Expanded Explanations/Lecture 3#Why Use Bagging\|here]]

**How it works:**

1. **Bootstrap Sampling:** The algorithm creates multiple new training sets by randomly sampling the original dataset _with replacement_ (Usually the same size of original dataset). This means some data points might appear multiple times in a bootstrap sample, while others might not appear at all.
    
2. **Training:** A separate base classifier is trained on each of these bootstrap samples in parallel.
    
3. **Aggregation:** When a new data point arrives, all the trained classifiers make a prediction, and the final output is determined by majority voting (or averaging).
    

### 4. Boosting

Unlike bagging, which trains models independently in parallel, boosting trains models **sequentially**. It focuses on turning "weak learners" (models only slightly better than random guessing) into a single "strong learner".

**How it works (e.g., AdaBoost):**

1. **Initialize:** Start by giving every data point in the training set an equal weight.
    
2. **Train & Evaluate:** Train the first weak model.
    
3. **Update Weights:** Identify which data points the model classified incorrectly. _Increase_ the weights of these difficult examples, and _decrease_ the weights of the ones it got right.
    
4. **Repeat:** Train the next model, which will now focus heavily on those difficult, highly-weighted examples.
    
5. **Combine:** The final prediction is a weighted sum of all the sequential models.
    

### 5. Stacking (Stacked Generalization)

Stacking takes ensemble learning to another level by using a machine learning model to learn _how_ to combine the other models.

**How it works:**

1. **Level 1 (Base Models):** Train several different types of models (e.g., a Decision Tree, a KNN, and an SVM) on the original dataset.
    
2. **Generate Meta-Data:** Have these base models make predictions on the dataset.
    
3. **Level 2 (Meta-Classifier):** Use these _predictions_ as the input features to train a brand new model (the Meta-Classifier or Combiner, often a Logistic Regression model). This meta-classifier learns which base models to trust in different situations.
    
```python
from sklearn.datasets import load_iris
from sklearn.model_selection import train_test_split
from sklearn.ensemble import StackingClassifier
from sklearn.linear_model import LogisticRegression
from sklearn.tree import DecisionTreeClassifier
from sklearn.neighbors import KNeighborsClassifier
from sklearn.metrics import accuracy_score

data = load_iris()
X = data.data
y = data.target

X_train, X_test, y_train, y_test = train_test_split( X, y, test_size=0.2, random_state=42)

base_models = [ ('dt', DecisionTreeClassifier()), ('knn', KNeighborsClassifier())]
meta_model = LogisticRegression()

stack_model = StackingClassifier(estimators=base_models,final_estimator=meta_model)
stack_model.fit(X_train, y_train)

y_pred = stack_model.predict(X_test)
accuracy = accuracy_score(y_test, y_pred)
print("Accuracy:", accuracy)
```

### 6. Random Forest

A Random Forest is essentially a specialized, highly effective version of Bagging built specifically for Decision Trees.

**The Formula:** `Bagging with trees + random feature subsets`.

**Why is it called "Random"?**

It injects randomness in two ways to ensure the trees don't all look identical (which would defeat the purpose of an ensemble):

1. **Random Data:** Like standard bagging, each tree is trained on a random bootstrap sample of the data.
    
2. **Random Features:** When building each tree, the algorithm doesn't look at _all_ the features to decide how to split a node. Instead, it only considers a random subset of the features (e.g., only looking at $x_2$ and $x_3$ for one split, and $x_0$ and $x_1$ for another).
    

This forces the forest to explore different combinations and creates a highly robust, diverse set of trees that vote on the final classification.