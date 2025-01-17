---
title: PMP-Net++ 通过学习Transformer强化的多步骤点移动路径实现点云补全
excerpt: 点云补全关注不完整三维形状缺失部分的预测。一种常见的策略是根据不完整的输入来生成完整的形状。然而，点云的无序性会降低高质量三维形状的生成，因为在生成过程中，使用提取的潜在编码很难捕获无序点的结构和拓扑细节。因此，我们通过将补全定义为点云的变形过程来解决这个问题。具体来说，我们设计了一个全新的网络（PMP-Net++），以模拟地动的行为，通过移动每个不完整输入的点以获得一个完整的点云，其中点移动路径（Point Moving Path）的总距离满足最短约束。于是，PMP-Net++根据点移动距离的约束，为每个点预测唯一的PMP。该网络在点的级别上学习了严格的和唯一的对应关系，从而提高了预测的完整形状的质量。此外，由于移动点严重依赖于网络学习到的每个点的特征，我们就进一步推导了一个Transformer强化的表示学习网络，这显著地提高了PMP-Net++的补全性能。我们在形状补全方面进行了全面的实验，并且进一步探索了在点云的上采样方面的应用，这证明了PMP-Net++的比最先进的点云补全/上采样方法有了显著的改进。
categories:
  - 点云补全
tags:
  - 点云补全
  - 地动距离
  - 点移动路径
  - PCN
  - Completion3D
  - Transformer
  - 上采样
date: 2023-11-21
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

# Ch01 简介

点云是一种广泛使用的三维形状表示方法，可以很容易地通过深度摄像机或其他3D扫描设备获得。由于三维扫描设备的视角或遮挡的局限性，原始点云通常是稀疏的和不完整的[^1]。因此，对于下游的3D计算机视觉应用，如分类[^2]、[^3]、[^4]、[^5]、[^6]、[^7]、分割[^8]、[^9]等视觉分析[^10]，通常需要一个形状补全/合并过程来生成三维形状中缺失的区域。

在本文中，我们关注由点云表示的三维对象的补全任务，其中缺失部分是由于扫描仪视角的自遮挡造成的。以往的方法大多将点云补全表述为点云生成问题[^1]、[^11]、[^12]、[^13]，其中通常采用编解码器框架从输入的不完全点云中提取隐式编码，并将提取的隐式编码解码为完整的点云。得益于基于深度神经网络的点云学习方法，在这条线上的点云补全方法在过去几年获得了巨大的进展[^1][^13]。然而，利用深度神经网络生成点云仍然是一项困难的任务，因为点云的无序性使得生成模型难以捕获离散点之间详细的拓扑或结构[^13]。因此，基于点云补全的生成模型的性能仍然不理想。

为了提高点云补全性能，本文提出了一种新的深度神经网络PMP-Net++，从一个新的角度来阐述点云补全任务。与直接预测三维空间中所有点的坐标的生成模型不同，PMP-Net++学习将点从源三维形状移动到目标三维形状。通过点移动过程，PMP-Net++学习了源点云与目标点云之间的点级别的对应关系，从而捕获了两个点云之间的拓扑细节和结构关系。另一方面，将点从源移动到目标存在许多可能的解，这将使网络难以很好地训练。因此，为了鼓励网络学习一个独特的最优规划的点移动路径，我们从地动距离（Earth Mover's Distance, EMD）中获得灵感，提出了正则化的Transformer强化的点移动路径网络（Point Moving Path Network, PMP-Net++），该网络的约束为所有点的移动距离（Point Moving Distance, PMD），这个约束保证了源点云与目标点云之间路径规划的唯一性。

图1给出了一个详细的说明。以完成短线$AB$到长线$A'B'$的任务为例，基于生成的神经网络旨在预测$A'B'$的坐标，通常是优化通过倒角距离（CD）或地动距离（EMD）。然而，由于点云数据的离散性，目标矩阵可以说明线$A'B'$拥有多重规划，所有这些规划都将满足CD和EMD的最小损失约束。因此，网络存在多个最优目标，不能引导网络学习短线$AB$和长线$A'B'$之间的详细形状对应关系。相比之下，基于形状变形的神经网络可以在路径约束的指导下，在源输入（$AB$）和目标输出（$A'B'$）之间建立一种直接的和逐个点的对应关系，原因见图1(c)。在网络的输入和输出之间路径约束产生效果，它正则化了点的移动路径，并优化了网络，以预测每个点的唯一位移。在这种情况下，网络的输出是位移，它与每个起点和终点都有局部相关。另一方面，基于生成的网络的输出是坐标矩阵表示的线$A'B'$，其唯一的有监督的源是整体形状约束的CD/EMD，它在输出和目标地面真实值之间生效。

![image-20231121180149897](images/机器学习/计算机视觉/三维处理/点云/点云补全/PMP-Net++%20Point%20Cloud%20Completion%20by%20Transformer-Enhanced%20Multi-step%20Point%20Moving%20Paths/image-20231121180149897.png)

图1：基于生成的方法与基于变形的方法之间的差异，其中的任务是完成一个短线$AB$到一个长线$A'B'$（参见(a)和(b)），进一步说明了路径约束与广泛使用CD/EMD之间的差异（参见(c)）。

此外，为了预测点的移动路径更加准确，我们提出了一种在多尺度搜索半径下连续细化点移动路径的多步骤路径搜索策略。具体来说，如图2所示，路径搜索以从粗到细的方式重复执行多个步骤。每一步都将考虑到前面预测的路径，然后根据前面的路径规划其下一个移动路径。为了记录和聚合点移动路径的历史信息，我们受门控循环单元（GRU）的启发，提出了一种新的循环路径聚合（RPA）模块。它可以记住和聚合每个点的路线序列，并将之前的信息与点的当前位置结合起来，以预测下一步移动的方向和长度。通过逐步减少搜索半径，PMP-Net++可以一致地为每个点细化一个越来越精确的路径，从不完整点云上的原始位置移动到完整点云上的目标位置。

![image-20231121180439909](images/机器学习/计算机视觉/三维处理/点云/点云补全/PMP-Net++%20Point%20Cloud%20Completion%20by%20Transformer-Enhanced%20Multi-step%20Point%20Moving%20Paths/image-20231121180439909.png)

图2：基于从粗到细的搜索半径使用多个步骤的路径搜索。PMP-Net++通过三步移动点$A$到点$A'$，每一步都减少其搜索半径，并且回顾考虑移动历史，以决定下一个移动的位置

本文提出的PMP-Net++是对我们最新工作PMP-Net [^14]的增强扩展。我们发现，在移动过程中学习到的点特征在预测高质量的完整形状中起着关键作用。而PMP-Net中使用的基于PointNet++ [^15]的骨架不能提供更多的鉴别点特征，因为其基于最大池的特征聚合策略[^4]。因此，受最近Transformer在点云表示学习[^16]中成功应用的启发，我们在PMP-Net++中引入了一种新的基于Transformer的框架，以增强网络学习到的点特征，目标是预测每个点更准确的位移。总之，我们工作的主要贡献可以总结如下。

