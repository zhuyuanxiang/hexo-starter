---
title: PyCharm学习关键点总结
excerpt: 学习过程中使用 PyCharm 的一些经验，PyCharm 常用的一些插件。
categories:
- 编程
tags:
- PyCharm
- 插件
- 编程
date: 2019-12-29
updated: 2020-07-21
toc: true
toc_sticky: true
---

## Plugins

- Gazie : 帮助进行语法检查
- Key Promoter X : 提示使用的功能的快捷键是什么
- Nyan Progress Bar : 猫猫图案的进度条，看起来很乖。
- Paste Images into Markdown : 快速贴图到 Markdown 文件中。
- QuickJump : 14 以上的版本已经不再支持
- Rainbow Brackets : 括号类符号使用不同的颜色显示，方便识别。
- Statistic : 统计代码量
- TabNine : 基于机器学习的方法，支持所有语言的自动完成，极大地加速输入代码的速度。
- TeXiFy IDEA : 支持 LaTeX 和 BibTeX 的。
- Database Browser : 对多种数据库操作
  - MySQL 8.0 加入对 UTC 支持后，连接会出错，可以使用 Custom 方式输入连接串，在连接串的最后加入 `?serverTimezone=UTC` 即可
    - 完整连接串：`jdbc:mysql://localhost:3306/mysql?serverTimezone=UTC `
  - 插件支持：数据库连接，数据导出，SQL文件中直接执行SQL命令

## Console Startup

File → Settings → Build, Execution, Deployment → Console → Python Console → Starting Script
就可以把自己的启动脚本加入其中了。

## Python Script

File → Settings → File and Code Templates → Python Script

PyCharm 中的文件模版变量 :

- ${PROJECT_NAME} - 当前的项目名
- ${PRODUCT_NAME} - IDE 的名称
- ${NAME} - 在文件创建过程中，新文件对话框的命名
- ${USER} - 当前的登录用户
- ${DATE} - 现在的系统日期
- ${TIME} - 现在的系统时间
- ${YEAR} - 当前年份
- ${MONTH} - 当前月份
- ${DAY} - 当前月份中的第几日
- ${HOUR} - 现在的小时
- ${MINUTE} - 现在的分钟
- ${FILE} - 文件的名称
- ${MONTH_NAME_SHORT} - 月份的前三个字母缩写
- ${MONTH_NAME_FULL} - 完整的月份名

## Errors

### Console REPL Communication

这个是 PyCharm 的 BUG，解决办法

- Settings: `File > Settings > Build, Execution, Deployment > Console`，取消 `Use IPython if available` 的设置
  - 不过 IPython 很好用，建议不取消
- Settings: `File----> Settings---->Editor---->General---->Code Completion`，取消 `Rank completion suggestions based on Machine Learning`
  - 虽然使用更先进的代码提示很诱人，但是这个问题也比较麻烦，建议取消

## PyCharm 注册

### Windows

### Ubuntu

修改 `snap` 目录下的文件，遇到错误“只读文件，没有权限”。使用 `chmod` 不能修改其权限。 

```shell
chmod 777 pycharm64.vmoptions
```

可以直接修改 `~/.config/JetBrains/PyCharmxxxx.xx/` 目录下的 `pycharm64.vmoptions` 文件，如果没有这个文件可以直接创建这个文件，然后再将 `ja-netfilter-all/vmoptions/` 目录下的 `pycharm.vmoptions` 文件中的内容拷贝过来即可。注意对比 `snap` 目录下的 `pycharm64.vmoptions` 文件与其的不同。接着，就按着 Windows 中的内容注册成功。

[（只读文件系统无法修改权限 Ubuntu18.04）](https://blog.csdn.net/qq_44861675/article/details/108940136)