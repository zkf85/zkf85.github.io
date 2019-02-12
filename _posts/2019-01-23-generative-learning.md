---
layout: post
comments: true
title: "[CS229] Descriminative Learning v.s. Generative Learning Algorithm"
date: "2019-01-23 16:30:00"
category: [Notes]
tag: [machine learning, cs229]
---

> A Review of Generative Learning Algorithms.
>
> Keep Updating:
> - 2019-01-22 Add Part 1 
> - 2019-01-23 Add Part 2, Gausian discriminant analysis


![](/public/img/20190122-bayes-theorem.jpg)

<!--more-->

* TOC
{:toc}

## 1. Basics of Generative Learning Algorithms

*(01/22/2019)*

### 1.1. An Example to Explain the Initiative Ideas:
Consider a classification problem in which we want to learn to distinguish between cats $$(y=0)$$ and dogs $$(y=1)$$:

#### **Approach I:** 

Based on some features of an animal, given a training set, an algorithm like ***logistic regression*** or ***perceptron*** algorithm, tries to find a **decision boundary** (e.g. a straight line) to separate the cats and the dogs. 

**To classify a new animal:** 

Just check on which side of the decision boundary it falls.

#### **Approach II:**

First, look at cats, build a model of **what cats look like**. Then, look at dogs, build another model of **whawt dogs look like**.

**To classify a new animal:**
    
Match the new animal against the cat model and the dog model respectively, to see whether the new animal looks more like the cats or more like the dogs we had seen in the training set.

### 1.2. Definitions:
#### **Discriminative** learning algorithms:

Algorithms that try to learn $$p(y \vert x)$$ directly (such as logistic regression) or algorithms that try to learn mappings directly from the space of input $$chi$$ to the labels $$\{0,1\}$$ (such as perceptron algorithm) are called **discriminative** learning algorithms.

#### **Generative** learning algorithms:
    
Algorithms that instead try to model $$p(x \vert y)$$ and $$p(y)$$ are called **generative** learning algorithms.
    
### 1.3. More about Generative Learning Algorithms:

Continue with the example of cats and dogs, if $$y$$ indicates whether an sample is a cat (0) or a dog (1), then 
- $$p(x \vert y=0)$$ models the distribution of cats' features.
- $$p(x \vert y=1)$$ models the distribution of dogs' features.

After modeling $$p(y)$$ (called the **class priors**) and $$p(x \vert y)$$, our algorithm can then use **Bayes Rule** to derive the posterior distribution on $$y$$ given $$x$$:

$$ p(y \vert x) = \frac{p(x \vert y)p(y)}{p(x)}. $$

Here, the denominator is given by $$p(x)=\sum_i p(x \vert y_i)p(y_i)$$. In cats & dogs example, $$p(x)=p(x \vert y=0)p(y=0)+p(x \vert y=1)p(y=1)$$.

Actually, if we we're calculating $$p(y \vert x)$$ in order to make a prediction, then we don't need to calculate the denominator, since


$$
\begin{align}
\arg\underset{y}{\max} p(y \vert x) & = \arg \underset{y}{\max} \frac{p(x \vert y)p(y)}{p(x)} \\
& = \arg \underset{y}{\max} p(x \vert y)p(y).
\end{align}
$$

## 2. Gaussian Discriminant Analysis (GDA) 

*(01/23/2019)*

In this model (GDA), we assume that $$p(x \vert y)$$ is distributed according to a **multivariate normal distribution**. First let's talk briefly about the properties of multivariate normal distributions.

### 2.1. The multivariate normal distribution
The Definition of so-called *multivariate normal distribution*:

- A distribution with a **mean vector** $$\mu \in \mathbb{R}^n$$ and a **covariance matrix** $$\Sigma \in \mathbb{R}^{n \times n}$$, where $$\Sigma \geq 0$$ is symmetric and positive semi-definite. Also written "$$\mathcal{N}(\mu, \Sigma)$$", it's density is given by:

$$
p(x; \mu, \Sigma) = \frac{1}{(2\pi)^{n/2}|\Sigma|^{1/2}}\exp\left(-\frac{1}{2}(x-\mu)^T\Sigma^{-1}(x-\mu)\right).
$$

where "$$\vert\Sigma\vert$$" denotes the determinant of the matrix $$\Sigma$$.

- For a random variable $$X$$ distributed $$\mathcal{N}(\mu, \Sigma)$$, the mean is given by $$\mu$$:

$$
\text{E}[X] = \int_x x\ p(x;\mu,\Sigma)dx = \mu
$$

- The **covariance** of a vector-valued random variable $$Z$$ is defined as $$\text{Cov}(Z) = \text{E}[(Z-\text{E}[Z])(Z-\text{E}[Z])^T]$$, which can also be written as $$\text{Cov}(Z) = \text{E}[ZZ^T]-(\text{E}[Z])(\text{E}[Z])^T$$. If $$X \sim \mathcal{N}(\mu, \Sigma)$$, then:

$$
\text{Cov}(X) = \Sigma.
$$

#### //TODO - Practice:

### 2.2. The Gaussian Discriminant Analysis model

## Reference 
1. [>> Stanford CS229 Lecture Note Part IV - Generative Learning Algorithms](https://see.stanford.edu/materials/aimlcs229/cs229-notes2.pdf)

<br><br>***KF***
