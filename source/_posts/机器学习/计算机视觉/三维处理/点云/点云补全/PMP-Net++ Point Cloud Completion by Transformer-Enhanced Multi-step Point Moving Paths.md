---
title: PMP-Net 通过学习多步的点移动路径实现点云补全
excerpt: 点云补全任务的目的是预测不完整的三维形状中的缺失部分。一种广泛使用的策略是从不完整的点云中生成一个完整的点云。然而，点云的无序性削弱了生成高质量三维形状的可能，因为离散点的结构和拓扑细节很难通过仅使用隐式代码的生成过程来捕获。在本文中，我们通过从一个新的角度来重新考虑如何解决上述问题，即将预测表述为一个点云的变形过程。具体来说，我们设计了一个新的神经网络，名为PMP-Net，来模拟地壳运动的方式。通过移动不完整输入的每个点来补全点云，其中点移动路径（Point Moving Paths，PMP）的总距离应该是最短的。因此，PMP-Net根据所有点移动距离这个约束，为每个点预测一个唯一的点移动路径。因此，该网络在点水平上学习了一个严格的和唯一的对应关系。从而提高了预测的完整形状的质量。我们在 Completion3D 和 PCN 数据集上进行了全面的实验，并且证明了我们相对于最先进的点云补全方法存在优势。
categories:
  - 点云补全
tags:
  - 点云补全
  - 地动距离
  - 点移动路径
  - PCN
  - Completion3D
date: 2023-11-18
updated: 2023-11-18
toc: true
typora-root-url: D:\Projects\Github\zhuyuanxiang\hexo_pages\hexo-starter\source\_posts\
---

# PMP-Net++ 通过学习Transformer强化的多步骤点移动路径实现点云补全

Wen X, Xiang P, Han Z, et al.  PMP-Net++: Point cloud completion by transformer-enhanced multi-step  point moving paths[J]. IEEE Transactions on Pattern Analysis and Machine Intelligence, 2022, 45(1): 852-867.

[PDF 下载链接](https://arxiv.org/pdf/2202.09507)

[原始代码](https://github.com/diviswen/PMP-Net)

# 摘要

点云补全关注不完整三维形状缺失部分的预测。一种常见的策略是根据不完整的输入来生成完整的形状。然而，点云的无序性会降低高质量三维形状的生成，因为在生成过程中，使用提取的潜在编码很难捕获无序点的结构和拓扑细节。因此，我们通过将补全定义为点云的变形过程来解决这个问题。具体来说，我们设计了一个全新的网络（PMP-Net++），以模拟地动的行为，通过移动每个不完整输入的点以获得一个完整的点云，其中点移动路径（Point Moving Path）的总距离满足最短约束。于是，PMP-Net++根据点移动距离的约束，为每个点预测唯一的PMP。该网络在点的级别上学习了严格的和唯一的对应关系，从而提高了预测的完整形状的质量。此外，由于移动点严重依赖于网络学习到的每个点的特征，我们就进一步推导了一个Transformer强化的表示学习网络，这显著地提高了PMP-Net++的补全性能。我们在形状补全方面进行了全面的实验，并且进一步探索了在点云的上采样方面的应用，这证明了PMP-Net++的比最先进的点云补全/上采样方法有了显著的改进。

