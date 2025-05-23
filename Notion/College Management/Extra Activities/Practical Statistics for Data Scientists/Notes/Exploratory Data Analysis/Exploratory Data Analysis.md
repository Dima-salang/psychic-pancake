---
Created: 2023-11-06T13:16
Type: Lecture
Materials:
  - "[[Chapter_1.pdf]]"
Reviewed: true
---
Classical statistics focused almost exclusively on inference. Jown W. Tukey called for a reformation of statistics in his seminal paper “The Future of Data Analysis”. He proposed a new scientific discipline called data analysis that included statistical inference as just one component.

  

# Elements of Structured Data

- Data comes from many sources.

There are two basic types of structured data: numeric and categorical

- Numeric - data that are expressed on a numeric scale
    - Continuous - data that can take on any value in an interval
    - Discrete - data that can take on only integer values such as counts
- Categorical - data that can take only a specific set of values representing a set of possible categories
    - Ordinal - has explicit ordering
    - Cardinal
    - Binary

  

Why bother with data types?

- Knowing the data type can act as a signal telling software how statistical procedures should behave.
- Storage and indexing can be optimized
- The possible values a given categorical variable can take a re enforced in the software.

  

# Rectangular Data

The typical frame of reference for an analysis in data science is a rectangular data object, like a spreadsheet of database table.

  

Rectangular data is the general term for a two-dimensional matrix with rows indicating records and columns indicating features or variables; data frame is the specific format in R and Python.

  

# Data Frames and Indexes

- Traditional database tables have one or more columns designated as an index, essentially a primary key.

  

# Nonrectangular Data Structures

- Time series
- Spatial data structures
- Graph data structures

  

# Estimates of Location

A basic step in exploring your data is getting a typical value for each feature: an estimate of where most of the data is located.

Mean

- the sum of all values divided by the number of values. Basically the average value. This is usually represented by the symbol x-bar.
- can be sensitive to outliers

![[/Untitled 17.png|Untitled 17.png]]

> [!important] N (or n) refers to the total number of records or observations. N for population and n if it refers to a sample from a population.

Weighted Mean

- The sum of all values times a weight divided by the sum of the weights.
- You calculate by multiplying each data value $x_i$ by a user-specified weight $w_i$, and dividing their sum by the sum of the weights.
- Motivations:
    - Some values are intrinsically more variable than others, and highly variable observations are given a lower weight.
    - The data collected does not equally represent the different groups that we are interested in measuring.

![[/Untitled 1 6.png|Untitled 1 6.png]]

Median

- The value such that one-half of the data lies above and below. It is the middle number on a sorted list of the data.
- If there is an even number of data values, the middle value is one that is not actually in the data set, but rather the average of the two values that divide the sorted data into upper and lower halves.
- Compared to the mean, which uses all observations, the median depends only on the values in the center of the sorted data.

Percentile

- The value such that P percent of teh data lies below

Weighted median

- The value such that one-half of the sum of the weights lies above and below the sorted data
- As with the median, we first sort the data, although each data value has an associated weight. Instead of the middle number, the weighted median is a value such that the sum of the weights is equal for the lower and upper halves of the sorted list. It is also robust to outliers.

Trimmed mean

- The average of all values after dropping a fixed number of extreme values.
- A trimmed mean eliminates the influence of extreme values.

![[/Untitled 2 3.png|Untitled 2 3.png]]

Robust

- Not sensitive to extreme values

Outliers

- A data value that is very different from most of the data.
- The median is referred to as a robust estimate of location since it is not influenced by outliers (extreme cases) that could skew the results.

  

```Python
from scipy.stats import trim_mean

state = pd.read_csv('state.csv')
state['Population'].mean()

# trim_mean from scipy.stats
trim_mean(state['Population'], 0.1)
state['Population'].median()

# Weighted mean is available with NumPy. For weighted median, we use the specialized package wquantiles
np.average(state['Murder.Rate'], weights=state['Population'])
wquantiles.median(state['Murder.Rate'], weights=state['Population'])
```

  

  

# Estimates of Variability

Location is just one dimension in summarizing a feature. A second dimension, variability, also referred to as dispersion, measures whether the data values are tightly clustered or spread out.

  

