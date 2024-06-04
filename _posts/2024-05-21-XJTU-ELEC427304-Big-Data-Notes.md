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
Step 1:function with unknown \\(y=f_\theta(x) \\)

Step 2: define loss from training data \\( L(\theta) \\)

Step 3: optimization \\( \theta^* = \arg \min_{\theta} \mathcal{L} \\)

**DM Techniques - Classification**:  Decision Trees, K-Nearest Neighbours, Neural Networks, Support Vector Machines

**Evaluation method of model**: hold-out, cross-validation, boostrapping
Error rate

accuracy

Precision, recall and F1

ROC and AUC

Cost-sensitive error rate and cost curve

**DM Techniques - Classification**: K-Means, Sequential Leader, Affinity Propagation

**DM Techniques – Association Rule**

**DM Techniques – Regression**

**Overfitting** 

# Data Preprocessing

## Conditional Independence   
**Independent** ≠ **Uncorrelated**

e.g. When \\( X \in [-1, 1] \\), then \\( Y = x^2 \\). 

\\( \text{Cov}(X, Y) = 0 \\) indicating that \\( X \\) and \\( Y \\) are **uncorrelated**, even though \\( Y \\) is **completely determined** by \\( X \\).


## ID3 Framework

<img src="https://github.com/Sihan0229/Sihan0229.github.io/blob/master/assets/ID3Framework.png?raw=true" width="100%">

+ 叶节点为纯节点，stop.

+ 叶节点为空节点，如何决定该节点的分类？根据父节点的比例

+ 属性用完了也无法分类：树无法生长，根据占优比例决定类别。

**奥卡姆剃刀**：倾向于选择更简单的模型

**over fitting**：泛化误差小，测试误差大（额外划出Validation Set来判定是否过拟合）

解决：早停，Loss加入正则项，利用新模型

决策树解决过拟合的方法：**剪枝**
+ 预剪枝：生成决策树时，对每个节点在划分前先进行估计，若不能带来泛化性能的提升，则停止划分。（主要用到的标准是验证集的精度）
+ 后剪枝:生成完整的决策树，自底向上向叶节点进行考察，若该节点对应的子树替换成叶节点能带来泛化性能的提升，则替换成叶节点。

**Entropy Bias**

信息增益率
\\( \text{Cov}(X, Y) = 0 \\) indicating that \\( X \\) and \\( Y \\) are **uncorrelated**, even though \\( Y \\) is **completely determined** by \\( X \\).

# Convolutional Neural Networks
## Backpropagation 反向传播
## Convolution arithmetic 卷积
## Pooling arithmetic 池化