Excellent. Let me now give you a **full professor-style lecture on the Normal Distribution** based on your Mapúa MATH181 course material. I will **keep all the official definitions, terms, and structure** but expand with deep explanations, theoretical insights, and practical examples from real-world data analysis and industry applications.

---

# 📊 **Normal Distribution – Quantitative Methods (MATH181)**

*Mapúa University – Department of Mathematics*
*Course Outcome 1*

---

## 🎯 **Lesson Objectives**

By the end of this lesson, you should be able to:

1. Differentiate between a **normal distribution** and a **standardized normal distribution**.
2. Identify the **properties of a normal distribution**.
3. Use the **standardized normal distribution table**.
4. Find the **area under the standard normal distribution curve**, given various z-values.
5. Find the **z-value**, given an area under the curve.
6. Compute **probabilities** for a normally distributed variable by transforming it into a standard normal variable.

---

## 🔹 1. Definition of Normal Distribution

The **normal distribution** (also called the **Gaussian distribution**) is the single most important probability distribution in statistics.

* **Shape**: continuous, symmetric, bell-shaped curve.
* **Behavior**:

  * Most values cluster around the mean.
  * Probabilities taper off smoothly in both directions (the tails).
* **Importance**:

  * Many **natural and social phenomena** (heights, test scores, measurement errors, incomes, etc.) follow it approximately.
  * It is the **foundation of inferential statistics**, used in **hypothesis testing, confidence intervals, regression, quality control, and machine learning**.

**Formal Definition**:

If a random variable $X$ follows a normal distribution with mean $\mu$ and standard deviation $\sigma$, we write:

$$
X \sim N(\mu, \sigma^2)
$$

Its probability density function (PDF) is:

$$
f(x) = \frac{1}{\sigma \sqrt{2 \pi}} e^{-\frac{(x - \mu)^2}{2\sigma^2}}, \quad -\infty < x < \infty
$$

---

## 🔹 2. Properties of Normal Distribution

1. **Bell-shaped curve**.
2. **Mean = Median = Mode** (all at the center).
3. **Unimodal** (only one peak).
4. **Symmetric about the mean** (mirror image on both sides).
5. **Continuous curve** (no gaps).
6. **Asymptotic to the x-axis** (tails get closer to zero but never touch).
7. **Total area under the curve = 1 (100%)**.
8. **Standard normal distribution** has mean = 0, SD = 1.
9. **Empirical Rule (68–95–99.7 Rule)**:

   * \~68% of data falls within **1 standard deviation** of the mean.
   * \~95% within **2 standard deviations**.
   * \~99.7% within **3 standard deviations**.

👉 These properties make the normal distribution extremely predictable.

---

## 🔹 3. Standard Normal Distribution (Z-Distribution)

The **standard normal distribution** is a special case of the normal distribution where:

* Mean = 0
* Standard deviation = 1

We convert any normal variable $X$ into this form using a **z-score**:

$$
z = \frac{x - \mu}{\sigma}
$$

* $z$ = number of standard deviations an observation $x$ is away from the mean.
* If $z = 0$: value equals the mean.
* If $z = +1.5$: value is 1.5 SD above the mean.
* If $z = -2.5$: value is 2.5 SD below the mean.

**Why Standardize?**

* Allows us to compare values from different normal distributions.
* Enables us to use **one universal Z-table** instead of recalculating probabilities for each distribution.

---

## 🔹 4. Areas Under the Standard Normal Curve

The **area under the curve = probability**.

We use **Z-tables** (or software like Python/Excel/R) to find probabilities.

* **Left of a z-value** → cumulative probability.
* **Right of a z-value** → 1 − (cumulative probability).
* **Between two z-values** → difference of areas.

---

### ✅ Example: Converting to Z-scores

The daily income of a bank auditor is normally distributed:

* Mean = Php 675
* Standard deviation = Php 50

1. Income = Php 750

$$
z = \frac{750 - 675}{50} = \frac{75}{50} = 1.5
$$

→ Auditor earns **1.5 SD above average**.

2. Income = Php 550

$$
z = \frac{550 - 675}{50} = \frac{-125}{50} = -2.5
$$

→ Auditor earns **2.5 SD below average**.

---

### ✅ Example: Using Z-tables

1. **Between z = 0 and z = 1.88**

   * Area from Z-table ≈ 0.4699
   * Probability = 46.99%

2. **To the right of z = -1.15**

   * From table, area to left of +1.15 = 0.8749
   * So probability = 87.49%

