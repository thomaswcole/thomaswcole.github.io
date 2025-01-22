---
title: 'Properties of Large Random Matrices - Tracy-Widom and Marchenko-Pastur'
date: '2025-12-31'
categories: ['Probability','Statistics','Eigenvalues','Random Matrix Theory']
use_math: True
math: True
---

### **Introduction**

***NOTE: This has not yet been completed.***

The Tracy-Widom Distribution and Marchenko-Pastur Law describe the statistical properties of eigenvalues of large random matrices. In this post, I'll be providing an overview of both of these, the relationships between them, and how they can be applied in finance.  That said, I have already utilized both of these results in a recent research report which you can read ***here***.

Before we continue, some primers on random matrices are helpful. 

#### **Background**

Note, I assume that you are familiar with foundational linear algebra concepts such as eigenvalues and eigenvectors.

##### Random Matrices

One of the main assumptions in both of these concepts is the type of matrix we're dealing with. The Marchenko-Pastur Law, described later, assumes that our matrix is with i.i.d gaussian entries. Tracy Widom applies to a wider class of matrices, but the most important for our use case is a matrix which belongs to the class of *Gaussian Orthogonal Ensemble* matrices. 

*Gaussian Orthogonal Ensemble*

A $$n \times n$$ rectangular matrix $$M$$  belongs to the Gaussian Orthogonal Ensemble (GOE) if:

-  $$M$$ is real symmetric ($$M = M^T$$)
- The entries of $$M$$ are distributed as follows:
    -  The diagonal entries $$M_{ii}$$ are independent Gaussian random variables with mean \( 0 \) and variance \( \sigma^2 \):
    - $$M_{ii} \sim \mathcal{N}(0, \sigma^2)$$.
    - The off-diagonal entries $$M_{ij}$$ are independent Gaussian random variables with mean  $$0$$ and variance $$\frac{\sigma^2}{2}$$:
    - $$M_{ij} \sim \mathcal{N}(0, \frac{\sigma^2}{2})$$, $$\quad M_{ji} = M_{ij}$$.

### **The Marchenko-Pastur Law**

**Formal Definition**

Let $$X$$ be a $$p \times n $$ random matrix with i.i.d entries with mean $$0$$ and variance $$1$$. The sample covariance matrix is given by 

$$
S = \frac{1}{n}XX^T
$$

The Marchenk-Pastur Law states that as $$n,p \rightarrow \infty$$, the eigenvalue density of $$S$$ converges to the **Marchenko-Pastur Distribution**. This distribution, the limiting distribution fo the eigenvalues $$\rho(\lambda)$$ is given by 

$$

\rho(\lambda) = \frac{n}{2\pi p \lambda}\sqrt{(\lambda_{+} - \lambda)(\lambda - \lambda_{-})}, \lambda \in [\lambda_{-},\lambda_{+}]
$$

where 

$$
\lambda_{\pm} = (1 \pm \sqrt{\frac{p}{n}})^2
$$

**Implications**

Firstly, what this tells us in words is that- asymptotically, when our matrix has the above properties, the majority of the eigenvalues of this matrix should lie within the range $$[\lambda_{-},\lambda_{+}]$$. We can thus utilize this property to determine which of our eigenvalues are signifigant.

### **The Tracy-Widom Distribution**


**Statement**


**Implications**


### **Applications in Finance**