- 我们提出了一种新的点云补全任务网络，称为PMP-Net++，可以将不完整形状上的每个点移动到完整形状上的对应点的位置，以实现高精度的点云补全。与以往的生成补全方法相比，PMP-Net++能够通过点移动路径来学习和预测点级别的对应关系，学习不完整形状和完整形状之间拓扑细节和结构关系。
- 我们提出在不完整的点云和完整的点云之间，利用地动距离约束来正则化网络，从而学习一个唯一的点移动距离规划。于是，网络不会被移动点的多个解所混淆，并在原始点云和目标点云之间最终预测出一个有意义的点级别的对应关系。
- 我们建议以从粗到细的方式搜索具有多步骤的点移动路径。每个步骤都将使用提出的循环路径聚合（RPA）模块，从而通过由前一步路径和当前位置聚合的信息来决定下一步的规划。
- 与我们最新的PMP-Net相比，我们在PMP-Net++中进一步引入了一个Transformer强化的点云网络，以改进点特征学习。我们在Completion3D[^13]和PCN [^12]数据集上进行了全面的实验，并进一步探索了点云上采样应用，所有这些都证明了PMP-Net++相比最先进的点云补全/上采样方法（包括我们最新的PMP-Net）有了显著的改进。

# Ch02 相关工作

在三维重建[^17]、[^18]、[^19]、[^20]和表示学习[^21]、[^22]、[^23]、[^24]中的深度学习技术促进了三维形状补全的研究，补全方法大致可分为两类：(1)传统的三维形状补全方法[^25][^26][^27][^28]通常公式化手搓特征，如：表面平滑性或对称轴来推断缺失的区域，而其他一些方法[^29][^30][^31][^32]则考虑大规模三维形状补全数据集的辅助，通过执行搜索找到类似的补丁来填补不完整的三维形状。(2)另一方面，基于深度学习的方法[^33]、[^34]、[^35]、[^36]利用其强大的表示学习能力，从不完整的输入形状中提取几何特征，并根据所提取的特征直接推断出完整的形状。与传统的补全方法相比，这些可学习的方法不需要预定义的手搓特征，并且可以更好地利用大尺度补全数据集中丰富的形状信息。所提出的PMP-Net++也属于深度学习领域，其中沿此方向的方法可以进一步分类和细化如下。

## 2.1. 体素辅助的形状补全

卷积神经网络（CNN）的表示学习能力已广泛应用于二维计算机视觉研究中，近年来有关二维图像修复的应用研究风起云涌。2D CNN在[^37]、[^38]、[^39]领域的成功可以直接扩展到三维空间，从而将其借鉴为三维形状补全的直观思想。近年来，人们提出了几种基于3D CNN结构的体积辅助形状补全方法。请注意，我们使用术语“体素辅助”来描述这类方法，是因为3D体素通常不是网络的最终输出。相反，预测的体素将被进一步细化，并转换为其他表示形式，如网格[^11]或点云[^40]，以产生更精细的3D形状。因此，体素更像是一个中间辅助工具，以帮助完成网络推断出完整的形状。沿着这个思路出现的值得注意的工作，如3D-EPN [^11]和GRNet [^40]已经提出以粗到细的方式重建完整的3D体素。他们首先在编码器-解码器框架下使用三维CNN预测一个粗糙的完整形状，然后使用从一个补全形状数据集[^11]中选择相似的补丁来细化输出，或根据输出的体素[^40]进一步重建点云的细节。此外，也有一些研究考虑使用纯粹的体素数据来实现形状补全任务。例如，Han等人[^41]提出通过同时推断全局结构和局部几何形状，来预测补全形状的细节，从而直接生成高分辨率的三维体素形状。Stutz等人[^42]提出了一种基于变分自动编码器的方法来在弱监督下补全三维体素。尽管3D CNN在特征学习方面令人动心，但输入体素数据的分辨率的立方计算成本，使得处理细粒度形状[^1]变得困难。

## 2.2. 基于点云的形状补全

近年来，基于点云的形状补全[^1]、[^13]、[^43]、[^44]任务越来越受到关注。由于点云是许多三维扫描设备的直接输出形式，而且点云的存储和处理需要的计算成本比体素数据低得多，因此最近的许多研究考虑对三维点云进行直接补全。从点云表示学习[^15]、[^45]的改进中可以看出，之前的方法，如：TopNet [^13]、PCN [^12]和SA-Net [^1]将解决方案制定为编码器-解码器框架下的生成模型。他们采用PointNet [45]等编码器从不完整点云中提取全局特征，并使用解码器根据提取的特征推断完整点云。与PCN [^12]相比，TopNet [^13]改进了解码器的结构，以便在一个根树架构[^13]中隐式地建模和生成点云。SANet [^1]进一步通过跳跃注意机制将不完整形状的详细几何信息传递为完整形状。其他值得注意的方法，如：RL-GAN-Net[^34]，Render4Completion[^44]和VRCNet [^46]，专注于对抗性学习和变分自动编码器的框架，以提高生成的完整形状的真实性和一致性。近年来，三维形状的逐步细化（如：CRN [^47]，PF-Net [^35]）已成为点云补全研究中的一个流行思想，因为它可以帮助网络生成具有结构细节的三维形状。

综上所述，上述方法大多是点云补全任务的生成式解决方案，不可避免地受点云的无序性影响，使得使用生成式解码器重建结构或者拓扑细节变得困难。因此，为了避免预测无序数据的问题，PMP-Net++使用了一种不同的方法来重建完整的点云，它学习从初始输入中移动所有的点，而不是直接从隐式编码中生成最终的点云。

PMP-Net++的思想也与三维形状变形[^48]的研究有关，它[^48]主要考虑了一步变形。然而，不完整形状和完整形状之间的变形更具挑战性，这需要在没有任何其他先验信息的情况下，推断缺失区域中完全未知的几何形状。相比之下，我们建议采用多步搜索来鼓励PMP-Net++对缺失区域推断出更详细的几何信息，以及通过正则化点移动距离，以保证多步推理的有效性。

# Ch03 PMP-Net的架构

提出的PMP-Net++的（概述见图4）由三部分组成：

1. 使用编码器抽取点云特征
2. 使用特征传播模块（Feature Propagation Module，FP-Module）预测每个点的点移动路径
3. 使用RPA模块循环地融合和聚集当前步骤的点特征与前一步的路径信息

每个部分的细节描述如下。

![image-20231121180758767](images/机器学习/计算机视觉/三维处理/点云/点云补全/PMP-Net++%20Point%20Cloud%20Completion%20by%20Transformer-Enhanced%20Multi-step%20Point%20Moving%20Paths/image-20231121180758767.png)

图3：第$k$步PMD-模块的结构细节。主要由三个部分组成：(1)点云编码器；(2)特征传播模块（FP-模块）抽取每个点的特征；(3)RPA模块用于循环学习和遗忘前一步的路径搜索信息

![image-20231121181002766](images/机器学习/计算机视觉/三维处理/点云/点云补全/PMP-Net++%20Point%20Cloud%20Completion%20by%20Transformer-Enhanced%20Multi-step%20Point%20Moving%20Paths/image-20231121181002766.png)

图4：基于由粗到细的搜索半径使用多个步骤的路径搜索。PMP-Net++通过三步移动点$A$到点$A'$，每一步减少其搜索半径，并且回顾考虑移动历史，以决策下一个移动的位置。

## 3.1. 预测点的位移

### 3.1.1. 多步骤架构

