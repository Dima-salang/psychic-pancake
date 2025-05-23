Excellent — you’re now entering the heart of **descriptive statistics** with **estimates of location**, which are absolutely fundamental to **statistical reasoning, machine learning, and AI**. These are the first tools we use to **summarize a distribution**, identify **central tendencies**, and begin to reason about the data.

---

# 📍 Estimates of Location in Statistics: The Core Concepts

---

## 🌟 Overview: What Are Estimates of Location?

> Estimates of location are summary measures that indicate where the center of a distribution lies.

They help answer questions like:

- What is the **typical value**?
- Where is the **center** of the data?
- What value is the data **clustered around**?

---

## 📈 1. **Arithmetic Mean (Average)**

### 🧠 Definition:

Mean=1n∑i=1nxi\text{Mean} = \frac{1}{n} \sum_{i=1}^{n} x_i

- Add up all values
- Divide by the number of values

```Python
import numpy as np

data = [4, 5, 6, 8]
np.mean(data)  # 5.75
```

### ✅ Pros:

- Easy to compute and understand
- Used in most statistical and ML algorithms (e.g., regression, gradient descent)

### ❌ Cons:

- **Not robust**: extremely sensitive to **outliers**

### 📌 Use Case:

- When data is roughly **symmetric** and **outliers are not an issue**

---

## ⚖️ 2. **Weighted Mean**

### 🧠 Definition:

Weighted Mean=∑wixi∑wi\text{Weighted Mean} = \frac{\sum w_i x_i}{\sum w_i}

Each value is multiplied by a **weight** that reflects its **importance or frequency**.

```Python
values = [10, 20, 30]
weights = [0.2, 0.3, 0.5]
np.average(values, weights=weights)  # 23.0
```

### ✅ Pros:

- Allows for **importance scaling**
- Reflects **real-world priorities** (e.g., sample sizes, confidence)

### 📌 Use Case:

- When observations contribute **unequally** to the overall estimate (e.g., school grades, portfolio returns)

---

## 📏 3. **Median**

### 🧠 Definition:

The **middle value** of a sorted dataset.

- If odd number of values → middle
- If even → average of two middle values

```Python
np.median([1, 3, 7])     # 3
np.median([1, 3, 7, 10]) # 5.0
```

### ✅ Pros:

- **Robust** to outliers
- Works well for **skewed** distributions

### ❌ Cons:

- Not as mathematically convenient as mean (e.g., harder in algebraic manipulation)

### 📌 Use Case:

- Income distributions, house prices, when **data is skewed**

---

## 🎯 4. **Percentiles / Quantiles**

### 🧠 Definition:

A percentile is a value below which a given percentage of observations fall.

- **50th percentile** = median
- **25th and 75th** = quartiles
- **90th, 95th** = tail behavior

```Python
np.percentile([1, 3, 5, 7, 9], 25)  # 3.0
np.percentile([1, 3, 5, 7, 9], 75)  # 7.0
```

### 📌 Use Case:

- Summarizing **distribution shape**
- **Detecting outliers**
- Understanding **relative standing**

---

## ⚖️ 5. **Weighted Median**

### 🧠 Definition:

The value that divides the data into two equal **cumulative weighted halves**.

Unlike weighted mean, it **preserves robustness** of median but accounts for weights.

There’s no NumPy built-in, but you can use `statsmodels` or implement it manually.

```Python
from statsmodels.stats.weightstats import DescrStatsW

values = [10, 20, 30]
weights = [1, 2, 1]
d = DescrStatsW(values, weights=weights)
d.quantile(0.5)  # Weighted median
```

### 📌 Use Case:

- Survey data with **sampling weights**
- Fairer when combining data with **variable credibility**

---

## ✂️ 6. **Trimmed Mean**

### 🧠 Definition:

The mean after **removing a certain percentage of the lowest and highest values** (e.g., 10%).

```Python
from scipy.stats import trim_mean

trim_mean([1, 2, 3, 4, 100], 0.2)  # trims 10% from each side
```

### ✅ Pros:

- Reduces influence of **extreme values**
- A compromise between mean and median

### 📌 Use Case:

- **Finance**, **sports**, **benchmarking**
- Where **moderate robustness** is needed without discarding too much info

---

## 🛡️ 7. **Robust Location Estimators**

Robust statistics resist the influence of outliers or **non-normality**.

### Examples:

- **Median**
- **Trimmed mean**
- **Winsorized mean** (extremes replaced, not removed)

### Winsorized Mean Example:

```Python
from scipy.stats.mstats import winsorize

data = [1, 2, 3, 100, 101]
winsorized = winsorize(data, limits=[0.1, 0.1])
np.mean(winsorized)  # less affected by 100, 101
```

---

## 🚨 8. Outliers and Their Effects

### What’s an outlier?

- A value **much higher or lower** than most others
- Often defined as being **1.5× IQR** (interquartile range) beyond Q1 or Q3

```Python
import seaborn as sns
import matplotlib.pyplot as plt

sns.boxplot(data=[1, 2, 3, 4, 100])
plt.show()
```

### ⚠️ Effect on Mean vs Median:

- **Mean** gets pulled towards the outlier
- **Median** stays near the bulk of the data

---

## 📌 Summary Table

|   |   |   |
|---|---|---|
|Estimator|Sensitive to Outliers?|Use When|
|Mean|Yes|Data is symmetric, no outliers|
|Weighted Mean|Yes|Values have different importance|
|Median|No|Skewed data, need robustness|
|Weighted Median|No|Skewed data + importance weighting|
|Trimmed Mean|Less|Moderate robustness + data efficiency|
|Winsorized Mean|Less|Need to keep all data, control impact|
|Percentiles|No|Summary of distribution tails|

---

## 📋 Best Practices

✅ **Visualize your data first**

Before choosing a location estimator, use:

- Histograms
- Box plots
- Violin plots

✅ **Use median for skewed or messy data**

✅ **Use trimmed or Winsorized mean** if you need **robustness but still want to use means**

✅ **Don’t trust the mean blindly** — always check for **outliers**!

✅ In **machine learning pipelines**, normalize/skew-aware transforms (e.g., log transforms) help make mean-based methods work better.

---

## 🧠 Deeper Thought: Why It Matters in AI

|   |   |
|---|---|
|Field|Role of Location Estimators|
|ML Preprocessing|Centering (e.g., mean subtraction), outlier detection|
|Computer Vision|Image normalization (mean/std of pixels)|
|NLP|Embedding averages, document centroids|
|Data Cleaning|Imputation, anomaly detection|

---

Would you like to move on to **estimates of spread/dispersion** next (like variance, standard deviation, IQR, MAD)? Or dive deeper into **robust statistics** or **real-world preprocessing pipelines** in AI and ML?