---
title: "IT2120 - Random Variables & Probability Distributions"
date: 2026-06-25
module: "IT2120"
author: Ms. K. G. M. Lakmali
institution: SLIIT
tags: [probability, statistics, distributions, random-variables, semester-notes]
---
## Study Tracker
- [ ] Define Random Variables and distinguish between Discrete and Continuous types.
- [ ] Understand Probability Mass Functions (p.m.f.) and Cumulative Distribution Functions (c.d.f.).
- [ ] Calculate Expected Value, Variance, and Covariance, and apply their mathematical properties.
- [ ] Identify and apply Bernoulli, Binomial, and Poisson discrete probability distributions.
- [ ] Use the Poisson Approximation for Binomial distributions.

## 1. Random Variables and Functions

A **Random Variable (r.v.)** is a function defined on a sample space ($S$) that associates a real number, $X(\omega) = x$, with each outcome $\omega$ in $S$ . Simply put, it maps outcomes in a sample space to a set of real numbers . They are denoted by capital letters (e.g., $X, Y$) .

* **Discrete Random Variables:** Can assume only distinct, countable values (e.g., the number of heads in 5 coin tosses: $X \in \{0, 1, 2, 3, 4, 5\}$) .
* **Continuous Random Variables:** Can assume an infinite number of values within a given interval.

### Probability Functions for Discrete Variables

> [!NOTE]
> The **Probability Mass Function (p.m.f.)**, denoted by $P_X(x)$, represents the probability that a discrete random variable is exactly equal to some value: $P_X(x) = Pr(X = x)$ .

**Properties of a p.m.f.:**
1.  Always non-negative: $P_X(x) \ge 0$ .
2.  The sum of all probabilities equals 1: $\sum P_X(x) = 1$ .

The **Cumulative Distribution Function (c.d.f.)**, denoted by $F_X(x)$, represents the probability that the variable takes a value less than or equal to $x$:
$$F_X(x) = Pr(X \le x)$$ ## 2. Expected Value, Variance, and Covariance

These metrics summarize the distribution of a random variable mathematically .

### Formulas
* **Expected Value / Mean ($E(X)$):** $$E(X) = \sum x \cdot Pr(X = x)$$ * **Variance ($V(X)$):** $$V(X) = E(X - E(X))^2 = E(X^2) - [E(X)]^2$$ * **Covariance ($Cov(X, Y)$):** Measures how two random variables vary together.
    $$Cov(X, Y) = E(XY) - E(X)E(Y)$$ ### Mathematical Properties

| Operation | Expected Value $E(X)$ | Variance $V(X)$ |
| :--- | :--- | :--- |
| **Constants ($c$)** | $E(c) = c$ | $V(c) = 0$ |
| **Adding a Constant** | $E[g(X) + c] = E[g(X)] + c$ | $V[g(X) + c] = V[g(X)]$ |
| **Multiplying by Constant**| $E[c \cdot g(X)] = c \cdot E[g(X)]$ | $V[c \cdot g(X)] = c^2 \cdot V[g(X)]$ |
| **Adding Variables** | $E[X + Y] = E[X] + E[Y]$ | $V[X \pm Y] = V[X] + V[Y] \pm 2Cov(X,Y)$ |

> [!IMPORTANT]
> If $X$ and $Y$ are independent random variables, their covariance is zero ($Cov(X,Y) = 0$) . Therefore, $V[X+Y]$ simply becomes $V[X] + V[Y]$.

## 3. Standard Discrete Probability Distributions

Certain random events occur frequently enough that their probabilities can be modeled using standardized distribution formulas .

