---
title: Scikit-Learn学习笔记
excerpt: ""
categories:
  - 编程
tags:
  - 编程
  - 机器学习
  - 数据科学
  - Python
  - Scikit-Learn
date: 2019-07-16
updated: 2019-07-16
toc: true
toc_sticky: true
---

解决 sklearn 中 fetch_lfw_people 安装问题

在学习特征脸时，要加载 lfw_people，代码如下

```python
from sklearn.datasets import fetch_lfw_people
faces = fetch_lfw_people()
```

第一次使用时，因为 sklearn 中没有自带数据，所以会报错。提醒去以下地址下载 :

- [pairsDevTrain.txt](https://ndownloader.figshare.com/files/5976012)
- [pairsDevTest.txt](https://ndownloader.figshare.com/files/5976009)
- [pairs.txt](https://ndownloader.figshare.com/files/5976006)
- [lfw-funneled.tgz](https://ndownloader.figshare.com/files/5976015)
- [lfw.tgz](https://ndownloader.figshare.com/files/5976018)

前三个是图片的元数据，第四个是处理成锥子脸的图片数据，后一个没有处理过的原始图片数据 ( 可以不用下载 ) 。

下载以后，找到 Python 的运行环境，例如 : 我的环境是“D:\Users\Administrator\PycharmProjects\MachineLearning\venv\Lib\site-packages\sklearn\datasets\data\”，建目录“lfw_home”，将三个文本文件拷到目录里面，再把“lfw_funneled.tgz”解压到目录里面，于是目录里面还会自动建立一个“lfw_funneled”目录。最后的结果是“D:\Users\Administrator\PycharmProjects\MachineLearning\venv\Lib\site-packages\sklearn\datasets\data\lfw_home\”中会有三个文本文件和一个“lfw_funneled”目录，在“lfw_funneled”目录中就是用所有人的名字命名的目录。

注 : 也可以直接把“lfw_funneled.tgz”拷贝到“lfw_home”目录中，但是导入图片的速度会非常慢
