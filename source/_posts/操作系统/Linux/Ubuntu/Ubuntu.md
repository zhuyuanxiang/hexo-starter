---
title: "Ubuntu 学习日志"
excerpt: "Ubuntu"
categories:
- Ubuntu
tags:
- Ubuntu
- Python
- 编程
- 机器学习
- Data Science
date: 2020-12-06
updated: 2020-12-07
toc: true
toc_sticky: true
---

# Change Source

[从公钥服务器接收失败：Server indicated a failure](https://www.codeleading.com/article/89801020493/)
`sudo apt-key adv --keyserver hkp://keyserver.ubuntu.com:80 --recv 76F1A20FF987672F`

更换国内源的操作流程

```bash
sudo cp /etc/apt/sources.list /etc/apt/sources.list.bak
sudo chmod 777 /etc/apt/source.list
sudo gedit /etc/apt/source.list
sudo apt update
sudo apt upgrade
```

阿里源：

```txt
deb http://mirrors.aliyun.com/ubuntu/ xenial main restricted universe multiverse
deb http://mirrors.aliyun.com/ubuntu/ xenial-security main restricted universe multiverse
deb http://mirrors.aliyun.com/ubuntu/ xenial-updates main restricted universe multiverse
deb http://mirrors.aliyun.com/ubuntu/ xenial-proposed main restricted universe multiverse
deb http://mirrors.aliyun.com/ubuntu/ xenial-backports main restricted universe multiverse
deb-src http://mirrors.aliyun.com/ubuntu/ xenial main restricted universe multiverse
deb-src http://mirrors.aliyun.com/ubuntu/ xenial-security main restricted universe multiverse
deb-src http://mirrors.aliyun.com/ubuntu/ xenial-updates main restricted universe multiverse
deb-src http://mirrors.aliyun.com/ubuntu/ xenial-proposed main restricted universe multiverse
deb-src http://mirrors.aliyun.com/ubuntu/ xenial-backports main restricted universe multiverse
```

清华源:

```txt
deb https://mirrors.tuna.tsinghua.edu.cn/ubuntu/ bionic main restricted universe multiverse
deb-src https://mirrors.tuna.tsinghua.edu.cn/ubuntu/ bionic main restricted universe multiverse
deb https://mirrors.tuna.tsinghua.edu.cn/ubuntu/ bionic-updates main restricted universe multiverse
deb-src https://mirrors.tuna.tsinghua.edu.cn/ubuntu/ bionic-updates main restricted universe multiverse
deb https://mirrors.tuna.tsinghua.edu.cn/ubuntu/ bionic-backports main restricted universe multiverse
deb-src https://mirrors.tuna.tsinghua.edu.cn/ubuntu/ bionic-backports main restricted universe multiverse
deb https://mirrors.tuna.tsinghua.edu.cn/ubuntu/ bionic-security main restricted universe multiverse
deb-src https://mirrors.tuna.tsinghua.edu.cn/ubuntu/ bionic-security main restricted universe multiverse
deb https://mirrors.tuna.tsinghua.edu.cn/ubuntu/ bionic-proposed main restricted universe multiverse
deb-src https://mirrors.tuna.tsinghua.edu.cn/ubuntu/ bionic-proposed main restricted universe multiverse
```

163源：

```txt
deb http://mirrors.163.com/ubuntu/ bionic main restricted universe multiverse
deb http://mirrors.163.com/ubuntu/ bionic-security main restricted universe multiverse
deb http://mirrors.163.com/ubuntu/ bionic-updates main restricted universe multiverse
deb http://mirrors.163.com/ubuntu/ bionic-proposed main restricted universe multiverse
deb http://mirrors.163.com/ubuntu/ bionic-backports main restricted universe multiverse
deb-src http://mirrors.163.com/ubuntu/ bionic main restricted universe multiverse
deb-src http://mirrors.163.com/ubuntu/ bionic-security main restricted universe multiverse
deb-src http://mirrors.163.com/ubuntu/ bionic-updates main restricted universe multiverse
deb-src http://mirrors.163.com/ubuntu/ bionic-proposed main restricted universe multiverse
deb-src http://mirrors.163.com/ubuntu/ bionic-backports main restricted universe multiverse
```

官方源：

```txt
deb http://cn.archive.ubuntu.com/ubuntu/ groovy main restricted
deb http://cn.archive.ubuntu.com/ubuntu/ groovy -updates main restricted
deb http://cn.archive.ubuntu.com/ubuntu/ groovy universe
deb http://cn.archive.ubuntu.com/ubuntu/ groovy -updates universe
deb http://cn.archive.ubuntu.com/ubuntu/ groovy multiverse
deb http://cn.archive.ubuntu.com/ubuntu/ groovy -updates multiverse
deb http://cn.archive.ubuntu.com/ubuntu/ groovy main restricted universe multiverse
deb http://security.ubuntu.com/ubuntu/ groovy main restricted
deb http://security.ubuntu.com/ubuntu/ groovy universe
deb http://security.ubuntu.com/ubuntu/ groovy multiverse
```

# Errors

## Transaction Failed

换源后安装或者更新软件时会出现「无法解决的软件包依赖」问题，将源换回到**官方源**就可以了。

# Packages

## Install

1. APT 方式
   1. 普通安装：`apt-get install softname1 softname2 ...`
   2. 修复安装：`apt-get -f install softname1 softname2 ...`
   3. 重新安装：`apt-get -reinstall install softname1 softname2 ...`
2. DPKG 方式
   1. 普通安装：`dpkg -i package_name.deb`
3. 源码安装
   1. 解压文件
      1. 文件后缀 gz ：`tar zxf xx.tar.gz`
      2. 文件后缀 Z ：`tar zxf xx.tar.Z`
      3. 文件后缀 tgz ：`tar zxf xx.tar.tgz`
      4. 文件后缀 tar ：`tar xf xx.tar`
      5. 文件后缀 bz2 ：`bunzip2 xx.tar.bz2`
   2. 编译安装

```bash
./configure
make
sudo make install
```

## Uninstall

1. APT方式
   1. 移除式卸载：`apt-get remove softname1 softname2 ...` (当包尾部有 `+`时，则为安装)
   2. 清除式卸载：`apt-get -purge remove softname1 softname2 ...`  (同时清除配置文件)
   3. 清除式卸载：`apt-get purge softname1 softname2 ...` (同时清除配置文件)
2. DPKG方式
   1. 移除式卸载：`dpkg -r pkg1 pkg2 ...`
   2. 清除式卸载：`dpkg -P pkg1 pkg2 ...`

## DPKG Search

1. 查看软件包的状态(支持模糊查询)：`dpkg -l pkgname`
2. 查看软件包的详细信息：`dpkg -s pkgname`
3. 查询软件包的相关文件：`dpkg -query -L pkgname`
4. 查询软件包存储的数据库：数据库为文本文件
   1. 在`/var/lib/dpkg/status` 文件中存在软件状态、控制信息
   2. 在 `/var/lib/dpkg/info/`目录下备份控制文件
      1. 在`.list` 文件中记录安装文件的清单
      2. 在 `.mdasums` 文件中保存文件的 MD5 编码

输出数据的格式说明

1. 第一行描述「期望状态」
   1. 未知(u)：软件包未安装，用户也未发出安装请求
   2. 安装(i)：用户请求安装的软件包
   3. 删除(r)：用户请求卸载的软件包
   4. 清除(p)：用户请求清除的软件包
   5. 保持(h)：用户请求锁定版本的软件包
2. 第二行描述「状态」
   1. 未安装(n)：
   2. 已安装(i)：已经安装，并且完成配置
   3. 仅存配置(c)：安装过，已经卸载，保留了配置文件在系统中
   4. 仅解压缩(U)：被解压缩，还未配置
   5. 配置失败(F)：配置软件包的过程失败
   6. 不完全安装(H)：未成功安装的软件包
   7. 触发器等待(W)：
   8. 触发器未决(T)：
3. 第三行描述「错误」
   1. 无标识的状态：软件包安装没有问题
   2. 标识(H)：软件包被强制保持，因为有其他软件包依赖需求，无法升级
   3. 标识(R)：软件包被破坏，可能需要重新安装才能正常使用
   4. 标识(X)：软件包被破坏，并且被强制保持

## Others

[详情参考](https://www.cnblogs.com/forward/archive/2012/01/10/2318483.html)

```bash
apt-cache search # ------(package 搜索包)
apt-cache show #------(package 获取包的相关信息，如说明、大小、版本等)
apt-get install # ------(package 安装包)
apt-get install # -----(package --reinstall 重新安装包)
apt-get -f install # -----(强制安装, "-f = --fix-missing"当是修复安装吧...)
apt-get remove #-----(package 删除包)
apt-get remove --purge # ------(package 删除包，包括删除配置文件等)
apt-get autoremove --purge # ----(package 删除包及其依赖的软件包+配置文件等（只对6.10有效，强烈推荐）)
apt-get update #------更新源
apt-get upgrade #------更新已安装的包
apt-get dist-upgrade # ---------升级系统
apt-get dselect-upgrade #------使用 dselect 升级
apt-cache depends #-------(package 了解使用依赖)
apt-cache rdepends # ------(package 了解某个具体的依赖,当是查看该包被哪些包依赖吧...)
apt-get build-dep # ------(package 安装相关的编译环境)
apt-get source #------(package 下载该包的源代码)
apt-get clean && apt-get autoclean # --------清理下载文件的存档 && 只清理过时的包
apt-get check #-------检查是否有损坏的依赖
dpkg -S filename -----查找filename属于哪个软件包
apt-file search filename -----查找filename属于哪个软件包
apt-file list packagename -----列出软件包的内容
apt-file update --更新apt-file的数据库

dpkg --info "软件包名" --列出软件包解包后的包名称.
dpkg -l --列出当前系统中所有的包.可以和参数less一起使用在分屏查看. (类似于rpm -qa)
dpkg -l |grep -i "软件包名" --查看系统中与"软件包名"相关联的包.
dpkg -s 查询已安装的包的详细信息.
dpkg -L 查询系统中已安装的软件包所安装的位置. (类似于rpm -ql)
dpkg -S 查询系统中某个文件属于哪个软件包. (类似于rpm -qf)
dpkg -I 查询deb包的详细信息,在一个软件包下载到本地之后看看用不用安装(看一下呗).
dpkg -i 手动安装软件包(这个命令并不能解决软件包之前的依赖性问题),如果在安装某一个软件包的时候遇到了软件依赖的问题,可以用apt-get -f install在解决信赖性这个问题.
dpkg -r 卸载软件包.不是完全的卸载,它的配置文件还存在.
dpkg -P 全部卸载(但是还是不能解决软件包的依赖性的问题)
dpkg -reconfigure 重新配置


apt-get install
下载软件包，以及所有依赖的包，同时进行包的安装或升级。如果某个包被设置了 hold (停止标志，就会被搁在一边(即不会被升级)。更多 hold 细节请看下面。
apt-get remove [--purge]
移除 以及任何依赖这个包的其它包。
--purge 指明这个包应该被完全清除 (purged) ，更多信息请看 dpkg -P。

apt-get update
升级来自 Debian 镜像的包列表，如果你想安装当天的任何软件，至少每天运行一次，而且每次修改了
/etc/apt/sources.list 後，必须执行。

apt-get upgrade [-u]
升 级所有已经安装的包为最新可用版本。不会安装新的或移除老的包。如果一个包改变了依赖关系而需要安装一个新的包，那么它将不会被升级，而是标志为 hold。apt-get update 不会升级被标志为 hold 的包 (这个也就是 hold 的意思)。请看下文如何手动设置包为 hold。我建议同时使用 '-u' 选项，因为这样你就能看到哪些包将会被升级。

apt-get dist-upgrade [-u]
和 apt-get upgrade 类似，除了 dist-upgrade 会安装和移除包来满足依赖关系。因此具有一定的危险性。

apt-cache search
在软件包名称和描述中，搜索包含xxx的软件包。

apt-cache show
显示某个软件包的完整的描述。

apt-cache showpkg
显示软件包更多细节，以及和其它包的关系。

dselect
console-apt
aptitude
gnome-apt
APT 的几个图形前端(其中一些在使用前得先安装)。这里 dselect 无疑是最强大的，也是最古老，最难驾驭。

普通 Dpkg 用法
dpkg -i
安装一个 Debian 包文件，如你手动下载的文件。

dpkg -c
列出 的内容。

dpkg -I
从 中提取包信息。

dpkg -r
移除一个已安装的包。

dpkg -P
完全清除一个已安装的包。和 remove 不同的是，remove 只是删掉数据和可执行文件，purge 另外还删除所有的配制文件。

dpkg -L
列出 安装的所有文件清单。同时请看 dpkg -c 来检查一个 .deb 文件的内容。

dpkg -s
显示已安装包的信息。同时请看 apt-cache 显示 Debian 存档中的包信息，以及 dpkg -I 来显示从一个 .deb 文件中提取的包信息。

dpkg-reconfigure
重 新配制一个已经安装的包，如果它使用的是 debconf (debconf 为包安装提供了一个统一的配制界面)。你能够重新配制 debconf 它本身，如你想改变它的前端或提问的优先权。例如，重新配制 debconf，使用一个 dialog 前端，简单运行：

dpkg-reconfigure --frontend=dialog debconf (如果你安装时选错了，这里可以改回来哟：)

echo " hold" | dpkg --set-selections
设置 的状态为 hlod (命令行方式)

dpkg --get-selections ""
取的 的当前状态 (命令行方式)

支持通配符，如：
Debian:~# dpkg --get-selections *wine*
libwine hold
libwine-alsa hold
libwine-arts hold
libwine-dev hold
libwine-nas hold
libwine-print hold
libwine-twain hold
wine hold
wine+ hold
wine-doc hold
wine-utils hold

dpkg -S
在包数据库中查找 ，并告诉你哪个包包含了这个文件。(注：查找的是事先已经安装的包)
```

# Terminal

## bash config

Ubuntu 的「终端」的配置文件为 `~/.bashrc` ，配置说明：

- HISTCONTROL：ignoreboth=ignoredups+ignorespace，不记录重复；不记录空格
- HISTTIMEFORMAT：记录的命令保存时间格式
- HISTSIZE：放入内存的历史命令行数，会话未结束时历史命令放在内存中
- HISTFILESIZE：放入文件的历史命令行数，会话结束时历史命令存入文件中
- HISTIGNORE：忽略的命令，引号包括，冒号隔开。例：`"ps:ls:ll:history"`
- HISTAPPEND：追加还是覆盖，默认是「追加」

## stdout to file

将运行命令的屏幕输出重定向到文件中

```bashell
# 将标准输出重定向到文件中(清空历史)
command > file.txt  # 1 可以省略不写
command 1> file.txt
# 将错误输出重定向到文件中(清空历史)
command 2> file.txt
# 将标准输出重定向到文件中(追加历史)
command >> file.txt
# 将错误输出重定向到文件中(追加历史)
command 2>> file.txt
# 将标准输出和错误输出重定向到文件中(清空历史)
command &> file.txt
command > file.txt 2>&1
# 将标准输出和错误输出重定向到文件中(追加历史)
command &>> file.txt
command >> file.txt 2>&1
```
