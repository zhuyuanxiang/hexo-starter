---
title: 函数einsum详解
excerpt: 函数torch.einsum()用于爱因斯坦求和，可以乘法地表示乘积、点积、转置等方法。
categories:
  - 编程
tags:
  - Python
  - PyTorch
  - 编程
date: 2023-12-19
updated: 
toc: true
toc_sticky: true
---

# torch.einsum()

函数torch.einsum()用于爱因斯坦求和，可以乘法地表示乘积、点积、转置等方法。

## 乘积

### 相同维度的乘积

#### 输出中消失两个矩阵中的维度

假设矩阵 $A\in\mathbb{R}^{I\times J}$、矩阵 $B\in\mathbb{R}^{J\times K}$，两个矩阵的乘积 $C\in\mathbb{R}^{I\times K}$ [^注1]，公式如下：

$$
\begin{equation}
C^i_k=A^i_j B^j_k \quad 或 \quad C_{ik}=(AB)_{ik}=\sum^J_{j=1}A_{ij}B_{jk}
\end{equation}
$$

[^注1]: 输出结果中不出现的下标作为求和的下标

在代码中上面的式子可以表示为字符串：`'ij,jk->ik'`

代码实现：

```python
import torch
A = torch.arange(6).reshape(2, 3)
B = torch.arange(12).reshape(3, 4)
C = torch.einsum('ij,jk->ik', A, B)
C.shape
# torch.Size([2, 4])
```

#### 输出中消失一个矩阵中的维度

假设矩阵 $A\in\mathbb{R}^{I\times J}$、矩阵 $B\in\mathbb{R}^{J\times K}$，两个矩阵的乘积 $C\in\mathbb{R}^{I\times J}$ [^注1]，公式如下：

$$
\begin{equation}
C^i_j=A^i_j B^j_k \quad 或 \quad C_{ij}=(AB)_{ij}=\sum^K_{k=1}A_{ij}B_{jk}
\end{equation}
$$

在代码中上面的式子可以表示为字符串：`'ij,jk->ij'`

代码实现：

```python
A = torch.arange(6).reshape(2, 3)
B = torch.arange(12).reshape(3, 4)
C = torch.einsum('ij,jk->ij', A, B)
C.shape
# torch.Size([2, 3])
```

#### 输出中不消失维度

假设矩阵 $A\in\mathbb{R}^{I\times J}$、矩阵 $B\in\mathbb{R}^{J\times K}$，两个矩阵的乘积 $C\in\mathbb{R}^{I\times J\times K}$，公式如下：

$$
\begin{equation}
C_{ijk}=(AB)_{ijk}=A_{ij}B_{jk}
\end{equation}
$$

代码实现：
```python
A = torch.arange(6).reshape(2, 3)
B = torch.arange(12).reshape(3, 4)
C = torch.einsum('ij,jk->ijk', A, B)
C.shape
# torch.Size([2, 3, 4])
```

### 不同维度的乘积

#### 输出中消失两个矩阵中的维度

假设矩阵 $A\in\mathbb{R}^{I\times J\times K}$矩阵 $B\in\mathbb{R}^{J\times L}$，两个矩阵的乘积 $C\in\mathbb{R}^{I\times L\times K}$[^注1]，公式如下：

$$
\begin{equation}
C_{ilk}=(AB)_{ilk}=\sum^J_{j=1}A_{ijk}B_{jl}
\end{equation}
$$

```python
import torch
A = torch.arange(24).reshape(2, 3, 4)
B = torch.arange(15).reshape(3, 5)
C = torch.einsum('ijk,jl->ilk', A, B)
C.shape
# torch.Size([2, 5, 4])
```

假设矩阵 $A\in\mathbb{R}^{I\times J\times K}$矩阵 $B\in\mathbb{R}^{K\times L}$，两个矩阵的乘积 $C\in\mathbb{R}^{I\times J\times L}$[^注1]，公式如下：

$$
\begin{equation}
C_{ijl}=(AB)_{ijl}=\sum^K_{k=1}A_{ijk}B_{kl}
\end{equation}
$$

代码实现：

```python
import torch
A = torch.arange(24).reshape(2, 3, 4)
B = torch.arange(20).reshape(4, 5)
C = torch.einsum('ijk,kl->ijl', A, B)
C.shape
# torch.Size([2, 3, 5])
```

#### 输出中消失一个矩阵中的维度

假设矩阵 $A\in\mathbb{R}^{I\times J\times K}$矩阵 $B\in\mathbb{R}^{J\times L}$，两个矩阵的乘积 $C\in\mathbb{R}^{I\times J\times K}$[^注1]，公式如下：

$$
\begin{equation}
C_{ijk}=(AB)_{ijk}=\sum^L_{l=1}A_{ijk}B_{jl}
\end{equation}
$$

