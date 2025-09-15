Excellent — let’s walk through **Measures of Central Tendency** as a full lecture, following the provided material, but with the depth and elaboration you’d expect from an experienced statistician and software engineer.

---

# 📊 Measures of Central Tendency

**MATH181 – Quantitative Methods**
Mapua University – Department of Mathematics

---

## 🎯 Lesson Objectives

By the end of this lesson, you should be able to:

1. Compute the **mean, median, and mode** for both grouped and ungrouped data.
2. Discuss the **properties of mean, median, and mode**.
3. Discuss the **properties of the midrange**.
4. Calculate the **midrange** of a dataset.
5. Understand the **effects of changing the units** on the mean and median.

---

# I. **The Mean**

### Definition

The **mean** is the arithmetic average — a value obtained by dividing the sum of all observations by the number of observations.
It is the most commonly used measure of central tendency.

---

### Formula – **Ungrouped Data**

$$
M = \frac{\sum_{i=1}^{n} x_i}{n}
$$

Where:

* $M$ = mean
* $x_i$ = individual observation
* $n$ = number of observations

💡 **Example:**
Scores: 25, 30, 35, 40, 50

$$
M = \frac{25+30+35+40+50}{5} = \frac{180}{5} = 36
$$

The **average score** is **36**.

---

### Formula – **Grouped Data (Method 1)**

$$
M = \frac{\sum (f \cdot x)}{N}
$$

Where:

* $f$ = frequency
* $x$ = class mark (midpoint of the class interval)
* $N = \sum f$ = total frequency

---

### Formula – **Grouped Data (Method 2)**

$$
M = A + \frac{\sum f d}{N} \cdot C
$$

Where:

* $A$ = assumed mean
* $d = \frac{x - A}{C}$ = deviation
* $C$ = class size

👉 This method simplifies calculation when data values are large.

---

# II. **The Median**

### Definition

The **median** is the middle value of an ordered dataset.

* For **odd n**: it is the middle observation.
* For **even n**: it is the average of the two middle observations.

The median is less sensitive to **outliers** than the mean.

---

### Formula – **Grouped Data**

$$
Md = L + \left(\frac{\frac{N}{2} - F}{f}\right) \cdot C
$$

Where:

* $L$ = lower class boundary of the median class
* $N$ = total frequency
* $F$ = cumulative frequency before the median class
* $f$ = frequency of the median class
* $C$ = class size

---

### Example (Ungrouped)

Data: 12, 10, 9, 8, 5, 6, 4

Step 1: Arrange in order → 4, 5, 6, 8, 9, 10, 12
Step 2: Middle value → **8**

So, $Md = 8$.

---

# III. **The Mode**

### Definition

The **mode** is the value that occurs most frequently in a dataset.

* A dataset can be **unimodal**, **bimodal**, or **multimodal**.
* If no value repeats, the dataset has **no mode**.

---

### Formula – **Grouped Data**

$$
Mo = L + \left(\frac{f_m - f_1}{2f_m - f_1 - f_2}\right) \cdot C
$$

Where:

* $L$ = lower class boundary of the modal class
* $f_m$ = frequency of the modal class
* $f_1$ = frequency before modal class
* $f_2$ = frequency after modal class
* $C$ = class size

---

### Example (Ungrouped)

Scores: 55, 45, 40, 60, 55, 70

* Most frequent = **55** → $Mo = 55$.

If multiple values tie for highest frequency → dataset is **bimodal or multimodal**.

---

# IV. **The Midrange**

### Definition

The **midrange** is the midpoint between the lowest and highest values.

$$
MR = \frac{\text{Lowest Value + Highest Value}}{2}
$$

👉 It is simple to compute but very sensitive to outliers.

---

# V. **Effects of Changing Units**

* If all data values are **multiplied by a constant**:

  * Both **mean and median** are multiplied by the same constant.
* If a **constant is added** to all data values:

  * Both **mean and median** increase by that constant.

⚖️ This property makes mean and median **linear measures of central tendency**.

---

# VI. **Summary**

* **Mean**: arithmetic average; sensitive to extreme values; best for interval/ratio data.
* **Median**: middle value; robust to outliers; suitable for ordinal or skewed data.
* **Mode**: most frequent value; best for categorical data.
* **Midrange**: midpoint of extremes; easy but unstable.

📌 Together, these measures give different perspectives on the “center” of a dataset.

---

# VII. **Practical Insights**

* In **symmetrical distributions**, mean = median = mode.
* In **positively skewed distributions** (tail to the right): mean > median > mode.
* In **negatively skewed distributions** (tail to the left): mean < median < mode.

