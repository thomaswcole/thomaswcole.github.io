---
title: 'Properties of Returns and Volatility'
date: '2025-02-28'
categories: ['Probability','Statistics','Monthly','Returns','Risk','Volatility']
use_math: True
math: True
---

February 2025

## **Introduction**

This post is another installment of a year-long series where I’ll be exploring various topics in quantitative finance. Again, you can expect one of these every month. To read the very first, please see the following link: **HERE**.

In the previous post (if you didn't see it), we covered properties of returns which are, for obvious reasons, crucial to the investment process. After all, everyone likes portfolios that generates returns - so it pays to understand them, literally. For similar reasons, this month I'll focus on measures of risk, and how we can think about it in our portfolios.

The way that I find helpful to evaluating risks of a portfolio can broadly be categorized into two different approaches, the first being risk statistics and the second being model based measures of risk. Risk statsitics would include things such as volatility (standard deviation), drawdown, maximum drawdown, sharpe ratio etc. Model based measures of risk would include decomposing risk by looking at beta or decomposing returns using statistical or factor models.

The following references were used in writing this post:

- (3) Paleologo, G. A. (2025). The Elements of Quantitative Investing. Hoboken, NJ: Wiley.


### **Risk Statistics**

As mentioned, we'll first cover risk. The three that I highlight below are perhaps the most common, but of course there are many others. 

#### 1. Volatility 

The most common and perhaps most natural measure of risk is the volatility of returns. As we mentioned in the previous article, there are some notable properties of volatility to be aware of. That said, summarizing risk with volatility is pretty common, even in spite of the "risks" associated with doing this naively. 

#### 2. Drawdown + Maximum Drawdown

Another fairly common risk statistic is both drawdown and maximum drawdown. This is often viewed as a more realistic question to ask yourself during the portfolio construction process. It's much easier to say "I dont want to lose more than 20% of my portfolio at once" instead of "I dont want my portfolio value to deviate 20% annually", which is what one might say if using volatility as a measure of risk.

#### 3. VaR + CVaR

Similar to drawdown and maximum drawdown, Value at Risk or Conditional Value at Risk summarize worst case scenarios. Value at Risk measures the maximum expected loss at some given %. CVaR measures the average of the losses beyond the VaR threshold.

### **Risk Models**

#### 1. Beta

#### 2. Factor Models 

#### 3. Statistical Models