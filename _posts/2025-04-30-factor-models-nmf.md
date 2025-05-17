---
title: 'Statistical factor models and an extension with NMF'
description: >-
  A review of statistical factor models and an extension with non-negative matrix factorization
date: '2025-05-30'
categories: ['Monthly']
use_math: True
math: True
---

### Introduction

It's April- and this is another one of my monthly posts. To catch up on the others, please visit the monthly folder [here](https://thomaswcole.github.io/categories/monthly/).

Today were going to look at generating factors using a matrix factorization techinque known as Non-Negative Matrix Factorization (NMF), more on that later.Importantly, the content of this post will be somewhat self contained and not much background knowledge is required, however, it does have close ties to a few topics that I have already covered in previous posts, such as eigenportfolios see this [here], or a brief introduction to factor models given in this post [here]. 

The motivation for this is driven by a few factors (ha.). First, I've spent the better part of the last few weeks completing a research project for my computation statistics course where I'm looking at faster and better ways to solve NMF problems. That is to say, its been top of mind. Secondly, the formulation of the problem has close ties to the way traditional factor models are derived, which I have also spent a fair bit of time reading into recently.

### Non-Negative Matrix Factorization (NMF)

I'll start by introducing NMF, its formulation and we'll briefly review how one might solve such a problem. The formulation is fairly simple. 

Given a non-negative matrix $$A \in \mathbb{R}^{m \times n}_{\geq 0}$$, NMF finds factor matrices:
 
$$ A \approx WH $$

where:
- $$W \in \mathbb{R}^{m \times k}_{\geq 0}$$ (basis matrix)
- $$H \in \mathbb{R}^{k \times n}_{\geq 0}$$ (coefficient matrix)
- $$k \ll \min(m,n)$$ (target rank)