提出的PMP-Net++（概述见图4）给定了一个输入点云$P=\{\mathbf{p}_i\}$和一个目标点云$P'=\{\mathbf{p}'_ j\}$，其目标是预测一个位移向量集$\Delta P=\{\Delta\mathbf{p}_i\}$，基于该向量集可以将每个点从$P$移动到$P'$的位置，使$\{(\mathbf{p}_i+\Delta\mathbf{p}_i)\}=\{\mathbf{p}'_j\}$。PMP-Net++总共用$k=3$步移动每个点$\mathbf{p}_i$。步骤$k$的位移向量用$\Delta\mathbf{p}_i^k$表示，所以$\Delta\mathbf{p}_i=\sum_{k=1}^3\Delta\mathbf{p}_i^k$。对于步骤$k$，网络将最后一步$k−1$的变形点云$\{\mathbf{p}^{k−1}_i\}=\{\mathbf{p}_i+\sum^{k−1}_{j=1}\Delta\mathbf{p}^j_i\}$作为输入，并根据输入点云计算新的位移向量。因此，预测的形状将一步步地细化，最终产生一个高质量的完整形状。

### 3.1.2. Transformer增强的位移预测

在第$k$步，为了预测每个点的位移向量$\Delta\mathbf{p}^k_i$，我们首先从点云中提取每个点的特征。在之前的实现PMP-Net [14]时，首先采用PointNet++[15]基础框架提取输入的三维形状的全局特征，然后使用特征传播模块传播将全局特征传播到三维空间的每个点上，最终为每个点$\mathbf{p}^k_i$输出其对应的特征$\mathbf{h}^{k,l}_i$。在PMP-Net++中，我们采用最近成功实现的Transformer[49]来强化由PointNet++学习得到的点特征，在其中我们遵循Point Transformer[16]的实践成果，并在每个PointNet++的集合抽象（Set Abstraction, SA）层之间添加一个额外的Transformer模块。PMP-Net和PMP-Net++的结构细节之间的比较如图5所示。具体来说，在PMP-Net++中，由上一层集合抽象（SA）层学习到的局部特征（为了方便表示为$\{x_{in}\}$）被输入到后序的Transformer模块。在Transformer模块中，$\{x_{in}\}$作为计算自注意的“键”（key）和“查询”（query），根据自注意力计算，通过多个MLP和逐元素的操作得到一个新的局部特征集合$\{x_{out}\}$。请注意，图5(c) 中编码的位置用于引导网络学习不同局部特征之间的空间关系。我们遵循与之前的工作[16]相同的做法，采用可学习的位置编码，学习参数依赖于两点$p_i$和$p_j$之间的三维坐标：
$$
\begin{equation}
\epsilon=\text{MLP}(p_i-p_j|\theta_\epsilon)
\end{equation}
$$
由于我们的实验实现应用了三个层次的特征传播来层次化地生成每一个点的特征（见图3），我们在$\mathbf{h}^{k,l}_i$使用上标$k$来表示步骤，下标$l$来表示层。然后，将每个点的特征$\mathbf{h}^{k,l}_i$与随机噪声向量$\hat{x}$连接，根据[48]所述该噪声向量可以对点产生微小的干扰，迫使它离开原来的位置。然后，将步骤$k$和第$3$级的最后一个点特征$\mathbf{h}^{k,3}_i$输入多层感知器（MLP），后接一个$\tanh$激活函数，生成一个三维向量作为点$\mathbf{p}^k_i$的位移向量$\Delta\mathbf{p}^k_i$：
$$
\begin{equation}
\Delta\mathbf{p}^k_i=\tanh(\text{MLP}([\mathbf{h}^{k,3}_i:\hat{x}]))
\end{equation}
$$
其中冒号“:”表示拼接操作。

![image-20231122101307284](images/机器学习/计算机视觉/三维处理/点云/点云补全/PMP-Net++%20Point%20Cloud%20Completion%20by%20Transformer-Enhanced%20Multi-step%20Point%20Moving%20Paths/image-20231122101307284.png)

图5：在PMD模块中使用的编码器架构。而且，我们还对比了先前的工作与Transformer的结构细节。

### 3.1.3. 不同步骤之间的循环信息流

前面的移动的信息对于网络决定当前的移动是至关重要的，因为前面的路径可以用来推断单个点的最终目的地的位置。此外，这些信息可以引导网络找到下一步移动的方向和距离，防止网络在点移动路径搜索的多个步骤中改变目的地。为了实现这一目标，我们建议在每一步和每一层的特征传播模块之间使用一个特定的RPA单元，用于记忆之前路径的信息，并推断每个点的下一个位置。如图3所示，RPA模块（步骤$k$，层$l$）将第$k-1$层的输出作为输入，然后输出$\mathbf{f}^{k,l-1}_i$，并将其与特征$\mathbf{h}^{k-1,l}_i$（步骤$k-1$，层$l$）合并，输出当前层的特征$\mathbf{h}^{k,l}_i$（步骤$k$，层$l$）：
$$
\begin{equation}
\mathbf{h}^{k,l}_i=\text{RPA}(\mathbf{f}^{k,l-1}_i,\mathbf{h}^{k-1,l}_i)
\end{equation}
$$
RPA模块的结构细节如上所述。

## 3.2. 循环路径聚合

路径聚合模块的详细结构如图6所示。之前的点移动路径可以看作是顺序数据，其中每个移动的信息应该在这个过程中被选择性地记忆或遗忘。遵循这个想法，我们从循环神经网络获得灵感，模仿门控制循环单元（GRU）计算更新门$z$和重置门$r$从而编码和遗忘信息，计算的依据是点特征$\mathbf{h}^{k-1,l}_i$（步骤$k-1$，层$l$）和点特征$\mathbf{f}^{k,l-1}_i$（步骤$k$，层$l-1$）。两个门的计算可以表示为：
$$
\begin{align}
z=\sigma(W_z[\mathbf{f}^{k,l-1}_i:\mathbf{h}^{k-1,l}_i]+\mathbf{b}_z)\\
r=\sigma(W_r[\mathbf{f}^{k,l-1}_i:\mathbf{h}^{k-1,l}_i]+\mathbf{b}_r)
\end{align}
$$
其中，$W_z,W_r$是权重矩阵，$\mathbf{b}_z,\mathbf{b}_r$是偏差。$\sigma$是`sigmoid`激活函数，用于预测一个$0$到$1$之间的值，表示信息通过门的比例。冒号“:”表示两个特征的拼接。

与标准GRU不同，在GRU中计算当前步骤$k$的输出特征$\mathbf{h}^{k,l}_i$时，更强调对先前信息的保留，在RPA中则更重视对当前输入信息的保留。RPA计算输出特征$\mathbf{h}^{k,l}_i$的公式为：
$$
\begin{equation}
\mathbf{h}^{k,l}_i=z\odot\hat{\mathbf{h}}^{k,l}_i+(1-z)\odot\mathbf{f}^{k,l-1}_i
\end{equation}
$$
其中，$\hat{\mathbf{h}}^{k,l}_i$是当前步骤的中间特征，它包含了从过去开始的所有保留信息，是依据当前输入特征计算得到的，公式如下：
$$
\begin{equation}
\hat{\mathbf{h}}^{k,l}_i=\varphi(W_h[r\odot\mathbf{h}^{k-1,l}_i:\mathbf{f}^{k,l-1}_i)]+\mathbf{b}_h)
\end{equation}
$$
其中，$\varphi$是`relu`激活函数。

