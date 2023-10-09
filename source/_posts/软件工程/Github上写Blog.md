---
title: Github 上写 Blog
categories:
  - 版本管理
tags:
  - GitHub
  - Blog
  - Jekyll
  - Markdown
  - Pages
date: 2019-01-08
updated: 2019-01-08
toc: true
---


## Ready

必备软件

- Git
- GitHub
- [Jekyll](http://ju.outofmemory.cn/entry/98471)
- [Markdown](http://wow.kuapp.com/markdown/index.html)
- [YAML](https://blog.csdn.net/vincent_hbl/article/details/75411243)

可选软件

- HTML
- JavaScript
- CSS
- XML

DNS

- Ali：223.5.5.5、223.6.6.6

## Tools

可选 : VSCode+Markdown Preview Github Styling，GitHub Desktop

## Procedure

- 注册一个 GitHub 的账号，可以使用 GitHub Desktop 或者 GitHub 网站；
- 创建一个空的项目
  - 在项目里面创建 index.md ，或者 index.html，提交后，博客首页就建好了。
    - 注 1 : index.html 的优先级比 index.md 的高，系统会默认使用 index.html 显示。(系统需要编译，编译的进度请参考 [^footnote1])
  - 进入项目的 Settings，找到 Github Pages，修改 Source 的选项，默认是 None ( 也就是不开启主页显示模式 ) ，改为 master branch，点 save。页面重新刷新，再找到 Source 就会出现系统提示的显示地址。 ( 编译没有通过的项目是无法正常显示的 )
  - 如果还想换个漂亮的皮肤，就点 theme 就可以了。
- 如果不甘心使用这样简单的主页，你就需要 fork 一个别人的项目 barryclark/jekyll-now；
  - 修改_config.yml 文件
    - name : 博客的名字
    - description : 显示在浏览器上的标题栏的名字
    - avatar : 博主的照片 ( 可以从 Git 的 Profile 里面取照片的地址 )
    - \# Flags below are optional : 表示以下的内容可以选择性的修改
        - email，github 是我有的，其他的就看个人喜欢，不填内容就不会在网页上显示，也不会出错
        - sina，douban 等国内的需要修改代码才能实现 ( 人懒就先放一下了 )
        - permalink : 在前面加个、#把它屏蔽掉，因为默认的方式似乎无法正常显示。
    - Settings 设置参考前面的说明。
  - 按照“_post”目录下的“2014-3-3-Hello-World.md”的模式，依据“YYYY-MM-DD-文件名称。md”的标准创建一个新文件，就是你发表的新文章了。
  - 打开主页，就会看到文章的主题出现在主页，点进去就可以看到文章的内容。(系统需要编译，编译的进度请参考 [^footnote2])
- 如果还想更漂亮的的页面，更完整功能可以 fork 这个项目 minimal-mistakes
  - 具体修改可以 fork 我的项目 zhuyuanxiang.github.io，改了中文的地方就是我修改的，并且将需要的一些文件从 docs 目录下移到了根目录下。

## Notes

[^footnote1]: 了解编译的进度，可以点 Code 的 commits，进去后会看到自己本次提交的记录，如果后面有个绿色的√就是编译成功了，如果是个红色的×就是编译失败了，失败了主页就无法显示你想要展示的内容。

[^footnote2]: 如果不喜欢远程编译结果出来后才知道有没有问题，可以自己在本机安装 Jekyll，具体方法可以参考其他文章。
