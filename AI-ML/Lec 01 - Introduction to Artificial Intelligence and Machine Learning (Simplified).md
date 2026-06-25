---
title: "IT2011 - Introduction to Artificial Intelligence and Machine Learning"
date: 2026-06-25
module: "IT2011"
author: Dulith Divisekara
institution: SLIIT
tags: [information-technology, artificial-intelligence, machine-learning, semester-notes]
---
## Study Tracker
- [ ] Understand the evolution of AI from ENIAC to modern Generative AI.
- [ ] Differentiate between traditional programming and ML.
- [ ] Identify AI subfields and ML paradigms (Supervised, Unsupervised, Reinforcement).
- [ ] Categorize Deep Learning architectures (CNN, RNN, Transformers).

---

## 1. AI Evolution & Hierarchy

AI has evolved from logic-based symbolic systems in the 1950s to complex neural networks today.

### AI Hierarchy


```mermaid
graph TD
    A[Artificial Intelligence]:::gray
    A --> B[Machine Learning]:::blue
    A --> C[Expert Systems]:::gray
    A --> D[Robotics]:::gray
    
    B --> E[Deep Learning]:::green
    E --> F[Generative AI]:::yellow

    classDef gray fill:#27272a,stroke:#a1a1aa,color:#f4f4f5;
    classDef blue fill:#0c4a6e,stroke:#38bdf8,color:#f0f9ff;
    classDef green fill:#14532d,stroke:#4ade80,color:#f0fdf4;
    classDef yellow fill:#78350f,stroke:#fbbf24,color:#fffbeb;
````

## 2. Programming Paradigms: Traditional vs. ML

|**Feature**|**Traditional Programming**|**ML Approach**|
|---|---|---|
|**Logic**|Input + Rules = Output|Input + Output = Rules|
|**Source**|Coded by Developers|Learned from Data|
|**Flexibility**|Static|Adaptive|

## 3. Types of Machine Learning

- **Supervised:** Trained on labeled data (Input/Output pairs). Used for classification/regression.
    
- **Unsupervised:** Finds patterns in unlabeled data. Used for clustering.
    
- **Reinforcement:** Agent learns via rewards/penalties in an environment.
    

## 4. Deep Learning Architectures

Deep Learning uses multi-layered neural networks to solve complex tasks.

- **CNN (Convolutional Neural Networks):** Used for Image/Vision.
    
- **RNN (Recurrent Neural Networks):** Used for Sequential data (Speech/Text).
    
- **Transformers:** Uses self-attention to process data in parallel (Foundation of LLMs).
    

## 5. Generative AI Concepts

- **GANs:** Two networks (Generator vs. Discriminator) compete to create realistic data.
    
- **Diffusion Models:** Reverse noise to construct high-quality images.
    
- **Prompt Engineering:** The skill of providing specific inputs to guide model output.
    

## 6. Technical Logic: Paradigm Shift

```Java
// Traditional: Developer defines the rule
if (input > 0.5) return "Class A";

// ML Concept: The 'threshold' is learned from training data weights
double score = model.predict(input);
if (score > learnedThreshold) return "Class A";
```

## Active Recall Self-Check

> [!TIP]
> 
> Supervised learning uses labeled data (the answer is provided), while Unsupervised learning uses unlabeled data (the model must find patterns itself).

> [!TIP]
> 
> The Generator creates fake data, and the Discriminator evaluates if it is real or fake. They improve each other through competition.

> [!TIP]
> 
> Transformers use self-attention to process entire sequences of data in parallel, whereas RNNs process data sequentially, which is slower and often struggles with long-range context.
