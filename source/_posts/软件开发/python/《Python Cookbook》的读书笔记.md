---
title: 《Python Cookbook》的读书笔记
excerpt: Python 的菜谱，肯定是很香的，内容也是很全的。
categories:
  - 编程
tags:
  - Python
  - 编程
  - Data Science
date: 2019-03-04
updated: 2019-03-07
toc: true
toc_sticky: true
---

# 全书总评

- 书本印刷质量: 4 星。印刷清楚，排版合适，错误很少。最好还能找本电子书，可以少输入一些数据代码。
- 著作编写质量: 4 星。胆敢称自己的菜谱的书，要不就是好书；要不就是差书；绝对不会不好不差。所以这是本长期使用 Python 人员必备的案头书。
- 代码质量: 4 星。常常学习几个里面的代码，保证你心情愉悦！
- 笔记目的: 记录重点，方便回忆。

## C01. 数据结构和算法

### 序列处理

- (01) 序列分解，一对一匹配: x, y, z=(4, 8, 2)
- (02) 序列分解，一对多匹配: first, *middle, last = ('start', 1, 2, 3, 'last')
- (10) 移除序列中出现的重复元素: 生成器(yield)
- (11) 对序列切片命名: slice(start,stop,step)
- (12) 统计序列中元素出现次数: collections.Counter()
- (16) 筛选序列中的元素:
    - 列表推导式( list comprehension ): [n for n in myList if n>0]
    - filter(func(), values): 采用定义的函数，处理更加复杂的过滤机制
    - itertools.compress(iter_obj, boolSequence): 将可迭代对象与布尔选择器序列进行对应，显示布尔序列中为 True 的对应的可迭代对象中的元素
- (18) 将名称映射到序列的元素中: collections.namedtuple(命名元组), 避免使用位置(索引或下标)来访问列表或元组，而通过名称来访问元素。
- (19) 同时对数据做转换和换算: 在函数参数中使用生成器表达式；对于可接受 key 参数的，还可以使用 lambda 方式。
- (20) 将多个映射合并为单个映射: collections.ChainMap(), 使用原始的字典进行合并，原字典改变时也可以反映到合并后的字典上。

### 列表处理

- (03) collections.deque: 限制队列长度，保留最后几项，后来的挤掉最前面的；
- (04) 寻找特定的 N 个元素:
    - 寻找 N 个最大元素: heapq.nlargest()
    - 寻找 N 个最小元素: heapq.nsmallest()
- (05) 按优先级排序的队列:
    - 向队列压入数据: heapq.heappush()
    - 从队列弹出数据: heapq.heappop()
- (13) 对字典列表排序:  sorted(dict_list, key=itemgetter(keys)),  operator.itemgetter()创建一个可调用对象
- (14) 对对象列表排序:  sorted(object_list, key=attrgetter(prop)), operator.attrgetter()创建一个可调用对象
- (15) 对字典列表分组:  itertools.groupby(rows, key=itemgetter(keys))

### 字典处理

- (06) 一键多值字典: collections.defaultdict(defaultValueType)
- (07) 有序的字典: collections.OrderedDict(), 保证字典迭代时按元素添加顺序进行
- (08) 字典计算
    - zip(): 将字典转换成元组；
    - min(): 找出元组列表中的最小值；
    - max(): 找出元组列表中的最大值；
- (09) 寻找两个字典中的共同元素: keys() 和 items() 返回的对象都支持集合操作。
- (17) 从字典中提取字集: 利用字典推导式 ( dictionary comprehension )
