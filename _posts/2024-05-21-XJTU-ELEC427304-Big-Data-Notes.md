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
<style>
        h1 { font: 26pt Times !important; }
        h2 { font: 20pt Times !important; }
        h3 { font: 16pt Times !important; }
</style>

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
E(𝑓;𝐷)=\frac{1}{m}\sum_{i=1}^{m}I(f(𝒙_i)≠𝑦)
$$

**Accuracy 精度**

$$
Acc(𝑓;𝐷)=\frac{1}{m}\sum_{i=1}^{m}I(f(𝒙_i)=𝑦)=1-E(𝑓;𝐷)
$$

**Precision 查准率**

$$
P=\frac{TP}{TP+FP}
$$

**Recall 查全率**

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

<img src="https://github.com/Sihan0229/Sihan0229.github.io/blob/master/assets/pr.png?raw=true" width="60%">

**BEP 平衡点 Break-Even Point** 查准率 = 查全率的取值

**ROC 受试者工作特征曲线**

<img src="https://github.com/Sihan0229/Sihan0229.github.io/blob/master/assets/roc.png?raw=true" width="100%">

**AUC**：ROC曲线下面积

How to Construct an ROC curve

<img src="https://github.com/Sihan0229/Sihan0229.github.io/blob/master/assets/rocCurve.png?raw=true" width="100%">

<img src="https://github.com/Sihan0229/Sihan0229.github.io/blob/master/assets/rocCurve2.png?raw=true" width="100%">

**Cost Matrix**
Cost-sensitive error rate and cost curve

C(i,j)：Cost of misclassifying class j example as class i

**Cost VS Accuracy**

<img src="https://github.com/Sihan0229/Sihan0229.github.io/blob/master/assets/cost_vs_accuracy.png?raw=true" width="100%">

**Lift Analysis**

