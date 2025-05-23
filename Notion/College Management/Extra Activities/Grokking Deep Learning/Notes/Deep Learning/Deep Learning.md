---
Created: 2023-10-05T00:46
Type: Lecture
Materials:
  - "[[Chapter_1.pdf]]"
Reviewed: true
---
> [!important] Deep learning is a subset of ML. It is used to solve practical tasks such as computer vision, NLP, and text-to-speech. Deep learning is a subset of methods in the machine learning toolbox, primarily using artificial neural networks, which are a class of algorithm loosely inspired by the human brain.

> [!important] Machine learning is a subfield of computer science wherein machines learn to perform tasks for which they were not explicitly programmed. In short, machines observe a pattern and attempt to imitate it in some way that can be either direct or indirect.

  

# **Supervised Learning**

Supervised learning is a method for transforming one dataset into another. **The ML algorithm is attempting to imitate the pattern between the two datasets in such a way that it can use one dataset to predict the other. It is the bread and butter of applied AI.**

![[/Untitled 18.png|Untitled 18.png]]

  

# Unsupervised Learning

Same as supervised learning but the dataset that it transforms into is not **previously known or understood**. An example of this is clustering a dataset into groups. Clustering transforms a sequence of datapoints into a sequence of cluster labels.

> [!important] **Even though there are many forms of unsupervised learning, all forms of it can be viewed as a form of clustering.**

![[/Untitled 1 7.png|Untitled 1 7.png]]

  

# Parametric vs. Nonparametric Learning

Basically, **trial-and-error learning vs. counting and probability.**

Whereas the previous section on supervision is about the type of pattern being learned, parametricism is about the way the learning is stored and often, by extension, the method for learning.

> [!important] A parametric model is characterized by having a fixed number of parameters, whereas a nonparametric model’s number of paramters is infinite (based on data).

- Imagine babies jamming a square peg everywhere until they get it correct. However, a teenager will count the sides.

  

# Supervised Parametric Learning

Basically, trial-and-error learning using knobs.

Supervised learning machines are machines with a fixed number knobs, where in the learning occurs by turning the knobs. Input data comes in, is processed based on the angle of the knobs, and is transformed into a prediction.

  

Step 1: Predict

- gather data and make a prediction

Step 2: Compare to the truth pattern

- compare the prediction to the correct answer.

Step 3: Learn the pattern

- adjust the knobs to make a more accurate prediction given the input data.

  

# Unsupervised Parametric Learning

It uses knobs to group data. But in this case, it usually has several knobs for each group, each of which maps the input data’s affinity to that particular group.

  

  

# Nonparametric Learning

- Counting-based methods

Nonparametric learning is a class of algorithm wherein the number of parameters is based on data (instead of predefined). This lends itself to methods that generally count in one way or another, thus increasing the number of parameters based on the number of items being counted within the data.

  

  

# Introduction to Neural Prediction: Forward Propagation

![[/Untitled 2 4.png|Untitled 2 4.png]]

The number of datapoints you process at a time has a significant impact on what a network looks like. Then, “How do I choose how many datapoints to propagate at a time?”

> [!important] Always present enough information to the network, where “enough information” is defined loosely as how much a human might need to make the same prediction. This is the shape of the input or the columns, or the number of datapoints we are processing at a time.

- For example, if you want to predict whether an image is a dog. If you only feed it a single pixel, it can’t predict it properly. You need to feed the whole array of pixels.

![[/Untitled 3 4.png|Untitled 3 4.png]]

# A simple neural network

- For now, it’s one or more **weights** that you can multiply by the **input data** to make a **prediction**.

```Python
weight = 0.1
def neutral_network(input, weight):
		prediction = input * weight
		return prediction

def main():
		num_of_toes = [8.5, 9.5, 10, 9]
		input = num_of_toes[0]
		pred = neural_network(input, weight)
		print(pred)
```

  

## What does it do?

- It multiplies the input by a weight. It “scales” the input by a certain amount.

> [!important] A neural network, in its simplest form,, uses the power of multiplication. It takes an input datapoint and multiplies it by the weight.

The interface for a neural network is simple. It accepts an input variable as information and a weight variable as knowledge and outputs a prediction. Every neural network works this way. It uses the knowledge in the weights to interpret the information in the input data. The weights are then scaled depending on the learning.

  

# Making a prediction with multiple inputs

![[/Untitled 4 3.png|Untitled 4 3.png]]

```Python
weights = [0.1, 0.2, 0]
def neural_network(input, weights):
		pred = weighted_sum(input, weights)
		return pred

def weighted_sum(a, b):
		assert(len(a) == len(b))
		output = 0
		for i in range(len(a)):
				output += (a[i] * b[i])
		return output

toes = [8.5, 9.5, 9.9, 9.0]
wlrec = [0.65, 0.8, 0.8, 0.9]
nfans = [1.2, 1.3, 0.5, 1.0]
input = [toes[0],wlrec[0],nfans[0]]
pred = neural_network(input,weights)
```

We have multiple inputs so we sum their respective predictions. Thus, you multiply each input by its respective weight and then sum all the local predictions together. This is called a weighted sum of the input, or a weighted sum. It is also the dot product.

This is why we need vectors. Vectors are just a list of numbers.

Anytime we perform a mathematical operation between two vectors of equal length and have a one-to-one correspondence between the elements, this is an elementwise operation.

  

Loosely stated, a dot product gives a notion of similarity between two vectors.

  

# Converting to NumPy

```Python
import numpy as np
weights = np.array([0.1, 0.2, 0])
def neural_network(input, weights):
	pred = input.dot(weights)
	return pred

toes = np.array([8.5, 9.5, 9.9, 9.0])
wlrec = np.array([0.65, 0.8, 0.8, 0.9])
nfans = np.array([1.2, 1.3, 0.5, 1.0])

input = np.array([toes[0],wlrec[0],nfans[0]])
pred = neural_network(input,weights)
print(pred)
```

  

# Making predictions with multiple outputs using a single input

![[/Untitled 5 3.png|Untitled 5 3.png]]

```Python
weights = [0.3, 0.2, 0.9]
def neural_network(input, weights):
	pred = ele_mul(input, weights)
	return pred

def ele_mul(input, vector):
	output = [0, 0, 0]
	assert(len(output) == len(vector))
	for i in range(len(vector)):
		output[i] = number * vector[i]
	return output

wlrec = [0.65, 0.8, 0.8, 0.9]
input = wlrec[0]
pred = neural_network(input,weight)
print(pred)
```

  

  

# Multiple inputs and outputs

- It performs independent weighted sums of the input to make predictions.

![[/Untitled 6 3.png|Untitled 6 3.png]]

  

# Predicting on predictions

- Neural networks can be stacked. You can take the output of one network and feed it as input to another network.

![[/Untitled 7 3.png|Untitled 7 3.png]]