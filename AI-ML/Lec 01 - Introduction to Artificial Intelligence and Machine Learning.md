---
title: "IT2011 - Introduction to Artificial Intelligence and Machine Learning"
date: 2026-06-25
module: "IT2011"
author: Dulith Divisekara
institution: SLIIT
tags: [information-technology, artificial-intelligence, machine-learning, semester-notes]
---
## Study Tracker
- [ ] Differentiate between Traditional Programming and AI/ML paradigms.
- [ ] Categorize the core branches of Machine Learning (Supervised, Unsupervised, Reinforcement).
- [ ] Understand Deep Learning architectures (CNNs, RNNs, Transformers).
- [ ] Define Generative AI and fundamental prompt engineering principles.

## 1. Evolution and Foundations of AI

The foundation of computational thinking began with systems like ENIAC in 1946, the first general-purpose electronic digital computer. The official birth of the Artificial Intelligence field occurred at the 1956 Dartmouth Workshop, organized by John McCarthy, Marvin Minsky, Nathaniel Rochester, and Claude Shannon.

## 2. Programming Paradigms: Traditional vs. AI/ML

> [!IMPORTANT]
> **Traditional Programming:** A programmer explicitly writes predefined rules and logic to process input data. The system operates strictly based on this hand-coded logic.
> **AI/ML Systems:** Algorithms infer patterns and learn rules automatically when provided with input data and correct outputs.

### Traditional Programming Characteristics

| Feature | Traditional Programming |
| :--- | :--- |
| **Logic Source** | Written by human developers |
| **Flexibility** | Low - must update code to change behavior |
| **Predictability** | High - same input always gives same output |
| **Learning Capability** | None - does not improve with data |

### AI/ML-Based Systems Characteristics

| Feature | AI/ML-Based Programming |
| :--- | :--- |
| **Logic Source** | Learned from data using algorithms |
| **Flexibility** | High - adapts to new patterns in data |
| **Predictability** | Moderate - may vary depending on training |
| **Learning Capability** | Yes - improves with more data and feedback |

### Paradigm Shift Comparison

| Aspect | Traditional Programming | AI/ML Approach |
| :--- | :--- | :--- |
| **Input/Output Relation** | Input + Rules → Output | Input + Output → Learn Rules |
| **Development Process** | Rule design, implementation | Data preparation, model training |
| **Adaptability** | Hard to change | Easily retrained on new data |
| **Debugging** | Through step-by-step logic | Through performance metrics (accuracy, loss) |

## 3. The Taxonomy of Artificial Intelligence

> [!NOTE]
> AI is the overarching domain focused on simulating human intelligence, encompassing reasoning, learning, and perception. Machine Learning, Deep Learning, and Generative AI are hierarchically nested subfields.

### Core Machine Learning Branches
1. **Supervised Learning:** The algorithm learns from a labeled dataset to predict outputs for unseen data (e.g., Classification, Regression).
2. **Unsupervised Learning:** The algorithm works on unlabeled data to discover hidden structures and groupings (e.g., Clustering, Dimensionality Reduction).
3. **Reinforcement Learning:** An agent learns by interacting with an environment, optimizing actions to maximize cumulative rewards (e.g., Game AI, Robotics).

```mermaid
graph TD
    classDef ai fill:#e1f5fe,stroke:#0288d1,stroke-width:2px,color:#01579b;
    classDef ml fill:#e8f5e9,stroke:#388e3c,stroke-width:2px,color:#1b5e20;
    classDef dl fill:#fff3e0,stroke:#f57c00,stroke-width:2px,color:#e65100;
    classDef gen fill:#f3e5f5,stroke:#8e24aa,stroke-width:2px,color:#4a148c;

    AI[Artificial Intelligence]:::ai --> ML[Machine Learning]:::ml
    ML --> SL[Supervised Learning]:::ml
    ML --> UL[Unsupervised Learning]:::ml
    ML --> RL[Reinforcement Learning]:::ml
    ML --> DL[Deep Learning]:::dl
    DL --> CNN[CNNs - Spatial Data]:::dl
    DL --> RNN[RNNs - Sequential Data]:::dl
    DL --> TR[Transformers - Parallel Processing]:::dl
    TR --> GenAI[Generative AI]:::gen
````

## 4. Deep Learning & Generative AI

Deep Learning utilizes multi-layered artificial neural networks to extract abstract representations from large datasets.

- **Convolutional Neural Networks (CNNs):** Apply filters to extract spatial features; optimal for computer vision and image processing.
    
- **Recurrent Neural Networks (RNNs):** Maintain memory of previous inputs via architectural loops; optimal for sequential data like text and time series.
    
- **Transformers:** Utilize self-attention mechanisms to process tokens in parallel; foundational for modern Large Language Models (LLMs).
    

> [!TIP]
> 
> **Generative AI** is a specialized subfield utilizing Transformers, GANs, and Diffusion Models to create original content (text, images, code) rather than merely classifying existing data.

## Technical Implementation

To highlight the difference between paradigms, below is a standard Python implementation representing a traditional rule-based system. In this approach, human-written logic dictates the output, unlike a machine learning model which would require training data to infer these rules.


```Python
# Traditional Programming Approach
# Logic and rules are explicitly defined by the developer.

def classify_number(number: int) -> str:
    """
    Classifies an integer as Even or Odd based on hard-coded rules.
    """
    # Explicit rule: Modulo division by 2
    if number % 2 == 0:
        return "Even"
    else:
        return "Odd"

if __name__ == "__main__":
    # Test dataset
    test_data = [10, 15, 22, 33]
    
    print("Executing traditional rule-based classification:")
    for num in test_data:
        classification = classify_number(num)
        print(f"Input: {num} -> Output: {classification}")
```

## Active Recall Self-Check

> [!TIP]
> 
> RNNs process sequential data by maintaining a memory of previous inputs via loops, which limits speed. Transformers utilize a self-attention mechanism, allowing the model to focus on different parts of the input simultaneously and process tokens in parallel.

> [!TIP]
> 
> GANs consist of two competing neural networks: a Generator that creates fake data, and a Discriminator that attempts to detect real versus fake data. This competition improves the output quality over time.

> [!TIP]
> 
> Unsupervised learning algorithms operate on unlabeled data without explicit instructions, aiming to discover hidden groupings, associations, or structural patterns (such as clustering or dimensionality reduction).