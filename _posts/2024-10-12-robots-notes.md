---
layout: post  
title: "XJTU-AUTO402227机器人学基础（钱）01(20241) notes"  
date: 2024-10-10 00:10 +0800  
last_modified_at: 2024-10-12 14:00 +0800  
tags: [Course Note]  
math: true  r
toc: true  
excerpt: "机器人学课程一些疑难点的解决"
---

# 第三章 机器人运动学（p40）

位置与姿态描述：位置矢量、旋转矩阵。

位置矢量

$${^A}P=\begin{bmatrix}
  P_{x}\\
  P_{y} \\
  P_{z}  \\
  \end{bmatrix}$$

旋转矩阵

$${^A_B}R=\begin{bmatrix}
  {^A}\hat{X}{_B}&  
  {^A}\hat{Y}{_B}& 
  {^A}\hat{Z}{_B} 
  \end{bmatrix}$$

平移变换

$${^A}P={^B}P+{^A}P{_{BORG}}$$

旋转变换

$$\hat{X}_A=\hat{X}_B$$
$$\hat{Y}_A=\hat{Y}_B cos{\theta}-\hat{Z}_B sin{\theta}$$
$$\hat{Z}_A=\hat{Y}_B sin{\theta}+\hat{Z}_B cos{\theta}$$

$$\bold{R}(x,\theta)=\begin{bmatrix}
  1&0&0\\ 
  0&cos{\theta}&-sin{\theta}\\
  0&sin{\theta}&cos{\theta}
  \end{bmatrix}$$

考虑XYZ， YZX，ZXY的轮换，可以得到

$$\bold{R}(y,\theta)=\begin{bmatrix}

  cos{\theta}&0&sin{\theta}\\
  0&1&0\\
  -sin{\theta}&0&cos{\theta}
  \end{bmatrix}$$

$$\bold{R}(z,\theta)=\begin{bmatrix}
  cos{\theta}&-sin{\theta}&0\\
  sin{\theta}&cos{\theta}&0\\
  0&0&1
  \end{bmatrix}$$