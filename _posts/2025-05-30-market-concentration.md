---
title: 'The impact of market concentration and crowding'
date: '2025-05-10'
description: >-
  A simulation study on the impact of market concentration and crowding.
categories: ['Monthly']
use_math: True
math: True
---

### **Introduction**


In this post, which is another install of the year long series, we'll be exploring the impact of market concentration and crowding on investors. If you're interested in reading past posts, you can find the others in the monthly folder [here](https://thomaswcole.github.io/categories/monthly/).

This topic in particular is motivated by a recent academic competition which I competed in, the IAQF Academic Student Competition. My team and I were also fortunate enough to be announced as one of the winners. You can read the original problem formulation and the announcement [here](https://thomaswcole.github.io/categories/monthly/), and our full paper [here](https://thomaswcole.github.io/categories/monthly/).

### **Motivation**

In 2024, it has been reported that % of the returns within the SP500, were attributed to just seven companies. These seven companies have become known as the Magnificent Seven. They are: AAPL, TSLA, META, GOOG, NVDA, MSFT, AMZN. This dominance of returns in equity markets has driven the following research questions which I'll hope to answer through a simulation study. 

**Setup**

To keep our results in line with what we might expect in the SP500,our goal here will be to model the dynamics of the SP500. We'll do this by simulating the returns of 500 unique assets, which we'll then split up into 11 sectors, generate synthetic returns and then analyze the impact of market concentration.


