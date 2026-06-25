---
title: "IT2120 - Descriptive Statistics"
date: 2026-06-25
module: "IT2120"
author: Ms. K. G. M. Lakmali
institution: SLIIT
tags: [probability, statistics, descriptive-statistics, semester-notes]
---
## Study Tracker
- [ ] Differentiate between Graphical and Numerical methods for exploratory analysis.
- [ ] Determine the appropriate charts for Categorical vs. Numerical variables.
- [ ] Calculate the Five-Number Summary and identify outliers for a Box Plot.
- [ ] Compute measures of Central Tendency (Mean, Median, Mode).
- [ ] Compute measures of Dispersion (Variance, Standard Deviation, Coefficient of Variation).

## 1. Overview of Descriptive Statistics

Descriptive statistics, or preliminary exploratory analysis, provides an initial understanding of data behavior . It relies on two primary approaches depending on the variable type: **Graphical Methods** and **Numerical Methods** .

```mermaid
graph TD
    classDef cat fill:#e1f5fe,stroke:#0288d1,stroke-width:2px,color:#01579b;
    classDef num fill:#e8f5e9,stroke:#388e3c,stroke-width:2px,color:#1b5e20;
    classDef root fill:#fff3e0,stroke:#f57c00,stroke-width:2px,color:#e65100;

    V[Variable Type]:::root --> C[One Categorical Variable]:::cat
    V --> N[One Numerical Variable]:::num
    
    C --> BC[Bar Charts]:::cat
    C --> PC[Pie Charts]:::cat
    C --> FT[One-way Frequency Tables]:::cat
    
    N --> H[Histograms]:::num
    N --> BP[Box Plots]:::num
    N --> SL[Stem and Leaf Plots]:::num
````

## 2. Graphical Methods for Numerical Data

### Histograms

A histogram represents numerical data by dividing the range into intervals (classes) of equal width .

- The x-axis represents the classes, while the y-axis represents frequency, relative frequency, or percentage .
    
- Unlike bar charts, histogram bars are drawn adjacently with no gaps to represent continuous data distributions .
    

### Box Plots

A Box Plot visually summarizes the distribution of data based on the **Five-Number Summary**: Minimum, $Q_1$ (First Quartile), $Q_2$ (Median), $Q_3$ (Third Quartile), and Maximum .

> [!IMPORTANT]
> 
> **Outlier Detection:** Before drawing the whiskers of a box plot, calculate the acceptable data range using the Interquartile Range ($IQR$).
> 
> - **Upper Bound:** $Q_3 + 1.5 \times IQR$ > * **Lower Bound:** $Q_1 - 1.5 \times IQR$ > Any values falling strictly outside these bounds are considered outliers and are denoted by asterisks (`*`) .
>     

## 3. Numerical Methods

Numerical methods summarize data distributions into single statistical values. They are exclusively applied to numerical variables .

### 3.1 Measures of Central Tendency

Indicates the central location of the dataset .

|**Measure**|**Formula**|**Characteristics**|
|---|---|---|
|**Population Mean** ($\mu$)|$\mu=\frac{\sum_{i=1}^{N}x_i}{N}$|Sum of all elements divided by population size $N$ .|
|**Sample Mean** ($\bar{x}$)|$\bar{x}=\frac{\sum_{i=1}^{n}x_i}{n}$|Sum of elements divided by sample size $n$. Default assumption if not specified .|
|**Mode**|N/A|The value with the highest frequency. A dataset can have multiple modes or none at all .|

### 3.2 Measures of Dispersion

Describes the spread or variability of the data around its mean .

> [!WARNING]
> 
> Both the **Range** (Max - Min) and **Variance** are highly sensitive to outliers. The **IQR** ($Q_3 - Q_1$) is robust against outliers and provides a better measure of spread for skewed data .

- **Population Variance** ($\sigma^2$): $\sigma^2=\frac{\sum_{i=1}^{N}(x_i-\mu)^2}{N}$ * **Sample Variance** ($s^2$): $s^2=\frac{\sum_{i=1}^{n}(x_i-\bar{x})^2}{n-1}$ * **Standard Deviation**: The square root of the variance ($\sigma$ for population, $s$ for sample) .
    

### 3.3 Measures of Relative Dispersion

The **Coefficient of Variation (CV)** is used to compare the magnitude of variation relative to the magnitude of the mean across different datasets .

$$CV = \frac{sd}{\bar{x}} \times 100$$

## Technical Implementation

Since this module utilizes **R** for its 2-hour lab sessions, below is a standard R script to compute the descriptive statistics and outlier boundaries discussed in this lecture.

```R
# Descriptive Statistics and Outlier Detection in R

# Sample dataset representing cycles survived by plastic hinges
data <- c(72, 35, 63, 67, 87, 71, 64, 47, 60, 81, 39, 52, 57, 74, 43, 55, 37, 83, 48, 91, 53, 44, 94, 65, 75)

# 1. Central Tendency
sample_mean <- mean(data)
# Mode function (R does not have a built-in mode function for numerics)
get_mode <- function(v) {
  uniqv <- unique(v)
  uniqv[which.max(tabulate(match(v, uniqv)))]
}
sample_mode <- get_mode(data)

cat("--- Central Tendency ---\n")
cat("Mean:", sample_mean, "\n")
cat("Mode:", sample_mode, "\n\n")

# 2. Dispersion
sample_var <- var(data) # In R, var() computes the SAMPLE variance (n-1)
sample_sd <- sd(data)
cv <- (sample_sd / sample_mean) * 100

cat("--- Dispersion ---\n")
cat("Sample Variance:", round(sample_var, 2), "\n")
cat("Sample Standard Deviation:", round(sample_sd, 2), "\n")
cat("Coefficient of Variation (CV):", round(cv, 2), "%\n\n")

# 3. Five-Number Summary & Outlier Detection
summary_stats <- summary(data)
Q1 <- quantile(data, 0.25)
Q3 <- quantile(data, 0.75)
IQR_val <- IQR(data)

lower_bound <- Q1 - (1.5 * IQR_val)
upper_bound <- Q3 + (1.5 * IQR_val)

outliers <- data[data < lower_bound | data > upper_bound]

cat("--- Box Plot Summary ---\n")
print(summary_stats)
cat("Lower Bound:", lower_bound, "\n")
cat("Upper Bound:", upper_bound, "\n")
cat("Detected Outliers:", if(length(outliers) > 0) outliers else "None", "\n")
```

## Active Recall Self-Check

> [!TIP]
> 
> Using $(n-1)$ provides an unbiased estimator of the population variance. It corrects for the fact that sample data is used to estimate the sample mean first, reducing the degrees of freedom by one.

> [!TIP]
> 
> Calculate the Interquartile Range ($IQR = Q_3 - Q_1$). A data point is considered an outlier if it is strictly greater than $Q_3 + (1.5 \times IQR)$ or strictly less than $Q_1 - (1.5 \times IQR)$ .

> [!TIP]
> 
> The Interquartile Range (IQR) is robust against outliers because it only considers the middle 50% of the dataset. Range, Variance, and Standard Deviation are all highly sensitive to extreme outlier values .