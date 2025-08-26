Excellent. Let’s now build a **complete lecture** on this content, treating it as a full introductory module in statistics. I’ll follow the flow of the provided material, but I’ll also **expand with deeper explanations, real-world illustrations, and professional insights** to make sure you grasp both theory and practice.

---

# 📊 Lecture: Introduction to Statistics (MATH181 – Quantitative Methods)

*Mapúa University – Department of Mathematics*

---

## 🎯 Lesson Objectives

By the end of this lesson, you should be able to:

1. **Define** basic statistical terms and concepts.
2. **Explain** the importance of statistics in daily life and professional practice.
3. **Differentiate** between descriptive and inferential statistics.
4. **Understand** methods of data collection and presentation.

---

## 1. Definition of Statistics

**Statistics** is the field concerned with the **collection, organization, presentation, analysis, and interpretation of data** to make decisions, solve problems, and design processes.

👉 In simple terms: **Statistics is the science of data.**

* It transforms **raw data** into **meaningful information**.
* In engineering, business, and research, it provides the **evidence base for decision-making**.

**Example:**

* A manufacturing firm collects defect rates on an assembly line. Without statistics, those numbers are just “counts.” With statistics, they can determine **trends**, **root causes**, and whether a new process actually improves quality.

---

## 2. Branches of Statistics

Statistics is broadly divided into two areas:

### 2.1 Descriptive Statistics (DS)

* **Definition:** Concerned with summarizing, organizing, and describing characteristics of a dataset.
* **Focus:** Present facts in a clear way (tables, graphs, measures like mean, variance).
* **Key Point:** DS does **not** go beyond the data at hand—no predictions or generalizations.

**Examples:**

* “How many passed in the last Electrical Engineering Licensure exam?”
* A dataset of **breakdown times of an insulating fluid** at 34 kV, where we compute descriptive measures like **mean, median, standard deviation**.

---

### 2.2 Inferential Statistics (IS)

* **Definition:** Concerned with **drawing inferences about a population** based on a sample.
* **Focus:** Uses probability theory to generalize, predict, and test hypotheses.
* **Key Point:** IS allows decision-making under **uncertainty**.

**Examples:**

* “Is there a significant correlation between study hours and final grades in programming?”
* “Do ABET-accredited programs significantly attract more students to Mapúa?”

👉 **Connection:**

* **Descriptive Statistics** → provides summaries.
* **Inferential Statistics** → goes further, making predictions and conclusions.

---

## 3. Population and Sample

### 3.1 Population

* **Definition:** The **totality of all possible observations**.
* Described by **parameters** (e.g., population mean μ, population variance σ²).

**Example:**

* Population: All 5,786 students enrolled in MATH10-1.
* Parameter: 5,786 (population size).

---

### 3.2 Sample

* **Definition:** A **subset of the population**, ideally representative.
* Described by **statistics** (e.g., sample mean $\bar{x}$, sample variance s²).

**Example:**

* Sample: The 3,456 female students out of 5,786.
* Statistic: 3,456 (sample size).

👉 **Key Principle:** A **good sample** must be heterogeneous enough to represent the population but **randomly chosen** to avoid bias.

---

## 4. Variables

**Variables** are characteristics or attributes measured in statistics.

### 4.1 Qualitative Variables (Categorical)

* **Definition:** Non-numeric, categorical responses.
* **Examples:** Gender, religion, marital status, city of residence.

### 4.2 Quantitative Variables (Numerical)

* **Definition:** Countable or measurable values.
* **Examples:** Height, weight, voltage, exam grades.

---

### 4.3 Continuous vs. Discrete Data

* **Continuous Data:** Measurable with infinite possible values within an interval.

  * Example: Height = 172.3 cm, weight = 65.27 kg.
* **Discrete Data:** Countable, finite values.

  * Example: Number of students in a class, months in a year.

👉 **Analogy:** Think of **continuous data** as a *smooth line* and **discrete data** as *stepping stones*.

