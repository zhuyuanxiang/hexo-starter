---
title: VSCode学习关键点总结
excerpt: 学习过程中使用 VSCode 的一些经验，VSCode 常用的一些插件。
categories:
  - 编程
tags:
  - VSCode
  - 插件
  - 编程
date: 2019-03-10
updated: 2023-06-13
toc: true
toc_sticky: true
---

# Visual Studio Code

## Settings

### Workspace

#### Python

```json
{
  "python.pythonPath": "C:\\ProgramData\\Anaconda3\\envs\\PyHanlp\\python.exe",
  "python.envFile": "C:\\Users\\Administrator\\IdeaProjects\\Hanlp-Books-Examples\\.vscode\\prod.env",
  "python.autoComplete.extraPaths": [
    "C:\\Users\\Administrator\\IdeaProjects\\Hanlp-Books-Examples\\src\\main\\python\\"
  ],
  "python.analysis.extraPaths": [
    "C:\\Users\\Administrator\\IdeaProjects\\Hanlp-Books-Examples\\src\\main\\python\\"
  ],
  "jupyter.notebookFileRoot": "${workspaceFolder}"
}
```

1. `python.pythonPath`: python 的环境路径
1. `python.envFile`: Debug 时使用的环境变量
1. `python.autoComplete.extraPaths`: 自定义软件包的路径用于自动补全功能
1. `python.analysis.extraPaths`: 自定义软件包的路径用于分析开发环境
1. `jupyter.notebookFileRoot`: \$\{workspaceFolder} Notebook 的默认路径为 workspaceFolder

#### Conda

VSCode 找不到 Conda 创建的 环境，就将 Python 和 Jupyter 插件删除再安装就可以了。

## Variables Reference

### Predefined variables

