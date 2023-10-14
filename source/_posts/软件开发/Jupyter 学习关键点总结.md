---
title: Jupyter 学习关键点总结
excerpt: 学习过程中发现自己不熟悉的知识点
categories:
  - 编程
tags:
  - Jupyter Notebook
  - 编程
  - Python
date: 2020-09-24
updated: 2021-02-27
toc: true
toc_sticky: true
---

# Jupyter

Jupyter notebook 是一个很强大的交互式和展示数据科学项目的工具，它可以作为一个开发文档，包含代码、解释说明文字、代码运行结果、数学公式等等，功能非常强大，也是现在非常流行的工具。

## Notebook

### Install Notebook

- 使用 AnaConda 安装 Notebook
- 使用 pip 安装 Notebook：`pip install jupyter`
- 使用 VSCode 远程调用服务器的环境就可以使用远程服务器资源开发 Notebook

### Run Notebook

- 界面方式：使用 AnaConda 运行 Notebook
- 命令行方式：在选定的 Python 运行环境下，输入 `jupyter notebook`
    - 指定根目录：`jupyter notebook filepath`

### Browse Notebook

浏览器：`http://localhost:8888/tree`

## Shortcuts

- 通过菜单查看：`Help->Keyboard Shortcuts`
- 通过快捷键查看：`Ctrl+Shift+P`

### Common Shortcuts

- 编辑模式：`Eenter`
- 命令模式：`Esc`
    - 上下箭头：不同`cell`之间切换
    - 插入新的`cell`：`A`
    - 切换`cell`的模式
        - 切换为`Markdown Cell`：`M`
        - 切换为`Code Cell`：`Y`
    - 删除当前`cell`：连续两次`D`
    - 撤销上次操作：`Z`
    - 选择多个`cell`：`Shift`+上下箭头；合并多个`cell`：选择多个`cell`，再`Shift+M`

## Kernels

1. 检查是否安装kernel：`python -m ipykernel --version`
1. 安装kernel到没有安装的环境：`python -m pip install ipykernel`
1. 为 Jupyter Notebook 添加内核：`python -m ipykernel install --user --name=kernelname --display-name 'py37'`
    1. 安装后的说明文件为：`kernel.json`
1. 查看Jupyter Notebook安装的内核：`jupyter kernelspec list`
1. 删除Jupyter Notebook的内核：`jupyter kernelspec remove kernelname`

## Magic Methods

IPython 的常用魔术命令

| 命令                     | 说明                                  |
| ---------------------- | ----------------------------------- |
| `%cat a.py`            | 查看某一个文件的内容                          |
| `%run a.py`            | 执行某一个文件的内容，-i 代表在当前命名空间中执行          |
| `%quickref`            | 显示快速参考                              |
| `%who`                 | 显示当前命名空间中的变量                        |
| `%debug`               | 进入调试模式(按 q 键退出)                     |
| `%hist`                | 查看输入历史                              |
| `%magic`               | 查看所有魔术命令                            |
| `%env`                 | 查看系统环境变量                            |
| `%timeit`              | 检测某条语句的执行时间                         |
| `%paste`               | 执行剪贴板中的代码，如果使用快捷键则只粘贴不执行            |
| `%xdel`                | 删除变量，并且删除其在IPython上的一切引用            |
| `%run?`                | 魔术方法支持自省，在命令后面加「？」查看帮助文档；加「？？」查看源代码 |
| `%matplotlib inline`   | 图表会嵌入在输出的结果中，VSCode 默认支持就可以不加       |
| `%matplotlib`          | 可以与输出的图表进行交互操作（改变大小，导出图像）           |
| `%matplotlib notebook` | 默认参数，与上面的效果一样                       |
