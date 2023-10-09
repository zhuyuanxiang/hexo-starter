---
title: "Ubuntu的ZSH安装说明"
excerpt: "Ubuntu"
categories:
- Ubuntu
tags:
- Ubuntu
- ZSH
- Shell
date: 2022-04-18
updated: 2022-04-18
toc: true
toc_sticky: true
---

# ZSH 介绍

Z shell（Zsh）是一款可用作交互式登录的shell及脚本编写的命令解释器。Zsh对Bourne shell做出了大量改进，同时加入了Bash、ksh及tcsh的某些功能。

[WIKI 百科](zh.wikipedia.org/wiki/Z_shell)

## 安装 ZSH

```shell
# 安装
sudo apt install zsh

# 将 zsh 设置为默认 shell
chsh -s /bin/zsh

# 检查
echo $SHELL

# 返回 /usr/bin/zsh 即表示成功；若没成功，重启试试看
```

## 安装 Oh My Zsh

Oh My Zsh 是基于 Zsh 命令行的一个扩展工具集，提供了丰富的扩展功能，如：主题配置，插件机制，内置的便捷操作等，，可以给我们一种全新的命令行使用体验。[[1\]](https://matnoble.me/tech/ubuntu/install-zsh/#fn:1)

```shell
# 1. 通过 curl
sh -c "$(curl -fsSL https://raw.githubusercontent.com/ohmyzsh/ohmyzsh/master/tools/install.sh)"

# 2. 通过 wget
sh -c "$(wget -O- https://raw.githubusercontent.com/ohmyzsh/ohmyzsh/master/tools/install.sh)"

# 3. 通过 git
cd ~
git clone https://github.com/ohmyzsh/ohmyzsh ohmyzsh
cd ./tools
sh install.sh
```

## 配置

```shell
vim ./.zshrc

# 更换主题，默认主题安装目录`ohmyzsh/themes`
ZSH_THEME='ys'
# 注： ys 是我喜欢的主题

# 启动错误命令自动更正
ENABLE_CORRECTION="true"

# 在命令执行的过程中，使用小红点进行提示
COMPLETION_WAITING_DOTS="true"

# 启用已安装的插件，默认插件安装目录 `ohmyzsh/plugins`
plugins=(
    colorize 
    colored-man-pages 
    extract 
    git
    screen
    web-search
    z
    zsh-autosuggestions 
    zsh-syntax-highlighting 
    git-open
)
```

### 主题效果列表

[Themes · ohmyzsh/ohmyzsh Wiki (github.com)](https://github.com/ohmyzsh/ohmyzsh/wiki/Themes)

[ZSH主题 - Alvin, - 博客园 (cnblogs.com)](https://www.cnblogs.com/chenyangqit/p/11571228.html)

### 自带插件

- 跟踪最常访问的目录 - `z`
- 网页搜索插件 - `web-search plugin`
- 解压压缩包插件 - `extract`

### 安装插件

```shell
# 手工安装插件
cd ~/.oh-my-zsh/custom/plugins
# 1. 自动补全
git clone https://github.com/zsh-users/zsh-autosuggestions zsh-autosuggestions
# 2. 代码高亮
git clone https://github.com/zsh-users/zsh-syntax-highlighting zsh-syntax-highlighting
# 3. 快速打开 GitHub 仓库
git clone https://github.com/paulirish/git-open git-open
```

### 安装字体

```shell
# 克隆字体
git clone https://github.com/powerline/fonts --depth=1

# 安装字体
cd fonts
./install.sh

# 清理环境
cd ..
rm -rf fonts
```



在Terminal（终端）中选择“配置文件→配置文件首选项→自定义字体→Ubuntu Mono derivative Powerline Regular”

测试字体安装是否正常

```
echo "\ue0b0 \u00b1 \ue0a0 \u27a6 \u2718 \u26a1 \u2699"
```

正常显示结果：![在这里插入图片描述](https://img-blog.csdnimg.cn/2020102112380298.png#pic_center)

注：Windows 的 Terminal 的字体也是不足的，有些符号显示不出来，“幼圆”是字体相对最全的，不过没必要。

## shell 常用命令

```shell
#  查看当前所用的 Shell
echo $SHELL

# 查看系统内已安装的 Shell
cat /etc/shells

# 用 GUI 文件管理器或编辑器打开指定的的文件或目录
xdg-open fileOrDir

# 更新 .zshrc
source ~/.zshrc
```

## 参考资料

1. [Zsh + Oh My Zsh 全程指南「程序员必备」](https://segmentfault.com/a/1190000013612471)
2. [Zsh 全程指南](https://link.segmentfault.com/?enc=lCHwzOzwdIY%2F2wW8z3htAQ%3D%3D.9qhaw4GGJceqcSEcq%2BofPuiZ5%2Bj24QEy0kzP0TrjCuQduDZaW5BYDd4BCspbsbB1SpiByb42ZPoS%2B3mQ7PL0vw%3D%3D)
3. [Ubuntu 16.04 下安装 Zsh 和 Oh My Zsh](https://link.segmentfault.com/?enc=kdnwxqzWYbgWoxx5JPPfaA%3D%3D.QcRUqDYxgtLooogt%2B6X%2FlUqz%2Fo1R9jllL6JXRorZIQ0wHhEDIZVPWPS8C9jFiPbM)
4. [Ubuntu 下 Oh My Zsh 的最佳实践「安装及配置」 - SegmentFault 思否](https://segmentfault.com/a/1190000015283092)
5. [Ubuntu20.04 配置Oh My Zsh](https://blog.csdn.net/baidu_26678247/article/details/120616902?utm_medium=distribute.pc_relevant.none-task-blog-2~default~baidujs_baidulandingword~default-0.topblog&spm=1001.2101.3001.4242.1&utm_relevant_index=3)
6. [Welcome to the Oh My Zsh wiki](https://github.com/ohmyzsh/ohmyzsh/wiki)
7. [Installing ZSH](https://github.com/ohmyzsh/ohmyzsh/wiki/Installing-ZSH#how-to-install-zsh-in-many-platforms)
8. [Powerline fonts](https://github.com/powerline/fonts)
9. [Zsh (简体中文)](https://wiki.archlinux.org/index.php/Zsh_(简体中文))
10. [Download zsh from sourceforge](https://sourceforge.net/projects/zsh/files/)
11. [Ubuntu 界面美化](https://blog.csdn.net/u011254082/article/details/52725512)
12. [利用Oh-My-Zsh打造你的超级终端](https://blog.csdn.net/czg13548930186/article/details/72858289)
13. [Ubuntu 下zsh的安装与配置](https://www.jianshu.com/p/4fde9ae77922)
14. [oh-my-zsh 插件](https://hufangyun.com/2017/zsh-plugin/)
15. [Linux终极shell-zsh的完美配置方案！——oh-my-zsh](https://blog.csdn.net/amoscykl/article/details/80616873)