[[模型解釋策略]Lift Chart, Permutation Importance, LIME](https://yulongtsai.medium.com/lift-chart-permutation-importance-lime-c22be8bdaf48)

例题：假设目标客户占人群的5%，现根据用户模型进行打分排序，
取1000名潜在客户中排名前10%的客户，发现其中包含25名目
标客户，问此模型在10%处的提升度是多少?

答案：5

例题：我们通常将数据集划分为训练集，验证集和测试集进行模型的训
练，参数的验证需要在**验证集**上进行，参数确定后**需要**重新训练模型。

例题：当西瓜收购公司去瓜摊收购西瓜时既希望把好瓜都收走又保证收
到的瓜中坏瓜尽可能的少，请问他应该考虑什么评价指标？

正确：**F1调和平均**与**BEP**

例题：假设我们已经建立好了一个二分类模型,输出是0或1,初始阈值设
置为05 超过0.5概率估计就判别为1,否则就判别为0;如果我们现
在用另一个大于0.5的阈值，一般来说，下列说法正确的是：**查准率会上升或不变，查全率会下降或不变**

**DM Techniques - Classification**: K-Means, Sequential Leader, Affinity Propagation

**DM Techniques – Association Rule**

**DM Techniques – Regression**

# Data Preprocessing
**Typical lssues** : 缺少属性值Missing Attribute Values, 不同的编码/命名方案Different Coding/Naming Schemes, 不可行的值Infeasible Values, 不一致的数据InconsistentData, 异常值Outliers

**Data Quality** : Accuracy （准确性）, Completeness（完整性）, Consistency （一致性）, Interpretability（可解释性）, Credibility（可信性）, Timeliness（时效性）

**数据集成** : 组合来自不同来源的数据。

**数据缩减** : 特征选择、抽样

**Privacy**

<img src="https://github.com/Sihan0229/Sihan0229.github.io/blob/master/assets/privacy.png?raw=true" width="100%">

**No Free Lunch**

Why bother so many different algorithms?

+ No algorithm is always superior to others.
+ No parameter settingis optimal over all problems.

Look for the best match between problem and algorithm.

+ Experience
+ Trial and Error

Factors to consider:
+ Applicability
+ Computational Complexity
+ Interpretability

Always start with simple ones.

OUTLINES of Data Preprocessing
## Data Cleaning 数据清洗
填充缺失值、更正不一致的数据、识别异常值和噪声数据。
### 缺失值

数据缺失类型分为三种：完全随机缺失、随机缺失、非随机缺失。
参考[数据缺失类型](https://blog.csdn.net/jwtning/article/details/116125819)

<img src="https://github.com/Sihan0229/Sihan0229.github.io/blob/master/assets/missing.png?raw=true" width="100%">

如何处理缺失数据？
+ 忽略：删除有缺失值的样本/属性，最简单、最直接的方法，低缺失率时效果很好
+ 手动填写缺失值：重新收集数据或领域知识，繁琐/不可行
+ 自动填写缺失值：全局常数/平均值或中位数/最可能的值

例题：学生小明在调查问卷中没有回答下述问题:“你去年的工资收入和前年相比是否有所增加?”对这种情况最恰当的描述是:N/A

以下参考[劉智皓 (Chih-Hao Liu) 機器學習_學習筆記系列(96)：區域性異常因子(Local Outlier Factor)](https://tomohiroliu22.medium.com/%E6%A9%9F%E5%99%A8%E5%AD%B8%E7%BF%92-%E5%AD%B8%E7%BF%92%E7%AD%86%E8%A8%98%E7%B3%BB%E5%88%97-96-%E5%8D%80%E5%9F%9F%E6%80%A7%E7%95%B0%E5%B8%B8%E5%9B%A0%E5%AD%90-local-outlier-factor-a141c2450d4a)

**Outliers离群点** : Outliers≠Anomaly

**Local Outliner Factor**
关于LOF算法，它是基于空间密度来寻找异常值的，这里我们定义**可达距离reachability distance** = max(B点到离B第k近的点的距离, A和B的距离)

$$
reachability_{-}distance_{k}(A,B)=m a x\Big[k_{-}distance(B),distance(A,B)\Big]
$$ 
假设有两个点A和B，`k_distance(B)`代表的就是B点到离B第k近的点的距离，`distance(A,B)`则就是A和B的距离。所以这里的意思是：如果点和点之间相距够近，就将他们一视同仁，视为密度较高的区域。

而接下來我們會計算**local reachability density: (平均距离)**

$$
IRD_{k}(A)=\frac{1}{\left( \frac{\sum_{B\in{\cal N}_{k}(A)}reachability_{-}distance_{k}(A,B)}{|{N}_{k}(A)|}\right)}
$$

其中N_k為A點的neighbor。所以這個式子代表的就是，我們A點neighbor其reachability distance**平均**的**倒數**，所以我們可以說，**如果IRD很大，代表以A點為中心的區域很密集**，反之則是很疏鬆。
而當我們求得了IRD之後，我們最後就會計算

**Local Outlier Factor**:

$$L O F_{k}(A)=\frac{\sum_{B\in N_{k}(A)}I R D_{k}(B)/I R D_{k}(A)}{\left|N_{k}(A)\right|}=\frac{1}{I R D_{k}(A)}\frac{\sum_{B\in N_{k}(A)}I R D_{k}(B)}{\left|N_{k}(A)\right|}$$

我們可以看到LOF，他做的事情就是計算A所有**neighbor的**IRD值並且將其平均除以IRD(A)。而LOF在意義上來說，**如果接近1代表，A和其Neighbor的空間密度都非常接近，如果小於1非常多，代表A的密度大於他的neighbor，也就是密度較高的區域，若大於1非常多，則代表A的密度小於他的neighbor。**

**相似度与不相似度** 
+ 相似度: 两个数据对象相似程度的数值测量,对象越相似，相似度越高,通常在 [0,1] 范围内
+ 相异度: 两个数据对象差异程度的数值测量,对象越相似，相似度越低,最小不相似度通常为 0,上限各不相同

简单属性的相似性/不相似性

<img src="https://github.com/Sihan0229/Sihan0229.github.io/blob/master/assets/simidissimi.png?raw=true" width="100%">

**distance**
	
+ Euclidean Distance
$$dist={\sqrt{\frac{n}{k-1}}}(p_{k}-q_{k})^{2}$$
+ Minkowski Distance
$$dist=(\sum_{k=1}^{n}|p_{k}-q_{k}|^r)^{1/r}$$

<img src="https://github.com/Sihan0229/Sihan0229.github.io/blob/master/assets/Minkowski.png?raw=true" width="100%">

+ Mahalanobis Distance

$$mahalanobis(p,q)=\sqrt{(p-q)\Sigma^{-1}\left(p-q\right)^{r}}$$

$$
\Sigma_{j,k}=\frac{1}{n-1} \sum_{i=1}^{n}(X_{ij}-\overline{{X}}_{j})(X_{ik}-\overline{{X}}_{j})
$$

Duplicate Data重复数据处理方法

Large Data : create keys -> sort -> merge

## Data Transformation 数据转换
现在我们有了一个无错误的数据集，还需要标准化 standardized。类型转换、规范化Normalization、采样、 聚合Aggregation

例如：选取变换$$\phi(x)$$将非线性变为线性、使用独热编码标记类别(不能因为编码而产生新的参数影响，如因为编码1 2 3而导致距离不相等)等

**采样**：什么是采样？

有效抽样的关键原则如下：
+ 如果样本具有代表性，则使用样本的效果几乎与使用整个数据集一样好 
+ 如果样本具有与原始数据集大致相同的属性（感兴趣的），则该样本具有代表性

**Imbalanced Datasets不平衡的数据集**

**G-means**

$${G-m e a n}=(A c c^{+}\times A c c^{-})^{1/2}$$

$$w h e r e\ A c c^{+}={\frac{T P}{T P+F N}};\ \ \ A c c^{-}={\frac{T N}{T N+F P}}$$

**F-measure**

$$F-m e a s u r e={\frac{2\times P r e c i s i o n\times R e c a l l}{P r e c i s i o n+R e c a l l}}$$

$$w h e r e\;\;\;P r e c i s i o n=\frac{T P}{T P+F P};\;\;\;\;{R e c a l l}=\frac{T P}{T P+F N}=A c c^{+}$$

**Over-Sampling**

可以参考[机器学习之类别不平衡问题 (3) —— 采样方法
](https://www.cnblogs.com/massquantity/p/9382710.html),这里面的Border-line SMOTE个人认为比较符合ppt上的下一个要点**Boundary Sampling**

不平衡的数据集要采样、扩增、调整loss function

**Normalization**

<img src="https://github.com/Sihan0229/Sihan0229.github.io/blob/master/assets/Normalization.png?raw=true" width="100%">


## Data Description

<img src="https://github.com/Sihan0229/Sihan0229.github.io/blob/master/assets/description.png?raw=true" width="80%">
e
<img src="https://github.com/Sihan0229/Sihan0229.github.io/blob/master/assets/description2.png?raw=true" width="100%">

<img src="https://github.com/Sihan0229/Sihan0229.github.io/blob/master/assets/description3.png?raw=true" width="100%">e

## Feature Selection

## Feature Extraction


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

$$ y_i(w\cdot x_i+b)-1\ge 0 $$

$$ a_i \ge 0 $$ 

$$ a_i[y_i(w\cdot x_i+b)-1]=0 $$

只有支持向量在起作用

### Soft Margin

$$
y_{i}(w x_{i}+\partial)-1+\xi_{i}\geq0
$$

$$\Phi(w)=\frac{1}{2}\,w^{t}w+C\sum_{i}\xi_{i}$$

$$\xi_{r}\geq0$$

$${\cal L}_{P} \equiv\frac{1}{2}\Bigl|w\Bigr|^{2}+C\sum_{i=1}^{l}\xi_{i}-\sum_{i=1}^{l}\alpha_{i}[y_{i}(w\cdot x_{i}+b)-1+\xi_{i}]-\sum_{i=1}^{l}\mu_{i}\xi_{i}$$

### 非线性SVMs （Non-linear SVMs）

**Feature Space**：
向高阶空间进行映射，可以把很多不能用linear SVM解决的问题使用linear SVM解决

<img src="https://github.com/Sihan0229/Sihan0229.github.io/blob/master/assets/nonlinearSVMs.png?raw=true" width="100%">

Which kind of $$\varphi(x)$$ can solve this problem?

**Kernel Trick** 

$$K(x_{i}x_{j})=\varphi(x_{i})*\varphi(x_{j})$$

$$\mathcal{\Phi}\colon\,x\longrightarrow\mathcal{\varphi}(x)$$

$$x_{i}*x_{j}\longrightarrow\varphi(x_{i})*\varphi(x_{j})$$

**Solutions of w & b**


问题：在SVM当中进行空间映射的主要目的是:
- A 降低计算复杂度 （提高）
- B 提取较为重要的特征
- C 对原始数据进行标准化
- D 提高原始问题的可分性 √

问题：对于SVM，在映射后的高维空间直接进行计算的主要问题是:
- A 模型可解释性差
- B 计算复杂度高 √
- C 容易出现奇异矩阵
- D 容易出现稀疏矩阵

问题：所谓kernel trick，指的是:
- A 利用在原始空间定义的函数替代高维空间的向量内积操作 √
- B 利用在高维空间定义的函数替代原始空间的向量内积操作
- C 核函数的导数具有简单的解析解，简化了运算
- D 核函数具有固定的上下界，可以输出$$(-1,+1)$$区间中的连续值



# 集成学习 

## bagging

<img src="https://github.com/Sihan0229/Sihan0229.github.io/blob/master/assets/bootstrap.png?raw=true" width="100%">

## 有放回采样
有多少样本没有被勇敢但是被以为是可以test的 OOB
好处：可以帮我们构建具有分散性的基础分类器
充分利用所有样本

## 保证基础分类器多样性的方法
+ 算法多样性
+ 训练集（随机又放回采样）
+ 选择不同的属性，决策树中用$$\sqrt{K}$$个属性构造500-5000棵树
+ 超参数


## Stack

<img src="https://github.com/Sihan0229/Sihan0229.github.io/blob/master/assets/stack.png?raw=true" width="100%">

<img src="https://github.com/Sihan0229/Sihan0229.github.io/blob/master/assets/stacking.png?raw=true" width="100%">

## Boosting
 **Adaboost**

