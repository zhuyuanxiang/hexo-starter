---
title: Tensorflow-and-Keras使用心得
excerpt: ''
categories:
  - 编程
tags:
  - 编程
  - 机器学习
  - 数据科学
  - 深度学习
  - Python
  - Keras
  - Tensorflow
date: 2020-06-13
updated: 2020-06-13
toc: true
toc_sticky: true
---

## GPU  Configuration

### Memory Growth

显存设置使用默认状况最好
按需要分配显存，显存可以节约，但是内存占用会变大

1. 使用 os 来实现

    ```python
    import os

    os.environ["TF_FORCE_GPU_ALLOW_GROWTH"] = "true"
    ```

1. 使用 tensorflow 来实现

    ```python
    import tensorflow as tf

    if tf.__version__.startswith("1."):  # tensorflow 1
        config = tf.ConfigProto()  # allow_soft_placement=True
        config.gpu_options.allow_growth = True
        sess = tf.Session(config=config)
    else:  # tensorflow 2
        tf.config.gpu.set_per_process_memory_growth(enabled=True)
    ```

1. 使用配置实现

    ```python
    gpus = tf.config.experimental.list_physical_devices("GPU")
    if len(gpus) > 0:
        tf.config.experimental.set_memory_growth(gpus[0], True)
    ```

### Memory Proportion

```python
from tensorflow.python.keras.backend import set_session

config = tf.ConfigProto()
config.gpu_options.per_process_gpu_memory_fraction = 0.9
set_session(tf.Session(config=config))
```

### Set Memory Limit

对需要进行限制的GPU的显存使用进行设置，设置的显存大小不合适的时候，程序可以正常启动，但是无法运行，即无法进行深度学习的计算，这个功能主要满足于多个程序使用同块显卡的需要

```python
tf.config.experimental.set_virtual_device_configuration(
    gpus[0], [tf.config.experimental.VirtualDeviceConfiguration(memory_limit=5000)]
)
```

## Keras

### Help

Keras 的网站 [keras.io](https://keras.io/)

Keras 的搜索功能不能使用，请使用 [Bing](http://en.bing.com) ，在搜索框中输入 `key1 key2 site: keras.io`，`key1` 和 `key2` 是需要搜索的关键词。
