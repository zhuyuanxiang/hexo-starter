---
title: "Python 学习关键点总结"
excerpt: "学习过程中发现自己不熟悉的知识点"
categories:
- 编程
tags:
- Python
- 编程
date: 2019-02-27
updated: 20201-10-2
toc: true
toc_sticky: true
---

# Python

## Program

### with

1. 执行 contextor 以获取上下文管理器
2. 加载上下文管理器的 exit ( ) 方法以备稍后调用
3. 调用上下文管理器的 enter ( ) 方法
4. 如果有 as var 从句，则将 enter ( ) 方法的返回值赋给 var
5. 执行子代码块 with_body
6. 调用上下文管理器的 exit ( ) 方法，如果 with_body 的退出是由异常引发的，那么该异常的 type、value 和 traceback 会作为参数传给 exit ( ) ，否则传三个 None
7. 如果 with_body 的退出由异常引发，并且 exit ( ) 的返回值等于 False，那么这个异常将被重新引发一次；如果 exit ( ) 的返回值等于 True，那么这个异常就被无视掉，继续执行后面的代码

### Style Guide

#### Variable

1. 「单下划线」开始的成员变量：叫做保护变量，用于标明只有类、子类及其对象可以访问。实际上仍然可以访问。
  - 例子：以单下划线开头 `_foo` 的代表不能直接访问的类属性，需通过类提供的接口进行访问，不能用 `from xxx import *` 导入。
2. 「双下划线」开始的成员变量：叫做私有变量，用于标明只有类及其对象可以访问，子类也不可以访问。解释器会更改变量的名称，称之为「名称修饰」，以便在类被扩展的时候不容易产生冲突。
  - 例子：以双下划线开头的 `__foo` 代表类的私有成员，并且无法直接访问，会在变量前面增加类名称
3. 「单下划线」结尾的成员变量：标明这个是类内部的参数，不是需要用户设置的参数；也可能是因为变量名称被关键字占用，加「单下划线」避免冲突。
4. 「单下划线」结尾的大写变量:  标明是不会发生改变的全局变量
5. 「单下划线」用途临时或者无意义变量的名称

#### Function

1. 「单下划线」开始的函数: 标明是一个私有函数， 只用于标明，
2. 「双下划线」开始和结尾的函数:  标明是特殊函数，但是不会发生「名称修饰」。
  - 例子：以双下划线开头和结尾的 `__foo__` 代表 python 里特殊方法专用的标识，如 `__init__ ( )` 代表类的构造函数。

#### Directories

图1：建议的目录格式

```shell
Project/
|-- bin/                # 项目的可执行文件目录，也可以命名`script`
|   |-- project
|
|-- docs/               # 项目文档目录
|   |-- examples/       # 项目示例代码目录
|   |-- conf.py
|   |-- config.rst
|   |-- generated
|   |-- index.rst
|   |-- installation.rst
|   |-- preface.rst
|   |-- quickstart.rst
|
|-- project/            # 项目的所有源代码都应该放在此目录，不要置于顶层目录
|   |-- tests/          # 单元测试代码
|   |   |-- __init__.py
|   |   |-- test_main.py
|   |
|   |-- __init__.py
|   |-- config.py       # 项目配置参数
|   |-- exception.py    # 项目所有异常
|   |-- main.py         # 项目入口
|   |-- preface.py      # 项目运行前的前置环境
|
|-- setup.py            # 安装、部署、打包的脚本
|-- requirements.txt    # 项目依赖的Python包列表，使用`pip`进行安装
|-- HISTORY.md          # 项目更新历史文件
|-- LICENSE             # 版权文件
|-- README.md           # 项目说明文件
|-- TODO.md             # 待处理问题
```

#### References

