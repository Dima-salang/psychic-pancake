Perfect. Let’s build a **complete lecture on Measures of Shape** that follows your Mapúa MATH181 material closely, but expands it into a **professor-level explanation** with theoretical depth, technical rigor, and practical examples.

---

# 📊 **Measures of Shape (Skewness and Kurtosis)**

*MATH181 – Quantitative Methods*
*Mapúa University – Department of Mathematics*

---

## 🔹 1. Introduction

In statistics, we often summarize data with:

* **Measures of Central Tendency** (mean, median, mode) → *where is the center?*
* **Measures of Dispersion** (range, variance, standard deviation, CV) → *how spread out are the values?*

But **shape matters too.** Two datasets can have the same mean and standard deviation but look very different in their **distribution form**.

👉 **Measures of Shape** (Skewness and Kurtosis) tell us:

* **Skewness** → Is the distribution symmetric, or is it stretched more to one side?
* **Kurtosis** → How sharp is the peak, and how heavy are the tails compared to a normal distribution?

These measures add a **third dimension** to our understanding of data, beyond center and spread.

---

## 🔹 2. Skewness

### Definition

* The **degree of asymmetry** of a distribution about the mean.
* Tells us whether the distribution is:

  * **Symmetric** (balanced left and right)
  * **Positively skewed** (right-skewed)
  * **Negatively skewed** (left-skewed)

---

### Symmetry vs. Skewness

* **Symmetric distribution** → can be folded along the vertical axis (mirror image).
* **Skewed distribution** → one tail is longer than the other.

**Types of skewness:**

1. **Positive Skew (Right-skewed)**

   * Long tail on the right side.
   * Example: Income distribution (majority low–middle income, few very high earners).

2. **Negative Skew (Left-skewed)**

   * Long tail on the left side.
   * Example: Age at retirement (most retire around 60–65, but some retire early).

3. **Symmetric Distribution**

   * Tails balanced, e.g., adult human heights.

---

### Relationship between Mean, Median, and Mode

* **Positive skew**: Mode < Median < Mean
* **Symmetric**: Mode = Median = Mean
* **Negative skew**: Mode > Median > Mean

This relationship provides a quick diagnostic without computing formulas.

---

### Formula for Skewness

For **ungrouped data**:

$$
S_k = \frac{\sum (x - \bar{x})^3}{n s^3}
$$

For **grouped data**:

$$
S_k = \frac{\sum f(x - \bar{x})^3}{n s^3}
$$

Where:

* $S_k$ = coefficient of skewness
* $x$ = observation (or class mark for grouped data)
* $\bar{x}$ = mean
* $s$ = standard deviation
* $f$ = frequency
* $n$ = number of observations

---

### Interpretation of Skewness Values

* $S_k = 0$ → Symmetrical distribution
* $S_k > 0$ → Positively skewed (right tail longer)
* $S_k < 0$ → Negatively skewed (left tail longer)

**Strength of skewness:**

* $|S_k| < 0.5$ → Approximately symmetric
* $0.5 \leq |S_k| < 1$ → Moderately skewed
* $|S_k| \geq 1$ → Highly skewed

---

### Practical Example of Skewness

* **Income** → right-skewed: few billionaires drag the mean far right.
* **Exam scores** (when most did well) → left-skewed: majority near high scores, but a few very low ones.
* **Heights of adults** → symmetric: follows normal distribution.

---

## 🔹 3. Kurtosis

### Definition

* A measure of **peakedness** (sharpness of the peak) and **tail heaviness** of a distribution compared to the normal distribution.
* While variance measures spread, kurtosis looks at whether extreme values (outliers) are common or rare.

---

### Formula for Kurtosis

For **ungrouped data**:

$$
K = \frac{\sum (x - \bar{x})^4}{n s^4}
$$

For **grouped data**:

$$
K = \frac{\sum f(x - \bar{x})^4}{n s^4}
$$

Where:

* $K$ = coefficient of kurtosis
* $x$ = observation (or class mark for grouped data)
* $s$ = standard deviation
* $f$ = frequency
* $n$ = number of observations

---

### Types of Kurtosis

1. **Mesokurtic (K ≈ 3)**

   * Normal distribution.
   * Moderate peak and tails.

2. **Leptokurtic (K > 3)**

   * Very sharp peak, heavy tails.
   * More extreme values than normal.
   * Example: Stock market returns (occasional extreme gains/losses).

3. **Platykurtic (K < 3)**

   * Flat peak, light tails.
   * Fewer extreme values.
   * Example: Uniform-like distributions.

---

### Interpretation

* $K = 3$ → Normal (mesokurtic)
* $K > 3$ → Leptokurtic (heavy tails, more outliers)
* $K < 3$ → Platykurtic (flat, fewer outliers)

---

## 🔹 4. Worked Example (From Course Material)

**Dataset:** Number of dresses produced in 10 days:
52, 57, 55, 63, 50, 52, 60, 58, 54, 56

1. Compute mean: $\bar{x} = 55.7$
2. Compute SD: $s = 3.97$
3. Compute skewness:

   $$
   S_k = \frac{\sum (x - \bar{x})^3}{n s^3} = 0.39
   $$

   → Slightly **positively skewed** but close to symmetric.
4. Compute kurtosis:

   $$
   K = \frac{\sum (x - \bar{x})^4}{n s^4} = 1.87
   $$

   → **Platykurtic** (flatter than normal).

---

## 🔹 5. Summary

* **Skewness** measures **asymmetry** of distribution:

  * $Sk > 0$: right-skewed (long right tail)
  * $Sk < 0$: left-skewed (long left tail)
  * $Sk = 0$: symmetric

* **Kurtosis** measures **peakedness and tails**:

  * $K = 3$: mesokurtic (normal-like)
  * $K > 3$: leptokurtic (sharp peak, heavy tails, more outliers)
  * $K < 3$: platykurtic (flat, light tails, fewer outliers)

* Together, these shape measures reveal whether your dataset:

  * is balanced or skewed
  * contains frequent or rare extreme values

---

✅ With this foundation, you can now interpret **not just the average and variability, but also the form and behavior of a dataset**—a critical skill in advanced data analysis, finance, quality control, and research.

---

Would you like me to **solve the dress-production skewness & kurtosis example step by step in Python** (with tables and code outputs), so you see the manual computation aligned with actual software results?