👉 This relationship is often used in **data analysis** to quickly assess skewness.

---

✅ That covers the **theory, formulas, and properties** of the mean, median, mode, and midrange with both **grouped and ungrouped data**.

Would you like me to now **work through one of the grouped data examples step by step** (with full solution tables), so you can see exactly how to compute mean, median, and mode in practice?


Perfect — let’s now focus **specifically on grouped data** and how to compute the **mean, median, mode, and midrange** step by step.
This is where students often get confused because we don’t have raw values anymore, only **class intervals** and **frequencies**.

---

# 📊 Measures of Central Tendency for Grouped Data

---

## 1. **The Mean (Grouped Data)**

When data is grouped into **class intervals**, we don’t know the exact values inside each class.
So, we approximate by assuming all values in a class are equal to the **class midpoint**.

### Formula

$$
\bar{x} = \frac{\sum (f \cdot x)}{N}
$$

Where:

* $f$ = frequency of each class
* $x$ = class midpoint
* $N = \sum f$ = total frequency

---

### Example

| Class Interval | Frequency $f$ | Midpoint $x$ | $f \cdot x$ |
| -------------- | ------------- | ------------ | ----------- |
| 0–10           | 5             | 5            | 25          |
| 10–20          | 8             | 15           | 120         |
| 20–30          | 12            | 25           | 300         |
| 30–40          | 10            | 35           | 350         |
| 40–50          | 5             | 45           | 225         |
| **Total**      | **40**        |              | **1020**    |

$$
\bar{x} = \frac{1020}{40} = 25.5
$$

👉 The **mean** is **25.5**.

---

## 2. **The Median (Grouped Data)**

The **median** is the value that divides the data into two equal parts.
We need the **median class**, the class where the cumulative frequency reaches or exceeds $N/2$.

### Formula

$$
Md = L + \left(\frac{\frac{N}{2} - F}{f}\right) \cdot C
$$

Where:

* $L$ = lower boundary of median class
* $N$ = total frequency
* $F$ = cumulative frequency before median class
* $f$ = frequency of median class
* $C$ = class size

---

### Example (using the same table)

* $N = 40$ → $N/2 = 20$
* Cumulative frequencies:

  * 0–10: 5
  * 10–20: 13
  * 20–30: 25 → **median class**

Now apply formula:

* $L = 20$
* $F = 13$
* $f = 12$
* $C = 10$

$$
Md = 20 + \left(\frac{20 - 13}{12}\right) \cdot 10
$$

$$
Md = 20 + \frac{7}{12} \cdot 10
$$

$$
Md = 20 + 5.83 = 25.83
$$

👉 The **median** is approximately **25.83**.

---

## 3. **The Mode (Grouped Data)**

The **mode** is the value that occurs most frequently. In grouped data, we use the **modal class** (class with highest frequency).

### Formula

$$
Mo = L + \left(\frac{f_m - f_1}{2f_m - f_1 - f_2}\right) \cdot C
$$

Where:

* $L$ = lower boundary of modal class
* $f_m$ = frequency of modal class
* $f_1$ = frequency before modal class
* $f_2$ = frequency after modal class
* $C$ = class size

---

### Example

From table:

* Modal class = 20–30 (frequency = 12)
* $f_m = 12, f_1 = 8, f_2 = 10$
* $L = 20, C = 10$

$$
Mo = 20 + \left(\frac{12 - 8}{2(12) - 8 - 10}\right) \cdot 10
$$

$$
Mo = 20 + \left(\frac{4}{24 - 18}\right) \cdot 10
$$

$$
Mo = 20 + \left(\frac{4}{6}\right) \cdot 10
$$

$$
Mo = 20 + 6.67 = 26.67
$$

👉 The **mode** is approximately **26.67**.

---

## 4. **The Midrange (Grouped Data)**

Since we only know **class intervals**, the midrange is approximated using:

$$
MR = \frac{\text{Lowest class boundary + Highest class boundary}}{2}
$$

From example:

* Lowest boundary = 0
* Highest boundary = 50

$$
MR = \frac{0 + 50}{2} = 25
$$

👉 The **midrange** is **25**.

---

# ✅ Summary (Grouped Data Example)

From our dataset:

* Mean = **25.5**
* Median = **25.83**
* Mode = **26.67**
* Midrange = **25**

📌 Notice they’re all close to each other — this suggests the data is **fairly symmetric**.

---

Would you like me to now **code this grouped data example in Python** (step by step), so you can see how to compute **mean, median, and mode** programmatically for grouped frequency tables?
