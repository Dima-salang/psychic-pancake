``Alright, let us dive into **Course Outcome 1: Data Presentation** from *MATH181 – Quantitative Methods*.

I will explain this lesson in a structured, lecture-style format: beginning with **conceptual foundations**, then moving into **technical details**, and finally **applications and interpretations**.

---

# 📊 Lecture on Data Presentation in Statistics

---

## 1. Introduction

Once we have collected data—whether from surveys, experiments, or observations—the next step is **data presentation**.

👉 Why do we need this step?

* Raw data is often overwhelming. Imagine looking at a list of 10,000 exam scores. Reading raw numbers is inefficient and may obscure meaningful patterns.
* **Presentation** organizes and summarizes the data, making it easier to **analyze, interpret, and communicate**.

---

## 2. Types of Data Presentation

There are **three main types**:

1. **Textual Form**

   * Uses sentences or paragraphs to describe data.
   * Example: *“Out of 100 students, 60% passed, 30% failed, and 10% were incomplete.”*
   * Advantage: Useful for short summaries.
   * Limitation: Cannot handle large datasets effectively.

2. **Tabular Form**

   * Data is arranged in **rows and columns**.
   * Enables comparison across categories or time periods.
   * Example: A table of student grades by section.
   * Advantage: Compact, structured, easy to reference.

3. **Graphical Form**

   * Uses **visuals** (bar graphs, histograms, frequency polygons, ogives).
   * Advantage: Makes trends, patterns, and comparisons immediately apparent.
   * Essential for reports, presentations, and decision-making.

---

## 3. Forms of Data: Ungrouped vs. Grouped

1. **Ungrouped Data**

   * Raw, individual observations.
   * Example: Exam scores of 10 students → 72, 85, 90, 66, 88, 95, 78, 84, 91, 87
   * Advantage: Preserves exact detail.
   * Limitation: Becomes unreadable for large datasets.

2. **Grouped Data**

   * Data organized into **class intervals (categories)**.
   * Example:

     * Heights:

       * 150–159 cm → 3 students
       * 160–169 cm → 6 students
       * 170–179 cm → 4 students
   * Advantage: Simplifies interpretation, prepares data for statistical analysis.
   * Limitation: Loses some detail (we only know ranges, not exact values).

📌 **Key Insight:** In practice, large datasets are almost always grouped. Grouping is what enables the construction of **frequency distribution tables and graphs**.

---

## 4. Frequency Distribution Table (FDT)

A **frequency distribution table** shows **how often each value or range of values occurs**.

### 4.1 Parts of an FDT

* **Class limits:** Smallest and largest values in a class interval.
* **Class boundaries:** Adjusted values (±0.5 for integer data) for precision.
* **Frequency (f):** Count of observations per class.
* **Cumulative Frequency (CF):** Running total of frequencies.

  * `<cf`: Less than CF (accumulated up to an upper boundary).
  * `>cf`: Greater than CF (accumulated down from a lower boundary).
* **Relative Frequency (%rf):** Percentage of observations in each class.

---

## 5. Constructing a Frequency Distribution

The procedure:

1. **Find Range:** $R = H - L$ (Highest – Lowest value).
2. **Decide Number of Classes (k):**

   * Should be between 5 and 15.
   * Can use **Sturges’ formula:**

     $$
     k = 1 + 3.322 \log n
     $$

     where $n$ = sample size.
3. **Find Class Interval (C):**

   $$
   C = \frac{R}{k}
   $$

   Round to nearest integer.
4. **Set Class Limits:** Choose a lower starting point and construct intervals of size $C$.
5. **Tally Frequencies:** Count how many observations fall into each interval.

---

## 6. Graphical Presentation of Frequency Distributions

Once the table is built, we can graphically represent it:

1. **Histogram**

   * A **bar graph** where the bars represent class intervals.
   * Width = class size, height = frequency.
   * Used for both frequency and relative frequency.

2. **Frequency Polygon**

   * A **line graph** connecting points plotted at class midpoints vs. frequency.
   * Useful for comparing two or more datasets.

3. **Ogive (Cumulative Frequency Curve)**

   * Line graph showing cumulative frequencies.
   * Two types:

     * `< ogive` → based on less than CF.
     * `> ogive` → based on greater than CF.
   * Useful for determining **percentiles, quartiles, and medians**.

---

## 7. Practical Examples

### Example 1 (Exam Scores of 50 students)

* Range: $90 - 43 = 47$.
* $n = 50$, Sturges’ formula → $k \approx 7$.
* Class interval: $C = R/k = 47/7 \approx 7$.
* Construct classes → (43–49, 50–56, 57–63, …).
* Count frequencies.

📊 This results in a frequency table with **relative frequency distribution** and cumulative frequencies.

