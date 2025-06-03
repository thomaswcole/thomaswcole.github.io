---
title: 'Statistical arbitrage with correlation matrix clustering'
description: >-
  Utilizing graph clustering algorithms for stat arb
date: '2024-12-11'
categories: ['Trading Strategies']
tags: ['Clustering','Statistical Arbitrage','Equities']
use_math: True
math: True
---


### **Introduction**

In this post, I present a detailed summary of a project that I recently completed, which replicates and improves upon the approach in *Correlation Matrix Clustering for Statistical Arbitrage Portfolios* by Cartea et al. (2023). The project builds on their methodology for using correlation matrix clustering techniques in the context of quantitative finance, particularly for constructing market-neutral portfolios. My replication of their work incorporates new insights, tests different aspects of the clustering process, and compares various clustering algorithms in greater depth. For those interested in the technical details, the code used to generate the figures and results presented here can be found on my GitHub, linked below.


### **Clustering Algorithms**

The core of this project involves the implementation of three classical clustering algorithms based on spectral graph theory. Each of these methods applies clustering to the correlation matrix of asset returns, treating each asset as a node in a graph and the correlation between asset pairs as the edge weights.

Below, I explain each of the clustering methods I used in this project:

#### **1. Unsigned Laplacian Clustering**

Unsigned Laplacian clustering is a widely used method in spectral clustering. The idea behind this technique is to first construct a graph using the correlation matrix of asset returns, where each node represents an asset, and edges between nodes are weighted based on the correlation between the respective assets. The unsigned Laplacian matrix $$L$$ is given by:

$$
L = D - A
$$

Where $$D$$ is the degree matrix (a diagonal matrix where each element represents the sum of the weights for each node), and $$A$$ is the adjacency matrix (a matrix where each element represents the weight of the edge between two nodes). 

The eigenvectors corresponding to the smallest eigenvalues of the Laplacian matrix are then used to form a lower-dimensional embedding of the graph. Clustering is performed by grouping the assets based on the eigenvectors that minimize the Laplacian's eigenvalue problem. This approach aims to group assets that exhibit similar return behaviors, thereby creating market-neutral portfolios by pairing long and short positions.

The cumulative return series for this approach is shown below.

![Unsigned Returns](assets/images/correlation_clustering/unsigned_cumret.png)

#### **2. Random Walk Normalized Laplacian Graph**

This variant of the Laplacian matrix is designed to account for the degree of each vertex in the graph. The random walk normalized Laplacian is defined as:

$$
L_{rw} = D^{-1/2} L D^{-1/2}
$$

Where $$L$$ is the original Laplacian matrix, and $$D^{-1/2}$$ is the inverse square root of the degree matrix. The idea behind the random walk normalization is to make the graph more balanced by accounting for the degree of each vertex, which prevents the clustering process from being dominated by high-degree (i.e., highly correlated) nodes. The normalized Laplacian is used for spectral decomposition, and the clustering is done similarly to the unsigned Laplacian method, but this normalization makes the method less sensitive to the global degree distribution of the graph.

The cumulative return series for this approach is again show below.

![Random Walk Returns](assets/images/correlation_clustering/rw_cumret.png)

#### **3. Symmetric Normalized Laplacian Graph**

The symmetric normalized Laplacian matrix is another variation that normalizes the graph symmetrically to avoid any distortion caused by differences in node degree. This matrix is defined as:

$$
L_{sym} = D^{-1} L
$$

Where $$L$$ is the Laplacian matrix, and $$D^{-1}$$ is the inverse of the degree matrix. The symmetric normalization ensures that each node’s contribution to the graph is scaled according to its degree, making the clustering method more balanced and robust to highly connected nodes. As with the other methods, spectral decomposition of this matrix yields eigenvectors that are used for clustering.

The cumulative return series for this approach is again shown below.

![Symmetric Returns](assets/images/correlation_clustering/symmetric_cumret.png)

### **Portfolio Construction**

For each clustering algorithm, I constructed equally weighted long/short market-neutral portfolios. The process involved the following steps:

1. **Asset Selection**: In my implementation and testing, I selected a subset of the S&P500, namely 50 equities. 
   
2. **Clustering**: For each clustering method, I applied two different approaches to grouping assets: the first based on their raw return correlations and the second using their regression residuals after removing systematic market factors. In my implementation, I fixed the number of clusters at three, though this parameter could be optimized or selected more dynamically based on additional criteria.
   
3. **Portfolio Construction**: Once the assets were clustered, I computed a cumulative window returns within cluster and entered long positions in the top assets in the cluster, and short positions in the bottom assets in the cluster. The portfolios were constructed such that the total dollar amount invested in each position was equal, ensuring market neutrality.

4. **Rebalancing**: The portfolios were rebalanced every 3 days.

Again, the primary goal of constructing these portfolios was to evaluate how well each clustering method performed in terms of capturing meaningful relationships between assets, and utilizing these to achieve positive returns.

### **Portfolio Results**

To assess the performance of the portfolios, I compared cumulative returns, the Sharpe ratio, and maximum drawdown. 

Below is a summary of the performance results for each clustering method applied to both raw returns and factor residuals.

| Graph Laplacian Type | Clustering Basis    | Max Drawdown (%) | Sharpe Ratio | Cumulative Return (%) |
|----------------------|--------------------|------------------|--------------|-----------------------|
| Unsigned            | Raw Returns        | -41.83%          | 2.09         | 3.23                  |
| Unsigned            | Factor Residuals   | -42.44%          | 1.92         | 2.43                  |
| Random Walk         | Raw Returns        | -39.61%          | 2.01         | 2.82                  |
| Random Walk         | Factor Residuals   | -44.21%          | 1.84         | 2.49                  |
| Normalized          | Raw Returns        | -37.84%          | 1.97         | 2.77                  |
| Normalized          | Factor Residuals   | -42.77%          | 1.86         | 2.49                  |


### **Conclusion**

The key takeaway from this project is that different clustering methods, each with their own strengths and weaknesses, can be used to identify clusters of assets that exhibit similar return behaviors. Furthermore, my results demonstrate that clustering on raw returns outperforms utilizing factor regression residuals. Overall, this project highlights the potential of spectral clustering techniques as applied to systematic investing.

For further details, feel free to visit my [GitHub](https://github.com/thomaswcole).