使用$\hat{\mathbf{h}}^{k,l}_i$而不是$\mathbf{h}^{k-1,l}_i$与$\mathbf{f}^{k,l-1}_i$融合的原因是，与标准的RNN单元相比，在决策下一步移动时点的当前位置影响更大。特别是，当RPA模块需要忽略对当前位置决策不重要的先前信息时，等式(6)可以简单地将更新门$z$设置为零向量从而遗忘所有历史，从而使RPA模块完全关注于当前输入$\mathbf{f}^{k,l-1}_i$的信息。

![image-20231122112551183](images/机器学习/计算机视觉/三维处理/点云/点云补全/PMP-Net++%20Point%20Cloud%20Completion%20by%20Transformer-Enhanced%20Multi-step%20Point%20Moving%20Paths/image-20231122112551183.png)

图6：RPA模块（步骤$k$，层$l$）的详细结构。

## 3.3. 优化后的搜索用于唯一路径

### 3.3.1. 最小化移动距离

如图7所示，点云的无序性质导致输入形状变形为目标形状容许出现多个解，并且对变形后的形状及其基准数据的直接约束（如倒角距离）也不能保证输入点集与目标点集之间建立的对应关系的唯一性。除此之外，网络还会被点移动的多种解所混淆，从而导致无法捕捉到不完整形状和完整形状之间的详细拓扑和结构关系。为了建立一个独特的和有意义的输入点云和目标点云之间的逐点对应关系，我们从地动距离[50]获得灵感，提出了基于所有点移动距离的约束来训练PMP-Net++从而学习在原始点云和目标点云之间的路径规划$\phi$。具体来说，给定源点云$\hat{X}=\{\hat{x}_i|i=1,2,3,\dots,N\}$和目标点云$X=\{x_i|i=1,2,3,\dots,N\}$，遵循EMD学习路径规划$\phi$，要求满足以下约束：
$$
\begin{equation}
\mathcal{L}_{\text{EMD}}(\hat{X},X)=
\min_{\phi:\hat{X}\rightarrow X}\frac1{\hat{X}}
\sum_{\hat{x}\in\hat{X}}\|\hat{x}-\phi(\hat{x})\|
\end{equation}
$$
在等式中(8)，$\phi$被看作是使$\hat{X}$和$X$中对应点之间的平均距离最小的双射。

这里，两个点云之间的唯一对应关系可以通过EMD（等式(8)）的定义来保证，解释如下：(1)将点云A变形为点云B（点数相同的点）等价于在点云A与B之间建立双射$\phi$；(2)当点位移之和达到最小值时，双射$\phi$是唯一的；(3)优化等式化(8)是为了最小化点位移的和，这将产生一个唯一的对应关系，遵循由输入点云确定的数据顺序。

![image-20231122142535275](images/机器学习/计算机视觉/三维处理/点云/点云补全/PMP-Net++%20Point%20Cloud%20Completion%20by%20Transformer-Enhanced%20Multi-step%20Point%20Moving%20Paths/image-20231122142535275.png)

图7：当输入点云（绿）变形为目标点云（红）时多个解的说明。PMD约束保证了输入点云与输出点云在点层级上对应关系的唯一性(a)，并且过滤了移动点时的其他解(b)

根据等式(8)，由网络建立的双射$\phi$应实现将点从输入形状移动到目标形状的最小移动距离。然而，即使输入点云和目标点云之间的对应关系是唯一的，但源点和目标点之间仍然存在不同的路径，如图8所示。因此，为了鼓励网络学习一个最优的点移动路径，我们选择最小化点移动距离损失（$\mathcal{L}_{PMD}$），它是通过PMP-Net中所有三步移动后得到的所有位移向量$\{\Delta\mathbf{p}^k_i\}$输出的和。点移动距离（Point Moving Distance, PMD）损失的公式为：
$$
\begin{equation}
\mathcal{L}_{PMD}=\sum_k\sum_i\|\Delta\mathbf{p}^k_i\|_2
\end{equation}
$$
等式(9)比EMD约束更为严格。它不仅要求所有点的整体位移达到最短的距离，而且还限制了每一步的点移动路径为最短的距离。因此，在每一步中，我们将鼓励网络按照之前的方向搜索新的路径，如图8所示，这将减少冗余的移动决策，提高搜索效率。

### 3.3.2. 多尺度搜索路径

PMP-Net++以从粗到细的方式搜索点移动路径。对于每一步，PMP-Net++将按$10$的幂次方减少移动的最大步幅，即对于步骤$k$，在等式(2)中计算的位移$\Delta\mathbf{p}^k_i$被限制为$10^{-k+1}\Delta\mathbf{p}^k_i$。这使得网络在训练期间能够更快地收敛。同时，减少的搜索范围将保证网络在下一步不会推翻其在上一步中做出的决定，特别是对于长距离移动。因此，它可以防止网络在路径搜索过程中做出冗余决策。

## 3.4. 稠密点云补全的扩展

点云的变形不能直接用于增加点的数量。因此，输入点云和输出点云必须具有相同的分辨率。这种基于PMP-Net++的变形特性在处理不同点数的三维形状时可能会是一个问题。为了解决这一问题，我们通过向每一步的输入中添加噪声，将PMP-Net++扩展到密集的点云补全场景。为了实现这一目标，在步骤$k$中，将输入点云$\{\mathbf{p}^k_i\}$与符合标准正态分布$N(0,1)$的噪声向量$\hat{n}$拼接起来：
$$
\begin{equation}
\{\mathbf{p}^k_i\}\leftarrow\{[\mathbf{p}^k_i:\hat{n}]\},\hat{n}\sim N(0,1)
\end{equation}
$$
因此，每次输入网络时，点云都与之前的数据不同。然后，通过多次变形点云，并将变形结果重叠在一起，可以得到比原始输入点更多的稠密点云。

请注意，等式(2)中的噪声是使这些点远离其在三维空间中的原始位置，而在每一步的输入中使用的噪声的是改变最终的变形结果。

### 更多讨论

通常在基于生成的方法中，扩展到稠密点云补全的关键思想是增加输入点的数量。对于PMP-Net，我们复制输入点，并将之与随机噪声进行拼接，以增加点的数量。而对于其他方法，如：TopNet [13]和PCN [12]，它们还需要复制特征，并与二维网络编码拼接，以增加输入点。PMP-Net++与其他方法的根本区别在于，复制操作是在网络的不同阶段完成的。

请注意，如果我们简单地重复点坐标，并不带额外操作地移动它们，那么重复的点将产生完全相同的位移。因此，为了解决这个问题，在PMP-Net++中，我们在每个点特征上添加噪声，以迫使它们移动到不同的地方。因为噪声增强的输入点在运动之前已经位于不同的空间位置，这些复制结果将不会被移动到相同的目标点。

## 3.5. 训练的损失函数

