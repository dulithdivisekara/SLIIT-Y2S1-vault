---
title: "IE1011 - Bias and Ethics in Artificial Intelligence"
date: 2026-06-25
module: "IE1011"
author: Dulith Divisekara
institution: SLIIT
tags: [information-technology, artificial-intelligence, machine-learning-ethics, semester-notes]
---
## Study Tracker
- [ ] Differentiate between AI bias and AI ethics.
- [ ] Identify sources and consequences of ML bias.
- [ ] Understand core ethical principles for AI.
- [ ] Review case studies (COMPAS, Amazon) and mitigation strategies.

---

## 1. Core Concepts: Bias vs. Ethics

AI/ML technologies drive innovation but introduce significant risks regarding fairness.

> [!NOTE]
> **Bias:** Systematic unfairness in data or algorithms leading to discriminatory outcomes.
> **Ethics:** Moral principles guiding the responsible design and deployment of AI.

---

## 2. Types and Sources of Bias

| Bias Type | Definition | Example |
| :--- | :--- | :--- |
| **Data Bias** | Historical data reflects social prejudices. | Hiring algorithm favoring men due to past tech hiring trends. |
| **Sampling Bias** | Training data lacks population diversity. | Western-trained healthcare AI failing on Asian demographics. |
| **Labeling Bias** | Subjective human annotation. | Labeling "aggressiveness" based on cultural stereotypes. |
| **Algorithmic Bias** | Model design favors majority classes. | Facial recognition failing on minority groups. |
| **Interaction Bias** | User behavior reinforces stereotypes. | Search algorithms amplifying sensationalist content. |

> [!WARNING]
> **Impact of Bias:** Denial of critical services (loans/jobs), reinforced social inequality, loss of public trust, and legal/regulatory penalties.

---

## 3. Core Ethical Principles

Ethics ensures AI remains strictly aligned with human values and rights.

*   **Fairness:** Equitable treatment without discrimination.
*   **Transparency:** Clear, explainable decision-making (avoiding "black boxes").
*   **Accountability:** Clear responsibility for AI-driven harm.
*   **Privacy:** Secure data handling (e.g., GDPR compliance).
*   **Human Oversight:** Maintaining human control in high-stakes areas (healthcare, military).
*   **Safety & Security:** System robustness against errors and adversarial attacks.
*   **Beneficence:** Actively promoting well-being and minimizing harm.

---

## 4. Mitigation Strategies

Addressing bias requires a combination of technical engineering and organizational policy.

| Approach | Solutions |
| :--- | :--- |
| **Technical** | Fairness-aware algorithms, routine bias detection audits, diverse/balanced datasets, Explainable AI (XAI). |
| **Organizational** | AI Ethics Boards, Model Cards (dataset documentation), adopting global guidelines (OECD/EU), interdisciplinary teams. |

---

## 5. Architectural View of Bias

```mermaid
graph TD
    classDef styleGray fill:#f4f4f5,stroke:#52525b,stroke-width:1px,color:#27272a;
    classDef styleBlue fill:#e0f2fe,stroke:#0284c7,stroke-width:1px,color:#0c4a6e;

    A[Data Collection] -->|Data & Sampling Bias| B[Model Training]
    B -->|Algorithmic Bias| C[AI Decision / Output]
    C -->|Interaction Bias| D[User Feedback Loop]
    D -.->|Reinforces Prejudices| A

    class A,D styleGray;
    class B,C styleBlue;
````

## 6. Case Studies

### COMPAS (U.S. Justice System)

- **Use Case:** Predicted recidivism to assist in parole decisions.
    
- **The Flaw:** Disproportionately flagged Black defendants as "high risk" (higher false positive rate).
    
- **Key Insight:** Mathematical fairness is complex; optimizing one metric often worsens another metric across demographics.
    

### Amazon Recruitment AI

- **Use Case:** Automated resume screening.
    
- **The Flaw:** Penalized female candidates because the model learned from 10 years of historically male-dominated tech resumes.
    
- **Key Insight:** Models will automatically replicate historical inequities if training data is not actively balanced.
    

## 7. Technical Implementation: Bias in Logic

```Java
public class LoanSystem {
    
    // Conceptual demonstration of flawed, biased logic without fairness constraints
    public static boolean checkEligibility(Applicant applicant) {
        
        // BIASED: Applying different thresholds based on group identity
        if (applicant.getDemographicGroup().equals("Majority_Class")) {
            return applicant.getCreditScore() >= 600; // Lower barrier
        } else {
            return applicant.getCreditScore() >= 750; // Higher, unfair barrier
        }
    }
}
```

> [!TIP]
> 
> **The Fix:** Implement **Fairness-Aware Algorithms** that either remove demographic identifiers from the decision tree or apply mathematical constraints to equalize acceptance rates across all groups.

## Active Recall Self-Check

> [!TIP]
> 
> **Data Bias:** The historical data itself contains prejudice (e.g., past sexist hiring trends).
> 
> **Sampling Bias:** The data may be neutral but is incomplete, missing key demographics (e.g., testing a medical model only on one specific ethnicity).

> [!TIP]
> 
> A perfectly accurate, unbiased model can still violate broader ethical principles. For example, a fully autonomous weapon may be mathematically unbiased but lacks necessary human oversight and violates the principle of non-maleficence (avoiding harm).

> [!TIP]
> 
> A Model Card is standard technical documentation detailing a dataset's limitations, intended use cases, and known demographic gaps. It ensures transparency and prevents the model from being deployed in scenarios it wasn't designed for.