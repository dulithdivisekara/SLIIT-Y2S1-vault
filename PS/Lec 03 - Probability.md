---
title: "IT2120 - Probability"
date: 2026-06-25
module: "IT2120"
author: Ms. K. G. M. Lakmali
institution: SLIIT
tags: [probability, statistics, bayes-theorem, semester-notes]
---
## Study Tracker
- [ ] Define core probability terminologies (Sample Space, Events, Trials).
- [ ] Differentiate between Mutually Exclusive, Collectively Exhaustive, and Independent events.
- [ ] Calculate Joint, Marginal, and Conditional probabilities.
- [ ] Apply the Theorem of Total Probability and Bayes' Theorem.

## 1. Foundational Terminology

Probability relies on a strict set of definitions to quantify uncertainty :

* **Experiment & Trial:** An experiment is a process leading to well-defined outcomes. A single repetition under identical conditions is a trial.
* **Sample Space ($S$):** The set containing all possible outcomes of an experiment. It can be finite (countable outcomes) or continuous (an interval of values).
* **Event:** A subset of the sample space.
    * **Simple Event:** Corresponds to a single possible outcome.
    * **Compound/Joint Event:** Corresponds to more than a single possible outcome (e.g., rolling an odd number on a die).
    * **Impossible Event ($\emptyset$):** The null subset of $S$, meaning it cannot occur.

### Event Relationships

| Relationship | Definition | Mathematical Condition |
| :--- | :--- | :--- |
| **Mutually Exclusive (Disjoint)** | Events that cannot happen at the same time . | $P(A \cap B) = 0$ |
| **Collectively Exhaustive** | A set of events where at least one must occur, covering the entire sample space . | $P(A \cup B \dots \cup Z) = 1$ |
| **Independent** | The occurrence of one event does not affect the probability of the other event occurring . | $P(A \cap B) = P(A) \times P(B)$ |

## 2. Definitions of Probability

Probability is the measure of the chance that an uncertain event will occur, bounded between 0 (impossible) and 1 (certain) .

1. **Classical Definition:** If there are $N$ equally likely outcomes and $n$ are favorable to the event, $P(A) = \frac{n}{N}$ .
2. **Empirical (Frequency) Definition:** The proportion of times an event occurs in a long run of repeated experiments. $P(A) = \frac{\text{observed favorable}}{\text{total observed}}$ .
3. **Subjective Probability:** An individual judgment or opinion about the probability of occurrence .

## 3. Probability Rules and Types

> [!IMPORTANT]
> Understanding the distinction between Marginal, Joint, and Conditional probabilities is critical for advanced statistical modeling.

* **Marginal Probability:** The unconditional probability of a single event occurring, denoted as $P(A)$ .
* **Joint Probability:** The probability of two events occurring together, denoted as $P(A \cap B)$ .
* **Conditional Probability:** The probability of one event occurring given that another has already occurred .
  $$P(A|B) = \frac{P(A \cap B)}{P(B)}$$

### The Addition Rule
Calculates the probability of event A *or* event B occurring :
$$P(A \cup B) = P(A) + P(B) - P(A \cap B)$$
*(Note: If A and B are mutually exclusive, $P(A \cap B) = 0$, so it simplifies to $P(A) + P(B)$)*.

## 4. Total Probability and Bayes' Theorem

When an event $A$ can occur across multiple mutually exclusive and collectively exhaustive branches ($B_1, B_2 \dots B_k$), we use the **Theorem of Total Probability** to find the overall marginal probability of $A$ .

$$P(A) = \sum_{i=1}^{k} P(A|B_i)P(B_i)$$

**Bayes' Theorem** reverses the condition. It calculates the posterior probability of a specific branch $B_i$ having occurred, given that the final event $A$ is known to have happened .

$$P(B_i|A) = \frac{P(B_i) \times P(A|B_i)}{\sum_{i=1}^{k} P(B_i) \times P(A|B_i)}$$

```mermaid
graph LR
    classDef root fill:#e1f5fe,stroke:#0288d1,stroke-width:2px,color:#01579b;
    classDef branch fill:#e8f5e9,stroke:#388e3c,stroke-width:2px,color:#1b5e20;
    classDef leaf fill:#fff3e0,stroke:#f57c00,stroke-width:2px,color:#e65100;

    S[Sample Space]:::root --> B1["Branch B1: P(B1)"]:::branch
    S --> B2["Branch B2: P(B2)"]:::branch
    
    B1 --> A1["Event A: P(A|B1)"]:::leaf
    B1 --> AC1["Event A': P(A'|B1)"]:::leaf
    
    B2 --> A2["Event A: P(A|B2)"]:::leaf
    B2 --> AC2["Event A': P(A'|B2)"]:::leaf
````

## Technical Implementation

Below is a self-contained R script demonstrating how to programmatically calculate Conditional Probability and implement Bayes' Theorem for a given scenario.


```R
# Probability Calculations in R

# 1. Calculating Conditional Probability
# Scenario: 70% cars have AC, 40% have CD, 20% have both.
# Find probability car has CD given it has AC.
prob_ac <- 0.70
prob_cd <- 0.40
prob_ac_and_cd <- 0.20

# P(CD | AC) = P(AC and CD) / P(AC)
prob_cd_given_ac <- prob_ac_and_cd / prob_ac

cat("--- Conditional Probability ---\n")
cat(sprintf("P(CD | AC): %.2f%%\n\n", prob_cd_given_ac * 100))

# 2. Implementing Bayes' Theorem
# Scenario: 3 factories (B1, B2, B3) produce items. 
# B1 produces 50%, B2 produces 30%, B3 produces 20%.
# Defect rates: B1 = 2%, B2 = 3%, B3 = 5%.
# If an item is found defective (Event A), what is the probability it came from B2?

# Prior probabilities P(Bi)
p_b1 <- 0.50
p_b2 <- 0.30
p_b3 <- 0.20

# Conditional probabilities P(A|Bi) - likelihood of defect given the factory
p_a_given_b1 <- 0.02
p_a_given_b2 <- 0.03
p_a_given_b3 <- 0.05

# Step 1: Theorem of Total Probability (Denominator)
# P(A) = Sum of (P(Bi) * P(A|Bi))
total_prob_a <- (p_b1 * p_a_given_b1) + (p_b2 * p_a_given_b2) + (p_b3 * p_a_given_b3)

# Step 2: Bayes' Theorem (Numerator / Denominator)
# P(B2 | A) = (P(B2) * P(A|B2)) / P(A)
prob_b2_given_a <- (p_b2 * p_a_given_b2) / total_prob_a

cat("--- Bayes' Theorem ---\n")
cat(sprintf("Total Probability of Defect P(A): %.3f\n", total_prob_a))
cat(sprintf("Probability item came from B2 given it is defective P(B2|A): %.2f%%\n", prob_b2_given_a * 100))
```

## Active Recall Self-Check

> [!TIP]
> 
> Generally, no. If two events are mutually exclusive, the occurrence of one guarantees the non-occurrence of the other ($P(A \cap B) = 0$). Because one event completely dictates the outcome of the other, they are highly dependent .

> [!TIP]
> 
> Joint Probability ($P(A \cap B)$) measures the likelihood of both events occurring simultaneously out of the _entire_ sample space. Conditional Probability ($P(A|B)$) measures the likelihood of event A occurring, but restricts the sample space _only_ to the universe where event B has already occurred .

> [!TIP]
> 
> You use it when you need to find the overall marginal probability of an event ($A$) that can occur through several different, mutually exclusive pathways or branches ($B_1, B_2, \dots$). It "sums up" the probabilities of $A$ occurring across all those branches .