[Help Document](https://code.visualstudio.com/docs/editor/variables-reference)

\$\{workspaceFolder} VS Code 打开的目录

\$\{workspaceFolderBasename}   当前打开的文件夹的名字

\$\{file} 当前打开正在编辑的文件名，包括绝对路径，文件名，文件后缀名

\$\{relativeFile} 从当前打开的文件夹到当前打开的文件的路径

```
如 当前打开的是test文件夹，当前的打开的是main.c，并有test / first / second / main.c

那么此变量代表的是  first / second / main.c
```

\$\{fileBasename}  当前打开的文件名+后缀名，不包括路径

\$\{fileBasenameNoExtension} 当前打开的文件的文件名，不包括路径和后缀名

\$\{fileDirname} 当前打开的文件所在的绝对路径，不包括文件名

\$\{fileExtname} 当前打开的文件的后缀名

\$\{cwd} task runner 在启动时的工作目录

```
跟 cmd 里面的 cwd 是一样的
```

\$\{lineNumber}  当前打开的文件，光标所在的行数

## Errors

### Restart with 4096M

VSCode 打开过大的文件时，会报错。

解决方案：

1. 在配置 ( Settings ) 中，输入``files.maxMemoryForLargeFilesMB`，就可以调整 VSCode 使用的最大内存
1. 如果不想永久调整使用的内存大小，也可以在命令行输入 `Code --max-memory=8192M` 就可以打开大文件。
    1. 8192M 就是希望设置的内存大小，也可以根据自己的硬件配置其他参数

### Terminal Fonts

如果 Terminal 窗口显示的字留了很大的空白，可能是因为字体设置错误，例如(Font Size=56)导致的显示不了那么大的字体，但是字体占的位置很大，才感觉每个字符后面都跟了较大的空白。

## Plugins

### Remote Development

- Remote Development : 安装这个插件从而支持远程开发
    - Remote SSH : 使用 SSH Server 进行远程开发
    - 安装过程中遇到 VS Code Server Failed，可以重启电脑，删除 `C:\\Users\\username\\.vscodeserver` 目录，然后再次通过 VSCode 激活安装
    - python 开发时，需要把 conda 的运行目录加入 path 中，这样才能使用 conda 进行安装
    - Remote WSL : 使用 WSL ( Windows Subsystem for Linux )
    - Remote Containers : 使用 Docker 进行远程开发
    - 注：WSL 的性能不如 SSH 好，因此开发环境建议使用 Windows 10，服务器环境建议使用 Ubuntu

### Latex in use

- LaTeX Workshop : provide all-in-one features and utilities for LaTeX typesetting with Visual Studio Code.

### Latex for test

- latex-formatter :
- Latex Extension Pack

### Markdown in use

- Markdown All in One : 各种指定 Markdown 需要的插件；
    - Keyboard shortcuts
    - Table of contents
    - List editing
    - Print Markdown to HTML
    - GitHub Flavored Markdown
    - Math
    - Auto completions
    - Paste Link
    - Open Preview
- Markdown Theme Kit
- Markdown YAML Preamble
- markdown lint
- Pangu Markdown VSCode: 定制开发的插件供自己使用
    - @see [中文文案排版指北](https://GitHub.com/sparanoid/chinese-copywriting-guidelines)
- indent-switcher: 将缩进的 2 个空格转换成 4 个空格

### Markdown for test

- auto-markdown
- Convert to Markdown table : Convert list to Markdown table
- docs-markdown
- Markdown Preview GitHub Styling : Preview what your markdown will look like rendered on GitHub
- markdown-cn-en
- YAML :

### Git in use

- GitHub Pull Requests : 在 VSCode 中直接 Pull 到 GitHub 上
- GitLens : 随时知道文件修改的保存在 Git 中的历史

### Git for test

### javascript in use

- jshint
    - `( use 'esversion: 6' )`警告
    - 在`*.js`文件头加入`/* jshint esversion: 6 */`，在这个文件中就不会再报这个警告了
    - 在`setting.json`文件中加入`"jshint.options": {"esversion": 6},`，在所有文件中就不会再报这个警告了
    - js 文件的头加上 `/* jshint -W032 */` 就可以关闭 jshint W032 的检查

### Python in use

- Python : Python 开发环境
- Anaconda Extension Pack : 用于配合 Anaconda 一起使用

### Theme in use

- Chinolor Theme : 使用中国传统颜色的 VSCode 主题，可以使用代码颜色符合中国人的审美风格，强烈推荐。
- Winter is Coming Theme : 是我喜欢的黑色主题；

### Others in use

- addEngWhitespace : 基于 vscode-pangu 为英文加空格
- All Autocomplete : 自动完成需要的内容，从不同的文件中引入参考内容
- Beautify: 优化以下代码的格式。Beautify javascript, JSON, CSS, Sass, and HTML in Visual Studio Code.
- Bracket Select : Ctrl+Alt+A 选择括号中的内容
- bracket-padder : 对括号自动关闭，并且添加空格
- Better Comments : 改善代码中的注释阅读效果
- Code Runner : 用于直接运行编辑中的代码，不过对 Python 的中文支持不好，其它不清楚。
- Code Spell Checker : 自动检测代码中的拼写错误，使用 Ctrl+. 提示输入
- Indent To Bracket : 括号类的缩进
- indent-rainbow : 缩进的彩虹显示
- Path Autocomplete : filePath 自动补全。
- Path Intellisense: fileName 自动补全。
- Rainbow Bracket : 括号的彩虹显示
- sneak mark : 检测英文语句中的中文符号
- Sort lines : 对文本排序
- Spaces Inside Braces : 为括号增加合适的空格
- TabNine : 基于机器学习的文本自动完成
- TODO Highlight : TODO 高亮显示
- ToDo Tree : ToDo 的树形显示结构
- vscode-icons : 为 VScode 增加更加易读的图标
- Visual Studio IntelliCode
- XML Tools :

### Others for test

- Bracket Pair Colorizer : 可以优化代码中符号的颜色，好像对<>符号优化有问题，如果这个版本不满意，可以去找 Bracket Pair Colorizer 2。
- Chinese Support for Language Tool: 菜单中文显示，极为屏幕的有限空间，对笔记本电脑不太友好
- Custom CSS and JS Loader :
- Dictionary Completion :
- Exchange CHS Simbols : 将所有的中文符号换成英文的
- Fold : 自动折叠文件内容
- Fold Plus : 增加了折叠命令
- Git History : 方便查看 Git 的历史
- Git History Diff : 方便的进行 Git 历史比较
- Git Merger : 简化 Git 的 Merge 过程
- Git Project Manager : 方便进行 Git 的项目管理
- Leaper : 使用 Tab 跳出括号，但是会与其他定义的 TAB 键冲突，经常不起作用
- Quantum : Take the leap with your editor and start coding in the New Age with this extension pack.
- Subtitles Editor : 视频字幕编辑
- Tab Out : 使用 Tab 跳出括号，但是会与其他定义的 TAB 键冲突，经常不起作用
- bibtexLanguage : bibtex 语法高亮
- comment-autocomplete :
- css-minify : css minifier，不需要任何配置，基于 Mocha 框架
- naming : 帮助中国程序员将 Class, Function……的英文命名的标准化。
- shell-format : 对 shell 文件的格式化

## VSCode Server

### 国内镜像快速下载vscode server

1. 使用VSCode，打开远程服务器，现在服务器上已经存在下载进程
1. 使用ssh登录远程服务器，找出下载地址
    1. 输入：`ps aux|grep wget`，得到：`wget --tries=1 --connect-timeout=7 --dns-timeout=7 -nv -O vscode-server.tar.gz https://update.code.visualstudio.com/commit:b380da4ef1ee00e224a15c1d4d9793e27c2b6302/server-linux-x64/stable`
    1. 去掉`-nv`再次执行`wget --tries=1 --connect-timeout=7 --dns-timeout=7 -O vscode-server.tar.gz https://update.code.visualstudio.com/commit:b380da4ef1ee00e224a15c1d4d9793e27c2b6302/server-linux-x64/stable`，得到真实地址：`https://az764295.vo.msecnd.net/stable/b380da4ef1ee00e224a15c1d4d9793e27c2b6302/vscode-server-linux-x64.tar.gz`
    1. 将`az764295.vo.msecnd.net`替换为`vscode.cdn.azure.cn`，在本地电脑下载完成
    1. 将下载后的`vscode-server-llinux-x64.tar.gz`文件上传到服务器`~/.vscode-server/bin/b380da4ef1ee00e224a15c1d4d9793e27c2b6302`目录下
    1. 执行解压命令：`tar xf vscode-server-llinux-x64.tar.gz`
    1. 执行移动命令：`mv vscode-server-llinux-x64/* .`
1. 关闭本地的VSCode，再次打开远程服务器访问，正常。
