---
title: "IT2011 - Machine Learning Pipeline"
date: 2026-06-25
module: "IT2011"
author: Dulith Divisekara
institution: SLIIT
tags: [information-technology, machine-learning, ml-pipeline, semester-notes]
---
## Study Tracker
- [ ] Understand the 8 chronological stages of the Machine Learning Pipeline.
- [ ] Apply data preprocessing and feature engineering techniques (Encodings, Scaling, Selection).
- [ ] Differentiate between Filter, Wrapper, and Embedded feature selection methods.
- [ ] Evaluate models using appropriate metrics and mitigate overfitting/underfitting.

## 1. The Machine Learning Pipeline Architecture

A Machine Learning Pipeline is a systematic sequence of steps that transforms raw, unstructured data into valuable, deployable predictions . Automating this flow ensures consistency from data ingestion to model deployment .


> [!IMPORTANT]
> The standard pipeline consists of 8 distinct stages: Problem Definition, Data Collection, Data Preprocessing, Feature Engineering, Model Selection, Model Training, Evaluation, and Deployment .

```mermaid
graph LR
    classDef stage fill:#e1f5fe,stroke:#0288d1,stroke-width:2px,color:#01579b;
    classDef data fill:#e8f5e9,stroke:#388e3c,stroke-width:2px,color:#1b5e20;
    classDef deploy fill:#fff3e0,stroke:#f57c00,stroke-width:2px,color:#e65100;

    P[Problem Def]:::stage --> DC[Data Collection]:::data
    DC --> DP[Preprocessing]:::data
    DP --> FE[Feature Eng]:::data
    FE --> MS[Model Selection]:::stage
    MS --> MT[Training & Tuning]:::stage
    MT --> EV[Evaluation]:::stage
    EV --> D[Deployment]:::deploy
````

## 2. Data Preprocessing & Feature Engineering

Raw data is often noisy, inconsistent, and incomplete. Preprocessing transforms this data into a format digestible by mathematical algorithms .

### 2.1 Categorical Encoding Methods

Machines require numerical input. Categorical encoding translates text labels into numeric values .

|**Encoding Type**|**Mechanism**|**Best Application**|**Drawbacks**|
|---|---|---|---|
|**Label Encoding**|Assigns a unique integer to each category (e.g., Yes=1, No=0).|Ordinal data (where order matters, e.g., Low, Medium, High).|Implies mathematical order even when none exists (nominal data) .|
|**One-Hot Encoding**|Creates a new binary column (0/1) for each distinct category.|Nominal data (unordered, e.g., Countries, Colors).|Can result in massive dimensionality (high-cardinality) .|
|**Target Encoding**|Replaces a category with the mean target value of that category.|High-cardinality nominal features (e.g., Product IDs).|High risk of overfitting; requires regularization .|

### 2.2 Feature Selection Techniques

Feature selection removes redundant or irrelevant data, improving model performance and interpretability .

|**Type**|**Mechanism**|**Speed**|**Accuracy**|**Overfitting Risk**|
|---|---|---|---|---|
|**Filter Methods**|Uses statistical tests (Variance Threshold, Chi-Square). Independent of ML models.|Fast|Moderate (may ignore feature interactions)|Low .|
|**Wrapper Methods**|Evaluates feature subsets using a predictive model (Forward Selection, RFE).|Slow|High|High .|
|**Embedded Methods**|Selection is integrated into the model training itself (Lasso L1, Decision Trees).|Medium|High|Medium .|

> [!NOTE]
> 
> **Feature Selection vs. Dimensionality Reduction:** Feature selection keeps a subset of the _original_ features. Dimensionality reduction (like PCA) creates entirely _new_ features by compressing and combining the originals, which reduces interpretability .

## 3. Model Training, Tuning, and Evaluation

Training involves splitting data into a Training Set (e.g., 80%) for learning patterns, and a Test Set (e.g., 20%) for evaluating generalization .

### 3.1 Hyperparameter Tuning

Hyperparameters are architectural settings defined _before_ training begins (e.g., maximum depth of a decision tree), unlike parameters (weights) which are learned from data .

- **Grid Search:** Exhaustively tries all possible combinations in a specified grid. Thorough but computationally expensive .
    
- **Random Search:** Samples random combinations. Highly efficient for high-dimensional parameter spaces .
    

### 3.2 K-Fold Cross-Validation

To ensure the model's performance isn't just a lucky split, K-Fold Cross-Validation splits the data into _K_ equal parts. The model trains on _K-1_ folds and tests on the remaining fold, repeating this process _K_ times. The final score is the average across all iterations .

### 3.3 Diagnosing Model Fit

|**State**|**Diagnosis (Train vs. Test Acc)**|**Mitigation Strategies**|
|---|---|---|
|**Underfitting**|Low Train (~60%) / Low Test (~60%)|Use a more complex model, engineer better features, train longer, reduce regularization .|
|**Optimal Fit**|High Train / High Test|Proceed to Deployment.|
|**Overfitting**|Very High Train (~100%) / Low Test (~60%)|Use a simpler model, apply regularization, increase training data, implement early stopping, use cross-validation .|

## Technical Implementation

Below is a self-contained Python script demonstrating standard data scaling and implementing K-Fold Cross-Validation logic using `scikit-learn`.

```Python
import numpy as np
from sklearn.model_selection import KFold
from sklearn.preprocessing import StandardScaler
from sklearn.linear_model import LogisticRegression
from sklearn.metrics import accuracy_score