Deviations

- The difference between the observed values and the estimate of location
- errors, residuals

  

Variance

- The sum of squared deviations from the mean divided by n-1 where n is the number of data values
- mean-squared error

![[/Untitled 3 3.png|Untitled 3 3.png]]

formula for variance where x-bar is the sample mean.

  

Standard Deviation

- The square root of the variance
- is much easier to interpret than the variance since it is on the same scale as the original data.
- It might seem peculiar that std deviation is preferred in statistics over the mean absolute deviation. It owes it to statistical theory: mathematically working with squared values is much more convenient than absolute values, especially for statistical models.

![[/Untitled 4 2.png|Untitled 4 2.png]]

  

Mean absolute deviation

- The mean of the absolute values of the deviations from the mean.
- 11-norm, Manhattan norm

![[/Untitled 5 2.png|Untitled 5 2.png]]

  

Median absolute deviation from the median

- The median of the absolute values of the deviations from the median.

Neither the variance, the std deviation, nor the mean absolute deviation is robust to outliers and extreme values. The variance and standard deviation are especially sensitive to outliers since they are based on the squared deviations.

This is a more robust estimate of variability where m is the median.

![[/Untitled 6 2.png|Untitled 6 2.png]]

Range

- the difference between the target and the smallest value in a data set.
- extremely sensitive to outliers

  

Order statistics

- Metrics based on the data values sorted from smallest to biggest.
- a different approach to estimating dispersion. It is based on sorted or ranked data.

  

Percentile

- THe value such that P percent of the values take on this value or less and (100-P) percent take on this value or more

  

Interquartile Range

- the difference between the 75th percentile and the 25th percentile.

  

```Python
state['Population'].std()
```

  

  

# Exploring the Data Distribution

  

Boxplot

- A plot introduced by Tukey as a quick way to visualize the distribution of data.
- Percentiles are also valuable for summarizing the entire distribution. Especially for summarizing the tails of the distribution.

```Python
state['Murder.Rate'].quantile([0.05, 0.25, 0.5, 0.75, 0.95])
```

  

pandas provides a number of basic exploratory plots for data frame, one of them is bloxplots.

  

```Python
ax = (state['Population']/1_000_000).plot.box()
ax.set_ylabel('Population (millions)')
```

Frequency Table

- A tally of the count of numeric data values that fill into a set of intervals (bins)

```Python
binnedPopulation = pd.cut(state['Population'], 10)
binnedPopulation.value_counts()
```

  

Histograms

- A plot of the frequency table with the bins on the x-axis and the count on y.

```Python
ax = (state['Population'] / 1_000_000).plot.hist(figsize=(4, 4))
ax.set_xlabel('Population (millions)')
```

Density plot

- A smoothed version of the histogram, often based on a kernel density estimate.

  

# Exploring Binary and Categorical Data

  

Mode

- The most commonly occurring category or value in a data set

  

Expected Value

- When the categories can be associated with a numeric value, this gives an average value based on a category’s probability of occurrence.

  

Bar charts

- the frequency or proportion for each category plotted as bars

```Python
ax = dfw.transpose().plot.bar(figsize=(4, 4), legend=False)
ax.set_xlabel('Cause of delay')
ax.set_ylabel('Count')
```

Pie charts

- The frequency or proportion for each category plotted as wedges in a pie.

  

  

# Data and Sampling Distributions

![[/Untitled 7 2.png|Untitled 7 2.png]]

  

## Random Sampling and Sample bias

  

A **sample** is a subset of data from a larger set, the **population**. A population is a large, defined set of data.

Random sampling is a process in which each available member of the population being sampled has an equal chance of being chosen for the sample at each draw. The sample that results is called a simple random sample.

Sampling can be dowe with replacement or without replacement.

Data quality often matters more than data quantity when making an estimate or a model based on a sample. Data quality in data science involves completeness, consistency of format, cleanliness, and accuracy of individual data points. Statistics adds the notion of representativeness.

Bias is a systematic error. Sample bias is a sample that misrepresents the population.

  

