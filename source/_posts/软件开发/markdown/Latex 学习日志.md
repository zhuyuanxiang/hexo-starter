---
title: Latex 学习日志
excerpt: Latex 使用过程中的常见问题
categories:
  - 编程
tags:
  - Latex
  - PDF
date: 2019-01-01
updated: 2020-04-30
toc: true
toc_sticky: true
---

# Q&A

## Chinese

### Chinese Support

- 推荐使用 XeLaTeX 或者 lualatex，这两个编译器天然支持 UTF-8，还支持多种字体。(这个功能最强而且最稳定)

```latex
% ctexart 已经提供了对 ctex 的集成
\documentclass{ctexart}
\begin{document}
\tableofcontents
\begin{abstract}
这是简介及摘要。
\end{abstract}
\section{前言}
\section{关于数学部分}
数学、中英文皆可以混排。
You can intersperse math,
Chinese and English (Latin script) without adding extra environments.
這是繁體中文。
\end{document}
```

- 在 XeLaTeX 编译器中使用 xeCJK 宏包(仅仅处理中文，第一次编译目录会报错，忽略，再编译一次就会成功)

```latex
\documentclass{article}
\usepackage{xeCJK}
\usepackage[unicode={true}]{hyperref}
\begin{document}
% 目录需要添加{hyperref}、{inputenc}
\tableofcontents
\begin{abstract}
这是简介及摘要。
\end{abstract}
\section{前言}
\section{关于数学部分}
数学、中英文皆可以混排。
You can intersperse math,
Chinese and English (Latin script) without adding extra environments.
這是繁體中文。
\end{document}
```

- 在 pdfLaTeX 编译器中使用 CJKutf8 宏包(这个速度最快)

```latex
\documentclass{article}
\usepackage{CJKutf8}
\usepackage[unicode={true}]{hyperref}
\usepackage[utf8]{inputenc}
\begin{document}
\begin{CJK*}{UTF8}{gbsn}
% 目录需要添加{hyperref}、{inputenc}、\newpage
\tableofcontents
\begin{abstract}
这是简介及摘要。
\end{abstract}
\section{前言}
\section{关于数学部分}
数学、中英文皆可以混排。
You can intersperse math,
Chinese and English (Latin script) without adding extra environments.
\newpage
\end{CJK*}
\bigskip  %% Just some white space
You can also insert Latin text in your document
\bigskip  %% Just some white space
\begin{CJK*}{UTF8}{bsmi}
% 简繁字体不同
這是繁體中文。
\end{CJK*}
\end{document}
```

详情参考[LaTeX support Chinese](https://www.overleaf.com/learn/latex/Chinese)

### Encode as UTF8

```latex
% !TEX TS-program = pdflatex
% !TEX encoding = UTF-8 Unicode
```

### Indent First Line

`\usepackage{indentfirst}`

### Chinese Number

`\usepackage{CJKnumb}`，例子： `\CJKnumber{123}`

### Chinese Pingyin

```latex
\usepackage{pinyin}
\usepackage{ruby}
```

例子：`\ruby{我}{\wo3}`

### Font Size

- 五号字， 10pt

- 小四号字， 12pt

使用 ctexart 类

- 五号字，`\zihao{5}`
- `小四号字，\zihao{-4}`

## Layout

### Contents

- 修改「Contents」为中文「目录」或者其他名称
    - `\renewcommand*\contentsname{目录}`
- 目录标题Content的字体改为18pt加粗
    - `\renewcommand{\contentsname}{\fontsize{18pt}{\baselineskip}\selectfont \textbf{Content}}`

### Section Style

```latex
\usepackage{titlesec}
\titleformat[]{\section}{\centering\CJKfamily{hei}\boldmath}{}{0em}
\arabic{section}
\titlespacing {\section}{0em}{\*2}{\*0.5}
```

### Page Layout

`\usepackage [margin=1in,paperwidth=5cm,paperheight=6cm]{geometry}`

页面格式：宽 5 厘米，高 6 厘米，页边距为 2.54 厘米。

### Page Head & Foot

```latex
\usepackage{fancyhdr}
\pagestyle{fancy}
\fancyhf{} % 清空默认内容
% L|C|R：表示左|中|右
% E|O：表示偶数|奇数，需要在documentclass[twoside]，才能支持奇偶显示
\fancyhead[LE,RO]{内容}
\fancyfoot[C]{内容}
```

详情参考[fancyhdr 包的帮助](http://tug.ctan.org/macros/latex/contrib/fancyhdr/fancyhdr.pdf)

### Insert Image

```latex
\usepackage{epsfig}
\epsfig{file= filename,width=0.6\textwidth}
```

### Compose Subfigures

```latex
\usepackage{subfigure}
\subfigure[子标题]{\epsfig{xxx}}
```

### Long Table

```latex
\usepackage{longtable}
\endfirsthead
\endhead
```

帮助参考 `\CTeX\texmf\doc\latex\tools`

### New Separator

```latex
\usepackage{caption2}
renewcommand{\captionlabeldelim}{新分隔符}
```

### Space between Enumerators

`\usepackage{paralist}`，用 `compactenum` 环境。

### Beamer

`\documentclass[]{beamer}`

具体参考 beamer 文档。

## Math

必备宏包是： `amsmath`

### Bold in Math

- 整个公式粗体，`\boldmath{公式}\unboldmath`
- 部分变量粗体，`\usepackage{bm}`，`\bm{要加粗的}`

### Symbols in Math

`\usepackage{amssymb}`

- 因为：`\because`
- 所以：`\therefore`

宏包：`amssymb` 支持的内容很多，常备帮助文档。

### Wide Parens

`\usepackage{yhmath}`

- `\wideparen{ABC}`

### New Theorem Style

`\usepackage{amsthm}`

- 定义样式：`\newtheoremstyle`
- 定义定理环境：`\theoremstyle{xxx}`
- 使用新环境：`\newtheorem`

### Fonts

#### Cambria Math

建议选用微软 2013 开始提供的 "Cambria Math" 字体，更加清晰。

字体中缺少的符号

- 垂直符号：`\perp`缺少，可以使用'\bot'替换

## Others

### Code Highlight

`\usepackage{listings}`

- `\lstinputlisting [language=xxx,caption=xxx]{file}`
