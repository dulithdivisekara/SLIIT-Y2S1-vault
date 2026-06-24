---
title: "IT2011 - Lecture 02 - Introduction to Generative AI"
date: 2026-06-25
module: "IT2011"
author: Dulith Divisekara
institution: SLIIT
tags: [information-technology, generative-ai, language-models, semester-notes]
---
## Study Tracker
- [ ] Distinguish between Discriminative, Predictive, and Generative AI paradigms.
- [ ] Understand the mechanics of Large Language Models (LLMs) and context windows.
- [ ] Apply effective Prompt Engineering principles.
- [ ] Comprehend model optimization techniques like Fine-Tuning and Distillation.

## 1. AI Paradigms: Discriminative vs. Predictive vs. Generative

Artificial Intelligence encompasses various methodologies designed to equip computers to emulate human behavior. Within this hierarchy, Generative AI operates as a specialized subset of Deep Learning, focusing on content creation rather than just classification.

> [!IMPORTANT]
> **Generative AI** utilizes a mix of supervised and unsupervised learning to detect patterns in vast datasets and create outputs without explicit instructions.

### Paradigm Comparison

| Feature | Discriminative AI | Predictive AI | Generative AI |
| :--- | :--- | :--- | :--- |
| **Goal** | Classifies existing data into categories. | Predicts future values based on past data. | Generates entirely new data. |
| **Output Type** | Discrete labels (e.g., cat vs. dog). | Continuous or categorical values. | New content (e.g., text, images). |
| **Example Models** | Logistic regression, SVMs, CNNs. | Regression models, LSTMs, ARIMA. | GANs, VAEs, Transformers (GPT, DALL-E). |

## 2. Fundamentals of Large Language Models (LLMs)

Modern Language Models predict the next word in a sequence and rely on the Transformer architecture introduced by Google in 2017. 

### Key LLM Terminology

* **Parameters:** The internal parts of the model that it learns during training. Parameters act as the 'memory' of the model; more parameters equal more knowledge capacity (e.g., GPT-3 has 175 billion parameters).
* **Context Window:** The maximum amount of text a model can read and understand at once, measured in tokens. This dictates the model's short-term memory capability.
* **Inference:** The process of using a fully trained model to get predictions, which is significantly faster than the training phase and can be run on local devices.

## 3. Model Optimization and Deployment

To adapt large models for specific tasks or resource-constrained environments, engineers employ optimization techniques.

> [!NOTE]
> **Small Language Models (SLMs)** are smaller LLMs (e.g., Mistral 7B, Phi-2) optimized for fast inference and deployment in low-resource environments.

* **Fine-Tuning:** Taking a pre-trained model and training it further on domain-specific data to customize its behavior.
* **Distillation:** Compressing a large "teacher" model into a smaller, lighter "student" model to reduce computational costs while retaining performance.

```mermaid
graph TD
    classDef foundation fill:#e1f5fe,stroke:#0288d1,stroke-width:2px,color:#01579b;
    classDef process fill:#e8f5e9,stroke:#388e3c,stroke-width:2px,color:#1b5e20;
    classDef output fill:#fff3e0,stroke:#f57c00,stroke-width:2px,color:#e65100;

    FM[Foundation Model / Teacher]:::foundation --> FT[Fine-Tuning]:::process
    FM --> DS[Model Distillation]:::process
    
    FT --> Domain[Domain-Specific Model]:::output
    DS --> SLM[Small Language Model / Student]:::output
    
    Domain --> INF[Inference Engine]:::process
    SLM --> INF
````

## 4. Prompt Engineering Framework

Effective prompt engineering dictates the quality of the generative output. A robust prompt architecture consists of four structural elements: **Context**, **Task**, **Instructions**, and **Clarify and Refine**.

> [!TIP]
> 
> **Be Specific:** Instead of a bad prompt like _"Write something about tech and security"_, use a good prompt like _"Generate a report on emerging trends in cybersecurity technologies for IoT devices from 2022-2024"_.

## Technical Implementation

Below is a self-contained Python implementation demonstrating how to programmatically interact with an LLM using the OpenAI API.

```Python
import os
import openai

def query_language_model(user_query: str) -> str:
    """
    Connects to the OpenAI API to generate a response based on a system and user prompt.
    """
    # Initialize the API key 
    openai.api_key = os.getenv("OPENAI_API_KEY", "your_api_key")
    
    try:
        # Standard API Call structure for ChatCompletion
        response = openai.ChatCompletion.create(
            model="gpt-4",
            messages=[
                {
                    "role": "system",
                    "content": "You are a helpful assistant that explains weather reports."
                },
                {
                    "role": "user",
                    "content": user_query
                }
            ]
        )
        
        # Parse and return the generated content
        return response['choices'][0]['message']['content']
        
    except Exception as e:
        return f"API Connection Error: {str(e)}"

if __name__ == "__main__":
    # Test execution
    test_prompt = "What does it mean when the weather report says 'partly cloudy with a chance of showers'?"
    print(f"User Prompt: {test_prompt}\n")
    print("Awaiting LLM response...")
    
    result = query_language_model(test_prompt)
    print(f"\nResponse:\n{result}")
```

## Active Recall Self-Check

> [!TIP]
> 
> Open models (e.g., LLaMA, Mistral) have publicly available weights and code, allowing for community-driven improvement and transparency. Closed models (e.g., GPT-4, Gemini) maintain proprietary, controlled access and are trained on private datasets.

> [!TIP]
> 
> The context window acts as the model's short-term memory. It dictates the maximum amount of input text (measured in tokens) the model can read, process, and consider simultaneously when generating a response.

> [!TIP]
> 
> Distillation compresses a large "teacher" model into a smaller "student" model, making it faster, lighter, and cheaper to run. This is particularly crucial for deploying models to edge devices or mobile environments.