Self-Selection Sampling Bias. The reviews of restaurants, hotels, cafe,s and so one are prone to bias because the people submitting them are not randomly selected; rather, they themselves have taken the initiative to write. The people motivated to write reviews may have had poor experiences, may have an association with the establishment, or may simply be a different type of person from those who do not write reviews.

  

## Bias

Statistical bias refers to measurement or sampling errors that are systematic and produced by the measurement or sampling process. An important distinction should be made between errors due to random chance and errors due to bias.

An unbiased process will produce errors, but it is random and does not tend strongly in any direction.

Bias comes in different forms, and may be observable or invisible.

  

  

## Random Selection

Random sampling. Proper definition of an accessible population is key. Then we need to specify a sampling procedure.

In stratified sampling, the population is divided up into strata, and random samples are taken from each stratum.

  

## Sample Mean vs. Population Mean

Symbol x-bar is used to represent the mean of a sample from a population whereas μ is used to represent the mean of a population.

Information about samples is observed, and information about large populations is often inferred from smaller samples. Statisticians like to keep the two things separate in symbology.

  

## Selection Bias

> If you don’t know what you’re looking for, look hard enough and you’ll find it  
> - Yogi Berra  

  

Selection bias refers to the practice of selectively choosing data—consciously or unconsciously—in a way that leads to a conclusion that is misleading or ephemeral.

Data snooping is the extensive hunting through data in search of something interesting.

  

## Regression to the Mean

- refers to a phenomenon involving successive measurements on a given variable: extreme observations tend to be followed by more central ones. Attaching special focus and meaning to the extreme value can lead to a form of selection bias.
- “Rookie of the year” is an example.

  

Specifying a hypothesis and then collecting data following randomization and random sampling principles ensures against bias.

  

  

## Sampling Distribution of a Statistic

  

Sampling distribution of a statistic refers to the distribution of some sample statistic over many samples drawn from the same population.

  

Sample Statistic

- a metric calculated for a sample of data drawn from a larger population.
- Example is the mean

  

Data distribution

- the frequency of distribution of individual values in a data set.

  

Sampling distribution

- The frequency distribution of a sample statistic over many samples or resamples.

  

Central Limit Theorem

- the tendency of the sampling distribution to take on a normal shape as sample size rises, even if the source population is not normally distributed, provided that the sample size is large enough and the departure of the data from normality is not too great.

  

Standard Error

- the variability (standard deviation) of a sample statistic over many samples.

![[/Untitled 8 2.png|Untitled 8 2.png]]

  

  

## The Bootstrap

- One easy and effective way to estimate the sampling distribution of a statistic, or of

  

  

  

  

# Binomial Distribution

  

Yes/no outcomes lie at the heart of analytics. Buy/don’t buy, click/don’t click, subscribe/don’t subscribe. Central to understanding the biomial distribution is the idea of a set of trials, each trial having two possible outcomes with definite probabilities.

  

Flipping a coin 10 times is a binomial experiment with 10 trials, each trial having two possible outcomes. These are binary outcomes, and they need not have 50/50 probabilities. Any probabilities that sum to 1.0 are possible.

It is conventional in statistics to term the “1” outcome the success outcome. It is also common practice to assign “1” to the more rare outcome.

  

Binomial trial

- A trial with two outcomes
- Bernoulli trial

  

Binomial distribution

- Distribution of number of successes in x trials.
- Bernoulli distribution
- The binomial distribution is the frequency distribution of the number of successes x in a given number of trials n with specified probability p of success in each trial.

  

```Python
from scipy.stats import binom

# probability of observing x=2 successes in size=5 trials where the prob
# of success for each trial is p=0.1
binomial_dist = binom.pmf(2, 5, 0.1)

# probability of x or fewer successes in n trials.
binomial_dist2 = binom.cdf(2, n=5, p=0.1)
print(binomial_dist)
print(binomial_dist2)
```

  

The mean of a binomial distribution is n x p. It can also be thought of as the expected number of successes in n trials, for success probability = p

  

A binomial trial is an experiment with two possible outcomes: one with probability p and other with probability 1-p

  

![[/Untitled 9 2.png|Untitled 9 2.png]]

the n choose x is just really the binomial coefficient formula.