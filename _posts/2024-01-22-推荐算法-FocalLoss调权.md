---
layout:     post
title:      Focal Loss调权思路
subtitle:   如何调节Focal Loss中的两个核心超参数
date:       2024-01-22
author:     BY LinShengfeng
header-img: img/post-bg-ios9-web.jpg
catalog: true
tags:
    - 算法
    - 推荐系统


---

# 前言

为缓解一个数据集中不同类的样本分布不平衡的问题，我们通常给其中的不同类别样本分配不同的Loss权重，以此来让模型有侧重地去学习不同类别的数据。

以往的权重分配方式按照样本类别来分配不同权重，忽视了样本分类的难易程度。

> 什么是样本分类的难易程度？
>
> 以CTR预估问题（典型的二分类问题）为例，正样本标签为1，负样本标签为0，如果我们预测的CTR值非常接近0.5，那么这个样本就是一个难分类样本。

如何在Loss中控制易分类样本权重就是Focal Loss要解决的问题。

# Focal Loss

公式如下：
$$
 \text{Focal Loss}(p_t) = -\alpha_t(1-p_t)^\gamma \log(p_t)
$$
$p_t$的生成过程如下:
$$
p_t = \begin{cases}
p & \text{if the true class label is 1} \\
1-p & \text{otherwise}
\end{cases}
$$
其中，$p$为模型预测的样本得分。

$\alpha$的生成过程与$p_t$类似，如下:
$$
\alpha_t = \begin{cases}
\alpha & \text{if the true class label is 1} \\
1-\alpha & \text{otherwise}
\end{cases}
$$
其中，$\alpha$和$\gamma$为两个超参数，其中前者控制正负样本的权重，后者控制难分样本的权重。

当$\gamma=0,\alpha=0.5$时，Focal Loss就退化为0.5倍的交叉熵Loss。

> 按道理来说，α大于0.5小于1可以增加正样本的权重，降低负样本的权重，γ大于1可以增加难样本权重，减少易分类样本权重。而在实验中证明，γ占据主导地位，当γ变大时， α需要变小才能让效果更好。
>
> 摘自 https://blog.csdn.net/slamer111/article/details/126717021
