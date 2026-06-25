---
title: "IT2120 - Continuous Probability Distributions"
date: 2026-06-25
module: "IT2120"
author: Ms. K. G. M. Lakmali
institution: SLIIT
tags: [probability, statistics, continuous-distributions, semester-notes]
---

## Study Tracker
- [ ] Define Continuous Random Variables and Probability Density Functions (p.d.f.).
- [ ] Calculate probabilities and expectations using integration for continuous variables.
- [ ] Differentiate and apply Uniform, Exponential, and Normal distributions.
- [ ] Perform Normal Approximations for Binomial and Poisson distributions using Continuity Corrections.

## 1. Continuous Random Variables & PDF

Unlike discrete variables which are counted, a **Continuous Random Variable** takes any value within a defined range (e.g., time, temperature, distance) . Because there are infinite possible exact values, the probability of the variable taking one specific exact value is zero: $Pr(X = a) = 0$ .

Instead of a probability mass function, continuous variables use a **Probability Density Function (p.d.f.)**, denoted as $f_X(x)$ . Probabilities are calculated as the *area under the curve* over an interval using integration .

### Properties of a Valid p.d.f.
1. It must be non-negative: $f_X(x) \ge 0$ for all $x \in \mathbb{R}$ .
2. The total area under the curve must equal 1: $\int_{-\infty}^{\infty}f_X(x)dx = 1$ .
3. Probability over an interval: $Pr(a < X < b) = \int_a^b f_X(x)dx$ .

### Expected Value and Variance
Expected value and variance are calculated using integration instead of summation:
* **Expected Value:** $E[X] = \int_{-\infty}^{\infty} x \cdot f_X(x)dx$ * **Variance:** $V[X] = E[X^2] - \{E[X]\}^2$ ## 2. Common Continuous Distributions

| Distribution | Concept & Use Case | Key Formulas |
| :--- | :--- | :--- |
| **Uniform Distribution** $X \sim U(a,b)$ | The simplest distribution where all outcomes in range $[a, b]$ are equally likely. | **p.d.f.:** $f(x) = \frac{1}{b-a}$<br>**Mean:** $E(X) = \frac{b+a}{2}$<br>**Variance:** $V(X) = \frac{(b-a)^2}{12}$ |
| **Exponential Distribution** $X \sim Exp(\lambda)$ | Models waiting times or duration between occurrences (e.g., time between ATM arrivals) . | **p.d.f.:** $f_X(x) = \lambda e^{-\lambda x}$<br>**Mean:** $E(X) = \frac{1}{\lambda}$<br>**Variance:** $V(X) = \frac{1}{\lambda^2}$ |
| **Normal (Gaussian) Distribution** $X \sim N(\mu, \sigma^2)$ | A perfectly symmetric, bell-shaped distribution. The most commonly used distribution in statistics . | **Mean:** $E(X) = \mu$<br>**Variance:** $V(X) = \sigma^2$ |

## 3. The Standard Normal Distribution & Approximations

Evaluating exact probabilities using the Normal distribution's complex p.d.f. requires difficult integration. Instead, we standardize any normal distribution to the **Standard Normal Distribution** ($Z$), which has a mean of 0 and a variance of 1 ($\mu=0, \sigma^2=1$) .

**Standardization Formula (Z-Score):**
$$Z = \frac{X - \mu}{\sigma} \sim N(0, 1)$$ Once converted to a Z-score, probabilities can be easily looked up in standard statistical Z-tables .

### 3.1 Normal Approximations

The Normal Distribution can approximate discrete distributions under specific conditions.