利用倒角距离（CD）和地动距离（EMD），通过完整的基准点云正则化变形后的形状。遵循与等式(8)中相同的符号，倒角距离（CD）的定义如下：
$$
\begin{equation}
\mathcal{L}_{CD}(X,\hat{X})=
\sum_{x\in X}\min_{\hat{x}\in\hat{X}}\|x-\hat{x}\|+
\sum_{\hat{x}\in\hat{X}}\min_{x\in X}\|\hat{x}-x\|
\end{equation}
$$
用于训练的完整损失函数定义如下：
$$
\begin{equation}
\mathcal{L}=\sum_k\mathcal{L}_{CD}(P^k,P')+\mathcal{L}_{PMD}
\end{equation}
$$
其中，$P^k$表示第$k$步的点云输出，$P'$表示目标完整点云。请注意，找到最优$\phi$的代价非常高。在实验中，我们遵循[^12]简化算法来估计$\phi$的近似值。

# Ch04 实验

在本节中，我们首先在通用的补全基准数据集PCN[12]和Completion 3D[13]上评估了PMP-Net++。然后进一步探索了PMP-Net++在点云上采样任务中的潜在应用。最后，将通过综合消融研究对PMP-Net++各部分的有效性进行定量评估。

## 4.1. 配置细节

我们使用PointNet++的单尺度分组（Single Scale Grouping, SSG）版本及其特征传播模块作为PMP-Net的基本框架。各部分的详细体系结构分别见表1和表2。

表1：编码器的详细结构

![image-20231122175142317](images/机器学习/计算机视觉/三维处理/点云/点云补全/PMP-Net++%20Point%20Cloud%20Completion%20by%20Transformer-Enhanced%20Multi-step%20Point%20Moving%20Paths/image-20231122175142317.png)

在表1中，“#Points”表示下采样点的数量，“Radius”表示球查询的半径，“#Sample”表示每个中心点采样的邻居点数，“MLPs”表示编码器各级MLPs的输出通道数。

表2：特征传播模块的细节架构

![image-20231122175238527](images/机器学习/计算机视觉/三维处理/点云/点云补全/PMP-Net++%20Point%20Cloud%20Completion%20by%20Transformer-Enhanced%20Multi-step%20Point%20Moving%20Paths/image-20231122175238527.png)

我们使用Adam优化器训练PMP-Net++，初始学习速率为$10^{−3}$，并在每20个周期以$0.5$倍指数衰减。训练过程使用一个NVDIA GTX 2080TI GPU完成，`batch_size`大小为24。PMP-Net++需要150个周期来收敛于PCN和Completion3D数据集。我们将Completion3D的所有输入训练形状缩放$0.9$倍，以避免出现在$\tanh$激活范围之外的点。

请注意，目前Transformer只涉及到编码器，当在网络的解码器中添加Transformer时，我们没有观察到显著的性能改善。由于解码器中的特征传播模块（Feature Propagation，FP）的关键操作是三线性插值（这是不可学习的），因此由解码器产生的点的特征的质量主要依赖于编码器。因此，在解码器中增加额外的Transformer可能对网络提高点特征的质量帮助不大。

## 4.2. 在 PCN 数据集上的点云补全

### 4.2.1. 数据集与评价指标

结果表明，在稀疏点云上学习到的PMP-Net++可以直接应用于稠密点云的补全。具体来说，我们持续在2,048个点的稀疏形状上训练PMP-Net++，并通过在PCN数据集[12]上预测16,384个点的密集完整形状来揭示其泛化能力。PCN数据集来源于ShapeNet数据集，其中每个完整的形状包含16,384个点。部分形状有不同的点数，所以我们首先通过随机复制点将超过2048点的形状下采样到2048，将不到2048点的形状上采样到2048。由于PMP-Net++学习移动点而不是生成点，因此它需要不完全点云中相同数量的点和完整的点。为了预测16,384个点的完整形状，我们在测试过程中对每个形状重复8次预测，每次移动2,048个点。需要注意的是，PMP-Net++仍然在具有2048个点的稀疏点云上进行训练，这些点云是从PCN数据集的密集点云中采样的。

在PCN数据集上，我们使用每个点L1倒角距离（CD）作为评价指标，这是按点数对等式(11)中的CD取平均值。

### 4.2.2. 定量比较

根据表3中的对比，PMP-Net++的性能与最先进的方法[47]相当，并且在PCN数据集上排名第一。Wang等人[47]的结果引用自其原始论文，而其他比较方法的结果均引用自[40]。请注意，大多数基于生成的方法（如表3中的Wang等人[47]和GRNet[40]）专门设计了一个从粗到细的生成过程，以便在稠密点云补全上获得更好的性能。相比之下，我们在2048个点上训练的PMP-Net++可以通过简单地复制点移动过程直接生成任意数量的稠密点，并且仍能获得与对应方法相当的结果。此外，我们消除了训练过程中的PMD损失，通过比较没有PMD变化的基线PMP-Net++，进一步讨论了PMD损失的有效性。从表3中可以看出，PMD损失有效地提高了PMP-Net++的性能，这与我们的观点一致，即点移动路径应该进行正则化，以更好地捕捉三维形状的详细拓扑和结构。

表3：在PCN数据集上依据每个点$L1倒角距离\times10^3$得出的点云补全率（越低越好）

![image-20231122180627760](images/机器学习/计算机视觉/三维处理/点云/点云补全/PMP-Net++%20Point%20Cloud%20Completion%20by%20Transformer-Enhanced%20Multi-step%20Point%20Moving%20Paths/image-20231122180627760.png)

### 4.2.3 定性比较

在图9和图10中，我们通过直观地比较PCN数据集上的补全结果，进一步展示了PMP-Net++相比其他方法的优势。具体来说，在图9中，我们将PMP-Net++与其他跨不同对象类别的对应对象进行了比较。例如，在图9的第三行中，任务是预测一个不完整的灯的完整形状。在这项任务中，大多数评估方法未能保留灯柱的详细几何形状，其中许多噪声点分布在灯柱周围。与图9中的生成方法相比，我们的PMP-Net++可以显示一个高质量的灯柱。此外，当比较PMP-Net++和PMP-Net时，我们可以发现PMP-Net++在灯柱的底部和顶部实现了更好的形状预测。PMP-Net++的推理和保留形状细节的优势也可以通过第四行的观察得到很好的证明：通过比较椅背的重建结果，我们可以发现PMP-Net++可以清晰地保留椅背上每根梁的详细形状；通过比较椅腿的完成结果，PMP-Net++的结果也比其他任何同类产品更接近基准数据。此外，对PMP-Net和PMP-Net++的沙发进行比较，直观地揭示了PMP-Net++相对于PMP-Net的改进。PMP-Net预测的完整沙发在沙发缺失区域的分布比PMP-Net++少。

![image-20231122181425456](images/机器学习/计算机视觉/三维处理/点云/点云补全/PMP-Net++%20Point%20Cloud%20Completion%20by%20Transformer-Enhanced%20Multi-step%20Point%20Moving%20Paths/image-20231122181425456.png)

图9：在PCN数据集上可视化先前的点云补全方法的对比

![image-20231122181614257](images/机器学习/计算机视觉/三维处理/点云/点云补全/PMP-Net++%20Point%20Cloud%20Completion%20by%20Transformer-Enhanced%20Multi-step%20Point%20Moving%20Paths/image-20231122181614257.png)

图10：在PCN数据集上可视化使用PMP-Net++的更多补全结果

## 4.3 在Completion3D数据集上的点云补全

### 4.3.1. 数据集与评价指标

我们在广泛使用的三维点云补全基准（即Completion3D[^13]）上评估了我们的PMP-Net++，这是一个来自ShapeNet数据集的大规模三维对象数据集。部分三维形状是通过将部分视图中的2.5D深度图像反向投影到三维空间中而生成的。Completion3D数据集关注的是稀疏点云的补全任务，对ShapeNet数据集中每个完整的三维对象只生成一个部分视图，并从完整形状和部分形状两种网络表面采样2048个点。我们遵循在Completion3D中训练/验证/测试配置以便公平地与其他方法比较。

根据之前的研究[1]，[12]，[13]，[40]，我们使用每个点的L2倒角距离（CD）作为Completion3D数据集的评价度量。L2倒角距离是按照点数对等式(11)中的CD取平均值，其中等式(11)中的L1-范数被替换为L2-范数。

### 4.3.2. 定量比较

PMP-Net++与其他最先进的点云补全方法的定量比较结果如表4所示，其中PMP-Net++在所有类别的平均倒角距离方面都取得了最好的性能。在Completion3D排行榜上发布的第二好的方法是VRCNet [46]，其平均CD达到8.12，PMP-Net++将这些最先进的平均CD性能提高了0.17。当考虑每个类别的性能时，PMP-Net++在所有比较对应方法中8个类别中的7个取得了最好的结果，这证明了PMP-Net++在不同形状类别中具有更好的泛化能力。需要注意的是，与PMP-Net相比，PMP-Net++显著降低了Completion3D数据集的平均CD损失13.8%，并且在每个类别CD的所有8个类别上都优于PMP-Net。正如我们在第2节中讨论的，GRNet [40]是一种体素辅助形状补全方法，其补全过程中使用的信息来自两个不同的数据形式（即：点云和体素）。PMP-Net++利用了点云中丰富的几何信息，获得比GRNet具有更好的性能，证明了其有效性。表4中的SA-Net [1]等其他方法都是典型的完全基于点云的生成类补全方法，而PMP-Net++对这些方法的显著改进证明了基于变形的解在点云补全任务中的有效性。

表4：在Completion3D数据集上依据每个点$L2倒角距离\times10^4$得出的点云补全率（越低越好）

![image-20231122183057583](images/机器学习/计算机视觉/三维处理/点云/点云补全/PMP-Net++%20Point%20Cloud%20Completion%20by%20Transformer-Enhanced%20Multi-step%20Point%20Moving%20Paths/image-20231122183057583.png)

### 4.3.3. 定性比较

在图11中，我们将PMP-Net++与其他Completion3D数据集进行了可视化比较，从中我们可以发现PMP-Net++在各种形状类别上预测了更准确的完整形状，而其他方法可能会对某些输入形状输出一些失败情况。例如，图11（第三行）中的输入“桌子”失去了一半的腿和曲面。GRNet和PMP-Net的完成结果几乎预测了完整桌面的正确整体形状，但没有重建干净的腿和光滑的表面等详细的结构。另一方面，像FoldingNet[51]、PCN [12]、TopNet [13]和SA-Net [1]这样的方法打算修复桌面，但无法预测一个完整的整体形状。此外，基于变形的PMP-Net++相对于生成方法的优势可以通过图11（第二行）中的“椅子”案例得到很好的证明。生成方法，特别是像GRNet这样，成功地学习了输入椅子的完整结构，但是在重建椅背的梁时失败了，这个梁还是输入形状的残留部分。另一方面，基于变形的PMP-Net++可以通过移动少量点来在特定区域进行补全，从而直接保留了输入的形状，并保持输入形状不变。在图12中，我们进一步可视化了PMP-Net的更多完成结果。

![image-20231122183251689](images/机器学习/计算机视觉/三维处理/点云/点云补全/PMP-Net++%20Point%20Cloud%20Completion%20by%20Transformer-Enhanced%20Multi-step%20Point%20Moving%20Paths/image-20231122183251689.png)

图11：在Completion3D数据集上可视化先前的点云补全方法的对比

![image-20231122183847264](images/机器学习/计算机视觉/三维处理/点云/点云补全/PMP-Net++%20Point%20Cloud%20Completion%20by%20Transformer-Enhanced%20Multi-step%20Point%20Moving%20Paths/image-20231122183847264.png)

图12：在Completion3D数据集上可视化使用PMP-Net++的更多补全结果

### 4.3.4. ScanNet椅子的扩展

表5：ScanNet椅子的定量评价

![image-20231122185057883](images/机器学习/计算机视觉/三维处理/点云/点云补全/PMP-Net++%20Point%20Cloud%20Completion%20by%20Transformer-Enhanced%20Multi-step%20Point%20Moving%20Paths/image-20231122185057883.png)

为了在点云补全任务上评估PMP-Net++的泛化能力，我们在Completion3D数据集上预先训练了PMP-Net++，还在ScanNet数据集的椅子补全上评估了没有微调的PMP-Net++性能，并与GRNet（这是表4中第二好的方法，也在Completion3D数据集上预先训练）进行了比较。可视化的比较结果如图14所示。由于Scannet数据集没有基准数据，我们通常遵循了[12]中报告的部件指标（保真度和MMD），基于图14中待定的椅子进行了评价，详见表5。PMP-Net++以比GRNet更少的噪声补全了形状，这得益于它的点移动操作。因为Completion3D和ScanNet之间的数据分布差异将不可避免地混淆网络，所以基于PMP-Net++的点移动可以简单地选择在对象的残余部分留下这些点在其自己的位置上，从而保持更好的形状，相比之下，基于生成的GRNet必须为对象的残余部分和缺失部分预测新点。

![image-20231122184302064](images/机器学习/计算机视觉/三维处理/点云/点云补全/PMP-Net++%20Point%20Cloud%20Completion%20by%20Transformer-Enhanced%20Multi-step%20Point%20Moving%20Paths/image-20231122184302064.png)

图14：在ScanNet椅子案例上可视化比较PMP-Net++和GRNet。

## 4.4. 点云上采样

### 4.4.1. 数据集和评价指标

在本节中，我们通过实验证明了PMP-Net++在其他类似任务上的有效性，即点云上采样任务。PMP-Net++在此任务中遇到的场景与稠密点云补全相同，其中的困难的是如何增加点的数量，以揭示更详细的三维模型几何信息。因此，我们在上采样任务上采用PMP-Net++稠密补全版本。为了与之前的对应方法进行公平的比较，我们遵循PU-GAN相同的做法，使用其数据集和实验配置进行评估。根据PU-GAN，该数据集是来自PU-Net、MPU和Vision-air仓库的数据集，该数据集包括147个模型的集合，在其中选择120个模型进行训练，其余27个模型用于测试。采用常用的倒角距离（CD）和Hausdorff距离作为我们的评价度量。

### 4.4.2 定量与定性比较

定量比较如表6所示，从中我们可以发现，我们的PMP-Net++在被比较的对手中表现最好。请注意，在点云上采样任务中，我们使用与PCN数据集上的点云补全任务完全相同的网络设置和结构。因此，我们的网络在点云上采样上较好的结果证明了PMP-Net++对各种任务的泛化能力。

表6：在点云上采样任务上的定量比较

![image-20231123105126606](images/机器学习/计算机视觉/三维处理/点云/点云补全/PMP-Net++%20Point%20Cloud%20Completion%20by%20Transformer-Enhanced%20Multi-step%20Point%20Moving%20Paths/image-20231123105126606.png)

在图13中，我们通过直观地比较我们的网络与PU-GAN，进一步说明了PMP-Net++更好的性能。例如，在完成鸟类头部时（图13的第一行），PMP-Net++的上采样结果的分布点比PU-GAN更均匀。从PU-GAN的结果来看，我们仍然可以观察到鸟的头部有几个洞，而PMP-Net++的结果是，鸟的头部表面没有出现这样不完整的区域。此外，与PU-GAN相比，使用PMP-Net++的上采样点分布更均匀，特别是在图13中第二行的鱼的头部。

![image-20231123105440945](images/机器学习/计算机视觉/三维处理/点云/点云补全/PMP-Net++%20Point%20Cloud%20Completion%20by%20Transformer-Enhanced%20Multi-step%20Point%20Moving%20Paths/image-20231123105440945.png)

图13：点云上采的可视化。最先进的方法PU-GAN[^55]与我们提出的PMP-Net++的对比，说明了在生成更好的形状细节上PMP-Net++所具备的优势。

## 4.5 模型分析

在本小节中，我们分析了PMP-Net++中不同部分的影响，并将其与PMP-Net进行了比较，以分析网络改善的有效性。默认情况下，我们在所有实验中使用PMP-Net [14]中相同的网络位置，为了方便，所有的研究通常都在Completion3D数据集的四类（即飞机、汽车、椅子和桌子）的验证集上进行。

### 4.5.1. 分析RPA模块和PMP损失

我们分析了用PMP-Net++中的其他单元替换RPA模块的有效性。对于PMP损失，我们通过从网络中去除PMP损失来分析其有效性。具体来说，我们开发了六种不同的变体进行比较： (1) `NoPath`：是将RPA模块从网络中删除的变体；(2) `Add`：是在网络中以元素添加层替换RPA模块的变体；用不同的循环单元(3) RNN、(4) LSTM和(5) GRU替换RPA模块的变体。

形状补全结果如表7所示，其中我们报告了PMP-Net和PMP-Net++的结果，以便进行综合比较。从表7中，我们可以发现在两个骨架网络上的基线变形（使用RPA模块）分别获得了最佳的性能，这证明了所提出的RPA模块在不同网络结构上的有效性。在不同类型的单元中，最差的结果是在PMP-Net骨架网络下的添加`Add`模块，而在PMP-Net++主干下，RNN单元产生最差的结果。跨不同骨架网络的Add变形和GRU变形展现的不同性能说明，循环网络结构不能为在PMP-Net++框架的路径搜索过程中生成的序列信息提供鲁棒性的聚合。此外，由于RPA模块起源于GRU单位，在两个骨架网络上比较RPA基线和GRU变形证明了我们设计的RPA模块的有效性，因为这种模块相比GRU单元对当前步骤中包含的信息给予了更多的关注，从而帮助网络在点移动问题上做出更加精准的决定。

通过对PMP-Net和PMP-Net++的骨架之间的比较，可以完全证明新增Transformer单元的有效性，其中PMP-Net骨架的最佳性能仍然比PMP-Net++的最佳性能差，无论它们使用哪种变形。

表7：RPA模块和PMP损失的分析（基线标注为星号“*”）

![image-20231123110025805](images/机器学习/计算机视觉/三维处理/点云/点云补全/PMP-Net++%20Point%20Cloud%20Completion%20by%20Transformer-Enhanced%20Multi-step%20Point%20Moving%20Paths/image-20231123110025805.png)

### 4.5.2. 多步路径搜索的有效性

在表8中，我们分析了不同步长对点云变形的影响，并比较了PMP-Net和PMP-Net++的性能。具体来说，每步之间的搜索半径之比固定为10，然后将使点云变形的步数分别设置为1、2和4。例如，当`step=4`时，搜索半径为$\{1.0, 10^{−1}, 10^{−2}, 10^{−3}\}$；当`step=2`时，搜索半径为$\{1.0, 10^{−1}\}$。通过比较表8中第1、2、3步的结果，我们可以发现，在两个骨架条件下，多步变形点云有效地提高了补全性能。另一方面，在步骤3和步骤4之间的比较表明，多步路径搜索的性能将达到其限制，因为过多的步骤可能会导致路径搜索中的信息冗余。我们也可以注意到，第1步与第2步之间的差距，PMP-Net++(9.45与8.36)比PMP-Net (12.26与11.90)的差距更大。这可能是专用于点Transformer强化模块造成的，它使得PMP-Net++能够预测比PMP-Net更加准确的补全形状。因此，参见表8，基于第1步的输出，PMP-Net++产生的倒角距离为9.45，而PMP-Net为12.26，到了第2步，PMP-Net++的点Transformer强化模块相比PMP-Net可以学得更加有效的几何信息，从而获得更好的性能。

表8：经过不同步骤后的输出效果（基线标注为星号“*”）

![image-20231123142708884](images/机器学习/计算机视觉/三维处理/点云/点云补全/PMP-Net++%20Point%20Cloud%20Completion%20by%20Transformer-Enhanced%20Multi-step%20Point%20Moving%20Paths/image-20231123142708884.png)

### 4.5.3. 对多步搜索的可视化分析

我们在图15中可视化了不同搜索步骤配置下的点变形过程。将最上面一行的3步搜索与其他两种进行比较，3步搜索的椅背上的空白空间形状更清晰，用矩形突出显示，这证明了多步搜索持续细化形状的有效性。此外，从图15的可视化中，我们还发现该网络实际上可以使用一些边缘信息来推断移动点云的目的地。以图15(b)中的椅子为例，我们可以得出以下两个结论：

- 在图15(b)的区域1中，网络将椅子上的点向后移动，以完成缺失的上部，即椅背边缘的点向上方移动，因为它们在几何上更接近缺失的部分。这种点移动模式利用了不完全形状的边缘信息。
- 在图15(b)的区域2中，网络将点从另一个椅子腿移动，以完成缺失的腿。我们可以发现，这些点的移动路径几乎是相互平行的，其中一条腿上的每一个点都被移动到另一条腿上的同一位置。区域2的点移动模式明显遵循两腿上点之间的一对一几何对应关系。

总之，该网络学习的不仅是点之间的一对一几何对应关系，而且在补全过程中还考虑了不完全形状的边界与缺失部分之间的几何距离。考虑到PMD损失侧重于几何对应关系的正则化，但是对学习边界信息则没有约束性，所以PMP-Net++的改进主要来自于几何对应。

![image-20231123142906034](images/机器学习/计算机视觉/三维处理/点云/点云补全/PMP-Net++%20Point%20Cloud%20Completion%20by%20Transformer-Enhanced%20Multi-step%20Point%20Moving%20Paths/image-20231123142906034.png)

图15：基于不同搜索步数的多步搜索展示。第一行为3步补全，第2行为2步补全，依次类推。4步补全与3步补全的效果相同。因为4步补全的搜索半径非常小，因此我们仅在图中展示了1步、2步和3步。

### 4.5.4. 应用于变形的形状

从不完全点云中变形形状并不是PMP-Net++的唯一选择。因此，我们提供并讨论了另一种可能的形状变形选择，即将512个栅格点（角度为8×8×8立方）变形为一个完整的形状。具体来说，为了方便起见，我们使用了一步版本的PMP-Net++，并使用三线性插值将点云特征传播到这些栅格点上。为了生成2048个点，我们将每个栅格点重复4次，并用随机噪声连接起来。因此，鼓励每个栅格点分成4个子点，所有512个网格点最终输出2048个点。其性能和可视化效果分别见表9和图16所示。

表9：栅格点变形的比较

![image-20231123145618500](images/机器学习/计算机视觉/三维处理/点云/点云补全/PMP-Net++%20Point%20Cloud%20Completion%20by%20Transformer-Enhanced%20Multi-step%20Point%20Moving%20Paths/image-20231123145618500.png)

表9显示，栅格点的倒角距离（CD）的均值是$9.98$，虽然较低，但与PMP-Net++相当。我们认为栅格点的有效性来自于有序数据的预测，而更好的PMP-Net++的性能来自于利用了更多的几何信息，这种几何信息是由不完整点云的变形过程提供的。

![image-20231123153208516](images/机器学习/计算机视觉/三维处理/点云/点云补全/PMP-Net++%20Point%20Cloud%20Completion%20by%20Transformer-Enhanced%20Multi-step%20Point%20Moving%20Paths/image-20231123153208516.png)

表16：尺寸$8\times8\times8$的栅格点的变形可视化

### 4.5.5. 搜索半径分析

默认情况下，我们将每一步的搜索半径的比例减小10。在表10中，我们分析了不同配置的搜索半径，并评估了它们对PMPNet++框架性能的影响。此外，我们还测试了两种不同的策略来执行点移动路径搜索，即不减少搜索半径的策略（每一步：$[1.0, 1.0, 1.0]$）和递减比较小的策略（每一步：$[1.0, 0.5, 0.25]$）。基线结果是PMP-Net++的默认设置（每一步：$[1.0, 0.1, 0.01]$）。表10显示，PMP-Net和PMP-Net++在$[1.0, 1.0, 1.0]$时都得到了最差的性能。这是一种非减小策略，其中每一步的搜索半径对网络的权重相同，而其他两种方法所取得的较好的实验结果证明了减小搜索半径策略的有效性。当将$[1.0, 0.5, 0.25]$与$[1.0, 0.1, 0.01]$的策略进行比较时，我们可以发现，越大比例地减小搜索半径越可以提高模型的性能，因为大比例可以更好地防止网络推翻前面步骤中的决策。我们还注意到，当下降的比率变得很大时，PMP-Net++将用表8中的`step=1`来近似网络的行为，结果可能会损害形状补全的性能。

表10：搜索半径的有效性（基线标注为星号“*”）

![image-20231123153912197](images/机器学习/计算机视觉/三维处理/点云/点云补全/PMP-Net++%20Point%20Cloud%20Completion%20by%20Transformer-Enhanced%20Multi-step%20Point%20Moving%20Paths/image-20231123153912197.png)

### 4.5.6. 对不同半径下的点移动路径进行可视化分析

在图17中，我们可视化了在表8中不同半径搜索策略下的搜索过程。通过分析`step=1`的变形输出，我们可以发现PMP-Net++使用的从粗到细的搜索策略可以在早期步骤中学习到如何预测更好的形状，例如：`step=1`时的输出图17(a)要比图17(b)和图17(c)更加完整和有序。

![image-20231123155117605](images/机器学习/计算机视觉/三维处理/点云/点云补全/PMP-Net++%20Point%20Cloud%20Completion%20by%20Transformer-Enhanced%20Multi-step%20Point%20Moving%20Paths/image-20231123155117605.png)

图17：基于搜索半径的不同策略，每一步变形过程的描述。

此外，在早期阶段，一个更好的整体形状预测将使网络更加关注点云的结构细化，可以从图17的`step=3`的比较中得出结论，图17 (a)中使用红色矩形突出的区域比其他两个子图好得多。

### 4.5.7. 噪声向量的维数

在我们的论文中，等式(1)中的噪声向量是用来推动这些点离开它们原来的位置的。在本节中，我们将分析噪声的维数和标准差，这可能会潜在地决定噪声对点的影响。因为噪声向量的维数或标准差减小到0后，网络中不会再有干扰。另一方面，噪声向量的维数或标准差越大，对网络的干扰就越大。在表11中，我们首先分析了噪声向量维数的影响。通过将0维结果与其他结果进行比较，可以得出噪声向量引起的扰动对学习点变形具有重要意义。通过对不同长度噪声向量的性能分析，我们可以发现，向量长度与噪声向量的存在相比，其对网络的性能影响相对较小。

表11：噪声维度的影响（基线标注为星号“*”）

![image-20231123171520339](images/机器学习/计算机视觉/三维处理/点云/点云补全/PMP-Net++%20Point%20Cloud%20Completion%20by%20Transformer-Enhanced%20Multi-step%20Point%20Moving%20Paths/image-20231123171520339.png)

### 4.5.8. 噪声分布的标准差

在表12中，我们展示了PMP-Net++在不同噪声向量标准差下的补全结果。与向量维数的分析类似，我们可以得出结论，标准偏差越大，引起的干扰越大，有助于网络获得更好的补全性能。当标准差达到一定的阈值（根据表12，约为$10^{−1}$）时，噪声向量的影响较弱。

表12：标准差的有效性（基线标注为星号“*”）

![image-20231123172255785](images/机器学习/计算机视觉/三维处理/点云/点云补全/PMP-Net++%20Point%20Cloud%20Completion%20by%20Transformer-Enhanced%20Multi-step%20Point%20Moving%20Paths/image-20231123172255785.png)

### 4.5.9. 效率分析

在表13中，通过对Completion3D数据集的效率分析，我们可以发现PMP-Net++比GRNet拥有更少的参数和更小的计算量（FLOPs），这证明了我们的观点：体素辅助方法可能遭受计算效率低下的问题。此外，PMP-Net++的效率也可与PCN相媲美。由于PCN在其网络指定中采用线性层生成粗点云，即使是单步点云完成，仍然需要大量的计算资源。

表13：比较栅格点变形

![image-20231123173319673](images/机器学习/计算机视觉/三维处理/点云/点云补全/PMP-Net++%20Point%20Cloud%20Completion%20by%20Transformer-Enhanced%20Multi-step%20Point%20Moving%20Paths/image-20231123173319673.png)

# Ch05 结论

本文提出了一种新颖的PMP-Net++方法通过多步骤形状变形实现点云补全。通过多个步骤将点从源点云移动到目标点云，PMP-Net++可以持续细化预测形状的详细结构和拓扑结构，并建立不完整形状和完整形状之间的点层级的形状对应关系。在实验中，我们通过与其他方法在Completion3D基准测试和PCN数据集上的比较，证明了PMP-Net++的优越性，并证明了其在点云上采样任务中的良好性能。

总之，由于对PMP-Net++的研究受到了一定的限制。在本文中，虽然提出了基于PMD损失来正则化三维空间中所有点的移动距离，但由于缺乏点移动方向或最终目的地等有监督的指导，导致网络的输出结果在一些例子中可能并不令人满意。因此，网络在训练过程中可能难以学习最优解。我们认为，解决这个问题的一个潜在方法是进一步探索PMD损失，我们计划在未来的工作中尝试一下。这个解决方案不仅限制了总的移动距离，而且还可以指导网络学习每个移动的方向。
