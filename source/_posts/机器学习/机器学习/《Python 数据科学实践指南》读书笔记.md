---
title: 《Python 数据科学实践指南》读书笔记
excerpt: 一本 Python 的入门书，及数据科学实践中需要用到的 Python 包的入门书。
categories:
  - 编程
tags:
  - Python
  - 编程
  - 数据科学
date: 2019-03-04
updated: 2019-03-07
toc: true
toc_sticky: true
---

# 全书总评

- 书本印刷质量 : 4 星。印刷清楚，排版合适，错误很少。
- 著作编写质量 : 3 星。Python 入门和与数据处理相关的各种模块的入门，以及数据处理的入门。作者是原著，写的确实是自己的东西，不是东抄西抄。只是实践部分的内容实在太浅薄了些。
- 代码质量 : 4 星。Python 入门这本书的 IT 功底不够，中间可能会碰到一些坑，最好有点 Python 基础以后，用这本书查遗补缺。[下载地址](https://github.com/magigo/data_science_tool_book_code)
- 阅读笔记 : 记录需要记住的重点，方便快速回忆。

# C01.Python 介绍

## Python 版本

- Python 2.x : 2001 年发布，有许多资料和库基于这个版本编写；
- Python 3.x : 2009 年发布，与旧版本不兼容；

## Python 解释器

- Python 是开源的，因此它的解释器有许多种实现，主流的是 [官方的解释器](https://www.python.org)；

## Python 之禅

- import this

# C02.Python 基础知识

## 基础知识

- 基础数据类型 : 在 Python 中，所有的元素都是“对象”。
    - None : 表示什么都没有的类型；
    - int : 表示整数的类型；
    - float : 表示浮点数的类型；
    - bool : 表示布尔数值的类型；
    - str : 表示字符串的类型；
- 变量与赋值 : Python 的书写规范 ( PEP8 )
    - 序列解包
- 操作符与表达式 :
    - 算术操作符 :
    - 位操作符 :
    - 比较操作符 :
    - 逻辑操作符 : and，or，not
- 文本编辑器 :
    - 文件编码 : UTF-8

## 流程控制

- 条件判断 ( if...elif...else... ) :
- 循环 :
    - while :
    - for...in... :
- 缩进，空白与注释
    - 缩进 : 分割代码块；
    - 空白 : 没有任何意义，只为美观；
    - 注释 :
        - 单行注释 : “#”
        - 多行注释 : """..."""

## 函数及异常

### 函数

- 函数定义 : def func_name()
- 参数定义 :
    - 实参 :
    - 形参 :
    - 位置参数 ( 参数绑定 ) : 根据定义和调用函数时参数的位置进行参数的赋值；
    - 关键字参数 : 非关键字参数不能定义在关键字参数后面。
    - 可变数量的参数 : def func(*args, **kwargs)
        - *args : 位置形参，表示任意数量的位置参数都会合并成一个元组，绑定到 args 上；
        - **kwargs : 关键字形参，表示任意数量的关键字参数都会合并成一个元组，绑定以 kwargs 上；
- 递归 :
- 闭包 ( closure ) : 又称词法闭包 ( Lexical Closure ) 或函数闭包 ( Function Closure ) ，是引用了自由变量的函数。这个被引用的自由变量将和这个函数一同存在，即使已经离开了创建它的环境也不例外。

### 异常

- 异常 : ( try...except...finally... )

## 字符串

- 运算符 :
    - “+” : 字符串拼接；
    - “*” : 字符串复制多份，然后拼接；
    - 切片 [start,stop,step] :
    - len() : 字符串长度；

### 获取键盘输入

- input(str) :

### 字符串处理

- 字符集
    - 字符 ( Character ) : 是各种文字和符号的总称；
    - 字符集 ( Character Set ) : 是多个字符的集合，字符集的各类很多，每个字符集包含的字符个数也不同。
    - 字符编码 ( Character Encoding ) : 也称字集码，是把字符集中的字符编码按指定集合中的某一个对象，以便在计算机中存储和通过通信网络传递文本。
- ASCII 字符集和编码
- Unicode 字符集和 UTF-8 编码

### 字符串操作

- 基本操作 :
    - strip() : 移除字符串两侧的所有空白符；
    - capitalize() : 使字符串的首字母大写；
    - title() : 使字符串中每个单词的首字母大写；
    - lower() : 使字符串的所有字母小写；
    - upper() : 使字符串的所有字母大写；
    - isalnum() : 字符串中包含字母或者数字时为 True；
    - isdigit() : 字符串中只包含数字时为 True；
- 分割 :
    - split(delimiter) : 将字符串按指定分隔符分割；
    - rsplit(delimiter) : 将字符串从右边开始按指定分隔符分割；
- 格式化 :
    - % : print('%s' %name)
    - format() : print('\{one} and \{two}'.format(one='1',two='2'))

### 正则表达式

- 转义符 : P55
- re 模块 :
    - re.compile() : 匹配模式；
        - match() : 匹配字符串；

# C05. 容器 ( Container ) 与集合 ( Collections )

## 元组 ( Tuple )

- 元组 : 与字符串一样，是有序的序列，不可以改变内容
- 基本操作 : 连接、切片都与字符串保持一致；
- 序列解包 : 也称多重赋值；
- 支持迭代器协议，支持 for 循环

## 列表 ( List )

- 列表 : 也是序列类型的对象，但是可以改变列表中的内容；
- 基本操作 : 连接、切片都与字符串保持一致；
- 更改操作 :
    - pop() : 删除列表中的数据，并将删除的数据返回；
    - insert() : 插入数据；
    - append() : 追加数据；
    - extend() : 拼接列表；
- 引用传递 : 所有的赋值都只是引用的传递，并没有创建新的数据；
    - list[:] : 浅拷贝，只拷贝第一层引用的数据；
    - deepcopy() : 深拷贝，拷贝所有引用的数据；
- 列表解析式 : 构造列表的方式，将一个函数作用到整个列表中每个元素的方式；[x for x in range(1,3)]

## 字典 ( Dictionary )

- 字典在其他语言中被称为散列表，由 key:value 对通过｛｝组成的无序结构。
- 常用的函数 :
    - get(key) : 通过 key 取得对应的 value；还可以通过链式调用取值；
    - dict(list) : 构建新的字典；
    - dict.keys() : 获取字典的 keys 迭代；
    - dict.values() : 获取字典的 values 迭代；
    - dict.items() : 获取字典的 key:value 对的迭代；
    - dict.pop(key) : 取出指定关键字的值；
    - dict.update(key) : 更新字典中对应的 key 中的 value；

## 集合 ( Collections )

- namedtuple() : 具名元组。
- Counter() : 累加器，可以用来做经典的 word count；
- defaultdict() : 为字典设定一个默认值；
- OrderedDict() : 使字典有序；

# C06.Python 标准库

## 数学模块 : math

- 常见常量 : 默认精度为 15 位，最多可以取得 48 位
    - math.pi
    - math.e
- 无穷大与无效数字 :
    - math.inf : 表示“无穷大”，是 infinite 的缩写。math.inf*math.inf==math.inf
    - math.nan : 表示“无效数字”，是 Not a number 的缩写。math.inf/math.inf==math.nan
    - int 的范围 : 在 3.5 中 int 长度理论上是无限的
    - float 的范围 : 1e-309~1e+309
- 浮点数转换为整数 :
    - math.trunc(f) : 截掉浮点型小数点后面的数字；
    - math.floor(f) : 取最接近浮点型数字的整数；
    - math.ceil(f) : 取比当前浮点型数字大的整数；
- 绝对值和符号 :
    - math.fabs(f) : 浮点数的绝对值；
    - math.copysign(x,y) : 符号函数，将 y 的符号传递给 x；
- 常用计算 :
    - math.fsum(values) : 解决 sum(values) 不能精确计算的问题；
    - math.factorial(x) : 阶乘计算函数。
- 指数和对数 :
    - math.pow(x,y) : x 的 y 次幂；
    - math.log(x) : x 以 e 为底的对数；
    - math.log10(x) : x 以 10 为底的对数；

## 时间模块 : time，datetime，calendar

- time 模块 : 基础的时间处理模块；
    - time.time() : 表示为数字时间戳，即从格林威治时间 : 1970-1-1，0 : 0 : 0 ( 北京时间 : 1970-1-1，8 : 0 : 0 ) 以来所经历过的秒数；
    - time.ctime(x) : 没有 x 则返回当前时间的字符串版本，有 x 则返回从时间戳开始经过了 x 秒后的时间的字符串。
    - struct_time : 具名元组，可以分别获得年 (tm_year)、月 (tm_mon)、当月第几日 (tm_mday)、时 (tm_hour)、分 (tm_min)、秒 (tm_sec)，星期几 (tm_way，星期一是 0)，当年第几天 (tm_yday)，是否夏令时 (tm_isdst，没有为 0)；
        - time.gmtime() : 格林威治下的 struct_time；
        - time.localtime() : 当前电脑所在时区的 struct_time；
        - time.mktime(struct_time) : 还原成数字时间戳的方式；
        - time.strptime(string,format) : 将字符串格式的时间按照格式转换成 struct_time 格式；
        - time.strftime(format,struct_time) : 将 struct_time 格式的时间元组转换成字符串格式；
- datetime 模块 : 针对年月日和时分秒分别进行处理；
- calendar 模块 : 处理万年历；

## 随机数模块 : random

- 随机数生成器 :
    - `random.random()` : 生成均匀分布的浮点随机数，在半开半闭区间 $[0.0, 1.0)$；
    - `random.seed()` : 设置随机数种子；
    - `random.randint(a,b)` : 返回整型随机数，在闭区间 $[a,b]$；
    - `random.randrange()` : 返回整型随机数，在半开半闭区间 $[a,b)$；

## 取样

- `random.shuffle(list)` : 按随机性质将列表重新排列顺序；
- `random.choice(list)` : 按随机性质从列表中抽取数据；
- `random.sample(list,k)` : 按随机性质从列表中抽取指定长度的数据；

## 文件处理 : glob 和 fileinput

- `open(filename,mode)` : 基于 mode 模式打开 filename 文件；还支持上下文管理器 with 模式；
- glob 模块 : 目录处理；
- fileinput 模块 : 批量文件读入；是一个帮助类；

## 压缩 : bz2 和 gzip

- 不建议使用的压缩格式 :
    - rar : 专门服务于 Windows 下，Python 需要第三方库才能打开；
    - tar : 只用于打包文件，不对文件进行压缩；
    - zip : 既可以压缩文件，还可以打包文件，因为自带打包功能，不适合用于对文件单独压缩；
- 建议使用的压缩格式
    - bz2 模块 : 对单个文件可写可读，非常方便；
    - gzip 模块 : 对单个文件可写可读，非常方便；

## 漂亮打印 : pprint 模块

## 跟踪异常日志 : traceback 模块

## 网络数据传输 : JSON

- `json.loads()` : 将 JSON 转换成字典；
- `json.dumps()` : 将字典转换成 JSON；

# C07. 用 Python 读写外部数据

## CSV，csv 模块

- `csv.reader()` : 读取文件；
- `csv.writer()` : 写入文件；
- `csv.register_dialect()` : delimiter= 注册分割符；
- `csv.DictReader()` : 按照字典结构读取数据；*

## Excel，pandas 模块 ( 参考 C10 )

- `pandas.read_excel()` : 读取文件；
- `pandas.to_excel()` : 写入文件；
- `pandas.set_option()` : 设置属性值；
- `pandas.DataFrame()` : 表格结构；

## MySQL，MySQLdb 模块，torndb 模块

- `db=torndb.Connection(host,database,user,password)` : 建立数据库链接；
- `db.insert(SQL),db.insertmany(SQL)` : 向数据库中插入单行数据，插入多行数据；
- `db.query(SQL)` : 从数据库读取数据；

## PostgreSQL，psycopg2 模块

## MongoDB，pymongo 模块

## ElasticSearch，elasticsearch 模块

# C08. 用 Python 解决统计问题

- pandas 模块许多函数发生了改变；在 Python 3.7 下 `read_excel()` 运行有问题；统计都没办法测试了。

## 描述性统计

- 均值 :
- 中位数 :
- 方差 :
- 标准差 :

## 数据可视化

- 基本函数 :
    - `plot()`
    - `show()`
    - `figure()`
    - `title()`
    - `xlabel()`
    - `ylabel()`
    - `legend()`
- 图形 :
    - 折线图 :
    - 散点图 :
    - 柱状图 : `bar()`
    - 饼图 : `pie()`

# C09. 爬虫入门

## request 模块

- HTTP 协议 :
- 获取 HTML 内容 :

## Xpath 模块

- 解析 HTML 内容 :
- 这个模块已经没有匹配 Python3.7 的版本了，放弃这个学习。

# C10. 数据科学的第三方库

## Numpy 模块

- ndarray:
    - 创建 :
        - `arange()`
        - `linspace()` : 度量等宽
        - `random.random()` : 随机数
    - 属性 :
        - `ndim` : 数组的维度；
        - `shape` : 数组的形状；
        - `dtype.name` : 数组中数据的类型；
        - `itemsize` : 数组类型占用的内存空间大小；
        - `size` : 数组中元素个数；
    - 创建特定数组 :
        - `zeros()` : 全零矩阵；
        - `ones()` : 全 1 矩阵；
        - `empty()` : 随机的小值组成的矩阵；
- 基本运算 : 数组运算是基于元素计算的
    - 矩阵乘法 : `dot()`
    - 迭代 : 与列表类似，直接迭代按行取数，`flat()` 可以把数组摊平为一维数组
    - 变形 : `resize()` 原地修改数组；`reshape()` 输出一个变形后的数组，原数组不变；
    - 堆叠 : `hstack()` 行数相同，水平堆叠；`vstack()` 列数相同，垂直堆叠；
- 高级运算
    - `transpose()` : 转置；
    - `linalg.inv()` : 取逆；
    - `eye()` : 单位阵；
    - `trace()` : 取迹；
    - `linalg.solve()` : 解线性方程；
    - `linalg.eig()` : 解特征方程；

# 从这里开始，先弃了，这本书适合了解了以后，再来根据作者的实践角度查遗补缺

## Pandas 模块

有时候 DataFrame 中的行列数量太多，print 打印出来会显示不完全。

```python
# 显示所有列
pd.set_option("display.max_columns", None)
# 显示所有行
pd.set_option("display.max_rows", None)
# 设置 value 的显示长度为 100，默认为 50
pd.set_option("max_colwidth", 100)
```

`set_option()` 的所有属性 :
Available options:

- display.[chop_threshold, colheader_justify, column_space, date_dayfirst,
    date_yearfirst, encoding, expand_frame_repr, float_format, height, large_repr]
- display.latex.[escape, longtable, repr]
- display.[line_width, max_categories, max_columns, max_colwidth,
    max_info_columns, max_info_rows, max_rows, max_seq_items, memory_usage,
    mpl_style, multi_sparse, notebook_repr_html, pprint_nest_depth, precision,
    show_dimensions]
- display.unicode.[ambiguous_as_wide, east_asian_width]
- display.[width]
- io.excel.xls.[writer]
- io.excel.xlsm.[writer]
- io.excel.xlsx.[writer]
- io.hdf.[default_format, dropna_table]
- mode.[chained_assignment, sim_interactive, use_inf_as_null]

## Parameters

pat : str
Regexp which should match a single option.
Note: partial matches are supported for convenience, but unless you use the
full option name (e.g. x.y.z.option_name), your code may break in future
versions if new options with similar names are introduced.
value :
new value of option.

## Returns

None

## Raises

OptionError if no such option exists

## Notes

The available options with its descriptions:

display.chop_threshold : float or None
if set to a float value, all float values smaller then the given threshold
will be displayed as exactly 0 by repr and friends.
[default: None] [currently: None]

display.colheader_justify : 'left'/'right'
Controls the justification of column headers. used by DataFrameFormatter.
[default: right] [currently: right]

display.column_space No description available.
[default: 12] [currently: 12]

display.date_dayfirst : boolean
When True, prints and parses dates with the day first, eg 20/01/2005
[default: False] [currently: False]

display.date_yearfirst : boolean
When True, prints and parses dates with the year first, eg 2005/01/20
[default: False] [currently: False]

display.encoding : str/unicode
Defaults to the detected encoding of the console.
Specifies the encoding to be used for strings returned by to_string,
these are generally strings meant to be displayed on the console.
[default: UTF-8] [currently: UTF-8]

display.expand_frame_repr : boolean
Whether to print out the full DataFrame repr for wide DataFrames across
multiple lines, `max_columns` is still respected, but the output will
wrap-around across multiple "pages" if its width exceeds `display.width`.
[default: True] [currently: True]

display.float_format : callable
The callable should accept a floating point number and return
a string with the desired format of the number. This is used
in some places like SeriesFormatter.
See formats.format.EngFormatter for an example.
[default: None] [currently: None]

display.height : int
Deprecated.
[default: 60] [currently: 60]
(Deprecated, use `display.max_rows` instead.)

display.large_repr : 'truncate'/'info'
For DataFrames exceeding max_rows/max_cols, the repr (and HTML repr) can
show a truncated table (the default from 0.13), or switch to the view from
df.info() (the behaviour in earlier versions of pandas).
[default: truncate] [currently: truncate]

display.latex.escape : bool
This specifies if the to_latex method of a Dataframe uses escapes special
characters.
method. Valid values: False,True
[default: True] [currently: True]

display.latex.longtable :bool
This specifies if the to_latex method of a Dataframe uses the longtable
format.
method. Valid values: False,True
[default: False] [currently: False]

display.latex.repr : boolean
Whether to produce a latex DataFrame representation for jupyter
environments that support it.
(default: False)
[default: False] [currently: False]

display.line_width : int
Deprecated.
[default: 80] [currently: 80]
(Deprecated, use `display.width` instead.)

display.max_categories : int
This sets the maximum number of categories pandas should output when
printing out a `Categorical` or a Series of dtype "category".
[default: 8] [currently: 8]

display.max_columns : int
If max_cols is exceeded, switch to truncate view. Depending on
`large_repr`, objects are either centrally truncated or printed as
a summary view. 'None' value means unlimited.

```
In case python/IPython is running in a terminal and `large_repr`
equals 'truncate' this can be set to 0 and pandas will auto-detect
the width of the terminal and print a truncated object which fits
the screen width. The IPython notebook, IPython qtconsole, or IDLE
do not run in a terminal and hence it is not possible to do
correct auto-detection.
[default: 20] [currently: 20]
```

display.max_colwidth : int
The maximum width in characters of a column in the repr of
a pandas data structure. When the column overflows, a "..."
placeholder is embedded in the output.
[default: 50] [currently: 200]

display.max_info_columns : int
max_info_columns is used in DataFrame.info method to decide if
per column information will be printed.
[default: 100] [currently: 100]

display.max_info_rows : int or None
df.info() will usually show null-counts for each column.
For large frames this can be quite slow. max_info_rows and max_info_cols
limit this null check only to frames with smaller dimensions than
specified.
[default: 1690785] [currently: 1690785]

display.max_rows : int
If max_rows is exceeded, switch to truncate view. Depending on
`large_repr`, objects are either centrally truncated or printed as
a summary view. 'None' value means unlimited.

```
In case python/IPython is running in a terminal and `large_repr`
equals 'truncate' this can be set to 0 and pandas will auto-detect
the height of the terminal and print a truncated object which fits
the screen height. The IPython notebook, IPython qtconsole, or
IDLE do not run in a terminal and hence it is not possible to do
correct auto-detection.
[default: 60] [currently: 60]
```

display.max_seq_items : int or None
when pretty-printing a long sequence, no more then `max_seq_items`
will be printed. If items are omitted, they will be denoted by the
addition of "..." to the resulting string.

```
If set to None, the number of items to be printed is unlimited.
[default: 100] [currently: 100]
```

display.memory_usage : bool, string or None
This specifies if the memory usage of a DataFrame should be displayed when
df.info() is called. Valid values True,False,'deep'
[default: True] [currently: True]

display.mpl_style : bool
Setting this to 'default' will modify the rcParams used by matplotlib
to give plots a more pleasing visual style by default.
Setting this to None/False restores the values to their initial value.
[default: None] [currently: None]

display.multi_sparse : boolean
"sparsify" MultiIndex display (don't display repeated
elements in outer levels within groups)
[default: True] [currently: True]

display.notebook_repr_html : boolean
When True, IPython notebook will use html representation for
pandas objects (if it is available).
[default: True] [currently: True]

display.pprint_nest_depth : int
Controls the number of nested levels to process when pretty-printing
[default: 3] [currently: 3]

display.precision : int
Floating point output precision (number of significant digits). This is
only a suggestion
[default: 6] [currently: 6]

display.show_dimensions : boolean or 'truncate'
Whether to print out dimensions at the end of DataFrame repr.
If 'truncate' is specified, only print out the dimensions if the
frame is truncated (e.g. not display all rows and/or columns)
[default: truncate] [currently: truncate]

display.unicode.ambiguous_as_wide : boolean
Whether to use the Unicode East Asian Width to calculate the display text
width.
Enabling this may affect to the performance (default: False)
[default: False] [currently: False]

display.unicode.east_asian_width : boolean
Whether to use the Unicode East Asian Width to calculate the display text
width.
Enabling this may affect to the performance (default: False)
[default: False] [currently: False]

display.width : int
Width of the display in characters. In case python/IPython is running in
a terminal this can be set to None and pandas will correctly auto-detect
the width.
Note that the IPython notebook, IPython qtconsole, or IDLE do not run in a
terminal and hence it is not possible to correctly detect the width.
[default: 80] [currently: 80]

io.excel.xls.writer : string
The default Excel writer engine for 'xls' files. Available options:
'xlwt' (the default).
[default: xlwt] [currently: xlwt]

io.excel.xlsm.writer : string
The default Excel writer engine for 'xlsm' files. Available options:
'openpyxl' (the default).
[default: openpyxl] [currently: openpyxl]

io.excel.xlsx.writer : string
The default Excel writer engine for 'xlsx' files. Available options:
'xlsxwriter' (the default), 'openpyxl'.
[default: xlsxwriter] [currently: xlsxwriter]

io.hdf.default_format : format
default format writing format, if None, then
put will default to 'fixed' and append will default to 'table'
[default: None] [currently: None]

io.hdf.dropna_table : boolean
drop ALL nan rows when appending to a table
[default: False] [currently: False]

mode.chained_assignment : string
Raise an exception, warn, or no action if trying to use chained assignment,
The default is warn
[default: warn] [currently: warn]

mode.sim_interactive : boolean
Whether to simulate interactive mode for purposes of testing
[default: False] [currently: False]

mode.use_inf_as_null : boolean
True means treat None, NaN, INF, -INF as null (old way),
False means None and NaN are null, but INF, -INF are not null
(new way).
[default: False] [currently: False]

## Scikit-Learn 模块

## C11. 图数据分析

## 图论基础

## NetworkX 模块

## 利用 NetworkX 进行图分析

# C12. 大数据工具

## Hadoop

## Spark