```mermaid
graph TD
    classDef main fill:#e1f5fe,stroke:#0288d1,stroke-width:2px,color:#01579b;
    classDef sub fill:#e8f5e9,stroke:#388e3c,stroke-width:2px,color:#1b5e20;
    classDef detail fill:#fff3e0,stroke:#f57c00,stroke-width:2px,color:#e65100;

    D[Discrete Probability Distributions]:::main --> BER[Bernoulli Distribution]:::sub
    D --> BIN[Binomial Distribution]:::sub
    D --> POI[Poisson Distribution]:::sub

    BER -.-> |"Expands to multiple (n) trials"| BIN
    BIN -.-> |"Approximates when n is large & p is small"| POI
````

### Distribution Comparison

|**Feature**|**Bernoulli Distribution**|**Binomial Distribution**|**Poisson Distribution**|
|---|---|---|---|
|**Concept**|A single trial with exactly two possible outcomes (Success/Failure) .|Multiple ($n$) independent Bernoulli trials .|Occurrences within a continuous interval or for a given rate ($\lambda$) .|
|**Probability of Success ($p$)**|Constant for the single trial.|Constant for every independent trial .|N/A (Based on rate $\lambda$)|
|**p.m.f. Formula**|$P_X(x) = p^x(1-p)^{1-x}$|$P_X(x) = \binom{n}{x}p^x(1-p)^{n-x}$|$P_X(x) = \frac{e^{-\lambda}\lambda^x}{x!}$|
|**Mean $E(X)$**|$p$|$np$|$\lambda$|
|**Variance $V(X)$**|$p(1-p)$|$np(1-p)$|$\lambda$|

### Poisson Approximation of the Binomial

If $X \sim Bin(n,p)$, it can be approximated using a Poisson distribution with the rate parameter $\lambda = np$. This approximation is valid and useful when the number of trials is large ($n > 50$) and the probability of success is very small ($p < 0.1$) .

## Technical Implementation

Because this course includes R-based laboratory sessions, below is an R script demonstrating how to calculate exact probability values using the standard built-in distribution functions for Binomial and Poisson models.


```R
# IT2120: Probability Distributions in R

cat("--- Binomial Distribution ---\n")
# Scenario: A machine produces defective screws with a probability of p = 0.01.
# We pick n = 10 screws. What is the probability exactly x = 2 screws are defective?
n <- 10
p <- 0.01
x <- 2

# dbinom(x, size, prob) calculates the exact probability mass function (p.m.f.)
prob_exact_2_defects <- dbinom(x, size = n, prob = p)
cat(sprintf("Probability of exactly 2 defects: %.5f\n", prob_exact_2_defects))

# pbinom(q, size, prob) calculates the cumulative distribution function (c.d.f.)
# e.g., Probability of AT MOST 3 defects (x <= 3)
prob_at_most_3 <- pbinom(3, size = n, prob = p)
cat(sprintf("Probability of at most 3 defects: %.5f\n\n", prob_at_most_3))


cat("--- Poisson Distribution ---\n")
# Scenario: On average, a book has 1 typo every 2 pages. (Rate: lambda = 0.5 typos per page).
# What is the probability of exactly 0 errors on a single page?
lambda <- 0.5
x_poi <- 0

# dpois(x, lambda) calculates the Poisson p.m.f.
prob_zero_errors <- dpois(x_poi, lambda = lambda)
cat(sprintf("Probability of exactly 0 errors: %.5f\n", prob_zero_errors))

# Probability of AT LEAST 1 error on a page. P(X >= 1) = 1 - P(X = 0)
prob_at_least_1 <- 1 - prob_zero_errors
cat(sprintf("Probability of at least 1 error: %.5f\n", prob_at_least_1))
```

## Active Recall Self-Check

> [!TIP]
> 
> The variance is **90**. Using the properties of variance: $V(c \cdot X + k) = c^2 \cdot V(X)$. Therefore, $V(3X + 5) = 3^2 \cdot V(X) = 9 \cdot 10 = 90$. Adding a constant (5) does not change the spread of the data, only the location .

> [!TIP]
> 
> 1. The probability of any specific outcome must be non-negative: $P_X(x) \ge 0$ .
>     
> 2. The sum of the probabilities for all possible outcomes in the sample space must equal exactly 1: $\sum P_X(x) = 1$ .
>     

> [!TIP]
> 
> The Poisson approximation is generally used when the number of trials ($n$) is very large (typically $n > 50$) and the probability of success ($p$) is very small (typically $p < 0.1$). When these conditions are met, you set the Poisson rate $\lambda = np$ .