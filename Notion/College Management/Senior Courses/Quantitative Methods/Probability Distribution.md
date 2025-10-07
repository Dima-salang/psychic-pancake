Absolutely! Here is the revised deep-dive lecture with all mathematical expressions fully enclosed in `$$` for optimal compatibility with Obsidian's MathJax rendering.

# $\text{MATH181: Quantitative Methods - Probability Distribution}$
## $\text{A Deep Dive Lecture}$

Welcome to this deep dive into **Probability Distributions** $\text{—}$ a fundamental concept in quantitative methods. Understanding how to model randomness is crucial for data analysis, decision-making, and risk assessment. This lecture is thorough and clear, addressing all the objectives for Course Outcome 2 of $\text{MATH181}$.


***

## $\text{1. Defining the Landscape: Probability Distributions}$

A **Probability Distribution** is the mathematical function that gives the probabilities of occurrence of different possible outcomes for an experiment. It is a mathematical description of a random phenomenon.

### $\text{Basic Concepts}$

* **Sample Space ($\text{S}$):** The set of all possible outcomes.
* **Random Variable ($\text{X}$):** A variable whose value is determined by a random experiment.
    * $\text{X}$ (Upper case) denotes the random variable.
    * $x_1, x_2, x_3, \dots$ (Lower case) denote the specific values the random variable can take.

*Example:* If $\text{X}$ is the number of tails in two coin tosses, the distribution is:
| Number of Tails ($x$) | 0 | 1 | 2 |
| :---: | :---: | :---: | :---: |
| **Probability ($\text{P}(x)$)** | $\frac{1}{4}$ | $\frac{1}{2}$ | $\frac{1}{4}$ |

***

## $\text{2. Types of Probability Distributions}$

### $\text{2.1. Discrete Probability Distributions}$

For **Discrete Random Variables** ($\text{countable}$ values), the distribution is defined by a **Probability Mass Function ($\text{PMF}$)**, $\text{P}(x) = \text{P}(X=x)$.

#### $\text{Properties of a PMF}$

1.  Probability Range:
    $$\text{0} \le \text{P}(X=x_i) \le \text{1}$$
2.  Sum of Probabilities: The sum of all possible outcomes must equal 1:
    $$\sum_{i=1}^{n} \text{P}(X_i) = \text{1}$$

#### $\text{Common Discrete Distributions (Selection Objective)}$
* **Binomial Distribution:** For counting successes in a fixed number of trials ($n$).
* **Poisson Distribution:** For counting events in a fixed interval (time/space).
* **Geometric Distribution:** For counting trials until the first success.

### $\text{2.2. Continuous Probability Distributions}$

For **Continuous Random Variables** ($\text{uncountable}$ values within a range), the distribution is defined by a **Probability Density Function ($\text{PDF}$)**, $f(x)$.

#### $\text{Key Concept: Probability as Area}$

Probability is represented by the **area under the curve** of the $\text{PDF}$.

* The total area under the curve is 1:
    $$\int_{-\infty}^{+\infty} f(x) dx = \text{1}$$
* The probability that $\text{X}$ lies between $a$ and $b$ is:
    $$\text{P}(a \le X \le b) = \int_{a}^{b} f(x) dx$$

***

## $\text{3. Expected Value, Variance, and Standard Deviation}$

These are the measures used to describe the **central tendency** and **spread** of a probability distribution.

### $\text{3.1. Expected Value (Mean), } \text{E}[X] \text{ or } \mu$

The **Expected Value** is the **long-run average** ($\text{CO2 Objective: Calculate means}$).

For a discrete random variable:
$$\text{E}[X] = \mu = \sum [x_i \cdot \text{P}(x_i)]$$

*Example (Urn Problem):* Let $\text{X}$ be the number of red balls drawn.
$$\text{E}[X] = 2\left(\frac{2}{15}\right) + 1\left(\frac{4}{15}\right) + 1\left(\frac{4}{15}\right) + 0\left(\frac{1}{3}\right) = \frac{12}{15} = \mathbf{0.8}$$

### $\text{3.2. Variance, } \text{V}[X] \text{ or } \sigma^2$

The **Variance** measures the **spread** ($\text{CO2 Objective: Calculate variances}$).

The computationally preferred formula for a discrete random variable is:
$$\text{V}[X] = \sigma^2 = \sum [x^2 \cdot \text{P}(x)] - \mu^2$$

### $\text{3.3. Standard Deviation, } \sigma$

The **Standard Deviation** is the positive square root of the variance, expressed in the $\text{original units}$ of $\text{X}$.

$$\sigma = \sqrt{\text{V}[X]} = \sqrt{\sum [x^2 \cdot \text{P}(x)] - \mu^2}$$

***

## $\text{4. Application Example: Manager's Jacket Purchase}$

Let's calculate the mean and variance for the jacket sales using the formulas.

Given (using the material's final results): $\mu = 16.34$ and $\sigma^2 = 1.88$.

* **A. Mean ($\mu$):**
    $$\mu = \sum x \cdot \text{P}(x) = \mathbf{16.34}$$

* **B. Variance ($\sigma^2$):**
    $$\sigma^2 = \sum x^2 \cdot \text{P}(x) - \mu^2 = \mathbf{1.88}$$

* **C. Standard Deviation ($\sigma$):**
    $$\sigma = \sqrt{\text{1.88}} \approx \mathbf{1.37}$$

* **Manager's Purchase Decision:** The expected (average) sale is $\mu \approx 16.34$ jackets. If the manager wishes to cover the average expected demand, they should purchase $\mathbf{17}$ jackets (rounding up from $16.34$).

***

## $\text{Summary of Key Objectives}$

1.  **$\text{Determine probabilities from PMFs and the reverse}$**: Achieved by direct reading of $\text{P}(x)$ or ensuring $\sum \text{P}(x)=1$.
2.  **$\text{Calculate means and variances}$**: Achieved using:
    $$\mu = \sum x \cdot \text{P}(x) \quad \text{and} \quad \sigma^2 = \sum x^2 \cdot \text{P}(x) - \mu^2$$
3.  **$\text{Select an appropriate discrete probability distribution}$**: Achieved by analyzing the experiment structure (e.g., fixed trials $\rightarrow$ Binomial, rate $\rightarrow$ Poisson).
4.  **$\text{Calculate probabilities, determine means and variances for common distributions}$**: Achieved by applying the general formulas to specific contexts.