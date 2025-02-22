---
title: 'Principal Component Analysis on Implied Volatility Surfaces'
date: '2024-12-11'
categories: ['PCA','Equities','Options','Implied Volatility']
use_math: True
math: True
---


To be uploaded.

## **Introduction**

In the following post I'll provide a brief overview of a project I completed during my undergraduate degree at McGill University. There is a full report which is available on my github *here*, but I'm hoping to summarize the main points for quick digestion here.


The main portion of the project is to apply Principal Component Analysis (PCA) to the implied volatility surface of US Equity Options. The main focus in the original paper is largely re-interpolation, and is is titled "Modelling Systematic Risk in the Options Market" by Doris Dobi. In my project, however, I focus mainly on the implication for portfolio construction and how these PCA decompositions can reveal understandings for constructing portfolios. Further, I extend the research by providing a simulation study to test the performance of various principal component selection methodologies. The main references in the paper are as follows:

- (1) Doris Dobi. Modeling Systemic Risk in The Options Market. PhD thesis, New York University, May 2014.
- (2) Pedro R. Peres-Neto, Donald A. Jackson, and Keith M. Somers. How many principal components? stopping rules for determining the number of non-trivial axes revisited. Computational Statistics Data Analysis, 49(4):974–997, 2005.
- (3) Irene Aldridge and Marco Avellaneda. Big Data Science in Finance. John Wiley amp; Sons, Inc., 2021.

### Options and Implied Volatility Surfaces

Before diving into the empirical data and the technique we are applying to analyze it, it's important to provide some context. Our dataset focuses on U.S. equity options, specifically those on equities listed in the S&P 500. The key metric we will be analyzing is implied volatility, which reflects the market's expectation of the future realized volatility of the underlying security. Implied volatility is derived by solving for the volatility parameter in the Black-Scholes model, given observed market prices.

To structure our analysis, we will parameterize the implied volatility surface based on time to expiration and delta. The figure below shows an example of an implied volatility surface for AAPL options.

<img src="assets/images/AAPL_IVS.png" width="400">

### Data Acquisition

I obtained daily equity call options data for the period of August 31, 2004 to August 31, 2013 from OptionsMetrics, with time to expirations of 30, 91, 182 and 365 days, and delta ranging from 20 to 85. Furthermore, I obtained daily prices for the relevant underlying securities from CRSP. 

## **Applying Principal Component Analysis**

### A review of the mathematics


### Selecting Principal Components

## **Evaluating Selection Methods**