---

### 4.4 Variables in Research Context

* **Independent Variable (IV):** Manipulated or natural factor believed to influence another.

  * Example: Hours of study.
* **Dependent Variable (DV):** Observed outcome.

  * Example: Test scores.
* **Controlled Variable:** Held constant to isolate effects.

  * Example: Same textbook used for all students.
* **Extraneous Variable:** Uncontrolled factor that may affect results.

  * Example: Students’ prior knowledge.

👉 **In research design:** Careful control of variables ensures **valid causal conclusions**.

---

## 5. Scales of Measurement

### 5.1 Nominal Scale

* **Definition:** Categorical labeling, no ordering.
* **Examples:** Gender (1 = male, 2 = female), course codes.

### 5.2 Ordinal Scale

* **Definition:** Rank-ordered data, but differences are not uniform.
* **Examples:** Beauty pageant rankings, Moh’s hardness scale.

### 5.3 Interval Scale

* **Definition:** Equal differences between values, but **no true zero**.
* **Operations:** Addition/subtraction valid, ratios not meaningful.
* **Examples:** Years (1990, 2000), Celsius temperature.

### 5.4 Ratio Scale

* **Definition:** Has an absolute zero, allowing **all mathematical operations**.
* **Examples:** Weight, length, charge, energy.

👉 **Hierarchy Insight:**

* Nominal → Ordinal → Interval → Ratio.
* Each level adds more mathematical meaning and operations.

---

## 6. Sampling

### 6.1 Probability Sampling

* **Definition:** Every member has a known, non-zero chance of selection.
* **Types:**

  * **Simple Random Sampling:** Each element equally likely.
  * **Systematic Sampling:** Selecting every *k-th* element.
  * **Stratified Sampling:** Dividing population into strata, then sampling proportionally.
  * **Cluster Sampling:** Dividing into clusters (mini-populations) and sampling clusters.

---

### 6.2 Non-Probability Sampling

* **Definition:** Not all members have a chance of selection.
* **Types:**

  * **Convenience Sampling:** Based on availability.
  * **Quota Sampling:** Fixed numbers per group, not random.
  * **Purposive Sampling:** Based on researcher’s criteria.

---

## 7. Applied Example: Stratified Sampling

**Problem:**

* Population $N = 5000$, divided into strata:

  * A = 1000 families
  * B = 2500 families
  * C = 1500 families
* Margin of error = 5%

### Step 1: Apply Slovin’s Formula

$$
n = \frac{N}{1+Ne^2} = \frac{5000}{1+(5000)(0.05^2)} \approx 370
$$

### Step 2: Proportional Allocation

$$
n_h = \frac{N_h}{N} \cdot n
$$

* Stratum A: $(1000/5000)(370) = 74$
* Stratum B: $(2500/5000)(370) = 185$
* Stratum C: $(1500/5000)(370) = 111$

✅ Total = 370 families

---

## 🔑 Summary

* **Statistics = science of data.**
* **Two fields:** Descriptive (summarizes) vs. Inferential (generalizes).
* **Population vs. Sample:** Parameters vs. Statistics.
* **Variables:** Qualitative/Quantitative, Continuous/Discrete, Independent/Dependent.
* **Scales of measurement:** Nominal → Ordinal → Interval → Ratio.
* **Sampling:** Probability (random, systematic, stratified, cluster) vs. Non-probability (convenience, quota, purposive).

---

## 📌 Professional Insight

1. **In practice, poor sampling = poor results.** Always aim for probability sampling when generalization is the goal.
2. **Scales of measurement matter.** Choosing the wrong scale leads to invalid statistical methods (e.g., taking an average of nominal categories like religion makes no sense).
3. **Inferential statistics relies on probability.** Without understanding probability, inference becomes guesswork.

---

Would you like me to **extend this lecture by deriving Slovin’s formula from the theory of confidence intervals** so you understand **where it really comes from**, rather than just applying it? This will give you a much deeper mastery.
