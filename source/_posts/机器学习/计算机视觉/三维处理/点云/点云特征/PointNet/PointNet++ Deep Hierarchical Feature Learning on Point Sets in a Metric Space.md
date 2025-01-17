# PointNet++：点集在度量空间中的深度层次特征学习

Qi C R, Yi L, Su H, et al. Pointnet++: Deep hierarchical feature learning on point sets in a metric space[J]. arXiv preprint arXiv:1706.02413, 2017.

[PointNet++: Deep Hierarchical Feature Learning on Point Sets in a Metric Space | Papers With Code](https://paperswithcode.com/paper/pointnet-deep-hierarchical-feature-learning)

# 摘要

在先前的研究中很少有人关注点集的深度学习，而PointNet[^20]就是这个方向的先驱。然而，PointNet的设计无法捕捉由点所在的测度空间产生的局部结构，从而限制其识别细粒度模式和对复杂场景的泛化能力。在本次工作中，我们引入了一个层次神经网络，它递归地将PointNet应用于输入点集的嵌套区域。通过利用测度空间的距离，我们的网络可以随着不断增加的上下文尺度去学习局部特征。随着进一步的观测发现，点集是基于变化的密度进行采样，这就导致了在均匀密度上训练的网络的性能大大降低，于是我们提出新的集合学习层来自适应地组合来自于多个尺度的特征。实验表明，该网络能够高效地、稳健地学习深度点集特征。特别是，在具有挑战性的三维点云基准上，获得了明显优于最先进水平的结果。

# Ch01 介绍

我们关注于分析具有几何性质的点集，是因为这类点集是欧几里德空间中的点的集合。一种特别重要的几何点集是由三维扫描仪捕获的点云，例如：装备了三维扫描仪的自动驾驶汽车。作为一个集合，这样的数据必须对其成员的排列保持不变性。此外，距离测度定义了可能表现出不同性质的局部邻域。例如：不同位置上点的密度和其他属性可能一致——在三维扫描中密度的可变性可以来自透视效果、径向密度变化、运行等等。

在先前的研究中很少有人关注点集的深度学习，而PointNet[^20]就是直接处理点集的先驱。PointNet的基本思想是学习每个点的空间编码，然后将所有的单个点特征聚合成一个全局点云签名。根据PointNet的设计，它无法捕捉由测度产生的局部结构。然而，利用局部结构是成功地使用卷积结构的关键。卷积神经网络（Convolutional Neural Network，CNN）将数据定义在标准栅格上作为输入，并且能够基于多分辨率层次结构以越来越大的尺度分步骤地捕捉特征。在低层神经元上，拥有较小的感受野，而在高层神经元上，拥有较大的感受野。沿着层次结构对局部模式执行抽象的能力允许对未曾见过的样例执行更好地泛化。

我们引入了一个层次神经网络，命名为PointNet++，以分层的方式处理在度量空间中采样的一个点集。PointNet++的总体思想是简单的。首先，通过底层空间的距离度量将点集分割为重叠的局部区域。类似于CNN，我们从小的邻域中提取局部特征，从而捕捉精细的几何结构；这样的局部特征进一步分组为更大的单元，并且被处理以产生更高级别的特征。重复这个过程，直到我们获得了整个点集的特征。

PointNet++的设计必须解决两个问题：

-   如何生成点集的划分
-   如何通过局部特征学习器抽象点集和局部特征

这两个问题是相关的，因为点集的分割必须产生跨越分区的共享结构，以便可以像卷积设置中一样共享局部特征学习器的权重。我们选择PointNet作为局部特征学习器。正如那篇文章中所展示的，PointNet是一种有效的体系结构，可以处理无序的点用于提取语义特征。此外，对于受损的输入数据这种体系结构依然是稳健的。作为一个基本的构建块 ，PointNet抽象了局部的点或者特征的集合为更高级别的表示。在这个视图中，PointNet++递归地在输入集的嵌套划分上应用PointNet。

现在仍然存在一个问题：如何生成点集的重叠划分。每个划分被定义为潜在的欧几里德空间中的一个邻域球，其参数包括了质心坐标和尺度。为了均匀地覆盖整个集合，通过最远点采样（Farthest Point Sampling，FPS）算法在输入点集中选择质心。相比以固定步幅扫描空间的体素CNN（Volumetric CNN），我们的局部感受野既依赖于输入数据，还依赖于度量，因此效率更高、效果更好。

![image-20220111110544582](images/机器学习/计算机视觉/三维处理/点云/点云特征/PointNet/image-20220111110544582.png)

图1：从结构传感器（Structure Sensor）中扫描结果的可视化（左：RGB；右：点云）

然而，由于选择特征尺度的纠结和输入点集的不均匀性，确定局部邻域球的合适尺度是一个更具挑战性，但是也令人关注的问题。我们假设输入点集在不同的区域可能有不同的密度，这在真实的数据中（例如：结构传感器扫描[^18]）是很常见的（参见图1）。我们的输入点集与CNN的区别很大，CNN的输入可以被视为在具有均匀恒定密度的标准栅格上定义的数据。在CNN中，局部划分的尺度对应着核的大小。[^25]表明使用较小的核有助于提高CNN的能力。然而，我们在点集数据上的实验却给出了这个规则的反例。这是因为受采样不足的影响，小的邻域可能是由很少的点组成，这种数据的不充分可能导致PointNet无法稳健地捕获模式。

本文的显著贡献是PointNet++在多尺度上利用邻域来获得稳健性和细节捕捉。在训练过程中对输入随机地暂弃（Dropout），网络自适应地学习在不同尺度上检测到的权重模式，并且根据输入数据组合多尺度特征。实验表明，PointNet++能够高效地、稳健地处理点集。特别是面对具有挑战性的三维点云基准数据时，获得了明显优于最先进水平的结果。

# Ch02 问题陈述

假设$\mathcal{X}=(M,d)$是一个离散的度量空间，它的度量是从欧几里德空间$\mathbb{R}^n$继承而来的，其中$M\subseteq\mathbb{R}^n$是点集，$d$是距离度量。此外，环境的欧几里德空间中$M$的密度可能不是处处均匀的。我们关注的是学习集合函数$f$，这个函数将$\mathcal{X}$作为输入（连同每个点的附加特征一起），生成了对$\mathcal{X}$执行了语义兴趣分级的信息。在实践中，$f$可以是一种分类函数，用于分配标签给$\mathcal{X}$；或者是一种分割函数，用于分配每个点的标签到$M$的每个成员。

# Ch03方法

我们的工作可以看作是PointNet[^20]增加了层次结构的扩展。下面，我们先回顾PointNet（[Sec0301](#3.1. PointNet的回顾：通用连续集合函数的逼近)），然后引入增加了层次结构的PointNet作为扩展。最后，我们提出了PointNet++，它能够在非均匀的采样点集中稳健地学习特征（Sec0303）。

## 3.1. PointNet的回顾：通用连续集合函数的逼近

给定一个无序点集$\{x_1,x_2,\cdots,x_n\},x_i\in\mathbb{R}^d$，我们可以定义一个集合函数$f:\mathcal{X}\to\mathbb{R}$用于映射点集到向量：
$$
\begin{equation}
f(x_1,x_2,\cdots,x_n)=\gamma(\underset{i=1,\cdots,n}{\text{MAX}}\{h(x_i\})
\end{equation}
$$
其中，$\gamma$和$h$通常为多层感知机（Multi-Layer Perceptron，MLP）网络。

等式(1)中集合函数$f$对于输入点排列是不变的，并且可以任意逼近任何连续集合函数[^20]。请注意，$h$的响应可以被解释为点的空间编码（详见[^20]）。

PointNet在一些基准测试中获得了引入瞩目的效果。然而，它缺乏在不同尺度上捕捉局部上下文的能力。我们将在下一节引入一个层次特征学习框架解决这个问题

## 3.2. 层次点集特征学习

虽然PointNet使用单个最大池化操作来聚合整个点集，但是我们的新架构构建了一个点的层次分组，并且沿着层次结构逐步抽象出越来越大的局部区域。

![image-20220111140935967](images/机器学习/计算机视觉/三维处理/点云/点云特征/PointNet/image-20220111140935967.png)

图2：以二维欧几里德空间中的点为例，描述我们的层次特征学习架构及其在集合分割和分类中的应用。这里对单尺度点分组实现了可视化。密度自适应分组详见图3。

我们的层次结构由许多集合抽象层组成（图2）。在每一层，一组点集被处理和抽象，以生成元素更少的新集合。集合抽象层由三个关键层组成：采样层、分组层和PointNet层。

-   采样层：从输入点中选择点集，点集中的点定义了局部区域的质心。
-   分组层：通过寻找质心周围的“邻域”点构建局部区域集。
-   PointNet层：使用一个微型PointNet编码局部区域模式为特征向量。

集合抽象层次以$N\times(d+C)$的矩阵作为输入，该矩阵是具有$d$维坐标的$N$个点和$C$维的点特征组成。它的输出是$N'\times(d+C')$矩阵，该矩阵是具有$d$维坐标的$N'$子采样点和新的汇总局部上下文的$C'$维的特征向量组成。我们将在后续的段落中引入集合抽象层次的层。

### 采样层

给出输入点$\{x_1,x_2,\cdots,x_n\}$，我们迭代地应用最远点采样（Farthest Point Sampling，FPS）去选择点的子集$\{x_{i_1},x_{i_2},\cdots,x_{i_m}\}$，例如：$x_{i_j}$是考虑剩余点的集合$\{x_{i_1},x_{i_2},\cdots,x_{i_{j-1}}\}$中（测度距离上）的最远点。相比随机抽样，在质心数量相同的情况下，该方法对整个点集的覆盖结果更好。相比CNN这种扫描数据分布不可知的向量空间方法，我们的抽样策略以数据依赖的方式生成感受野。

### 分组层

该层的输入是大小为$N\times(d+C)$的点集和大小为$N'\times d$的质心集合的坐标，输出是大小为$N'\times K\times(d+C)$的点集组，其中每个组对应于一个局部区域，$K$是质心点的邻域中点的数目。请注意，$K$在不同的组之间有所不同，但是后续的PointNet层能够将变化的点的数目转换为固定长度的局部区域特征向量。

在卷积神经网络中，像素的局部区域是具有阵列索引的像素组成，并且这些组成像素与原像素之间的距离小于某个曼哈顿距离（核大小）。从度量空间中抽样的点集，其点的邻域由度量距离定义。

球查询寻找查询点半径内的所有点（实现中的设置是$K$的上限）。另一种查询是$K$近邻（$K$ Nearest Neighbor，KNN），其找出的邻域点数目是固定的。相比KNN，球查询的局部邻域保证了固定的区域规模，从而使局部区域特征在空间上更具普遍性，这对于需要局部模式识别的任务（如：语义点标注）效果更好。

### PointNet层

在本层中，输入是大小为$N'\times K\times(d+C)$的数据点的$N'$局部区域。输出中的每个局部区域通过其质心和编码质心邻域的局部特征完成抽象。输出是大小为$N'\times(d+C')$的数据。

首先，局部区域中点的坐标被转换成相对于质心点的局部帧：$x_i^{(j)}=x_i^{(j)}-\hat{x}^{(j)}$，其中$i=1,2,\cdots,K$，$j=1,2,\cdots,d$，$\hat{x}$是质心的坐标。我们使用[Sec0301]()中描述的PointNet基础构建块用于局部模式学习。通过使用相对坐标和点特征，我们可以捕捉局部区域中点到点的关系。

## 3.3. 非均匀采样密度下的稳健的特征学习

如前所述，一个点集在不同的区域具有不均匀的密度是很常见的。这种不均匀性给点集特征学习带来了重大的挑战。在密集数据中学习的特征可能无法泛化到稀疏的采样区域。结果，为稀疏点云训练的模型可能无法识别细粒度的局部结构。

理想情况下，我们希望尽可能仔细地审视点集以捕捉密集采样区域中的最细微的细节。然而，这种仔细审视受到低密度区域的限制，因为局部模式可能被采样的缺陷所破坏。在这种情况下，我们应该在更大的范围内寻找更大规模的模式。为了实现这个目标，我们提出了密度自适应PointNet层（图3），当输入采样密度改变时，这种层学习组合来自不同尺度区域的特征。我们将具有密度自适应的PointNet层的层次网络称为PointNet++。

在Sec0302中，每个抽象层次包含单个尺度的分组和特征抽取。在PointNet++中，每个抽象层次提取多个尺度的局部模式，并且根据局部点密度智能地组合。基于局部区域分组和不同尺度的特征混合的需要，我们提出了两种密度自适应层，如下所示。

### 多尺度分组（Multi-Scale Grouping，MSG）

如图3所示，一个简单但是有效的方法是应用不同尺度的分组层，然后根据PointNet提取每个尺度的特征，以实现对多尺度模式的捕捉。不同尺度的特征拼接起来构成一个多尺度特征。

我们训练网络去学习组合多尺度特征的优化策略。实际操作就是按照每个实例的随机概率执行随机暂弃输入点，称之为随机输入暂弃（Random Input Dropout）。具体而言，对于每个训练点集，我们选择的暂弃比例$\theta$是从$[0,p],p\leq1$中均匀地采样。对于每个点，我们按照概率$\theta$随机地暂弃这个点。实践中，我们设置$p=0.95$去避免生成空的点集。在这样做时，我们依据具有不同的稀疏性（由$\theta$引入）的训练集和变化的均匀性（由暂弃法中的随机性引入）来表示网络。测试期间，我们保留所有可用的点。

### 多分辨率分组（Multi-Resolution Grouping，MRG）

上述的MSG方法计算量大，因为它对每个质心点在大尺度邻域上执行局部PointNet。特别是，通常在最低的层次上质心点的数目最大，因此时间成本是惊人的。

![image-20220111163846642](images/机器学习/计算机视觉/三维处理/点云/点云特征/PointNet/image-20220111163846642.png)

图3：(a)多尺度分组（MSG）；(b)多分辨率分组（MRG）

在这里，我们提出了一个替代方法来避免这种巨量计算，同时仍然保留了根据点的分布性质自适应地聚合信息的能力。在图3(b)中，在某个层次$L_i$上一个区域的特征是两个向量的拼接。一个向量（图3左）是通过汇总使用了集合抽象层次的低一层$L_{i-1}$的每个子区域上的特征获得。另一个向量（图3右）是使用单个PointNet在局部区域上直接处理所有原始点获得的特征。

当局部区域的密度较低时，第一个向量可能不如第二个向量更有价值，因为计算第一个向量的子区域中包含的点更加稀疏，并且更加受到采样不足的影响。在这种情况下，第二个向量的权重更高。 另一方面，当局部区域的密度较高时，第一个向量提供了更加精细的细节信息，因为它具有在较低的层次以较高的分辨率递归地检查的能力。

与MSG相比，这个方法的计算更加高效，因为我们避免了在最低的层次的大规模邻域中进行特征提取。

## 3.4. 用于集合分割的点特征传播

在集合抽象层，原始点集合是二次采样获得的。然而，在语义点标注等集合分割任务中，我们希望获得所有原始点的点特征。一种解决方案是在所有集合抽象层次中，总是将所有的点都采样为质心，然而这样会导致计算成本过高。另一种方法是将特征从二次采样点传播到原始点。

我们采用一种层次传播策略，这种策略使用了基于插值的距离和跨越层次的跳跃链接（见图2）。在特征传播层次，我们从$N_l\times(d+C)$个点到$N_{l-1}$个点传播点特征，其中$N_l$和$N_{l-1}$是集合抽象层次$l$的输入与输出的点集合的大小，并且$N_l\leq N_{l-1}$。我们通过在$N_{l-1}$个点的坐标上插值$N_l$个点的特征值$f$实现特征传播。在插值的众多选择中，我们基于$k$个最近邻使用反向距离的加权平均（见等式2，默认值$p=2,k=3$）。在$N_{l-1}$个点上的插值特征与集合抽象层次中跳跃连接的点特征拼接在一起。然后，拼接的特征通过“单位PointNet”进行传递，这类似于CNN中的一个接一个的卷积。应用几个共享的全连接层和ReLU层更新每个点的特征向量。重复该过程，直到我们将特征传播到原始的点集：
$$
\begin{align*}
f^{(j)}(x)&=\frac{\sum_{i=1}^k w_i(x)f_i^{(j)}}{\sum_{i=1}^k w_i(x)}\\
&其中，w_i(x)=\frac1{d(x,x_i)^p},j=1,\cdots,C
\end{align*}
$$

# Ch04 实验

### 数据集

我们评估了四个数据集，覆盖了二维对象（MNIST[^11]）、三维对象（ModelNet40[^31]刚性对象、SHREC15[^12]非刚性对象）到真实的三维场景（ScanNet[^5]）。对象分类性能通过准确度评估。语义场景标注性能通过平均体素分类准确度[^5]评估。我们为每个数据集列出了如下的实验配置：

-   MNIST：带有6万个训练样本和1万个测试样本的手写数字图像
-   ModelNet40：40个类别的CAD模型（大部分是人造的）。我们使用官方划分，9843个训练用形状，2468个测试用形状。
-   SHREC15：50个类别1200个形状。每个类别包含24个形状，其中大部分是有机生物的姿势，如：马、猫等。我们使用五重交叉验证来获得该数据集的分类精度
-   ScanNet：1513扫描和重建的室内场景。我们遵循[^5]中的实验配置，使用1201个训练场景，312个测试场景。

## 4.1. 欧几里德度量空间中的点集分类

我们从二维（MNIST）和三维（ModelNet40）欧几里德空间中采样点云进行分类用于评估我们的网络。MNIST图像被转换为数字像素位置的二维点云（默认为512个点）。ModelNet40形状从网格表面抽象形成三维点云（默认为1024个点）。在表2的最后一行（with normal），我们使用面片的法向作为附加的点特征，同时还使用更多的点（$N=5000$）进一步提升性能。所有的点集被归一化为零均值，单位球。我们使用拥有三个全连接层的三层层次网络。

### 结果

![image-20220112151551575](images/机器学习/计算机视觉/三维处理/点云/点云特征/PointNet/image-20220112151551575.png)

表1：MNIST数字分类

![image-20220112151615404](images/机器学习/计算机视觉/三维处理/点云/点云特征/PointNet/image-20220112151615404.png)

表2：ModelNet40形状分类

在表1和表2中，我们比较了本文的方法和先前最先进方法的代表集合。请注意表2中的PointNet（基本版）[^20]不使用变形网络，这与我们仅有一层的层次网络等价。

首先，我们的层次学习框架相比非层次的PointNet[^20]获得了明显是更好的性能。在MNIST中，相比PointNet（基本版）与PointNet，本文的方法获得了$60.8\%$和$34.6\%$的错误率下降。在ModelNet40分类中，在和相同的输入数据尺寸（1024个点）和特征（仅有坐标），相比PointNet，本文的方法获得了显著地改善。

其次，我们观察到基于点集的方法相比成熟图像CNN可以获得相似的或者更好的性能。在MNIST中，本文的方法（基于二维点集）获得了逼近 NiN（Network in Network）CNN的准确度。在ModelNet40中，使用法向信息的本文方法的性能显著地优于先前最先进的方法MVCNN[^26]。

### 对采样密度变化的稳健性

直接从现实世界获取的传感器数据通常会遇到严重的不规则采样问题（见图1）。我们的方法是选择多个尺度的点邻域，并且通过适当加权的方式学着平衡描述性和稳健性。

![image-20220112152152539](images/机器学习/计算机视觉/三维处理/点云/点云特征/PointNet/image-20220112152152539.png)

图4：（左）使用随机暂弃点的点云。（右）曲线展示了我们的密度自适应策略在处理非均匀密度时的优势。DP意味着训练阶段随机输入的暂弃；否则训练是在均匀密度点上完成的。详见Sec0303。

我们在测试阶段随机的丢弃点（见图4左）去验证我们的网络对于非均匀的和稀疏的数据的稳健性。在图4（右），我们发现MSG+DP（Multi-Scale Grouping + Drop Out, 训练阶段随机输入暂弃的多尺度分组）和MRG+DP（Multi-Resolution Grouping + Drop Out, 训练阶段随机输入暂弃的多分辨率分组）对采样密度的变化是稳健的。测试点从1024下降到256时，MSG+DP的性能下降不到$1\%$。而且，与替代方案相比，它在几乎所有的采样密度上都实现了最佳性能。PointNet（基本版）[^20]在密度变化下相当稳健是因为它更关注全局抽象而不是细节。然而，与我们的方法相比，它在细节上的损失导致其性能有所不足。SSG（Single-Scale Grouping, 在每个层次下单尺度分组的消融PointNet++）未能泛化稀疏的采样密度，而SSG+DP通过在训练阶段随机暂弃点修正了这个问题。

## 4.2. 语义场景标注中的点集分割

为了验证我们的方法同样适用于大尺度点云分析，我们还对语义场景标注任务进行了评估。目标是为室内扫描中的点预测语义对象标签。[^5]在体素扫描中使用全卷积神经网络提供了基线。它们完全依赖于扫描的几何信息代替RGB信息，并且基于每个体素报告精度。为了公平比较，我们在所有实验中移除了RGB信息，并且将点云标签预测信息转换为体素标签信息（如[^5]）。我们还与[^20]进行了比较，图5中（蓝色条）基于每个体素基线报告了精度。

![image-20220112160943450](images/机器学习/计算机视觉/三维处理/点云/点云特征/PointNet/image-20220112160943450.png)

图5：ScanNet标注的精确度

我们的方法大幅度地超越了所有的基础方法。相比[^5]在体素扫描上学习，我们的方法是直接在点云上学习以避免额外的量化误差，并且执行与数据相关的采样，以实现更加有效的学习。与[^20]相比，我们的方法引入了层次特征学习，并且在不同的尺度上捕获了几何特征。这对于理解不同层次的场景和标注不同大小的对象非常重要。我们在图6中可视化了样例场景标注结果。

![image-20220112162215745](images/机器学习/计算机视觉/三维处理/点云/点云特征/PointNet/image-20220112162215745.png)

图6：ScanNet标注的结果。[^20]正确地捕捉了房间的整体布局，但是没有发现家具。相比之下，我们的方法除了房间布局外，还能更好地分割物体。

### 对采样密度变化的稳健性

为了测试我们训练好的模型在非均匀采样密度的扫描中的表现，我们合成了类似于图1的ScanNet场景的虚拟扫描，并且在这种数据上评估了我们的网络。读者可以参考补充材料了解如何生成虚拟扫描，我们基于三种配置（SSG、MSG+DP、MRG+DP）评估了我们的网络，并且比较了基线方法[^20]。

性能比较参考图5（黄色条）。我们发现，由于采样密度从均匀点云转移到虚拟扫描场景后，SSG的性能大幅下降。另一方面，MRG网络对于采样密度的转换更加稳健，因为当采样稀疏时，它能够自动切换到描述更加粗粒度的特征。即使在训练数据（具有随机暂弃的均匀点）和非均匀密度的扫描数据之间存在邻域鸿沟，我们的MSG网络也仅仅受到轻微地影响，并且在比较中获得了所有方法中最佳的精度。这些都证明了我们设计的密度自适应层的有效性。

## 4.3. 在非欧几里德度量空间中点集分类

在本节中，展示了我们的方法在欧几里德空间中的泛化性。在非刚性形状分类（图7）问题中，一个好的分类器应该能够对图7中的(a)和(c)正确地分类，即使它们的姿态不同也能够视为一类，而这需要内在的结构知识。SHREC15中的形状是嵌入在三维空间中的二维曲面。沿着曲面的测地线距离自然地会产生一个度量空间。我们通过实验表明，在这个度量空间中PointNet++是一种有效方法，用于捕获潜在点集的内在结构。

对于[^12]中的每个形状，我们首先构造了由成对的测地距离引入的度量空间。我们遵循[^23]以获得模拟测地距离的嵌入度量。然后，我们在度量空间中提取内在点特征（包括：WKS[^11]、HKS[^27]和多尺度高斯曲率[^16]）。我们使用这些特征作为输入，然后根据潜在的度量空间对点采样和分组。这样，我们的网络学习去捕获不受形状特定姿态影响的多尺度内在结构。替代的设计选择有：$XYZ$坐标作为点特征或者欧几里德空间$\mathbb{R}^3$作为潜在的度量空间。我们在后续展示了这些不是最佳的选择。

### 结果

我们在表3中比较了我们的方法和当前最先进的方法[^14]。[^14]提取测地线的矩作为形状特征，并且使用稀疏自动编码器来理解这些特征去预测形状的类别。$XYZ$特征无法提示内在结构，并且受到姿态变化的影响。比较我们的方法的第二个和第三个配置，可以看到使用测地线邻域相比欧几里德邻域是有益的。欧几里德邻域可能包含了离曲面很远的点，并且当形状发生非刚性变形时，这个邻域可能发生巨大的变化。这会给有效的权重分享带来困难，因为局部结构组合可能变得复杂。另一方面，曲面上的测地线邻域解决了这个问题，提升了学习的有效性。

![image-20220112164421515](images/机器学习/计算机视觉/三维处理/点云/点云特征/PointNet/image-20220112164421515.png)

表3：SHREC15非刚性形状分类

## 4.4. 特征可视化

在图8中，我们可视化了我们的层次网络的第一层核学习得到的结果。我们在空间中创建了一个体素网格，并且聚集了局部点集，这个点集激活了一定的神经元，大部分神经元都在栅格单元中（使用了最高的100个样本）。投票高的栅格单元被保留，并且转换回三维点云，这代表着神经元识别模式。因为模型在ModelNet40上训练，ModelNet40大部分由家具组成，如：平板、双层平板、线、角等的结构可视化参见图8。

![image-20220112165347979](images/机器学习/计算机视觉/三维处理/点云/点云特征/PointNet/image-20220112165347979.png)

图8：从第一层核中学习得到的三维点去模式。这个训练好的模型用于ModelNet40形状分类（随机地从128个核中造反0个）。颜色表示点的深度（红色表示近，蓝色表示远）

# Ch05 相关工作

层次特征学习的思想已经非常成功。在所有学习模型中，卷积神经网络[^10][^25][^8]是其中最突出的一个。然而，卷积不适合于具有距离度量的无序点集，而突破这个约束恰是我们工作的关注目标。

一些最近的工作[^20][^28]已经研究过如何在无序集上应用深度学习。它们忽略了潜在的距离度量，即使点集确实拥有这类度量。因为它们无法捕捉点的局部上下文，并且易受全局集合转换和规范化的影响。在这项工作中，我们的目标是从度量空间中采样点，并且通过在设计中明确地考虑潜在距离度量来解决这些问题。

从度量空间中采样点通常是带噪声的，并且采样密度是不均匀的。这影响了有效点特征的提取，给学习带来了困难。一个关键问题是为点特征的设计选择合适的比例。关于这个问题，以前开发了许多方法[^19][^17][^2][^6][^7][^30]，要么是几何处理的方式，要么是摄影测量和遥感方式。与这些方法相比，我们的方法学习去提取特征点和以端到端的方式平衡多个特征尺度。

在三维度量空间中，深度学习除了点集表示，还存在许多流行的表示方式，包括：体素栅格[^21][^22][^29]和几何图形[^3][^15][^33]。然而，这些研究中都没有明确地考虑非均匀采样密度问题。

# Ch06 结论

在这项工作中，我们提出了PointNet++，一个强大的神经网络架构，用于处理在度量空间中采样的点集。PointNet++递归地在输入点集的嵌套分区上运行，并且在学习关于距离度量的层次特征方面是有效的。为了解决非均匀点采样问题，我们提出了两个新的集合抽象层，根据局部点密度智能地聚集多尺度信息。这些贡献使得我们在具有挑战性的三维点云基准上获得了最先进的性能。

在未来，值得思考的事情是如何通过在局部区域共享更多的计算来加快我们提出的网络的推理速度，特别是对于MSG层和MRG层。令人关注的还有在高维度量空间中找到应用，因为在这些空间中基于CNN的方法在计算上是不可行的，而我们的方法可以很好地适应。

# 附录

## A. 概述

这个附加材料提供了主论文中实验的更多细节，并且包含了更多的实验去验证和分析我们提出的方法。

在SecB中，我们提供了主论文中使用的特定的网络结构用于实验，并且描述了数据准备和训练的细节。

在SecC中，我们展示了更多的实验结果，包括部件分割和邻域查询分析的基准性能，抽样随机性和时空复杂性的敏感程度。

## B. 实验细节

### 架构协议

我们使用以下符号来描述我们的网络架构。

$SA(K,r,[l_1,\cdots,l_d])$是具有球半径为$r$的$K$个局部区域的集合抽象（Set Abstraction，SA）层，该层使用$d$个全连接层构成的PointNet，每个全连接层的宽度分别为$l_i(i=1,\cdots,d)$。

$SA([l_1,\cdots,l_d])$是全局的集合抽象层，用于将集合转换为单个的向量。

在多尺度配置（如在MSG中），我们使用$SA(K,[r^{(1)},\cdots,r^{(m)}],[[l_1^{(1)},\cdots,l_d^{(1)}],\cdots,[l_1^{(m)},\cdots,l_d^{(m)}]])$去表示$m$个尺度的MSG。

$FC(l,dp)$表示宽度为$l$的全连接层，暂弃率为$d_p$。

$FP(l_1,\cdots,l_d)$是具有$d$个全连接层的特征传播（Feature Propagation，FP）层。用于更新来自于插值和跳跃连接拼接后的特征。除了最后一个分数预测层之外，所有的全连接层都紧接了批归一化和ReLU。

### B.1 网络架构

对于所有的分类实验，我们使用$K$（分类的类别数）不同的下述架构（我们的SSG）：

$SA(512,0.2,[64,64,128])\to SA(128,0.4,[128,128,256])\to SA([256,512,1024])\\\to FC(512,0.5)\to FC(256,0.5)\to FC(K)$

PointNet++的多尺度分组（MSG）网络如下：

$SA(512,[0.1,0.2,0.4],[[32,32,64],[64,64,128],[64,96,128]])\to\\SA(128,[0.2,0.4,0.8],[[64,64,128],[128,128,256],[128,128,256]])\to\\SA([256,512,1024])\to\\FC(512,0.5)\to FC(256,0.5)\to FC(K)$

交叉层次多分辨率分组（MRG）网络的架构使用三个分支：

-   分支1：$SA(512,0.2,[64,64,128])\to SA(64,0.4,[128,128,256])$
-   分支2：$SA(512,0.4,[64,128,256])$ 使用原始点的 $r=0.4$ 区域
-   分支3：$SA(64,128,256,512)$ 使用所有的原始点
-   分支4：$SA(256,512,1024)$

分支1与分支2拼接后输入到分支4。分支3和分支4的输出拼接后输入到网络：$FC(512,0.5)\to FC(256,0.5)\to FC(K)$用于分类。

用于语义场景标注的网络（FP的最后两个全连接层之后紧接的是丢弃率为$0.5$的暂弃层）：

$SA(1024,0.1,[32,32,64])\to\\SA(256,0.2,[64,64,128])\to\\SA(64,0.4,[128,128,256])\to\\SA(16,0.8,[256,256,512])\to\\FP(256,256)\to FP(256,256)\to FP(256,128)\to\\FP(128,128,128,128,K)$

用于语义和部件分割的网络（FP的最后两个全连接层之后紧接的是丢弃率为$0.5$的暂弃层）：

$SA(512,0.2,[64,64,128])\to\\SA(128,0.4,[128,128,256])\to\\SA([256,512,1024])\to\\FP(256,256)\to FP(256,128)\to\\FP(128,128,128,128,K)$

### B.2 虚拟扫描生成

在本节中，我们描述如何从ScanNet场景中生成具有非均匀采样密度的标记了的虚拟扫描。对于ScanNet中的每个场景，我们设置照相机的位置为地板平面质心上$1.5m$处，并且在水平面上沿着8个方向均匀地旋转照相机的方向。在每个旋转得到的方向上，我们使用大小为$100像素\times75像素$的图像平面，并且从照相机通过每个像素向场景投射光线。这是一种在场景中选择可视点的方法。然后，我们可以为每个相似的测试场景生成8次虚拟扫描，例子详见图9。请注意，靠近相机的区域中的点样本更加稠密。

![image-20220113103157403](images/机器学习/计算机视觉/三维处理/点云/点云特征/PointNet/image-20220113103157403.png)

图9：从ScanNet中生成的虚拟扫描

### B.3 MNIST和ModelNet40实验细节

对于MNIST图像，我们首先将所有像素强度归一化到$[0,1]$范围内。然后选择强度大于$0.5$的所有像素作为有效的数字像素。接着，我们将图像中的数字像素转换为坐标在$[-1,1]$内的二维点云，其中原点在图像的中心。创建增广点是为了将点集扩充到固定的基数（我们的例子中是512）。基于高斯分布$\mathcal{N}(0,0.1)$随机平移，并且剪切到$0.03$，我们抖动初始点云去生成增广点。对于ModelNet40，我们基于工作面区域从CAD模型曲面上均匀地采样$N$个点。

对于所有的实验，我们使用学习率为$0.001$的Adam[^9]优化器用于训练。对于数据增强，我们随机地缩放对象，以及扰动对象的位置和点采样的位置。我们还依照[^21]中那样随机地旋转对象用于ModelNet40数据增强。我们使用TensorFlow和GTX 1080和Titan X用于训练。所有的层都在CUDA中实现以运行在GPU上。我们的模型训练大约需要20个小时收敛。

### B.4 ScanNet实验细节

为了从ScanNet场景中生成训练数据，我们从初始场景中采样$1.5m\times1.5m\times3m$的立方体，然后保留立方体，其中$\geq2\%$的体素被占用，$\geq70\%$的表面体素被有效标注（这个与[^5]中的设置相同）。我们在训练期间采样这样的训练立方体，并且沿着右上轴随机地旋转。增广点被加入到点集中以形成固定的基数（我们的例子是8192）。在测试阶段，我们执行类似的操作，将测试场景分割成更小的立方体。首先获得立方体中每个点的预测标签，然后合并来自同一场景的所有立方体中的预测标签。如果一个点从不同的立方体中获得了不同的标签，我们将通过执行多数投票机制来获得最终的点预测标签。

### B.5 SHREC15实验细节

我们对每个形状都随机抽取1024个点用于训练和测试。为了生成输入的内在特征，我们分别提取了100维的WKS、HKS和多尺度高斯曲率，为每个点都得到了一个300维的特征向量。然后执行主成分分析（PCA）将特征降到64维。我们使用8维嵌入（如[^23]）来模拟测地线距离，用于描述我们在选择点邻域时的非欧几里德度量空间。

## C. 更多实验

在本节中，我们提供更多的实验结果来验证和分析我们提出的网络架构。

### C.1 语义部件分割

假设每个形状的类别标签是已知的，遵循[^32]的配置，评估我们在部件分割任务上的方法。任务以点云表示的形状作为输入，预测每个点的部件标签。数据集包含来自16个类别的16,881个形状，总共用50个部件进行标注。我们遵循[^4]采用官方的训练与测试分割。

我们为每个点都配备了其法线方向，以更好地描述潜在的形状。这样我们就可以不像[^32][^33]那样使用手工制作的几何特征。我们的框架与传统的基于学习的技术[^32]和最先进的深度学习方法[^20][^33]进行了比较（详见表4）。点的交并比（Intersection over Union，IoU）用作评估的指标，在所有的部件类别上取平均值。在训练阶段最小化交叉熵损失函数。平均而言，我们方法获得了最佳的性能。与[^20]相比，我们的方法在大多数类别上表现更好，这证明了层次特征学习对于细节的语义理解是非常重要的。请是，我们的方法可以被视为隐式地构建不同比例的邻近图（Proximity Graph），并且对这些图执行操作，因此与图CNN相关（如[^33]）。由于我们的网络具有多尺度邻域选择的灵活性以及集合运算单元的能力，相比[^33]可以获得更好的性能。请注意，我们的集合操作单元相比图卷积核更加简单，并且相比[^33]无需执行昂贵的特征分解。这些原因都使我们的方法更加适合大规模的点云分析。

![image-20220113135318527](images/机器学习/计算机视觉/三维处理/点云/点云特征/PointNet/image-20220113135318527.png)

表4：ShapeNet部件数据集上的分割结果

### C.2 邻域查询：KNN 与 球查询

在这里，我们比较了选择局部邻域的两种方式。我们在论文主体中使用基于半径的球查询，而在这里我们还实验了基于KNN的邻域搜索，并且还尝试了不同的搜索半径和$k$。在实验中，所有的训练和测试都在具有均匀采样密度的ModelNet40形状上执行，采样点数为1024。如表5所示，基于半径的球查询略优于基于KNN的方法。然而，我们在非常不均匀的点集中，基于KNN的查询会导致更差的泛化能力。此外，我们还发现稍大的半径有助于提升性能，可能是因为它捕捉了更加丰富的局部模式。

![image-20220113140017438](images/机器学习/计算机视觉/三维处理/点云/点云特征/PointNet/image-20220113140017438.png)

表5：邻域选择的影响。评估度量是ModelNet40数据集上的分类准确度（$\%$）。

### C.3 最远点采样中随机性的影响

对于集合抽象层次中的采样层，我们使用最远点采样（Farthest Point Sampling，FPS）进行点集的子采样。然而，FPS算法是随机的，二次采样取决于首先选择哪个点。在这里，我们评估了我们的模型对于随机性的敏感度。在表6中，我们测试了在ModelNet40上训练的模型的特征稳定性和分类稳定性。

![image-20220113141058426](images/机器学习/计算机视觉/三维处理/点云/点云特征/PointNet/image-20220113141058426.png)

表6：FPS中随机性的影响（使用MeshSegNet40）

为了评估特征的稳定性，我们基于不同的随机种子提取了10次所有测试样例的全局特征。然后，我们计算了10个采样中每个形状的平均特征。接着，我们从平均特征中计算了特征差的范数的标准方差。最后，我们平均了所有特征维度的所有的标准方差（结果如表6）。由于特征在处理前被归一化为$[0,1]$，因此$0.021$的意味着特征范数有$2.1\%$的偏差。

对于分类，基于所有的ModelNet40测试形状，我们在测试准确度上仅观测到$0.17\%$的标准方差，这意味着对于采样的随机性是稳健的。

### C.4 时空复杂度

表7总结了几种基于点集的深度学习方法在时间成本和空间成本上的比较结果。我们使用TensorFlow 1.1和单个GTX 1080，在批处理大小为8的配置下，记录了转发的时间。第一个批处理被忽略，因为GPU需要准备时间。而PointNet（基本版）[^20]拥有最好的时间效率，我们的模型（没有密度自适应层）在同等速度下得到了最小的模型尺寸。

值得注意的是我们的MSG，虽然它在非均匀采样数据下获得了最好的性能，但是受其多尺度区域特征提取的影响比SSG版本代价高出2倍。MRG与MSG相比，其效率更高，因为它使用跨层次的区域。

![image-20220113142612486](images/机器学习/计算机视觉/三维处理/点云/点云特征/PointNet/image-20220113142612486.png)

表7：多个网络的模型尺寸和推理时间（向前传递）