代码实现：

```python
import torch
A = torch.arange(24).reshape(2, 3, 4)
B = torch.arange(15).reshape(3, 5)
C = torch.einsum('ijk,jl->ijk', A, B)
C.shape
# torch.Size([2, 3, 4])
```

假设矩阵 $A\in\mathbb{R}^{I\times J\times K}$矩阵 $B\in\mathbb{R}^{K\times L}$，两个矩阵的乘积 $C\in\mathbb{R}^{I\times J\times K}$[^注1]，公式如下：
$$
\begin{equation}
C_{ijk}=(AB)_{ijk}=\sum^L_{l=1}A_{ijk}B_{kl}
\end{equation}
$$

代码实现：

```python
import torch
A = torch.arange(24).reshape(2, 3, 4)
B = torch.arange(20).reshape(4, 5)
C = torch.einsum('ijk,kl->ijk', A, B)
C.shape
# torch.Size([2, 3, 4])
```

### 多维矩阵乘法

假设矩阵 $A\in\mathbb{R}^{I\times J}$、矩阵 $B\in\mathbb{R}^{I\times J\times K}$、矩阵 $C\in\mathbb{R}^{J\times K}$，三个矩阵的乘积 $D\in\mathbb{R}^{I\times K}$，公式如下：
$$
\begin{equation}
D_{ik}=(ABC)_{ik}=\sum^J_{j=1}A_{ij}B_{ijk}C_{jk}
\end{equation}
$$



```python
A = torch.arange(6).reshape(2, 3)
B = torch.arange(24).reshape(2, 3, 4)
C = torch.arange(12).reshape(3, 4)
D = torch.einsum('ij,ijk,jk->ik', A, B, C)
D.shape
# torch.Size([2, 4])
```

假设矩阵 $A\in\mathbb{R}^{I\times J}$、矩阵 $B\in\mathbb{R}^{I\times J\times K}$、矩阵 $C\in\mathbb{R}^{J\times K}$，三个矩阵的乘积 $D\in\mathbb{R}^{I\times J\times K}$，公式如下：
$$
\begin{equation}
D_{ijk}=(ABC)_{ijk}=A_{ij}B_{ijk}C_{jk}
\end{equation}
$$



```python
A = torch.arange(6).reshape(2, 3)
B = torch.arange(24).reshape(2, 3, 4)
C = torch.arange(12).reshape(3, 4)
D = torch.einsum('ij,ijk,jk->ijk', A, B, C)
D.shape
# torch.Size([2, 4])
```

## 求和

假设矩阵 $A\in\mathbb{R}^{I\times J}$，$C\in\mathbb{R}$[^注1]，公式如下：
$$
\begin{equation}
C=\sum^I_{i=1}\sum^J_{j=1}A_{ij}
\end{equation}
$$

代码实现：
```python
A = torch.arange(6).reshape(2, 3)
C = torch.einsum('ij->', A)
C
# tensor(15)
```

### 列求和

假设矩阵 $A\in\mathbb{R}^{I\times J}$，$C\in\mathbb{R}^J$[^注1]，公式如下：
$$
\begin{equation}
C_{j}=\sum^I_{i=1}A_{ij}
\end{equation}
$$

代码实现：
```python
A = torch.arange(6).reshape(2, 3)
C = torch.einsum('ij->j', A)
C
# torch.Tensor([3, 5, 7])
```

### 行求和

假设矩阵 $A\in\mathbb{R}^{I\times J}$，$C\in\mathbb{R}^I$[^注1]，公式如下：
$$
\begin{equation}
C_{i}=\sum^J_{j=1}A_{ij}
\end{equation}
$$

代码实现：
```python
A = torch.arange(6).reshape(2, 3)
C = torch.einsum('ij->i', A)
C
# torch.Tensor([3, 12])
```

### 点积求和

假设矩阵 $A\in\mathbb{R}^{I\times J}$，$C\in\mathbb{R}$[^注1]，公式如下：
$$
\begin{equation}
C=\sum^I_{i=1}\sum^J_{j=1}A_{ij}A_{ij}
\end{equation}
$$

代码实现：
```python
A = torch.arange(6).reshape(2, 3)
C = torch.einsum('ij,ij->', A, A)
C
# tensor(55)
```

## 转置

假设矩阵 $A\in\mathbb{R}^{I\times J}$，$C\in\mathbb{R}^{J\times I}$[^注1]，公式如下：
$$
\begin{equation}
C_{ji}=A_{ij}
\end{equation}
$$

代码实现：
```python
A = torch.arange(6).reshape(2, 3)
C = torch.einsum('ij->ji', A)
C.shape
# torch.Size([3, 2])
```
