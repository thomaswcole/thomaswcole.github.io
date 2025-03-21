---
title: 'Risk Statistics and Risk Models'
date: '2025-02-28'
categories: ['Monthly']
use_math: True
math: True
---

February 2025

### **Introduction**

This post is another installment in my year-long series exploring various topics in quantitative finance. As always, you can expect a new post every month. You can catch up on the first post [here](https://thomaswcole.github.io/posts/returns-and-volatility/). In the previous post (if you missed it), we covered the properties of returns, which are, for obvious reasons, essential to the investment process. After all, everyone values portfolios that generate returns, so it pays to understand them—literally.

For similar reasons, this month I'll focus on measures of risk and how we can evaluate it in our portfolios. I've found it helpful to categorize the evaluation of portfolio risk into two broad approaches: the first being risk statistics, and the second, model-based measures of risk. Risk statistics include metrics such  as volatility, drawdown, maximum drawdown,and so on. On the other hand, model-based measures involve decomposing risk by looking at beta or using statistical or factor models to analyze returns.

For explanatory purposes, I'll use a sample portfolio, which is an equally weighted portfolio of the sector ETF's.

### **Risk Statistics**

As mentioned, we'll first start by covering risk statistics, which are static but common ways to present risk. The three that I highlight below are perhaps the most common (in my view), but of course there are many others. 

#### 1. Volatility 

The most common and perhaps most natural measure of risk is the volatility of returns. As we mentioned in the previous article, there are some notable properties of volatility to be aware of. 
That said, summarizing risk with volatility is pretty common, even in spite of the "risks" associated with doing this naively. 

To calculate volatility, you can use daily, weekly, or monthly returns but you'll need to adjust them to annualize. Specifically, volatility can be annualized by
multiplying by the root of the number of periods. For example for daily to annual,

$$
\sigma_{annualized} = \sigma_{daily}\sqrt{252}
$$

We compute daily, and annualized volatility for our portfolio as shown in the table below.

| Metric                 | Value     |
|------------------------|-----------|
| Daily Volatility       | 0.013029  |
| Annualized Volatility  | 0.206834  |

#### 2. Drawdown + Maximum Drawdown

Another fairly common risk statistic is both drawdown and maximum drawdown. This is often viewed as a more realistic question to ask yourself during the portfolio construction process. 
It's much easier to say "I dont want to lose more than 20% of my portfolio at once" instead of "I dont want my portfolio value to deviate 20% annually", which is what one might say if using volatility as a measure of risk.
Drawdown is calculated using cumulative returns, and is formulated as follows:

Let $$C_t$$ be the cumulative returns at time $$t$$. Then we define drawdown at time $$t$$, $$DD_t$$.

$$
DD_t = \frac{C_t}{C_t^{max}} - 1
$$

We can then similarly defined maximum drawdown as the minimum of drawdown

$$
MMD = \min_{t} DD_t  
$$

![Maximum Drawdown](assets/images/mdd.png)

The maximum drawdown is shown as the dotted red line.


#### 3. VaR + CVaR

Similar to drawdown and maximum drawdown, Value at Risk or Conditional Value at Risk summarize worst case scenarios. Value at Risk measures the maximum expected loss at some given %. 
CVaR measures the average of the losses beyond the VaR threshold. They can be calculated as follows:

$$
VaR_{\alpha} = -\text{Quantile}_{\alpha}(\text{returns})
$$

$$
CVaR_{\alpha} = -\mathbb{E}[\text{returns} \mid \text{returns} \leq -VaR_{\alpha}]
$$

VaR and CVaR calculated at a 95% confidence interval are given below for our portfolio.

| Metric   | Value     |
|----------|-----------|
| VaR (%)  | -1.72     |
| CVaR (%) | 3.01      |

### **Risk Models**

So far, we have focused on individual statistics, but another widely used approach to measure risk is through the use of risk models. In these models, risk is typically assessed in relation to 
some underlying factor. In this section, we will explore a few examples of how risk can be quantified using factor-based models.

#### 1. Beta

We begin with one of the simplest measures in factor-based risk models: beta. Beta measures the sensitivity of a portfolio's returns relative to the returns of a benchmark, which is typically the market. Essentially, it quantifies how much the portfolio's returns are expected to change in response to changes in the market. This makes beta a crucial tool for understanding the risk exposure of a portfolio to broader market movements.

We can compute our portfolios beta to the S&P500, either through running a linear regerssion model, or directly using the formula as given below:

$$
\beta = \frac{Cov(r_{portfolio},r_{index})}{Var(r_{index})}
$$

where:
- $$r_{portfolio}$$ is our portfolio returns
- $$r_{index}$$ is our index returns

Our portfolio beta is **0.94** to the S&P500. This result should be fairly unsurprising given the composition of our portfolio.

Importantly, however, as we covered in the previous artcile both return variance and covariance change over time. Thus, it often makes more sense to look at a rolling beta. Our portfolio beta, as calculated over a 120 day rolling window is shown below.

![Rolling Beta](assets/images/rolling_beta.png)

#### 2. Factor Models 

A logical next step to the above, would be of course to include additional variables in your regression calculation and measure sensitivities to various factors. 
This is where factor models come in, but of course we cannot do this without a little bit of thought, due to the threat of multi-colinearity between our regressors. That is, we'd want our "factors" to be somewhat uncorrelated. 

Perhaps the most well known factor models, are the Fama-French 3 and 5 factor models. In their paper, they show that returns can be explained sufficiently by their factor constructions. For our purposes we'll just use the 3 factor model. The three "factors" that are included in their model are: MKT, SMB, HML. The MKT factor, of course represents the market and is just our original factor from the previous section. SMB, or Small Minus Big, captures the return difference between small-cap and large-cap stocks. Finally, HML, or High Minus Low, reflects the return difference between value stocks (those with high book-to-market ratios) and growth stocks (those with low book-to-market ratios).

Running a multiple-linear regression against these factors we get the following results:

| Factor   | Beta      |
|----------|-----------|
| Mkt-RF   | 0.91865   |
| SMB      | -0.073345 |
| HML      | 0.261635  |


We can interpret this as though most of our returns can be explained by the market factor, which should be expected given the way our portfolio was constructed. Furthemore, our slightly positive beta on the HML factor suggests that our portfolio as a growth bias. As was the case with simple beta, its perhaps more informative to look at how these risk factor exposures change over time, but I've omitted this here. 

#### 3. Statistical Models

Finally, another widely used approach to understanding risk involves analyzing statistical factor exposures. This involves constructing factors, often through methods such as Principal Component Analysis (PCA), and then using these factors to explain the returns of a portfolio. In this context, we'll decompose the returns of our ETFs to identify the underlying factors that drive their performance. 

We have 10 ETFs, so I've fit a PCA decomposition with 10 components. We can first look at the scree plot to give us insight into our data.

![Scree Plot](assets/images/scree_plot.png)

This shows us that the majority of the variance within our returns is explained by the first and second factor. As a result, lets look at the ETF's which are weighted most heavily on these two factors.

![PCA Loadings](assets/images/pca_loadings.png)

This plot gives some good insight into the driving factors of our returns, in particular notice how XLK (Tech) and XLY (Consumer Disc.) have negative loadings on the first principal component. This suggests they are perhaps acting in a differentiated manner to the rest of the ETF's which have positive weights here. We can interpret this first factor as explaining the returns of the market (see eigenportfolios for an explanation of this), and so its fairly clear to understand why these two would be negative here (tech is severly outperforming).

### Conclusion

In this post we covered some basic concepts related to measuring and understanding our portfolio risk, which is a core component in the investment process. Of course, there are many other ways to measure risk that aren't highlighted in this post, and I encourage you to seek out new ways to analyze it.As always, you can find the code on my GitHub. Thank you for reading!
