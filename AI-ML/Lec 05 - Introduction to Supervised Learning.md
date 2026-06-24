---
title: "IT2011 - Introduction to Supervised Learning"
date: 2026-06-25
module: "IT2011"
author: Dulith Divisekara
institution: SLIIT
tags: [information-technology, supervised-learning, algorithms, semester-notes]
---
## Study Tracker
- [ ] Differentiate between Linear Regression and Logistic Regression.
- [ ] Understand the distance metrics and prediction logic of K-Nearest Neighbors (KNN).
- [ ] Calculate node purity using the Gini Index for Decision Trees.
- [ ] Explain Support Vector Machines (SVM), including Hyperplanes, Soft Margins, and the Kernel Trick.

## 1. Fundamentals of Supervised Learning

Supervised learning trains a machine learning model on a labeled dataset, pairing input data with the correct output . The objective is to learn the mathematical mapping between inputs and outputs to accurately predict unseen data .

## 2. Regression Models

> [!IMPORTANT]
> While both contain "Regression" in their name, Linear Regression outputs continuous numerical values, whereas Logistic Regression is fundamentally a classification algorithm outputting probabilities.

### Linear Regression
Linear Regression assumes a linear relationship between input variables ($x$) and a single continuous output variable ($y$) . The algorithm seeks the "best fit line" by minimizing the total prediction error (Ordinary Least Squares) .
* **Equation:** $y = \beta_0 + \beta_1x + \epsilon$ .
* **Evaluation:** Evaluated using the Coefficient of Determination ($R^2$), which calculates the fraction of variation explained by the model .

### Logistic Regression
Used for dichotomous/binary variables (e.g., Yes/No, Pass/Fail) . It bounds the linear equation's output between 0 and 1 using the **Sigmoid function** to return a probability .
* **Sigmoid Function:** $S(x) = \frac{1}{1 + e^{-x}}$ .
* **Decision Boundary:** Probabilities above a defined cutoff (usually 0.5) are assigned to Class 1; below are assigned to Class 0 .

## 3. K-Nearest Neighbors (KNN)

KNN is an instance-based learning algorithm that predicts the label of a new data point based on its proximity to existing training data.
* **Distance Metric:** Utilizes Euclidean Distance: $d = \sqrt{(x_2 - x_1)^2 + (y_2 - y_1)^2}$ .
* **Prediction Logic:** Identifies the $k$ closest points. For regression, it averages their values; for classification, it selects the most frequent category (majority vote) .

## 4. Tree-Based Models & SVMs

### Classification and Regression Trees (CART)
Decision trees utilize recursive partitioning to split data into sub-groups (nodes) based on feature thresholds .
* **Structure:** Root Node (entire dataset) $\rightarrow$ Internal Nodes (split points) $\rightarrow$ Leaf Nodes (final decision) .
* **Splitting Metric (Gini Index):** Measures node impurity. A Gini score of 0 represents a perfectly pure node (all samples belong to one class). The algorithm favors splits that minimize Gini impurity .
  $$Gini = 1 - \sum (p(i))^2$$ .

| Algorithm | Splitting Metric |
| :-------- | :--------------- |
| **CART**  | Gini Index       |
| **C4.5**  | Gain Ratio       |
| **ID3**   | Information Gain |

### Support Vector Machines (SVM)
SVM aims to find the optimal decision boundary (Hyperplane) that separates different classes while maximizing the margin (the perpendicular distance to the closest data points, known as Support Vectors) .

> [!NOTE]
> **Handling Non-Linearity:** Real-world data is rarely perfectly separable. SVM introduces the **Soft Margin** to tolerate a few misclassifications for a better overall fit, and the **Kernel Trick** to project data into higher dimensions to find non-linear boundaries (e.g., Polynomial, RBF) .

```mermaid
graph TD
    classDef algo fill:#e1f5fe,stroke:#0288d1,stroke-width:2px,color:#01579b;
    classDef metric fill:#e8f5e9,stroke:#388e3c,stroke-width:2px,color:#1b5e20;

    SL[Supervised Learning]:::algo --> REG[Regression]:::algo
    SL --> CLS[Classification]:::algo
    
    REG --> LR[Linear Regression]:::metric
    REG --> KNN_R[KNN Regressor]:::metric
    
    CLS --> LOG[Logistic Regression]:::metric
    CLS --> KNN_C[KNN Classifier]:::metric
    CLS --> DT[Decision Trees / CART]:::metric
    CLS --> SVM[Support Vector Machines]:::metric
````

## Technical Implementation

Below is a self-contained Python script implementing a Gini Impurity calculator, demonstrating the underlying math behind CART Decision Tree splits.


```Python
import numpy as np

def calculate_gini_impurity(class_counts: list) -> float:
    """
    Calculates the Gini Impurity for a given node.
    Formula: 1 - sum(p_i^2) where p_i is the probability of class i.
    """
    total_instances = sum(class_counts)
    if total_instances == 0:
        return 0.0
    
    gini = 1.0
    for count in class_counts:
        probability = count / total_instances
        gini -= (probability ** 2)
        
    return round(gini, 4)

if __name__ == "__main__":
    # Example 1: A perfectly pure node (All samples are 'Class A')
    pure_node = [10, 0] 
    print(f"Gini Impurity (Pure Node [10, 0]): {calculate_gini_impurity(pure_node)}")
    
    # Example 2: A completely impure/mixed node (50/50 split)
    impure_node = [5, 5]
    print(f"Gini Impurity (Impure Node [5, 5]): {calculate_gini_impurity(impure_node)}")
    
    # Example 3: Calculation from lecture (p(Yes)=0.67, p(No)=0.33)
    lecture_node = [4, 2] # 4 Yes, 2 No out of 6
    print(f"Gini Impurity (Lecture Example [4, 2]): {calculate_gini_impurity(lecture_node)}")
```

## Active Recall Self-Check

> [!TIP]
> 
> It processes the linear equation output through a Sigmoid function ($S(x) = \frac{1}{1 + e^{-x}}$). This transforms the output into a probability between 0 and 1. A threshold (e.g., 0.5) is then used to classify the result into Class 0 or Class 1 .

> [!TIP]
> 
> A hard margin strictly allows zero misclassifications, which only works if the data is perfectly linearly separable. A Soft Margin tolerates a few misclassified data points (dots on the wrong side of the boundary) to find a more generalized and robust fit for real-world, noisy data .

> [!TIP]
> 
> A Gini Index of 0 indicates a perfectly pure dataset or node. This means all the samples within that specific node belong to the exact same class, requiring no further splitting .