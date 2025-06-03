---
title: 'Eigenportfolios'
description: >-
  A nice connection between eigenvalues, eigenvectors and the market
date: '2024-12-20'
categories: ['Factor Models']
use_math: True
math: True
---

### Introduction

Eigenvalues and eigenvectors are fundamental concepts in linear algebra, appearing frequently in both statistics and machine learning, one key example being Principal Component Analysis (PCA). But what is an eigenportfolio, and how does it relate to these mathematical ideas? At first glance, portfolios and eigenvectors might seem unrelated, yet they share a somewhat interesting (and maybe expected) connection.

In this post, I’ll provide a concise yet insightful overview of eigenportfolios; what they are, how to construct them, and an interesting (or perhaps unsurprising) link to familiar concepts. Main sources listed here:

- (1) Avellaneda, M., & Lee, J.-H. (2008). Statistical Arbitrage in the U.S. Equities Market. arXiv. https://arxiv.org/abs/0807.1551
- (2) Aldridge, I., & Avellaneda, M. (2021). Chapter 6. In Big Data Science in Finance. John Wiley & Sons, Incorporated.

### Eigenvalues and Eigenvectors

Let's first review briefly what eigenvalues and eigenvectors are; and how you can find them. The eigenvectors $$x$$ and eigenvalues $$\lambda$$ of a matrix $$A$$ satisfy the following relation.

$$
Ax = \lambda x
$$

In words, this means that when we apply the matrix $$A$$ to $$x$$, it simply gets multiplied by a constant, and that constant,$$\lambda$$, is its corresponding eigenvalue.

#### Solving for them (1)

One might remember a common way to solve for eigenvalues is by solving the *characteristic polynomial*, which is obtained by solving the following equation:

$$
det|A - \lambda I| = 0
$$

This is fairly straightforward, however, if we know some special properties of our matrix, then we can actually solve "easier" equations instead. This may not seem relevant for low dimensional computation, but quickly becomes important at higher dimensions, which is where we're headed (unless?).

#### A smarter way 

Let's assume that our matrix $$A$$ is nice, and by nice, I mean that its ***positive semi-definite*** (PSD). In math terms this means that;

- For all $$x$$, $$x^{T}Ax \ge 0$$  
- All eigenvalues of $$A$$ are positive

Now, since this is the case, we can perform ***singular value decomposition*** on $$A$$. SVD, is a matrix factorization technique, where we re-write $$A$$ as 

$$
A = U\Lambda U^{T}
$$

where 
- $$U$$ is an orthogonal matrix
- $$\Lambda$$ is a diagonal matrix of eigenvalues

You'll have to trust me, and all the research that has gone this type of computation, that solving for this decomposition, is much faseter than solving our original equation $$1$$.

#### The covariance matrix is PSD

An important result, and one that we'll show here is that the covariance matrix is actually PSD. Let $$X$$ be a matrix of random variables, and let $$\Sigma$$ be our corresponding covariance matrix.

Then we want to show that 

$$
x^{T}\Sigma x \ge 0 \ \forall x \in R^{n}
$$

Then we apply the definition of the covaraince matrix

$$
\begin{align}
x^{T}\Sigma x & = x^{T}E[(X-E[X])(X-E[X])^{T}]x \\
& = E[x^{T}(X-E[X])(X-E[X])^{T}x] \\
& = E[(x^{T}(X-E[X]))^2] 
\end{align}
$$

We know that the expectation of a squared quantity is always positive, thus we have:

$$
x^{T}\Sigma x \ge 0 \ \forall x \in R^{n}
$$

This is what we wanted to show, $$\Sigma$$ is PSD.

### Constructing Portfolios

Since our covariance matrix is non-negative, this allows us to use tools such as PCA to decompose the factors that explain the variance within our dataset. Now, lets apply this to some real data. Below, I've just used the industry ETFs, with a date range of 2024-01-01 to 2025-01-01. 

The weights for each of the ETF's on the first two principal components are shown below.

![PCA Loadings](assets/images/eigenportfolio/pca_loadings.png)

The first principal component explains 62% of the variance within our data, as measured by the ratio of its eigenvalue to the sum of eigenvalues. Avellaneda and Lee (1) then define an eigenportfolio as a portfolio which is weighted by the eigenvector loadings normalized by the individual asset return volatility. This gives:

$$
w_i = \frac{v_i}{\sigma_i}
$$

The performance of the first eigenportfolio, and the market, which is proxied as the S&P500 is shown below. 

![Portfolio Returns](assets/images/eigenportfolio/ep-vs-market.png)


So the volatility normalized weights on the first eigenvector, essentially represents the market. Maybe this is surprising or not.

### Conclusion

The existence of these eigenportfolios is, in my opinion, somewhat interesting but more so a reflection of the underlying structure of the market, and the mathematics that we have used to arrive at this result. 

Additionally, while the eigenportfolios themselves aren't necessarily something that you would be interested in trading, you could imagine how they might be used for other cases - for example constructing hedging strategies against a market index but using these as a proxy. We'll perhaps leave that for another time. Thanks for reading.