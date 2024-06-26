---

layout:   post

title:   Lasso回归与Ridge回归

subtitle:  L1正则化与L2正则化

date:    2024-03-16

author:   BY LinShengfeng

header-img: img/post-bg-ios9-web.jpg

catalog: true

tags:

- 算法

- 深度学习

- 正则化

- 模型调优

---

# Lasso回归与Ridge回归

Lasso（Least Absolute Shrinkage and Selection Operator）和Ridge回归是线性回归模型中常用的两种正则化方法，它们的主要区别在于正则化项的不同。

- Lasso回归：
  - 在损失函数后面添加了一个**L1范数**作为正则化项。
  - 这个正则化项会惩罚模型参数的绝对值，使得一些不重要的特征参数趋向于0，从而简化模型。
  - Lasso回归既可以用于特征选择，也可以用于特征压缩。
  - 当惩罚项较大时，模型会变得简单，越来越多的特征系数被压缩到0，甚至在惩罚项无限大时，模型只剩下一个常数项。
  - 当惩罚项较小时，模型会变得复杂，但不会像Ridge回归那样所有特征系数都趋向于0。
- Ridge回归：
  - 在损失函数后面添加了一个**L2范数**作为正则化项。
  - 这个正则化项会惩罚模型参数的平方和，使得模型参数趋向于较小的值，但不会使参数为0。
  - Ridge回归主要用于模型压缩，不会丢弃特征，而是使特征系数趋向于较小的值。
  - Ridge回归不会导致特征系数为0，而是使它们无限接近于0。

总结来说，Lasso回归通过L1范数惩罚使一些特征参数为0，简化模型，而Ridge回归通过L2范数惩罚使模型参数趋向于较小的值，但不倾向于使特征参数为0。
