---
title: 'Properties of Returns and Volatility'
date: '2025-01-17'
categories: ['Probability','Statistics','Monthly','Returns','Volatility']
use_math: True
math: True
---

## Propeties of Returns and Volatility
January 2025


#### **Introduction**

This is the first of twelve (at minimum) posts that I'll be doing this year. Most of which will come around months end. 

In this first post I thought we'd look at some propeties or so called 'stylized' facts of both returns and volatility. Of course, code will be included on my github - see the link below. 
This posts takes insipiration from Chapter 2 of Elements of Quantitative Investing (EQI) by Gappy, which I have recently been working my way through. In addition, many of the ...



### **Returns**

We define the return $$r(t)$$ as the log return. That is, 

<p align="center">
$$
r(t) = \log\left(\frac{P_t}{P_{t-1}}\right) = \log(P_t) - \log(P_{t-1})
$$
</p>

#### 1. Lagged Auto-correlations are small. 

Lets first take a look at the auto-corelation plots for some fairly common equity names. Note that here were looking at daily log-returns.

**Log Return ACF**

![Log Return ACF](assets/images/log-return-acf.png)

It seem fairly logical that returns have little auto-correlation among them, but this doesn't mean returns are not predictable or independent - zero correlation does not imply independence.

#### 2. Heavy Tails

The heavy tailed nature of returns is perhaps the most well known properties of returns. A major assumption (and/or flaw) of traditional finance models is that returns are normally distributed. We can empirically show that log returns are infact leptokurtic (they have heavier tails than a normal distribution). 

| Stock  | Skew (Mean)   | Skew (lower CI)     | Skew (upper CI)     | Kurtosis (Mean) | Kurtosis (lower CI) | Kurtosis (upper CI) |
|--------|---------------|---------------------|---------------------|-----------------|---------------------|---------------------|
| AAPL   | -0.2          | -0.8                | 0.4                 | 5.3             | 2.8                 | 8.6                 |
| IBM    | -0.7          | -1.7                | 0.3                 | 10.2            | 5.9                 | 15.9                |
| SPY    | -0.8          | -2.1                | 0.5                 | 13.0            | 4.8                 | 22.9                |
| TSLA   | -0.1          | -0.6                | 0.4                 | 4.4             | 2.9                 | 6.2                 |

#### 3. Autocorrelations of Absolute and Square Returns

Another property of empircal return data is the high autocorrelations of absolute log returns and square log returns. The presence of autocorrelations between square returns and absolute returns can attributed to volatility clusteirng,
which will be covered later. 


**Absolute Log Returns** 
![Absolute Log Return ACF](assets/images/abs-log-return-acf.png)


**Square Log Returns**
![Square Log Return ACF](assets/images/square-log-return-acf.png)


#### 4. Aggregational Gaussinaity

This property of empirical returns is that, when returns are calculated at long horizons, they begin to approach normal distributions. As was pointed out in *2*, daily returns to 
not follow strictly normal distributions given their heavy-tailed nature. When taking returns at longer horizons, the normal distribution might be a better approximation. In the below graph, I show the distributions of weekly, daily and monthly log returns.

**Log Return Histograms**
![Log Return Histograms](assets/images/log-return-dist.png)

While we can see the approximate change in the histograms, we can make this more precise by applying the Kolmogorov-Smirnov Test. This statistical test allows one to compare
a given distrbution to a sample. In our case, the reference distribution is the normal distribution.

The null-hypothesis of the test (in our setting) is that the data follows a normal distribution, the alternative being it does not follow a normal distribution. The results are shown in the table below, categorized by frequency and ticker.

**Kolmogorov-Smirnov Test Results**

|   Ticker   | Daily - Statistic | Daily - p-value | Weekly - Statistic | Weekly - p-value | Monthly - Statistic | Monthly - p-value |
|-------|-------------------|-----------------|--------------------|------------------|---------------------|-------------------|
| **AAPL** | 0.0773            | 0.0             | 0.0535             | 0.0968           | 0.0800              | 0.4046            |
| **IBM**  | 0.0887            | 0.0             | 0.0781             | 0.0033           | 0.0639              | 0.6866            |
| **SPY**  | 0.1065            | 0.0             | 0.0811             | 0.0019           | 0.1100              | 0.1013            |
| **TSLA** | 0.0652            | 0.0             | 0.0479             | 0.1765           | 0.0574              | 0.8021            |

