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

#### **Naive Estimation and Its Challenges**

A natural approach to estimating the covariance matrix is to use all available data and apply the standard sample covariance estimator from introductory statistics. Given a dataset of asset returns $$X_k$$ ($$k = 1, \dots, n$$), the sample covariance matrix is:

$$
S = \frac{1}{n-1} \sum_{k=1}^{n} (X_k - \bar{X}) (X_k - \bar{X})^T
$$

where $$\bar{X}$$ is the mean return vector. Since this estimator is unbiased, it follows that as the number of observations $$n$$ increases, $$S$$ converges to the true covariance matrix.

However, despite its simplicity, this estimator is rarely used directly in practice. The key implicit assumptions which causes issue in this approach are:

1. **Stationarity** – The returns $$X_k$$ are assumed to be drawn from the same distribution over time. In reality, financial time series exhibit time-varying volatility and correlation, violating this assumption.
2. **Curse of Dimensionality** – The number of unique pairwise covariances to estimate grows as $$\frac{N(N-1)}{2}$$. When $$N$$ (the number of assets) approaches or exceeds $$n$$ (the number of observations), the covariance matrix becomes highly noisy and poorly conditioned.

These issues motivate alternative approaches, such as exponentially weighted covariance estimation, shrinkage methods, and factor models**, which aim to improve stability and robustness in high-dimensional settings.

### **Time Varying Correlation Models**

As we already explored, financial time series returns are not stationary, volatility and correlation, change over time. The naive sample covariance estimator assumes a constant covariance structure, which can lead to outdated or misleading estimates. 

To address this issue, two commonly used methods are Rolling Window Estimation and Exponentially Weighted Covariance.

#### **1. Rolling Window Estimation**
Instead of using the entire historical dataset, we estimate the covariance matrix over a fixed-length rolling window of past returns. Given a window size $$W$$, the rolling covariance matrix at time $$t$$ is:

$$
S_t = \frac{1}{W-1} \sum_{k=t-W+1}^{t} (X_k - \bar{X}_W) (X_k - \bar{X}_W)^T
$$

where $$\bar{X}_W$$ is the mean return over the window.

Note that choosing an optimal window length is not trivial. If our window length is too small, then we get noise estimates. If its too large, then it may not capture current conditions. 

#### **2. Exponentially Weighted Covariance (EWC)**

A natural next step to the above is instead of assigning equal weight to all past observations (as in rolling windows), Exponentially Weighted Covariance applies exponentially decreasing weights:

$$
S_t = \sum_{k=1}^{t} \lambda^{t-k} (X_k - \bar{X}_\lambda)(X_k - \bar{X}_\lambda)^T
$$

where $$\lambda$$ is the decay factor (typically **0.94** for daily data, following the RiskMetrics convention). More recent observations receive higher weights, allowing the estimate to adapt to changing market conditions.

Again, as was the case with the previous estimation method, this method is highly sensitive to a "good" the choice of lambda. 

### **Shrinkage**

The second issue we highlighted is commonly address through shrinkage methods such as those introduced by Ledoit and Wolf in 2004. The core idea behind shrinkage is to improve the stability of covariance matrix estimates, in the case where the number of assets ($$N$$) is large relative to the number of observations $$T$$. It does this by shrinking the sample covariance matrix towards a more stable target, often the identity matrix or a constant correlation matrix.

The shrinkage estimator is given by:

$$
S_{\text{shrink}} = \lambda \cdot T + (1 - \lambda) \cdot S
$$

where $$S$$ is the sample covariance matrix, $$T$$ is the target matrix, and $$\lambda$$ is the shrinkage parameter. $$\lambda$$ controls the amount of shrinkage, with $$\lambda = 1$$ giving the target matrix and $$\lambda = 0$$ giving the sample covariance.

### **Mean-Variance Optimization**

To evaluate the effectiveness of different covariance estimation methods, we conduct a simple backtest using mean-variance optimization. Our universe consists of all S&P 500 stocks, covering the period from 2022 to 2025. We allocate the first 500 trading days for covariance estimation, applying each of the methods discussed earlier.  

Expected returns are modeled using the CAPM framework, with the S&P 500 serving as the market index. Given these expected returns, the optimal mean-variance portfolio weights are computed as:  

$$
w^* = \frac{\Sigma^{-1} \mu}{\mathbf{1}^\top \Sigma^{-1} \mu}
$$

where:  
- $$\Sigma$$ is the estimated covariance matrix,  
- $$\mu$$ is the vector of expected returns under CAPM,  
- $$\mathbf{1}$$ is a vector of ones, ensuring proper normalization of weights.  

Finally, we implement these optimized weights and assess portfolio performance over the subsequent 252 trading days. The performance is shown in the plot below.

![Covariance Returns](assets/images/estimating-covariance/portfolio_performance.png)


### **Conclusion**

This analysis demonstrates the impact of different covariance estimation techniques on mean-variance portfolio optimization. Among the methods tested, the shrinkage estimator consistently outperforms the others, delivering more stable and robust portfolio allocations. This aligns with its theoretical advantage of balancing sample information with a structured prior, reducing estimation noise.  

Conversely, the sample covariance estimator performs the worst, suffering from extreme weight allocations due to its instability in high-dimensional settings $$(N \gg T)$$. This result underscores the well-known issue of overfitting when using naive sample estimates in finance.  

Overall, this again highlights the importance of incorporating improved covariance estimation techniques, such as shrinkage, when constructing optimal portfolios in practice.
