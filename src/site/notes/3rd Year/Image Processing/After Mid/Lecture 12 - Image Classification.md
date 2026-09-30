---
{"dg-publish":true,"permalink":"/3rd-year/image-processing/after-mid/lecture-12-image-classification/","dg-note-properties":{}}
---

The final lecture shifts from manipulating image pixels to teaching a computer how to understand what those pixels represent. **Image Classification** and **Pattern Recognition** are all about assigning unknown patterns (like a face, a fingerprint, or a handwritten letter) into known categories or classes.

---
### The Classifier Design Cycle

Building an image classification system follows a strict five-step cycle:

**1. Data Collection** Before a machine can learn, it needs examples (training data) and tests (testing data). The lecture outlines five methods for splitting up your dataset:

- **Resubstitution:** Uses all available data for both training and testing. It produces an "optimal" classifier but is highly prone to bias (testing on the training data).
    
- **Hold Out Method:** Splits the data, typically using two-thirds for training and one-third for testing.
    
- **K-Fold Cross-Validation:** Divides the data into $K$ subsets. It runs $K$ experiments, rotating which subset acts as the test set while the remaining $K-1$ subsets are used for training. The final error is the average across all trials.
    
- **Leave-One-Out Validation:** An extreme version of K-Fold where $K$ equals the total number of data points ($N$). It is computationally expensive but highly accurate.
    
- **Bootstrap:** Randomly selects training samples _with replacement_ (meaning the same sample can be picked multiple times). Unselected samples become the test set. It is great for very small datasets.
    

**2. Feature Selection** Instead of feeding raw pixels into a classifier, we extract specific measurements or "features" (like the length and width of an Iris flower's petals). We must avoid the "curse of dimensionality"—measuring too many useless features makes the system overly complex. Good features should be:

- **Robust:** Unaffected by translation, rotation, scale, or noise.
    
- **Discriminating:** Well-separated ranges for different classes.
    
- **Reliable:** Similar values for objects within the same class.
    
- **Independent:** Not correlated to each other (e.g., don't use both length and area if they tell you the same thing).
    

**3. Model (Classifier) Selection** This involves defining mathematical functions that assign a real-valued "score" to a set of features. The classifier evaluates the input against the functions for every possible class and assigns the object to the class with the maximum score.

**4. Training and Learning** This is how the system learns the rules to separate the classes.

- **Supervised Learning:** A human teacher provides the correct category labels for the training set.
    
- **Unsupervised Learning:** The system is left alone to find natural groupings or clusters in the data.
    
- **Reinforcement Learning:** The system guesses, and a teacher simply provides feedback on whether the decision was right or wrong.
    

**5. Evaluation (Performance Measures)** Once built, we must test how well the classifier works. This is done using a **Confusion Matrix**, an $N \times N$ grid that compares the actual target values against the model's predictions.

From the confusion matrix, we calculate several vital metrics:

- **Accuracy:** The percentage of all correctly classified observations: $(TP + TN) / Total$.
    
- **Precision:** Out of all the items the model _claimed_ were positive, how many actually were? $TP / (TP + FP)$.
    
- **Recall:** Out of all the _actual_ positive items, how many did the model successfully find? $TP / (TP + FN)$.
    
- **F1-Score:** The geometric average of Precision and Recall, useful when you need a single performance number: $2 \times \frac{Precision \times Recall}{Precision + Recall}$.
    
- **Misclassification Rate:** The fraction of incorrect predictions: $(FP + FN) / Total$.
    

### Interactive Multi-Class Evaluation

Calculating Precision, Recall, and F1-Scores is straightforward for a simple binary (Yes/No) classifier. However, as shown at the end of your lecture, calculating these metrics for a multi-class system (like classifying Apples, Oranges, and Mangoes) requires isolating each class one by one.