---

## 8. Importance in Real Applications

* **Education:** Test score distributions help identify performance levels.
* **Manufacturing:** Grouped thickness measurements (Example 3) detect quality control issues.
* **Business:** Income brackets and customer age groups guide marketing.
* **Engineering:** Voltage, stress, and measurement data are grouped to detect variation patterns.

📌 **Professional Note:**
In practice, frequency distribution and histograms are the foundation of **statistical quality control (SQC)** and **exploratory data analysis (EDA)**.

---

## 9. Summary

* Data can be presented in **textual, tabular, or graphical** forms.
* **Ungrouped data** = raw values; **grouped data** = organized intervals.
* **Frequency distribution tables** simplify analysis through class limits, frequencies, relative frequencies, and cumulative totals.
* Graphical tools (**histogram, frequency polygon, ogives**) enhance interpretation and communication.

---

✅ By mastering data presentation, you build the foundation for more advanced statistical concepts like **measures of central tendency, dispersion, correlation, and hypothesis testing**.

---

Would you like me, in the next step, to **show you how the same dataset can look very different when shown as a histogram, polygon, and ogive side-by-side** (with actual plots), so you can see why statisticians always prefer visualization over raw tables?


Got it 👍 You want to know how to **make and use a frequency table**, with clear examples. Let’s walk through it step by step.

---

## 1. What is a Frequency Table?

A **frequency table** is a way to organize data to show **how often (frequency)** each value (or group of values) appears in a dataset.

It usually has these columns:

* **Value / Class** → the data or range of data
* **Frequency** → how many times it appears
* (Optional) **Relative Frequency** → percentage of total
* (Optional) **Cumulative Frequency** → running total

---

## 2. Example 1: Simple Data (Un-grouped)

Let’s say we surveyed 20 students about how many siblings they have.

Data (siblings per student):

```
0, 1, 2, 2, 3, 1, 0, 4, 2, 3,
1, 2, 5, 3, 0, 1, 2, 3, 1, 2
```

### Step 1: List unique values

Possible number of siblings: 0, 1, 2, 3, 4, 5

### Step 2: Count frequencies

* 0 → 3 times
* 1 → 5 times
* 2 → 6 times
* 3 → 4 times
* 4 → 1 time
* 5 → 1 time

### Step 3: Make the table

| Siblings | Frequency | Relative Freq.    | Cumulative Freq. |
| -------- | --------- | ----------------- | ---------------- |
| 0        | 3         | 3/20 = 0.15 = 15% | 3                |
| 1        | 5         | 5/20 = 0.25 = 25% | 8                |
| 2        | 6         | 6/20 = 0.30 = 30% | 14               |
| 3        | 4         | 4/20 = 0.20 = 20% | 18               |
| 4        | 1         | 1/20 = 0.05 = 5%  | 19               |
| 5        | 1         | 1/20 = 0.05 = 5%  | 20               |

✅ Now you can see the distribution clearly.

---

## 3. Example 2: Grouped Data (Class Intervals)

If you have a **large dataset** (e.g., exam scores out of 100), you usually group them into **class intervals**.

Exam scores of 30 students (out of 100):

```
55, 67, 72, 85, 90, 45, 60, 75, 82, 95,
68, 70, 77, 58, 83, 92, 74, 81, 63, 89,
79, 88, 94, 52, 61, 73, 64, 87, 91, 66
```

### Step 1: Choose intervals (say, 10-point ranges)

40–49, 50–59, 60–69, 70–79, 80–89, 90–99

### Step 2: Count frequencies

* 40–49 → 1
* 50–59 → 3
* 60–69 → 7
* 70–79 → 6
* 80–89 → 7
* 90–99 → 6

### Step 3: Make the table

| Score Range | Frequency | Relative Freq. | Cumulative Freq. |
| ----------- | --------- | -------------- | ---------------- |
| 40–49       | 1         | 1/30 = 3.3%    | 1                |
| 50–59       | 3         | 10%            | 4                |
| 60–69       | 7         | 23.3%          | 11               |
| 70–79       | 6         | 20%            | 17               |
| 80–89       | 7         | 23.3%          | 24               |
| 90–99       | 6         | 20%            | 30               |

✅ This shows most students scored between 60–69 and 80–89.

---

## 4. How to Make One (Step-by-Step)

1. **Collect the raw data**
2. **Decide categories**:

   * If data is small, list unique values.
   * If large, create intervals (equal width is best).
3. **Tally frequencies** (count each occurrence).
4. **Add totals**: relative frequencies (%) and cumulative frequencies if needed.
5. **Check totals**: frequency sum = total data points.

---

👉 Do you want me to also show you **how to do a frequency table in Excel or Python**, or just by hand on paper?