Using a signifigance threshold of 0.05, we see that we'll reject the null hypothesis for all daily return distributions, fail reject the null hypothesis for the monthly distributions
and some mixed results for the weekly returns. These results both confirm that our daily returns are not normally distributed but our monthly returns are normally distribution according to this test.

### **Volatility**

We'll next cover a couple properties of empircal volatility data, which of course goes hand in hand with the properties of returns.

#### 1. Volatility Clustering 

One of the most observable and perhaps well known empircal facts of return volatility is known as volatility clustering. This is the idea that there are periods of both high and low volatility in the market.
We can observe this graphically by looking at the time series of return data.

**Log Return Time Series**

![Log Return Time Series](assets/images/log-return-time-series.png)

Its clear in all of the above graphs there are "clusters" of high and low volatilities. We could also see this by looking at both the absolute value or squared returns.

**Squared Log Return Time Series**

![Squared Log Return Time Series](assets/images/square-log-return-time-series.png)


**Absolute Log Return Time Series**

![Absolute Log Return Time Series](assets/images/abs-log-return-time-series.png)

The autocorrelation plots that we saw in **2** of returns should hopefully convince you this property - if not, lets make it more rigorous. To do this, we can look to the ARCH-LM test,
which tests for the presence of heteroskedacity (non constant-variance) in time series data. The results are shown in the table below.


|  Ticker   | Log Returns - Statistic | Log Returns - p-value | Absolute Log Returns - Statistic | Absolute Log Returns - p-value | Squared Log Returns - Statistic | Squared Log Returns - p-value |
|-----------|-------------------------|-----------------------|----------------------------------|-------------------------------|---------------------------------|------------------------------|
| **AAPL**  | 399.5481                | 0.0                   | 399.5481                         | 0.0                           | 425.3867                        | 0.0                          |
| **IBM**   | 309.2900                | 0.0                   | 309.2900                         | 0.0                           | 220.2913                        | 0.0                          |
| **SPY**   | 962.3074                | 0.0                   | 962.3074                         | 0.0                           | 820.3418                        | 0.0                          |
| **TSLA**  | 144.7202                | 0.0                   | 144.7202                         | 0.0                           | 80.1797                         | 0.0                          |

In simple words, the null hypothesis of the ARCH-LM test is that there is no heteroskedacity present, ie the time series has constant variacne. We can see across the board, using a signifigance level of 
0.05, we would reject the null hypothesis. That is, our variance is not constant across the time series, due to the volatility clustering.

#### 2. The Leverage Effect

The leverage effect suggest that returns and measures of volatility are negatively correlated. To asses this, we look at the correlations between raw returns and use lagged absolute returns as a measure
of volatility. 


**Volatility and Return Correlations**
![Volatility and Return Correlation](assets/images/corr-return-vol-scatter.png)


The above graph shows the correlations between returns and lagged volatility. We can see that in our sample, while some are negative, as is suggested,  there are also a few positive correlations that are observable. 
More importantly, from a statistical point of view, these correlations are near zero and thus perhaps not meaningful. In the original paper, *INSERT HERE*, they assert that this effect is fairly strong but as we can see in our data,
and the study performed by *INSERT HERE - NAME* in *INSERT HERE YEAR*, they also find negilgible effect. 

#### 3. Volatility and Volume

The idea that volatility and volume are positively correlated. 




### **Motivation for GARCH**

We covered some fairly well known properties of both returns and volatility. One might notice that many of the properties we discussed above are incredibly problematic for 
classical statistical models. We have non-normal distributions, possibly residuals, non-constant variance, signifigant autocorrelations and non-stationary data. This violates key assumptions of 
many linear models such as simple linear regression and simple time series models such as AR, MA or ARMA models. So what do we do?

There is of course, as given away in the title of this section a classical solution to many of these properties which improves the statistical properties of returns and hence
makes them more susceptible to modelling.

> *Note:*  GARCH solves financial time series problems, not *all* problems.

