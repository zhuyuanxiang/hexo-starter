---
title: "《Python 自然语言处理》学习笔记"
excerpt: ""
categories:
- 编程
tags:
- Python
- 自然语言处理
- 机器学习
- NLTK
updated: 2019-06-30
toc: true
toc_sticky: true
---

## 全书总评

- 书本印刷质量 : 4 星。
- 著作编写质量 : 4 星。
  - 除了成书时间较早以外，书本质量是可以保证的，通过这本书的学习能够对 NLTK 有个全面的了解。
- 著作翻译质量 : 4 星。
  - 至少没看到明显有翻译错误的地方。
- 读书建议 : 与《自然语言处理综论》结合起来学习。

## 环境准备

- Python 2.5，如果有喜欢 3.0 的同学建议直接去 [NLTK 官网](http://www.nltk.org/book/)看最新的英文版。
  - 我用的是 Python 3.0，跟中文书的使用不同，所以后面笔记中多描述 Python 3.0 下操作方式与书中不同的地方。
  - 具体命令的描述请参考英文版。
- 需要安装的软件包。有三个必备 ( NLTK，NumPy，Matplotlib )
  - NLTK_Data 数据包需要下载，可以使用书上的方法，也可以直接去 [NLTK 官网](http://www.nltk.org/nltk_data/)下载。下载完成后，解压缩到 `C:\nltk_data` 目录中即可。
- IDE 环境。建议使用 PyCharm 的社区版，可以把 Python 的 Console 环境直接单独全屏。

## 语言处理与 Python

- 主要介绍语言处理的一些基本手段和 Python 的基本语法。
- `from nltk.book import *` 就是用来导入 nltk_data 目录下的数据包
  - 如果后面某些命令执行时报错，那就是数据包没有下载完全
    - 可以使用下载命令 ( `nltk.download_gui()` 才能正确弹出窗口 ) 来完成安装
    - 也可以自己去官网下载后，自行解压到 `c:\nltk_data` 目录下，解压后需要再导入一次才行
    - 找不到资源的文件，需要去目录下把 zip 文件解开。
- `FreqDist()` 执行后不再排序，如果需要排序使用 `most_common(n)` n 为前多少个映射。
- `bigrams()` 执行报错 ( `<generator object bigrams at 0x0000000012D2E930>` )
  - 是因为这个命令在 3.0 版本中已经改变为输出一个对象，这个对象可以赋值，也可以使用 list()转换。
- `fdist.collocations()` 改为 `fdist.collocation_list()`
- `fdist.inc()` 命令已经取消，直接使用 `fdist[sample] +=1`
- `babelize_shell()` 命令无法执行，因为这个模块已经被取消。
  - 参考 [中文说明](http://47.102.43.245/2017/02/05/%e5%85%b3%e4%ba%8enltk%e6%9c%ba%e5%99%a8%e7%bf%bb%e8%af%91%ef%bc%88mt%ef%bc%89%e7%9a%84babelizer%e6%97%a0%e6%b3%95%e4%bd%bf%e7%94%a8%e5%8e%9f%e5%9b%a0/)
  - 参考 [英文说明](https://stackoverflow.com/questions/25215887/babelize-shell-not-working)
  - 参考 [移植说明](https://github.com/nltk/nltk/issues/265)

## 获得文本语料和词汇资源

- `ConditionalFreqDist()` 这个函数可以正确执行，但是后续的 `plot()` 会报错，建议通过更换 NLTK 的版本可以尝试找到与你的 NLTK_Data 匹配的代码。
  - 我的可以正确执行的是 NLTK 3.2.5
