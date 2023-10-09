---
title: "Numpy 学习关键点总结"
excerpt: "学习过程中发现自己不熟悉的知识点"
categories:
- 编程
tags:
- Python
- Numpy
- 机器学习
- 数据分析
date: 2020-09-25
updated: 2020-09-25
toc: true
toc_sticky: true
---

# Numpy

## Stack Data

建立数组

```python
a=np.arange(9).reshape(3,3)
b=a*2
```

- 水平叠加：`np.hstack((a,b))`
  - `np.concatenate((a,b),axis=1)`效果相同
  - `np.c_[a,b]`效果相同
- 垂直叠加：`np.vstack((a,b))`
  - `np.concatenate((a,b),axis=0)`效果相同
  - `np.r_[a,b]`效果相同
- 深度叠加：`np.dstack((a,b))`
- 列式叠加：`np.column_stack((a,b))`
- 维度扩展：`np.expand_dims(a,axis=0)`
- 维度连接：`np.concatenate((a,b),axis=0)`
- 将切片对象沿第一个轴(行)连接：`np.r_[a.ravel(),-1,b.ravel()]`
- 将切片对象沿第二个轴(列)连接：`np.c_[a.ravel(),b.ravel()]`
- 删除数组中shape为1的维度：`np.squeeze(a.reshape(9,1))`
- 把数组摊平为1维数组：`a.ravel()`
