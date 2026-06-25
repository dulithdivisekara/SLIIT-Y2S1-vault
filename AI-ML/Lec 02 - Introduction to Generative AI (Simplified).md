---
title: "IT2011 - Lecture 02 - Exam Study Guide (Visual Edition)"
date: 2026-06-25
module: "IT2011"
author: Dulith Divisekara
institution: SLIIT
tags: [information-technology, exam-prep, generative-ai, machine-learning, diagrams]
---
## 1. The Artificial Intelligence Hierarchy & Neural Networks

Understanding the exact boundaries between AI subsets is a common exam topic.

```mermaid
graph TD
    classDef ai fill:#212529,stroke:#ced4da,stroke-width:2px,color:#f8f9fa;
    classDef ml fill:#343a40,stroke:#dee2e6,stroke-width:2px,color:#f8f9fa;
    classDef dl fill:#495057,stroke:#e9ecef,stroke-width:2px,color:#f8f9fa;
    classDef gen fill:#6c757d,stroke:#f8f9fa,stroke-width:2px,color:#ffffff;

    AI[Artificial Intelligence] --> ML[Machine Learning]
    ML --> DL[Deep Learning]
    DL --> GenAI[Generative AI]

    class AI ai;
    class ML ml;
    class DL dl;
    class GenAI gen;

```

* **Artificial Intelligence (AI):** Emulates human behavior to make decisions and solve problems.
* **Machine Learning (ML):** Uses algorithms to learn from large datasets without explicit programming.
* **Deep Learning (DL):** Uses multiple layers of artificial neural networks to extract high-level features.
* **Generative AI:** A subset of DL that generates brand-new content (text, images, code).

### Neural Network Architecture

```mermaid
graph LR
    classDef input fill:#78350f,stroke:#fbbf24,stroke-width:2px,color:#fef3c7;
    classDef hidden fill:#14532d,stroke:#4ade80,stroke-width:2px,color:#f0fdf4;
    classDef output fill:#881337,stroke:#fb7185,stroke-width:2px,color:#fff1f2;

    I1((Input)):::input --> H1((Hidden)):::hidden
    I2((Input)):::input --> H1
    I3((Input)):::input --> H1
    
    H1 --> H2((Hidden)):::hidden
    H2 --> O1((Output)):::output

    %% Simplified for visual clarity

```

* **Input Layer:** Receives the raw data.
* **Hidden Layers:** Intermediate layers where the actual processing/pattern recognition happens.
* **Output Layer:** Delivers the final prediction or generation.

## 2. AI Paradigms

Expect multiple-choice or short-answer questions asking you to categorize a specific AI task.

```mermaid
graph LR
    classDef pred fill:#0c4a6e,stroke:#38bdf8,stroke-width:2px,color:#f0f9ff;
    classDef gen fill:#4c1d95,stroke:#c084fc,stroke-width:2px,color:#faf5ff;

    Data[(Historical Data)] --> P_AI[Predictive / Discriminative AI]:::pred
    P_AI --> Out1[Specific Outcome / Label]:::pred

    Prompt[User Prompt / Noise] --> G_AI[Generative AI]:::gen
    G_AI --> Out2[Brand New Content]:::gen

```

| **Paradigm** | **Goal** | **Output** | **Exam Examples** |
| --- | --- | --- | --- |
| **Discriminative** | Classify existing data into categories. | Discrete labels (e.g., cat vs. dog). | Spam detection, fraud detection. |
| **Predictive** | Predict future values based on past data. | Continuous/Categorical (e.g., stock price). | Weather forecasting, sales prediction. |
| **Generative** | Generates entirely new data. | New content (e.g., images, text, videos). | Text generation, image synthesis. |

## 3. Core LLM Concepts & Diffusion Models

You must know the terminology that defines how models operate.

* **Parameters:** The parts of the model that it learns during training. Think of this as the model's "brain capacity" (e.g., GPT-3 has 175 billion parameters).
* **Context Window:** The model's "short-term memory." It is the amount of text it can read and understand at one time, measured in **tokens**.

### Diffusion Models (Image Generation)

This is how image generators process prompts.

```mermaid
graph TD
    classDef steps fill:#212529,stroke:#ced4da,stroke-width:2px,color:#f8f9fa;
    
    A[Text Prompt: 'an astronaut riding a horse']:::steps --> B[Text Encoder]:::steps
    B --> C[Encoded Text]:::steps
    D[RNG / 64x64 initial noise patch]:::steps --> E{Diffusion Model Loop x50}:::steps
    C --> E
    E --> F[64x64 latent patch]:::steps
    F --> G[Decoder]:::steps
    G --> H[Final Image]:::steps

```

## 4. The Model Lifecycle

Be able to distinguish between these four stages of model development.

1. **Training:** The model learns patterns by adjusting parameters using massive datasets and high compute power.
2. **Inference:** Using the trained model to get predictions. This is much faster and happens on your device or a server.
3. **Fine-Tuning:** Taking a pre-trained model and training it further on a specific, domain-focused task to customize behavior.
4. **Distillation:** Compresses a large model (teacher) into a smaller one (student) for faster, lighter, and cheaper deployment.

## 5. Technical Implementation: App Integration Workflow

When developing an application, integrating a Generative AI model looks fundamentally different from a normal API call.

```mermaid
%%{init: {'theme': 'dark'}}%%
sequenceDiagram
    autonumber
    actor User
    participant Application as Your App
    participant LLM as LLM API (e.g., OpenAI)

    User->>Application: "What does partly cloudy mean?"
    Application->>LLM: Send System Prompt + User Prompt
    Note right of Application: System: "You are a weather assistant."<br/>User: "What does partly cloudy mean?"
    LLM-->>Application: Generates Brand New Text Response
    Application-->>User: "It means a mix of sun and clouds..."

```

> [!IMPORTANT]
> **Open vs. Closed Models:** Open models (LLaMA, Mistral) have publicly available weights and code. Closed models (GPT-4, Gemini) have proprietary and controlled access.

## Active Recall Self-Check

> [!TIP] Fine-tuning adjusts a pre-trained model to specialize in a specific task. Distillation shrinks a large model into a smaller, more efficient one to save computational resources.

> [!TIP] Parameters act as the model's memory learned during training. The Context Window acts as the short-term memory (how much of the current text it can read at once).

> [!TIP] 1. Input Layer 2. Hidden Layers 3. Output Layer
