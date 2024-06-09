---
layout: post  
title: "XJTU-ELEC427304 Big Data Notes"  
date: 2024-06-04 19:10 +0800  
last_modified_at: 2024-06-09 19:18 +0800  
tags: [Course Note]  
math: true  r
toc: true 
excerpt: "Course notes of XJTU-ELEC427304 (TBC)."
---

# Introduction

**PAC - probably Approximately Correct learning model** 

$$
P(|f(x)-y|\le \epsilon)\ge 1-\delta
$$

**Data Types** 
+ Continuous, Binary
+ Discrete, String
+ Symbolic

**Big Data** is high-volume, high-velocity, high-variety, demanding cost-effective, innovative forms of imformation processing, whose size is beyongd the ability of typical database software tools to capture, store, manage, and analyze.

**Data Mining** is the process of discovering patterns in large data set, including intersection of ML, statistic and database systems.

习题1：大数据的特点包括类型多、对处理实时性要求高、容量大

习题2：理想的数据挖掘算法得到的结果应该是：Useful, Hidden, Interesting

**Source & Materials**
[KDnuggets](https://www.kdnuggets.com/), [UCI Machine Learning Repository](http://archive.ics.uci.edu/ml/datasets.php)

**Framework of ML**

+ Step 1: function with unknown 
$$
y=f_\theta(x)
$$
+ Step 2: define loss from training data 
$$
L(\theta)
$$
+ Step 3: optimization 
$$
\theta^* = \arg \min_{\theta} \mathcal{L}
$$

**DM Techniques - Classification**
Decision Trees, K-Nearest Neighbours, Neural Networks, Support Vector Machines

# Evaluation method of model

## Data Segmentation

**hold-out 留出法** 保持数据分布一致性，多次重复随机划分，测试集不能太大、不能太小

**cross-validation 交叉验证法** k折交叉验证

**boostrapping 自助法** 有放回/可重复采样，数据分布有所改变，训练集与原样本集同规模
（包外估计 out-of-bag estimation）

## Model performance
**Error rate 错误率** 
$$
E(𝑓;𝐷)=\frac{1}{m}\Sigma_{i=1}^{m}I(f(𝒙_i)≠𝑦)
$$

**accuracy 精度** 
$$
Acc(𝑓;𝐷)=\frac{1}{m}\Sigma_{i=1}^{m}I(f(𝒙_i)=𝑦)=1-E(𝑓;𝐷)
$$

**Precision 查准率** 
$$
P=\frac{TP}{TP+FP}
$$

**recall 查全率** 
$$
R=\frac{TP}{TP+FN}
$$

**F1** 
$$
\frac{1}{F_1}=\frac{1}{2}(\frac{1}{R}+\frac{1}{P})
$$

**Fβ** 
$$
\frac{1}{F_\beta}=\frac{1}{1+\beta^2}(\frac{\beta^2}{R}+\frac{1}{P})
$$

**Confusion Matrix 混淆矩阵**

<img src="https://github.com/Sihan0229/Sihan0229.github.io/blob/master/assets/ConfusionMatrix.png?raw=true" width="100%">

**P-R曲线**

<img src="https://github.com/Sihan0229/Sihan0229.github.io/blob/master/assets/pr.png?raw=true" width="100%">

**BEP 平衡点 Break-Even Point** 查准率 = 查全率的取值

**ROC 受试者工作特征曲线**

<img src="https://github.com/Sihan0229/Sihan0229.github.io/blob/master/assets/roc.png?raw=true" width="100%">

**AUC**：ROC曲线下面积

How to Construct an ROC curve

<img src="https://github.com/Sihan0229/Sihan0229.github.io/blob/master/assets/rocCurve.png?raw=true" width="100%">

<img src="https://github.com/Sihan0229/Sihan0229.github.io/blob/master/assets/rocCurve2.png?raw=true" width="100%">

**Cost Matrix**
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

# Optimization

# Convolutional Neural Networks

## Backpropagation 反向传播

## FUlly connected layers
参数量

<img src="https://github.com/Sihan0229/Sihan0229.github.io/blob/master/assets/bd_fc_num.png?raw=true" width="100%">

## Convolution arithmetic 卷积
## Pooling arithmetic 池化

# Classifier
## Linear Classifier
## SVM 支持向量机
$ y_i(w\cdot x_i+b)-1\ge 0 $ <br>
$ a_i \ge 0 $ <br>
$ a_i[y_i(w\cdot x_i+b)-1]=0 $ <br>

只有支持向量在起作用

## Soft Margin