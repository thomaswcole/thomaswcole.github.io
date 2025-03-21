---
title: 'Estimating the covariance matrix - how hard is it?'
date: '2025-03-19'
categories: ['Monthly']
use_math: True
math: True
---

March 2025

### **Introduction**

We're back again for another post in this year long series, which as of this month is now 1/4th complete. Again, you can catch up on the most recent post [here](https://thomaswcole.github.io/posts/risk-measures/), and find the others in the monthly folder [here].

In this post we'll be looking at covariance matrix estimation - the problems that are commonly encountered and also some well known solutions to this. Near the end, we'll look at how better estimation can lead to more robust results in traditional mean-variance optimization problems. 

As usual, references below and all code can be found on my github.

### **The Covariance Matrix**

Covariance, and as a result the covariance matrix are central topics within both probability and statistics theory but also key to many financial models, wether they be used to measure risk or as inputs to an trading signal. Throughout the post we'll use the notation **$$\Sigma$$** to denote a $$n \times n$$. 

Let's first start with a couple important properties.

1) **Symmetric**: The covariance matrix is symmetric, that is $$\Sigma = \Sigma^T$$ 

2) **Positive Semi-Definite**: The covariance matrix is PSD, that is for all $$v \in R^n$$, $$v\Sigma v \ge 0$$ 

3) **Scaling Property**: If $$X_{1\times n}$$ is a random vector with covariance matrix $$\Sigma$$, then $$Cov(AX) = A\Sigma$$, where $$A_{1\times n}$$

#### **Naive Estimation**

Now that we've covered some basics about the matrix itself - how do you actually obtain it in practice? A naive approach would just utilize all of the data you have, and utilize the sample covariance estimators that you learn in an introductory statistics course.

As a reminder, let $$S$$ be our sample covariance matrix:

$$
S = \frac{1}{n-1} \sum_{k=1}^{n} (X_k - \bar{X}) (X_k - \bar{X})^T
$$

The idea of including all available data seems intuitive, right? Since the sample covariance matrix is an unbiased estimator, we know that as the number of observations increases, our estimate converges to the true population parameter. As we know, however, this isn't really what people do in practice or academically. The key, but implicit assumption that we made here was that all $$X_k$$ were drawn from the same distribution and also are stationary. Financial time series are not stationary - this is our first issue. 


#### **Types of Shrinkage**


### **Mean-Variance Optimization**

