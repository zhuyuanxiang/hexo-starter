---
title: Pandoc 学习笔记
excerpt: Pandoc 使用过程中的常见问题
categories:
  - 编程
tags:
  - pandoc
  - Markdown
  - Latex
  - PDF
  - DOCX
  - PPTX
date: 2020-05-01
updated: 2020-05-05
toc: true
toc_sticky: false
---

# Q&A

## Installation

- 下载 [pandoc 软件](https://www.pandoc.org/)
    - 安装目录：`C:\Program Files\Pandoc`
        - 帮助文件：`Pandoc User's Guide.html`
- 查看 [pandoc 项目](https://github.com/jgm/pandoc)
- 了解 [pandoc 模板](https://github.com/jgm/pandoc-templates)
- 下载 [TexLive 软件](http://tug.org/texlive/)
    - 尽量安装完全版，避免使用过程中缺失宏包
- 下载 [Python 软件](https://www.python.org/)
    - 使用过滤器的时候才会用到，不是必须！

## Output Chinese Document

### PDF

`pandoc test.md -o test.pdf --pdf-engine=xelatex -V mainfont=SimSun`

### Beamer

`pandoc test.md -o test.pdf -t beamer --pdf-engine=xelatex -V mainfont=SimSun`

### DOCX

`pandoc test.md -o test.docx`

### PPTX

`pandoc test.md -o test.pptx`

### HTML

`pandoc test.md -o test.html`

### LaTeX

`pandoc test.md -o test.tex`

- `$公式$` 包裹的公式转换为 `\(公式\)`
- `$$公式$$` 包裹的公式转换为 `\[公式\]`

### Standalone Document

生成 HTML 和 LaTeX 文档时，会发现只有文档的主体(body)部分，没有完整的文件头和尾。
如果需要生成完整的文档，可以增加 `-s` 在命令最后。

- 生成完整的 HTML：`pandoc test.md -o test.html -s`
- 生成完成的 LaTeX：`pandoc test.md -o test.tex -s`

注：灵活地应用这个功能，可以提高调试文档代码的效率。

### Snatch at WEB

`pandoc -f html -t markdown -o web.md --request-header=User-Agent:"Mozilla/5.0" https://zhuyuanxiang.github.io/`

- `-o`：指定输出文件，否则自动输出到 `stdout`
- `--request-header=User-Agent:"Mozilla/5.0"`：发起 HTTP 请求时，设置的请求头；
- `--request-header User-Agent:"Mozilla/5.0"`：与上面的意思相同，只是变量后面使用空格( )代替了等号(=)

### Code Highlight

`pandoc test.md -o test-yahei.pdf --pdf-engine=xelatex -V mainfont="Microsoft YaHei" --highlight-style=breezeDark`

- '--highlight-style='_STYLE_|_FILE_
    - _STYLE_ ： `pygments`(默认)、`kate`, `monochrome`, `breezeDark`, `espresso`, `zenburn`, `haddock`, 和`tango`
    - _FILE_ ： 以 JSON 为文件格式，以 `.theme` 为文件后缀，基于 KDE 语法解析。
        - 基于 pandoc 的默认模式生成自己的主题文件：`pandoc --print-highlight-style pygments > my.theme`
- `--syntax-definition=`_FILE_
    - 一个 KDE XML 语法定义的文件，用于语法高亮代码块。可以被用于支持新的语言 或者 替代已经定义的语言。

## Templates

### Latex Template

```batch
pandoc test.md -o test.pdf --pdf-engine=xelatex -V mainfont=SimSun --template=template/zYxTom.tex
```

zYxTom.tex 是模板文件。

### DOCX Reference

```batch
pandoc test.md -o test.docx --reference-doc=template/zYxTom.docx
```

zYxTom.docx 是提供格式的文档。

### PPTX Reference

```batch
pandoc test.md -o test.pptx --reference-doc=template/zYxTom.pptx
```

zYxTom.pptx 是提供格式的文档。

- 如果一级标题与二级标题之间没有内容，则一级标题作为节标题
- 如果一级标题与二级标题之间存在内容，则一级标题作为标题和内容

### Default Template

没有提供模板时，pandoc 使用自带的默认模板，如果想参考默认模板，可以：

- 使用命令`pandoc -D latex`，可以获取 latex 的默认模板
- 访问[项目仓库](https://github.com/jgm/pandoc-templates)，随时跟踪最新模板，也可以与自己制作的模板进行对比，了解最新版本 pandoc 的变化。

## Metadata blocks

### Extension: `pandoc_title_block`

文件可以由标题块开始，并且标题块会被解析为书目信息。

```batch
% title
% author(s) (separated by semicolons)
% date
```

如果某个信息不想输入，可以保持空白。详情参考[翻译文档](https://github.com/zhuyuanxiang/StudyNotes-CN/blob/master/Translate/Pandoc-User-Guide-%E4%B8%AD%E6%96%87%E7%89%88.md)

### Extension: `yaml_metadata_block`

[YAML](https://yaml.org/spec/1.2/spec.html "YAML v1.2 Spec") 元数据块是有效的 YAML 对象，开头使用三个连字符(`---`) ，结尾使用三个边字符(`---`) 或者 三个句点 (`...`) 分隔。YAML 元数据块可以放在文档的任意位置，但是如果它不在最开始，那么数据块前面一定要留空白行。 (注：因为 pandoc 可以将输入文件连接起来，因此也可以将元数据块放在一个独立的 YAML 文件中。作为一个参数，接在所有的 Markdown 文件后面传给 pandoc )

```batch
pandoc chap1.md chap2.md chap3.md metadata.yaml -s -o book.html
```

### Default files

`--defaults` 选项可以被用于定义一个选项包。将各种选项和变量都定义在里面

```batch
pandoc --defaults=test.yaml
```

文件中的内容参考如下，也可以参考[PRML的应用](https://github.com/zhuyuanxiang/StudyNotes-CN/blob/master/PRML/prml-pdf.yaml)：

```yaml
from: markdown+emoji
# reader: may be used instead of from:
to: html5
# writer: may be used instead of to:

# leave blank for output to stdout:
output-file:
# leave blank for input from stdin, use [] for no input:
input-files:
  - preface.md
  - content.md
# or you may use input-file: with a single value

template: letter
standalone: true
self-contained: false

# note that structured variables may be specified:
variables:
documentclass: book
classoption:
  - twosides
  - draft

# metadata values specified here are parsed as literal
# string text, not markdown:
metadata:
author:
  - Sam Smith
  - Julie Liu
metadata-files:
  - boilerplate.yaml
# or you may use metadata-file: with a single value

# Note that these take files, not their contents:
include-before-body: []
include-after-body: []
include-in-header: []
resource-path: [.]

# filters will be assumed to be Lua filters if they have
# the .lua extension, and json filters otherwise. But
# the filter type can also be specified explicitly, as shown:
filters:
  - pandoc-citeproc
  - wordcount.lua
  - type: json
path: foo.lua

file-scope: false

data-dir:

# ERROR, WARNING, or INFO
verbosity: INFO
log-file: log.json

# citeproc, natbib, or biblatex
cite-method: citeproc
# part, chapter, section, or default:
top-level-division: chapter
abbreviations:

pdf-engine: pdflatex
pdf-engine-opts:
  - -shell-escape
# you may also use pdf-engine-opt: with a single option
# pdf-engine-opt: "-shell-escape"

# auto, preserve, or none
wrap: auto
columns: 78
dpi: 72

extract-media: mediadir

table-of-contents: true
toc-depth: 2
number-sections: false
# a list of offsets at each heading level
number-offset: [0, 0, 0, 0, 0, 0]
# toc: may also be used instead of table-of-contents:
shift-heading-level-by: 1
section-divs: true
identifier-prefix: foo
title-prefix: ''
strip-empty-paragraphs: true
# lf, crlf, or native
eol: lf
strip-comments: false
indented-code-classes: []
ascii: true
default-image-extension: .jpg

# either a style name of a style definition file:
highlight-style: pygments
syntax-definitions:
  - c.xml
# or you may use syntax-definition: with a single value
listings: false

reference-doc: myref.docx

# method is plain, webtex, gladtex, mathml, mathjax, katex
# you may specify a url with webtex, mathjax, katex
html-math-method:
method: mathjax
url: https://cdn.jsdelivr.net/npm/mathjax@3/es5/tex-mml-chtml.js
# none, references, or javascript
email-obfuscation: javascript

tab-stop: 8
preserve-tabs: true

incremental: false
slide-level: 2

epub-subdirectory: EPUB
epub-metadata: meta.xml
epub-fonts:
  - foobar.otf
epub-chapter-level: 1
epub-cover-image: cover.jpg

reference-links: true
# block, section, or document
reference-location: block
atx-headers: false

# accept, reject, or all
track-changes: accept

html-q-tags: false
css:
  - site.css

# none, all, or best
ipynb-output: best

# A list of two-element lists
request-headers:
  - [User-Agent, Mozilla/5.0]

fail-if-warnings: false
dump-args: false
ignore-args: false
trace: false
```

## Debug

### Debug Output

`pandoc test.md -o test.pdf --pdf-engine=xelatex -V mainfont=SimSun --verbose`

给出详细的调试输出。这个仅对 PDF 输出时有效。

### Quite Model

`pandoc test.md -o test.pdf --pdf-engine=xelatex -V mainfont=SimSun --verbose --quiet`

安静模式。抑制所有的信息。

注：哪怕是打开调试输出，也不会产生任何信息。

### Fail if Warnings

`pandoc test.md -o test.pdf --pdf-engine=xelatex -V mainfont=SimSun --fail-if-warnings`

遇到警告就退出转换过程。

### Log File

`pandoc test.md -o test.pdf --pdf-engine=xelatex -V mainfont=SimSun --log=log.json`

- `verbosity=ERROR|WARNING|INFO`： 可以在 yaml 文件中定义

注：因为生成 PDF 的过程中，会遇到许多不可知问题，保存日志方便调试。

### References

pandoc 提供了三种方式：

- 脚注 ( footnote )
- 行内笔记 ( inline_notes )
- 参考文献 ( citations )
