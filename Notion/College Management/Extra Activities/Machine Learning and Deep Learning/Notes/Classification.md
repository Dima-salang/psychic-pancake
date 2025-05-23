---
Created: 2024-03-16T00:50
Type: Lecture
Materials:
  - "[[Chapter_2.pdf]]"
Reviewed: true
---
In many situations, the response variable is qualitative. Often, qualitative variables are referred to as **categorical.**

  

> [!important] Predicting a qualitative response for an observation is classification.

- It is referred to as classifying that observation, since it involves assigning the observation to a category, or class.
- On the other hand, often the methods used for classification first predict the probability that the observation belongs to each of the categories of a qualitative variable, as the basis for making the classification.

  

There are at least two reasons not to perform classification using a regression method

- a regression method cannot accommodate a qualitative response with more than two classes
- a regression method will not provide meaningful estimates of even with just two classes

$$Pr(Y|X)$$

  

## Logistic Regression

- logistic regression models the probability that Y belongs to a particular category.