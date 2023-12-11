---
title: 读书笔记：通过核相关和图池化来挖掘点云的局部结构
excerpt: 
categories:
  - 点云特征
tags:
  - 读书笔记
  - 点云特征
  - PointNet
  - Kernel Correlation
  - Graph Pooling
  - ShapeNet
  - ModelNet
date: 2023-11-28
updated: 2023-11-28
toc: true
typora-root-url: D:\Projects\Github\zhuyuanxiang\hexo_pages\hexo-starter\source\_posts\
---

## 3.2. 局部特征结构上的学习

![图2：我们的KCNet架构。](images/机器学习/计算机视觉/三维处理/点云/点云特征/FoldingNet/image-20231204170629437.png)

图2中网络前面生成的K-NNG即可以用于核相关化生成，还可以通过图最大池化操作生成局部特征结构。
形成局部曲面的邻居点通常共享相似的特征模式，因此通过图池化（常用最大池化）操作来聚合其邻域内的每个点的特征。
通过图的池化操作可以得到点的局部特征码，该码可以表示局部曲面聚合后的特征信息。
图的池化操作与PointNet++网络的相似性：每个点$i$的局部邻域类似于PointNet++的簇/段，因此这种图操作可以像PointNet++一样对PointNet的原始结构进行改进。