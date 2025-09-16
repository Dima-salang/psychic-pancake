Excellent — let us now construct a **full lecture on Measures of Dispersion (Variability)**, drawing directly from your course material while elaborating with deep theoretical insight and practical applications.

I’ll structure this lecture the way a senior professor would deliver it: starting with definitions, progressing into computation methods (ungrouped and grouped), interpreting results, and concluding with their uses in real-world contexts.

---

# 📊 **Lecture on Measures of Dispersion / Variability**

*MATH181 – Quantitative Methods*
Mapúa University – Department of Mathematics

---

## 🔹 1. Introduction

In descriptive statistics, the **measures of central tendency** (mean, median, mode) tell us the *center* of the data. However, they do not tell us how **spread out** the data is.

This is where **measures of dispersion (or variability)** come in.

👉 **Dispersion** refers to the degree to which the data are scattered, spread out, or clustered around the central value.

Two datasets may have the same mean but very different spreads. For example:

* Dataset A: {50, 49, 51} → Mean = 50, values are tightly clustered.
* Dataset B: {20, 50, 80} → Mean = 50, but values are widely spread.

Thus, without dispersion measures, we cannot fully describe a dataset.

---

## 🔹 2. The Major Measures of Dispersion

Your course material highlights **seven key measures**:

1. **Range**
2. **Mean Absolute Deviation (MAD)**
3. **Quartile Deviation (QD)**
4. **Interquartile Range (IQR)**
5. **Variance**
6. **Standard Deviation (SD)**
7. **Coefficient of Variation (CV)**

Let’s go through them one by one.

---

## 🔹 3. Measures of Dispersion for **Ungrouped Data**

### 3.1 **Range**

**Definition**: The simplest measure of spread, defined as the difference between the highest and lowest values.

$$
R = HV - LV
$$

* **Advantage**: Very easy to compute.
* **Disadvantage**: Based only on two extreme values, highly sensitive to outliers.

---

### 3.2 **Mean Absolute Deviation (MAD)**

**Definition**: The average of the absolute deviations from the mean.

$$
MAD = \frac{\sum |x - \bar{x}|}{n}
$$

This tells us, on average, how far each data point is from the mean, without squaring.

* Less sensitive to extreme values than variance.
* Useful in finance and quality control.

---

### 3.3 **Standard Deviation (SD)**

**Definition**: The positive square root of the variance.

Two equivalent formulas:

1. Direct (Deviation method):

$$
s = \sqrt{\frac{\sum (x - \bar{x})^2}{n-1}}
$$

2. Computation (Shortcut method):

$$
s = \sqrt{\frac{n(\sum x^2) - (\sum x)^2}{n(n-1)}}
$$

* Standard deviation is the **most widely used measure** of spread.
* It reflects how data cluster around the mean.

---

### 3.4 **Variance (v)**

**Definition**: The average of the squared deviations from the mean.

$$
v = s^2
$$

* Has units squared of the original data (less interpretable).
* Used heavily in inferential statistics and probability theory.

---

---

## 🔹 4. Measures of Dispersion for **Grouped Data**

When data are grouped into class intervals, we can’t directly apply raw formulas. Instead, we adapt using **class marks** (midpoints), frequencies, and cumulative frequencies.

---

### 4.1 **Range**

$$
R = UL_H - LL_L
$$

Where:

* $UL_H$ = upper limit of highest class interval
* $LL_L$ = lower limit of lowest class interval

---

### 4.2 **Mean Absolute Deviation (Grouped Data)**

$$
MAD = \frac{\sum f |x - \bar{x}|}{N}
$$

Where:

* $x$ = class midpoint
* $f$ = frequency
* $N = \sum f$

---

### 4.3 **Quartile Deviation (QD)**

$$
QD = \frac{Q_3 - Q_1}{2}
$$

Where:

* $Q_1$ and $Q_3$ are the first and third quartiles.
* Measures the spread of the middle 50% of the data.

---

### 4.4 **Interquartile Range (IQR)**

$$
IQR = Q_3 - Q_1
$$

* Resistant to outliers.
* Widely used in boxplots and robust statistics.

---

### 4.5 **Standard Deviation (Grouped Data)**

There are **three computation methods**, all equivalent:

1. Deviation Method:

$$
s = \sqrt{\frac{\sum f (x - \bar{x})^2}{N-1}}
$$

2. Computational Formula:

$$
s = \sqrt{\frac{N(\sum f x^2) - (\sum f x)^2}{N(N-1)}}
$$

3. Coding (Step Deviation method):

$$
s = \sqrt{\frac{N(\sum f d^2) - (\sum f d)^2}{N(N-1)}} \cdot C
$$

Where $d = \frac{x - A}{C}$ (a coded deviation), $C$ = class size.

---

### 4.6 **Variance (Grouped Data)**

$$
v = s^2
$$

---

### 4.7 **Coefficient of Variation (CV)**

**Definition**: A unitless, relative measure of dispersion.

$$
CV = \frac{s}{\bar{x}} \times 100\%
$$

* Expresses variability relative to the mean.
* Useful when comparing two datasets with different units or scales.
* Example: Comparing test score consistency between two classes, or risk in finance.

---

## 🔹 5. Worked Examples

Your material provides several worked examples. Key takeaways:

* For **ungrouped data** (e.g., {35, 51, 14, 57, 55, 63, 72}):

  * Range = 58
  * MAD = 14.36
  * SD = 19.36
  * Variance = 374.81
  * CV = 39.11%

👉 Interpretation: Values are moderately clustered around the mean.

* For **grouped data** (e.g., test scores of 40 students):

  * Range = 20
  * QD = 2.82
  * MAD = 2.94
  * SD = 4.34
  * Variance = 18.84

👉 Interpretation: Scores are compressed; variability is low.

* For **Coefficient of Variation comparison**:

  * Set A: CV = 15.41%
  * Set B: CV = 22.57%

👉 Since CV is higher in Set B, it is more **dispersed** relative to its mean.

---

## 🔹 6. Effects of Changing Units

An important property:

* If each observation is multiplied by a constant $k$,

  * Range, MAD, QD, IQR, SD, and Variance all scale with $k$ (or $k^2$ for variance).
  * **Coefficient of Variation (CV)** remains **unchanged** because both numerator and denominator scale by the same factor.

👉 This makes CV particularly valuable for comparing datasets across different units.

---

## 🔹 7. Practical Applications

* **Engineering & Manufacturing** → Standard deviation used for quality control (Six Sigma).
* **Finance & Risk Analysis** → CV used to compare risk of investments relative to returns.
* **Education & Social Sciences** → Variance and IQR used to analyze test scores and income inequality.
* **Data Science & AI** → Dispersion measures used to understand dataset variability before model training.

---

## 🔹 8. Summary

* **Range**: simplest, extreme-value based.
* **MAD**: average absolute deviation from mean.
* **QD & IQR**: robust measures, focus on middle spread.
* **Variance & SD**: most widely used, based on squared deviations.
* **CV**: relative, unitless measure, best for comparing datasets.

👉 Together, these measures allow us not just to **locate the center** of a dataset, but to **understand its spread, reliability, and consistency**.

---

Would you like me to now **take one of the grouped data examples** from your material and re-solve it **step by step in Python** (showing formulas and code outputs side by side), so you can see both the statistical theory and the computational implementation?