```mermaid
graph TD
    classDef discrete fill:#e8f5e9,stroke:#388e3c,stroke-width:2px,color:#1b5e20;
    classDef approx fill:#fff3e0,stroke:#f57c00,stroke-width:2px,color:#e65100;
    classDef normal fill:#e1f5fe,stroke:#0288d1,stroke-width:2px,color:#01579b;

    B["Binomial Distribution<br>X ~ Bin(n,p)"]:::discrete
    P["Poisson Distribution<br>X ~ Poisson(λ)"]:::discrete
    
    CondB["If np > 5 & n(1-p) > 5"]:::approx
    CondP["If λ > 20"]:::approx
    
    N["Normal Distribution<br>Y ~ N(μ, σ²)"]:::normal

    B --> CondB
    CondB -->|"Y ~ N(np, np(1-p))"| N
    
    P --> CondP
    CondP -->|"Y ~ N(λ, λ)"| N
````

### 3.2 Continuity Correction

> [!WARNING]
> 
> When approximating a _discrete_ distribution with a _continuous_ Normal distribution, you **must** apply a Continuity Correction factor of $0.5$ .

- $Pr(X \le a) \rightarrow P(Y < a + 0.5)$ * $Pr(X \ge a) \rightarrow P(Y > a - 0.5)$ * $Pr(X < a) \rightarrow P(Y < a - 0.5)$ * $Pr(X > a) \rightarrow P(Y > a + 0.5)$ * $Pr(X = a) \rightarrow P(a - 0.5 < Y < a + 0.5)$ ## Technical Implementation
    

Below is an R script demonstrating how to calculate continuous probabilities (Uniform, Exponential) and how to standardize Normal distributions using R's built-in probability functions.


```R
# IT2120: Continuous Probability Distributions in R

cat("--- Uniform Distribution ---\n")
# Scenario: Elevator arrives between 0 and 50 seconds. a = 0, b = 50.
# Find probability it arrives within 30 seconds: P(X <= 30)
a <- 0
b <- 50
x_uni <- 30
prob_uniform <- punif(x_uni, min = a, max = b)
cat(sprintf("P(X <= 30) for U(0, 50): %.4f\n", prob_uniform))
cat(sprintf("Expected Arrival Time E(X): %.2f seconds\n\n", (a+b)/2))

cat("--- Exponential Distribution ---\n")
# Scenario: Generator operational time follows Exponential with mean = 160 hours.
# Rate lambda = 1/mean = 1/160
lambda_val <- 1 / 160
# Find probability it lasts LESS than 40 hours: P(X < 40)
prob_exp <- pexp(40, rate = lambda_val)
cat(sprintf("P(X < 40) for Exp(1/160): %.4f\n\n", prob_exp))

cat("--- Normal Distribution & Z-Score ---\n")
# Scenario: Marks normally distributed with mean = 45, standard deviation (sigma) = 22.
# Find probability a student passes (Marks >= 45). P(X >= 45)
mu <- 45
sigma <- 22
x_norm <- 45

# 1. Using raw values
# P(X >= 45) = 1 - P(X < 45)
prob_pass_raw <- 1 - pnorm(x_norm, mean = mu, sd = sigma)

# 2. Using Z-score Standardization
# Z = (X - mu) / sigma
z_score <- (x_norm - mu) / sigma
prob_pass_z <- 1 - pnorm(z_score, mean = 0, sd = 1)

cat(sprintf("Calculated Z-Score: %.4f\n", z_score))
cat(sprintf("P(X >= 45) [Raw Method]: %.4f\n", prob_pass_raw))
cat(sprintf("P(Z >= 0) [Z-Score Method]: %.4f\n", prob_pass_z))
```

## Active Recall Self-Check

> [!TIP]
> 
> A continuous random variable has an infinite number of precise possible values within any interval. Because the probability is represented as the area under a curve, the area of a single, exact line (width of 0) is mathematically 0. To find a non-zero probability, you must evaluate an interval (e.g., $Pr(a < X < b)$) .

> [!TIP]
> 
> You use the Mean and Variance formulas inherent to the Binomial distribution. The Normal mean becomes $\mu = np$ and the Normal variance becomes $\sigma^2 = np(1-p)$ .

> [!TIP]
> 
> Because it is "greater than or equal to 25", the discrete value 25 must be included in the interval. The continuity correction expands the boundary downward by 0.5. Therefore, you calculate $P(Y > 24.5)$ on the continuous Normal curve .