---
layout: post  
title: "XJTU-ELEC427304 Big Data Notes"  
date: 2024-04-23 19:10 +0800  
last_modified_at: 2024-05-21 19:18 +0800  
tags: [Course Note]  
math: true  
toc: true  
excerpt: "Course notes of XJTU-ELEC427304 (TBC)."
---

# Introduction
**Data Types**:Continuous, Binary, Discrete, String, Symbolic

**Source & Materials**:[KDnuggets](https://www.kdnuggets.com/), [UCI Machine Learning Repository](http://archive.ics.uci.edu/ml/datasets.php)

**Framework of ML**
Step 1:function with unknown \\(y=f_\theta(\mathbit{x}) \\)

Step 2: define loss from training data \\( L(\theta) \\)

Step 3: optimization \\( \theta^* = \arg \min_{\theta} \mathcal{L} \\)

## Conditional Independence   
**Independent** ≠ **Uncorrelated**

e.g. When \\( X \in [-1, 1] \\), then \\( Y = x^2 \\). 

\\( \text{Cov}(X, Y) = 0 \\) indicating that \\( X \\) and \\( Y \\) are **uncorrelated**, even though \\( Y \\) is **completely determined** by \\( X \\).