[Python 中下划线的 5 种含义](https://www.runoob.com/w3cnote/python-5-underline.html)

[Python 软件目录结构规范](https://cloud.tencent.com/developer/article/1175298)

## Labels

### Create property

`@property` 可以将 python 定义的函数「当做」属性访问，从而提供更加友好访问方式，但是有时候 setter/deleter 也是需要的。

- 只有@property 表示只读。
- 同时有@property 和 `@x.setter` 表示可读可写。
- 同时有@property 和 `@x.setter` 和 `@x.deleter` 表示可读可写可删除

## Error

### UnicodeDecodeError

报错详情：进入 Python 交互式界面时，报错。

![在这里插入图片描述](images/Python%20学习关键点总结/20200721194330146.png)

情况分析：可能在交互式界面操作中输入了全角字符，重新启动时就会引起错误。

解决方案：

1. 找到 `python_history` 文件，存在于 `C: \Users\Administrator\.python_history`，删除里面的全角字符

2. 打开文件 `C: \Python\Python37\lib\site-packages\pyreadline\lineeditor\history.py`，修改第 82 行，增加 `utf-8` 编码

    ```python
    for line in open ( filename,'r',encoding='utf-8' ) :
        ...
    ```

### Memoery Error

原因：内存不够时，系统会报的错误，特别是处理大型数据集时，会面临这个问题

解决：

- `numpy.zeros ( [rows_num, cols_num], dtype=np.bool )` : 使用 `bool` 可以使用更大的维度
- `scipy.sparse.lil_matrix (( rows_num,cols_num ) ,dtype= )` : 使用稀疏矩阵可以使用比 `numpy` 的矩阵更大的维度，系统推荐 `lil_matrix ( )`
- 使用 64 位的 Python + 扩大 Windows 的虚拟内存
  - 查看 Python 的版本
    - 在 Python Console 中，输入 `sys.version'
    - 在 Command 环境中，输入 `python`
    - 看到输出中有 `AMD64`，就表示安装的 Python 是 64 位的
  - 查看 Numpy 的版本，1.16.5 会出现第一次申请失败的问题
    - 建议升级到 1.18.1，当内存不够时，申请会报错，并且告诉你错误的原因
  - 扩大 Windows 虚拟内存，请参考 Windows 环境设置
    - 不能使用「自动管理所有驱动器的分页文件的大小」
    - 不能使用「系统管理的大小」
    - 在「自定义大小」中定义，我直接定义了「20G~40G」

## Formatter

### [pep8](https://pep8.org)

#### pep8 Args

max_line_length = 120
ignore = E501

#### pep8 Tools

- [Autopep8](https://pypi.org/project/autopep8/) : Autopep8 是比较宽松的格式化器，主要目的是修正代码中的错误，不太注意代码格式的统一。主要的格式化功能：重新缩进行、修复缩进、删除无关的空白、并重构常见错误 ( 如 Bool 和 None 值 ) 。文档中可以看到完整的更改列表。但是对于大部分格式都会默认正确，因此不太满足格式化工具的需要。
- [Yapf](https://github.com/google/yapf) : 是一个严格的格式化器，能够满足格式化的需要，是可以配置的。来自于 Google，但不是官方出品，其标准参考了：pep8、google、chromium 开发中使用的规范，解决了违反 PEP 8 规范的问题。重新格式化了没有违反 PEP 8 规范的代码。 ( 推荐 )
  - 需要安装 yapf Package 到 Python Interpreter
- [Black](https://black.readthedocs.io/en/stable/) : 与 autopep8 和 Yapf 类似，是一个严格的格式化器，但是不可配置。
- 三者的比较可以参考 [Python code formatters comparison](http://blog.frank-mich.com/python-code-formatters-comparison-black-autopep8-and-yapf/)

### Yapf

- 安装 yapf 模块

    ```powershell
    pip install yapf
    ```

- 导出 yapf 配置文件

    ```powershell
    yapf --style-help > yapf_style.cfg
    ```

- 修改 yapf 配置文件

  - [Yapf 的配置文件](https://github.com/google/yapf#id7)
        1. 定义在命令行 `yapf --style='{based_on_style: pep8, indent_width: 2}'`
        2. 在文件 `.style.yapf` 目录中定义 `[style]` 块的内容，文件放在当前目录中 或者 任意一层的父级目录中
        3. 在文件 `setup.cfg` 目录中定义 `[yapf]` 块的内容，文件放在当前目录中 或者 任意一层的父级目录中
        4. 在文件 `~/.config/yapf/style` 目录中定义 `[style]` 块的内容，文件放在只能放在 `~/.config/yapf/` 目录中

- [常见参数说明](https://github.com/google/yapf#id9)

```json
[yapf]
based_on_style = google
column_limit = 120  // max_line_length = 120
spaces_before_comment = 4
split_before_logical_operator = true
```

注： [yapf 中文翻译](https://python.freelycode.com/contribution/detail/699)

## Refactoring

VSCode 可以支持的重构方法：

- Extract Method
- Extract Variable
- Rename Symbol

VSCode 支持重构的条件：

- 使用 `jedi` 作为 「Language Server」，安装 「Rope」，但是无法使用「Visual Studio IntelliCode」插件
- 使用 「Microsoft」作为「Language Server」，可以使用「Visual Studio IntelliCode」，也可以使用全部功能

注：

- VSCode 的重构功能极为有限，网上看到 VS 的重构都是 Visual Studio 的重构功能。
- `No Refactoring Fail` 估计是因为 `Refactor...` 功能是提供给其他语言使用的，Python 的重构是直接映射到 Python 提供的重构功能上，而没有与这个重构选项关联起来。