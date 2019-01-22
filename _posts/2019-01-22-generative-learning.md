---
layout: post
comments: true
title: "[CS229] Descriminative Learning v.s. Generative Learning Algorithm"
date: "2019-01-22 14:30:00"
category: [Notes]
tag: [machine learning, cs229]
---

> Generative Learning Algorithms:
> - the origin of the different ideas, 
> - the definitions,
> - more about generative learning algorithms.



![](/public/img/20190122-bayes-theorem.jpg)

<!--more-->

## 1. An Example to Explain the Initiative Ideas:
Consider a classification problem in which we want to learn to distinguish between cats $$(y=0)$$ and dogs $$(y=1)$$:

#### **Approach I:** 

Based on some features of an animal, given a training set, an algorithm like ***logistic regression*** or ***perceptron*** algorithm, tries to find a **decision boundary** (e.g. a straight line) to separate the cats and the dogs. 

**To classify a new animal:** 

Just check on which side of the decision boundary it falls.

#### **Approach II:**

First, look at cats, build a model of **what cats look like**. Then, look at dogs, build another model of **whawt dogs look like**.

**To classify a new animal:**
    
Match the new animal against the cat model and the dog model respectively, to see whether the new animal looks more like the cats or more like the dogs we had seen in the training set.

## 2. Definitions:
#### **Discriminative** learning algorithms:

Algorithms that try to learn $$p(y \vert x)$$ directly (such as logistic regression) or algorithms that try to learn mappings directly from the space of input $$chi$$ to the labels $$\{0,1\}$$ (such as perceptron algorithm) are called **discriminative** learning algorithms.

#### **Generative** learning algorithms:
    
Algorithms that instead try to model $$p(x \vert y)$$ and $$p(y)$$ are called **generative** learning algorithms.
    
## 3. More about Generative Learning Algorithms:

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


<br><br>***KF***