3. **To the right of z = 1.21**

   * Area left of 1.21 = 0.8869
   * Probability = 1 − 0.8869 = 0.1131 (11.31%)

---

### ✅ Example: Heights of Students

Heights of 1000 students follow:

* Mean = 174.5 cm
* SD = 6.9 cm

1. Less than 160 cm → \~18 students.
2. Between 171.5 cm and 182 cm → \~529 students.
3. Greater than 188 cm → \~25 students.

👉 This is how normal distribution allows us to **predict real-world counts**.

---

## 🔹 5. Practical Applications in Industry

* **Quality Control (Six Sigma)**: Ensuring products fall within ±3 SD of the mean.
* **Finance**: Modeling stock returns, risk, and portfolio distributions.
* **Medicine**: Understanding blood pressure, cholesterol levels, etc.
* **Education**: Standardized test scoring (SAT, IQ scores use normal distributions).
* **Machine Learning**: Gaussian distributions underlie algorithms (e.g., Naïve Bayes, Gaussian Mixture Models).

---

## 🔹 6. Summary

* The **normal distribution** is a continuous, symmetric, bell-shaped distribution.
* The **standard normal distribution** is a special case with mean = 0 and SD = 1.
* The **z-score** measures how far a value lies from the mean in units of SD.
* Probabilities are found using the **area under the curve** (via Z-tables or software).
* \~68%, 95%, and 99.7% of data fall within 1, 2, and 3 SD of the mean (empirical rule).

👉 Mastery of normal distribution is essential—it is the **backbone of inferential statistics**.

---

Would you like me to **demonstrate the student heights problem step by step in Python** (showing how to compute z-scores and probabilities), so you see both the manual Z-table method and the software approach side by side?

Great question 👍 Let’s break it down into **two parts**:

---

## 1. Using Software to Get Z-table Values

Traditionally, you’d look at a printed **z-table** to find the probability that a standard normal variable $Z$ is less than some value. With software, we can compute it directly:

### In **Python (SciPy)**:

```python
from scipy.stats import norm

# Example: P(Z < 1.25)
z = 1.25
area = norm.cdf(z)   # cumulative distribution function
print(area)  # ≈ 0.8944
```

* `norm.cdf(z)` gives the area **to the left** of z (like looking up a z-table).
* `norm.pdf(z)` gives the height of the normal curve at z (not the area).

### In **Excel / Google Sheets**:

* `=NORM.S.DIST(z, TRUE)` → gives **P(Z < z)**.
* Example: `=NORM.S.DIST(1.25, TRUE)` → 0.8944

### In **R**:

```R
pnorm(1.25)  # gives 0.8944
```

So, instead of manually searching a table, software gives you the probability instantly.

---

## 2. How to Approach Area Problems with Z-Scores

The **big picture** is:

* Convert your raw value (X) to a z-score.
* Use the cumulative distribution function (CDF) to find probabilities (areas).
* Decide whether you need **left tail, right tail, or between two z-scores**.

---

### Case A: Area to the Left

> “What is $P(Z < z)$?”

✅ Directly use `norm.cdf(z)`.
Example: $P(Z < 1.25) \approx 0.8944$.

---

### Case B: Area to the Right

> “What is $P(Z > z)$?”

Since the total area = 1:

$$
P(Z > z) = 1 - P(Z < z) = 1 - \text{CDF}(z)
$$

Example: $P(Z > 1.25) = 1 - 0.8944 = 0.1056$.

---

### Case C: Between Two Values

> “What is $P(a < Z < b)$?”

$$
P(a < Z < b) = P(Z < b) - P(Z < a)
$$

Example: $P(-1 < Z < 1) = \text{CDF}(1) - \text{CDF}(-1)$.
That’s $0.8413 - 0.1587 = 0.6826$ (≈ 68%).

---

### Case D: Convert Raw Value to Z

If data is not standardized:

$$
z = \frac{x - \mu}{\sigma}
$$

* $x$ = raw score
* $\mu$ = mean
* $\sigma$ = standard deviation

Then proceed as above.

---

✅ **Rule of Thumb** when solving:

1. **Sketch** the normal curve (helps visualize left/right).
2. **Convert raw X → z-score.**
3. **Decide which area** (left, right, between).
4. **Use CDF or table/software**.

---

Would you like me to show you a **full worked example** (say: “In a class, test scores are normally distributed with mean 75, σ = 8. What’s the probability a student scored more than 85?”) — both manually with z-table and in Python?
