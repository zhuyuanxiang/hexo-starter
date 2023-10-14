---
title: Gensim 学习关键点总结
excerpt: 学习过程中发现自己不熟悉的知识点
categories:
  - 编程
tags:
  - Python
  - Gensim
  - Word2Vec
  - 机器学习
date: 2020-06-27
updated: 2020-08-15
toc: true
toc_sticky: true
---

# Gensim

## Program

### [KeyedVectors](https://radimrehurek.com/gensim/models/keyedvectors.html#why-use-keyedvectors-instead-of-a-full-model)

1. Why use KeyedVectors instead of a full model?

    | 功能                       | KeyedVectors | full model | note                                                            |
    | ------------------------ | ------------ | ---------- | --------------------------------------------------------------- |
    | 继续训练向量                   |              | [x]        | 全模型可以继续训练和更新向量                                                  |
    | 更小的对象                    | [x]          |            | KeyedVectors 使用更少的内存，因为不存储模型的状态，也就没法继续训练。                       |
    | 存/本地格式的fasttext/word2vec | [x]          |            | 从 Facebook 和 Google 的工具中导出的向量不支持进一步的训练，但是仍然可以加载到 KeyedVectors.  |
    | 增加新的向量                   | [x]          | [x]        | 可以动态地向映射中增加新的实体向量                                               |
    | 并发性                      | [x]          | [x]        | 线程案例，允许并发地向量查询                                                  |
    | 共享内存                     | [x]          | [x]        | 多进程重用使用 [mmap](https://en.wikipedia.org/wiki/Mmap) 保存在内存中的相同的数据 |
    | 快速加载                     | [x]          | [x]        | 支持 [mmap](https://en.wikipedia.org/wiki/Mmap) 从磁盘中迅速加载数据        |

    TL;DR: 最主要的区别是 KeyedVectors 不支持继续训练。因为放弃了训练所需要的内部数据结构，因此 KeyedVectors 提供了更小的内存封装和更小的函数接口。

1. How to obtain word vectors?

训练一个完全模型，然后得到 `model.wv` 属性(只保留了独立的有键的向量)：

```python
from gensim.test.utils import common_texts
from gensim.models import Word2Vec

model = Word2Vec(common_texts, size=100, window=5, min_count=1, workers=4)
word_vectors = model.wv
```

将词向量在磁盘上存取：

```python
from gensim.test.utils import get_tmpfile
from gensim.models import KeyedVectors

fname = get_tmpfile("vectors.kv")
word_vectors.save(fname)
word_vectors = KeyedVectors.load(fname, mmap="r")
```

将 Google 的 C 格式的 word2vec 载入为 KeyedVectors 实例

```python
from gensim.test.utils import datapath

wv_from_text = KeyedVectors.load_word2vec_format(
    datapath("word2vec_pre_kv_c"), binary=False
)  # C text format
wv_from_bin = KeyedVectors.load_word2vec_format(
    datapath("euclidean_vectors.bin"), binary=True
)  # C bin format
```

### Word2Vec

```python
class gensim.models.word2vec.Word2Vec(sentences=None,size=100,alpha=0.025,window=5,min_count=5, max_vocab_size=None, sample=0.001,seed=1, workers=3,min_alpha=0.0001, sg=0, hs=0, negative=5, cbow_mean=1,hashfxn=<built-in function hash>,iter=5,null_word=0,trim_rule=None, sorted_vocab=1, batch_words=10000)
```

参数：

- sentences：可以是一个 List，对于大语料集，建议使用 `BrownCorpus`, `Text8Corpus`, `LineSentence` 构建。
- sg： 用于设置训练算法，默认为0，对应CBOW算法；sg=1则采用skip-gram算法。
- size：是指特征向量的维度，默认为100。大的size需要更多的训练数据,但是效果会更好. 推荐值为几十到几百。
- window：表示当前词与预测词在一个句子中的最大距离是多少
- alpha: 是学习速率
- seed：用于随机数发生器。与初始化词向量有关。
- min_count: 可以对字典做截断. 词频少于min_count次数的单词会被丢弃掉, 默认值为5
- max_vocab_size: 设置词向量构建期间的RAM限制。如果所有独立单词个数超过这个，则就消除掉其中最不频繁的一个。每一千万个单词需要大约1GB的RAM。设置成None则没有限制。
- sample: 高频词汇的随机降采样的配置阈值，默认为1e-3，范围是(0,1e-5)
- workers参数控制训练的并行数。
- hs: 如果为1则会采用hierarchica·softmax技巧。如果设置为0（defau·t），则negative sampling会被使用。
- negative: 如果>0,则会采用negativesampling，用于设置多少个noise words
- cbow_mean: 如果为0，则采用上下文词向量的和，如果为1（defau·t）则采用均值。只有使用CBOW的时候才起作用。
- hashfxn： hash函数来初始化权重。默认使用python的hash函数
- iter： 迭代次数，默认为5
- trim_rule： 用于设置词汇表的整理规则，指定那些单词要留下，哪些要被删除。可以设置为None（min_count会被使用）或者一个接受()并返回RU·E_DISCARD,uti·s.RU·E_KEEP或者uti·s.RU·E_DEFAU·T的函数。
- sorted_vocab： 如果为1（defau·t），则在分配word index 的时候会先对单词基于频率降序排序。
- batch_words：每一批的传递给线程的单词的数量，默认为10000