#### Beginging with ARMA and ARCH.

Before we jump immediately to our solution, it's important to provide some rational for *why* the GARCH model is a good choice. Sure, we could look at the results and then justify our choice, but I believe
it is better to understand why the othermodels are *not* as good of a choice.

##### 1. ARMA

First lets start with ARMA, the auto-regressive moving average model. What does this model say? It says that our returns are governed by a model that contains a moving average piece and auto regressive piece.
This is given by the following equation. 

$$
r_t = \phi_0 + \sum_{i=1}^p \phi_i r_{t-i} + \sum_{j=1}^q \theta_j \epsilon_{t-j} + \epsilon_t,
$$

where:
- $$r_t$$ is the return at time $$t$$,
- $$\phi_0$$ is the intercept term,
- $$\phi_i$$ are the coefficients of the AR terms ($$r_{t-i}$$, lagged returns),
- $$\theta_j$$ are the coefficients of the MA terms ($$\epsilon_{t-j}$$, lagged error terms),
- $$\epsilon_t$$ is white noise (mean zero, constant variance).

The **AR** terms model the dependence of $$r_t$$ on its own past values, while the **MA** terms model the dependence of $$r_t$$ on past error terms. A key assumption of this model is that
it assume stationary time series and constant variance. Let's fit model and see how this assumption breaks down.


##### 2. ARCH

Next, let's move on to ARCH, the **Autoregressive Conditional Heteroskedasticity** model. Unlike ARMA, which assumes constant variance, ARCH explicitly models the time-varying variance (volatility) of returns. The ARCH model is given by the following equations:

$$
r_t = \mu + \epsilon_t, \quad \epsilon_t \sim N(0, \sigma_t^2),
$$

$$
\sigma_t^2 = \alpha_0 + \sum_{i=1}^q \alpha_i \epsilon_{t-i}^2,
$$

where:
- $$r_t$$ is the return at time $$t$$,
- $$\mu$$ is the mean return,
- $$\epsilon_t$$ is the error term at time $$t$$, assumed to be normally distributed with zero mean and variance $$\sigma_t^2$$,
- $$\sigma_t^2$$ is the conditional variance at time $$t$$,
- $$\alpha_0$$ is a positive constant,
- $$\alpha_i$$ are the coefficients of the lagged squared error terms ($$\epsilon_{t-i}^2$$, past shocks).

The key feature of ARCH is that it captures **volatility clustering**, where periods of high volatility are followed by high volatility, and periods of low volatility are followed by low volatility. 
However, ARCH models can struggle when capturing more complex patterns of volatility. This is where GARCH models extend ARCH by including lagged conditional variance terms.


##### 3. GARCH

##### 3. GARCH

Our final model, as hinted to, is **GARCH** (Generalized Autoregressive Conditional Heteroskedasticity). GARCH extends the ARCH model by incorporating lagged values of the conditional variance into the model. It is given by the following equations:

$$
r_t = \mu + \epsilon_t, \quad \epsilon_t \sim N(0, \sigma_t^2),
$$

$$
\sigma_t^2 = \alpha_0 + \sum_{i=1}^q \alpha_i \epsilon_{t-i}^2 + \sum_{j=1}^p \beta_j \sigma_{t-j}^2,
$$

where:
- $$r_t$$ is the return at time $$t$$,
- $$\mu$$ is the mean return,
- $$\epsilon_t$$ is the error term at time $$t$$, assumed to be normally distributed with zero mean and variance $$\sigma_t^2$$,
- $$\sigma_t^2$$ is the conditional variance at time $$t$$,
- $$\alpha_0$$ is a constant (must be positive),
- $$\alpha_i$$ are the coefficients of the lagged squared error terms $$\epsilon_{t-i}^2$$,
- $$\beta_j$$ are the coefficients of the lagged conditional variance terms $$\sigma_{t-j}^2$$.

The **GARCH** model is more flexible than ARCH because:
- It captures volatility clustering (like ARCH),
- It accounts for the persistence of volatility through the lagged variance terms $$\sigma_{t-j}^2$$.

Lets now fit a GARCH model and see how it stacks up to the previous models, and what properties of returns it improves. 

