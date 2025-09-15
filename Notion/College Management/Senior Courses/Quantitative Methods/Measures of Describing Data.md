Excellent — let’s treat this as a **full lecture on “Measures of Describing the Data”** in line with your course outcome (MATH181 – *Quantitative Methods*). I’ll stick to the provided material, expand with professional depth, and show you how the pieces connect in both **theory and practice**.

---

# 📊 Lecture: Measures of Describing the Data

*Mapúa University – Department of Mathematics (MATH181)*

---

## 🎯 Lesson Objectives

By the end of this lecture, you should be able to:

1. **Define and differentiate** the various measures of describing data.
2. **Compute and describe** a dataset using these measures.
3. **Interpret** the values that arise from computation, both mathematically and in real-world contexts.

---

## 1. Introduction

Statistics is not only about collecting numbers; it is about **summarizing information in a meaningful way**. Raw data can be overwhelming, so statisticians use **descriptive measures** to capture the essence of a dataset.

These measures fall into **four main categories**:

1. **Measures of Central Tendency** → What is “typical”?
2. **Measures of Position** → Where does a value stand?
3. **Measures of Variation (Dispersion)** → How spread out are the values?
4. **Measures of Shape** → What does the distribution look like?

Think of them like a **health check-up** of data:

* Central Tendency = “average body temperature”
* Position = “percentile ranking” of your test result
* Variation = “range of blood pressure readings”
* Shape = “overall pattern in health measurements”

---

## 2. Measures of Central Tendency

Also called **measures of center** or **measures of central location**, these give us a single number that represents the dataset.

### 2.1 The Mean

* **Arithmetic Mean** (most common):

  $$
  \bar{x} = \frac{\sum x_i}{n}
  $$

  * **Characteristics**:

    * Uses all values.
    * Unique.
    * Deviations from the mean always sum to zero.
    * Sensitive to outliers.
  * **Use Case**: Average salary, average temperature.

* **Geometric Mean**:

  $$
  GM = \sqrt[n]{x_1 \cdot x_2 \cdot \cdots \cdot x_n}
  $$

  * Best for growth rates (e.g., investments, interest rates).

* **Harmonic Mean**:

  $$
  HM = \frac{n}{\sum (1/x_i)}
  $$

  * Useful for average rates, like speed.

* **Trimmed Mean**: Removes top and bottom outliers before averaging.

  * Example: Olympic judges trimming extreme scores.

* **Root Mean Square (RMS)**: Quadratic mean, important in physics and engineering for measuring “effective values” like AC current.

---

### 2.2 The Median

* The **middle value** once data are ordered.
* If $n$ is even → average of two middle values.
* **Characteristics**:

  * Not affected by extreme values.
  * Works with ordinal, interval, and ratio data.
  * Very useful when the dataset is skewed.

**Example**: Income data → Median income better represents the “typical” household than the mean.

---

### 2.3 The Mode

* The most frequently occurring value.
* **Characteristics**:

  * Works for nominal data (e.g., most common brand, color, vote).
  * Can be unimodal, bimodal, multimodal.
  * Least reliable, but most intuitive for categorical data.

---

### 2.4 Midrange

* Formula:

  $$
  \text{Midrange} = \frac{\text{Max} + \text{Min}}{2}
  $$
* Rarely used because it is too sensitive to extreme values.

---

## 3. Measures of Position

These describe **relative standing** — where a particular data point lies compared to others.

* **Quartiles (Q):** Divide data into 4 parts.

  * $Q_1$: Lower 25%
  * $Q_2$: Median
  * $Q_3$: Upper 25%

* **Deciles (D):** Divide data into 10 parts.

* **Percentiles (P):** Divide data into 100 parts.

  * Common in exams: *“You are in the 90th percentile”* → You scored higher than 90% of students.

* **Z-score (Standard Score):**

  $$
  z = \frac{x - \mu}{\sigma}
  $$

  * Tells how many standard deviations a value is from the mean.
  * Used in standardized testing, machine learning feature scaling, and outlier detection.

**Locators (interpolation formulas):**

$$
L_q = \frac{k(n+1)}{4}, \quad L_d = \frac{k(n+1)}{10}, \quad L_p = \frac{k(n+1)}{100}
$$

---

## 4. Measures of Variation (Dispersion)

Central tendency alone is not enough. Example: Two rivers may both average 3 feet deep, but one may be consistently 3 feet, while the other ranges from 1 to 12 feet — which is more dangerous? This is why we need **variation**.

* **Range:** Max – Min. Quick, but very sensitive to outliers.
* **Mean Absolute Deviation (MAD):** Average of absolute deviations from the mean.
* **Interquartile Range (IQR):** Spread of the middle 50% → $Q_3 - Q_1$.
* **Quartile Deviation (QD):** Half of IQR.
* **Variance:**

  $$
  \sigma^2 = \frac{\sum (x_i - \bar{x})^2}{n}
  $$
* **Standard Deviation (SD):** Square root of variance. Most widely used measure of spread.
* **Coefficient of Variation (CV):**

  $$
  CV = \frac{\sigma}{\bar{x}} \times 100\%
  $$

  * Useful for comparing variability between datasets with different units or scales.

---

## 5. Measures of Shape

Finally, statistics also cares about the **form of distribution**.

* **Skewness** → Measures asymmetry.

  * Positive skew: long tail to the right (e.g., income).
  * Negative skew: long tail to the left.
  * Zero: symmetric (normal distribution).

* **Kurtosis** → Measures peakedness or tail heaviness.

  * Leptokurtic (+): Tall peak, heavy tails (riskier distributions).
  * Platykurtic (–): Flat distribution.
  * Mesokurtic (0): Normal bell curve.

---

## 6. Summary

* **Central Tendency**: Mean, Median, Mode (plus variations).
* **Position**: Quartiles, Deciles, Percentiles, Z-score.
* **Variation**: Range, MAD, Variance, SD, IQR, CV.
* **Shape**: Skewness, Kurtosis.

Together, these measures give us a **complete descriptive picture of data** before we move to *inferential statistics* (making predictions or decisions based on samples).

---

## 7. Practical Applications

* **Business:** Median income for policy-making, CV for risk analysis.
* **Engineering:** Standard deviation for quality control.
* **Education:** Percentiles and z-scores for student ranking.
* **Data Science:** Skewness and kurtosis for choosing models (e.g., normality assumptions).

---

✅ That’s the **full theoretical lecture**.

Would you like me to now **work through a concrete dataset step by step** (say, 12 numbers) and compute *mean, median, mode, variance, standard deviation, quartiles, z-scores, skewness* so you can see exactly how these formulas play out in practice?
