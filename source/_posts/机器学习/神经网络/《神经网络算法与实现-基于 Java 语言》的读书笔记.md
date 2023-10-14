---
title: 《神经网络算法与实现-基于 Java 语言》的读书笔记
excerpt: 使用 Eclipse 和 Java 学习神经网络，编写基于 Java 的神经网络模型
categories:
  - 编程
tags:
  - Eclipse
  - Java
  - 编程
  - 神经网络
date: 2019-01-30
updated: 2019-01-31
toc: true
toc_sticky: true
---

## 全书总评

- 书本印刷质量 : 5 星。纸张很白，印刷清楚，文字排版合适，基本没有排版过程中引入的错误，阅读不累眼睛。
- 著作编写质量 : 3 星。入门书，看完后可能会对神经网络有个基本概念，但是也可能就只有个基本概念。基本概念描述还是清楚的，还给出了比较好的参考资料。几个例子讲的很浅，好处就是提供了代码，如果有开发方向的需要可以参考。深入学习神经网络还是参考 [《神经网络和机器学习》](https://book.douban.com/subject/5952531/)这本书吧。
- 著作翻译质量 : 4 星。用的都是常用词汇。对于不了解神经网络的读者，通过本书了解一些基本概念，为将来学习其他书籍打下基础；对于了解神经网络的读者不会造成概念混淆。
- 代码质量 : 3 星。基本没错误，但是思路跟书上不完全匹配。比如说 : 感知机一般都是单层的，就是一个神经元，一个偏置量，两个输入的值，一个输出的值，两个权值。但是作者在测试代码中放入两个输入权值，一个输出权值，不明白，后面都是这样的思路，于是只好弃了。

## 读书笔记

### C1. 初识神经网络

基本概念 : 人工神经元，激活函数，权值，偏置，层。

神经网络结构 :

- 神经元连接
    - 单层神经网络 : 单层感知机，自适应机，自组织映射、Elman 网络和 Hopfield 网络。
    - 多层神经网络 : 多层感知机，径向基函数。
- 信号流
    - 前馈神经网络 : 多层感知机，径向基函数。
    - 反馈神经网络 : 单层的 ( Elman 网络和 Hopfield 网络 ) 和多层的 ( 递归多层感知机和 Echo 网络 ) 。

### C2. 神经网络是如何学习的

学习范式 :

- 有监督学习 :
    - 描述 : 已知的数据集与学习得到的结果之间的误差最小，最小的评价基于某个代价函数。
    - 应用 : 图像分类，语音识别，函数逼近和趋势预测。
- 无监督学习 :
    - 描述 : 从已知的数据集提取知识，即将数据集分类，类间距离大，类内距离小，评价基于某个代价函数。
    - 应用 : 聚类分析，数据压缩，统计建模和语言建模。

学习算法 :

- 两个阶段 : 训练和测试。
- 重要细节 : 参数。
- 评价方式 : 误差度量和代价函数。
- 例子 : Perceptron 感知机和 Delta 规则。

### C3. 有监督学习 ( 运用感知机 )

- 单层感知机的作用和局限性 : 解决线性可分问题，不能解决非线性问题。
- 多层感知机 ( MLP ) :
    - 层 :
        - 输入层 :
        - 隐藏层 : 激活函数一般选择双曲正切或者 sigmoid，因为它们是可导的。
        - 输出层 :
    - 学习过程 :
        - 反向传播 : 敏感性反向传播。收敛速度比较慢。
        - Levenberg-Marquardt :

### C4. 无监督学习 ( 自组织映射 )

- 竞争学习或赢家通吃 : 产生最大值的神经元更新它的权值。
- Kohonen 自组织映射 ( Self-Organization Map ) :
    - 一维 SOM : 产生最大值的神经元更新它的权值，与之相邻的神经元以相对较低的学习率更新权值。
    - 二维 SOM : 领域函数判断相邻的神经元，使结构更“组织化”。

### References ( 参考文献 )

Bishop C M. Neural networks for pattern recognition. Oxford university press, 1995.
Duda R O, Hart P E, Stork D G. Unsupervised learning and clustering. Pattern classification, 2001: 517-601.
Freedman D A. Statistical models: theory and practice. cambridge university press, 2009.
Haykin S S, Haykin S S, Haykin S S, et al. Neural networks and learning machines. Upper Saddle River: Pearson, 2009.
