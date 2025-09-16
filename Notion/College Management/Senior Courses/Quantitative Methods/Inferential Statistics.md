Perfect. Let’s treat this like we are sitting in a lecture hall, and I’m guiding you carefully through **Inferential Statistics** — the first major course outcome in your **MATH181 – Quantitative Methods** class. I’ll take the material you provided, keep all the key definitions and terms, and then **expand** them into a full lecture with theoretical clarity, practical meaning, and examples.

---

# 📘 Lecture: Inferential Statistics

---

## 1. Introduction

Statistics, as you already know, has two great pillars:

1. **Descriptive Statistics** – concerned with summarizing, describing, and visualizing data.
2. **Inferential Statistics** – concerned with making *predictions, decisions, or generalizations* about a population using only a *sample* of data.

Today, we dive into **Inferential Statistics**, which allows us to answer questions like:

* *“If I only have 40 patients in my study, what can I say about all patients in the country?”*
* *“If I test 100 manufactured nails, can I be confident that the entire batch meets quality standards?”*

This is where mathematics, probability theory, and logic come together to bridge the gap between *sample* and *population*.

---

## 2. Definition of Inferential Statistics

Let’s take the **formal definition** and break it down:

> **Inferential statistics is one of the two main branches of statistics. It uses a random sample of data taken from a population to describe and make inferences about the population.**

* **Population**: the entire group we care about (e.g., all nails produced by a factory, all patients with diabetes in the Philippines).
* **Sample**: a subset of that population we can realistically collect data from (e.g., 100 nails, 40 patients).

Why not measure the whole population?

* Time-consuming
* Expensive
* Sometimes impossible (e.g., testing every nail would destroy them)

So instead, we use a **sample** and apply mathematical techniques to **infer** something about the population.

---

## 3. Sampling Error and Variability

Key point:

> *Study results will vary from sample to sample strictly due to random chance.*

This is called **sampling error**. Even if every sample is collected fairly, the numbers won’t be identical across samples.

Example:

* Suppose the true average height of college students is **167 cm**.
* One random sample of 30 students might yield an average of **165 cm**, another might yield **170 cm**.

The role of inferential statistics is to quantify this uncertainty.

* We don’t just report: “The mean is 165 cm.”
* We report: “We estimate the mean is 165 cm, **with a margin of error ±3 cm at 95% confidence**.”

This brings us to the core concept of **statistical significance**.

---

## 4. Statistical Significance

### Example: A Weight Loss Study

Imagine a study with **40 patients** testing a new diet pill.

* Suppose the average weight loss after 2 months is **1.5 kg**.

The big question:

* *Is this enough evidence to say the pill works for the whole population?*
* Or is the 1.5 kg just due to random fluctuations in the sample?

### The Tool: Confidence Intervals

Statistical significance gives us a **ballpark range** (called a **confidence interval**) for the true population parameter.

* If the 95% confidence interval is (0.2 kg, 2.8 kg), we can say:

  > “We are 95% confident the true average weight loss is between 0.2 and 2.8 kg.”

* If this range **does not include zero**, we say the effect is **statistically significant**.

### P-values

* If the probability of observing this result *by chance* is less than **5%** ($p < 0.05$), we conclude that the result is **statistically significant**.
* Important: *Statistical significance is not the same as practical significance.*

  * Losing 1.5 kg may be statistically real but practically useless for obesity treatment.

---

## 5. Descriptive vs. Inferential Statistics

Now let’s contrast the two:

| Aspect  | Descriptive Statistics                    | Inferential Statistics                                                    |
| ------- | ----------------------------------------- | ------------------------------------------------------------------------- |
| Purpose | Summarizes data from a sample             | Generalizes from a sample to a population                                 |
| Tools   | Mean, median, mode, dispersion, graphs    | Hypothesis tests, confidence intervals, regression                        |
| Scope   | Definitive measurement (no uncertainty)   | Includes margin of error (uncertainty)                                    |
| Example | “In my survey, average income = ₱25,000.” | “The average income in the city is between ₱24,000 and ₱26,000 (95% CI).” |

---

## 6. Descriptive Tools (Review)

Before making inferences, we often describe the data:

1. **Central Tendency** – mean, median, mode → where is the “center”?
2. **Dispersion** – range, variance, standard deviation → how spread out is the data?
3. **Skewness** – symmetry or asymmetry of the data distribution.

These provide the foundation for inference.

---

## 7. Inferential Tools

Now the real power:

### (a) Confidence Intervals

* Give a range for population parameters (mean, proportion).
* Express uncertainty with a confidence level (95%, 99%).

### (b) Hypothesis Testing

* Formal procedure to test claims about populations.
* Example:

  * Claim: “The new drug lowers blood pressure.”
  * Null Hypothesis ($H_0$): no effect.
  * Alternative Hypothesis ($H_a$): there is an effect.
  * Use sample data to decide whether to reject $H_0$.

### (c) Regression Analysis

* Studies the relationship between variables.
* Example: *Does height predict weight?*
* Regression allows us to build predictive models and quantify how one variable influences another.

---

## 8. Summary (Key Points to Remember)

1. **Inferential Statistics** allows us to generalize from a sample to a population.
2. It accounts for **sampling error** and gives us a **margin of error**.
3. **Statistical significance** ($p < 0.05$) means results are unlikely due to chance.
4. **Descriptive vs. Inferential**: descriptive is about *summarizing*, inferential is about *predicting/generalizing*.
5. Main techniques:

   * **Confidence Intervals**
   * **Hypothesis Testing**
   * **Regression Analysis**

---

✅ After mastering this, you’ll be ready to tackle more advanced inferential methods such as **ANOVA, chi-square tests, and correlation**, which extend these principles.

---

Would you like me to **expand this into worked examples** (like solving a confidence interval or hypothesis test step-by-step), so you can see how the formulas apply in practice?