def execute_ml_pipeline_mockup():
    """
    Demonstrates data scaling and K-Fold Cross Validation.
    """
    # 1. Generate synthetic dataset (100 samples, 2 features)
    # Feature 1 ranges 0-10, Feature 2 ranges 1000-5000 (Requires scaling)
    np.random.seed(42)
    X = np.column_stack((np.random.uniform(0, 10, 100), np.random.uniform(1000, 5000, 100)))
    y = np.random.randint(0, 2, 100) # Binary target

    # 2. Data Preprocessing: Standardization (Z-score scaling)
    scaler = StandardScaler()
    X_scaled = scaler.fit_transform(X)

    # 3. Initialize Model and K-Fold Configuration
    model = LogisticRegression()
    kf = KFold(n_splits=5, shuffle=True, random_state=42)
    
    fold_accuracies = []

    # 4. Execute 5-Fold Cross Validation
    print("Executing 5-Fold Cross Validation:\n")
    for fold, (train_index, test_index) in enumerate(kf.split(X_scaled), 1):
        # Split data
        X_train, X_test = X_scaled[train_index], X_scaled[test_index]
        y_train, y_test = y[train_index], y[test_index]
        
        # Train model
        model.fit(X_train, y_train)
        
        # Evaluate model
        predictions = model.predict(X_test)
        acc = accuracy_score(y_test, predictions)
        fold_accuracies.append(acc)
        
        print(f"Fold {fold} Accuracy: {acc:.4f}")

    # 5. Final Evaluation Metric
    print(f"\nAverage Cross-Validation Accuracy: {np.mean(fold_accuracies):.4f}")

if __name__ == "__main__":
    execute_ml_pipeline_mockup()
```

## Active Recall Self-Check

> [!TIP]
> 
> Use Label Encoding for ordinal data, where the categories have a meaningful mathematical order or hierarchy (e.g., Small, Medium, Large). One-Hot Encoding is better for nominal, unordered data (e.g., Red, Green, Blue) to prevent the algorithm from assuming false numerical relationships .

> [!TIP]
> 
> The model is **Overfitting**. It has memorized the training noise rather than learning general patterns. To fix it, you can use a simpler model algorithm, apply regularization, gather more training data, or implement early stopping .

> [!TIP]
> 
> Filter methods evaluate feature importance using statistical tests (like variance) independent of any ML algorithm, making them very fast. Wrapper methods evaluate features by actually training a specific predictive model on various combinations of features, making them highly accurate but computationally expensive .