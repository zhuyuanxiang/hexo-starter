---
title: "Linux 版本对比"
excerpt: "Linux 不同版本性能与用途对比"
categories:
- OS
tags:
- Ubuntu
- Debian
- CentOS
- Red Hat
- RHEL
- openSUSE
- Fedora
- Deepin
date: 2022-04-18
updated: 2022-04-18
toc: true
toc_sticky: true
---
# 总览

## Deepin

优点：开发人员为用户提供了几乎与Windows相同的体验，自主开发的Deepin桌面环境及一系列软件为其他发行版所不及，也提供了QQ，是一个非常适合于国人的操作系统。

缺点：几乎没有缺点。

## **Lubuntu**

**优点：** 轻快简洁，768m内存都能用出4G内存的快感。

**缺点：** 虽说是专为老旧电脑设计，但如果你的设备太古老…想安装上还是很费事的，可能连驱动关都过不了。我曾经尝试在我的一台256M RAM 1.9GHz cpu的笔记本上安装[lubuntu](https://www.zhihu.com/search?q=lubuntu&search_source=Entity&hybrid_search_source=Entity&hybrid_search_extra=%7B%22sourceType%22%3A%22answer%22%2C%22sourceId%22%3A2301636894%7D)，以失败告终。

## Puppy Linux

**优点：** Puppy是格外的小，能从64M的存储设备启动，并且，整套系统都能在内存中运行，是个很不错的[U盘应急系统](https://www.zhihu.com/search?q=U%E7%9B%98%E5%BA%94%E6%80%A5%E7%B3%BB%E7%BB%9F&search_source=Entity&hybrid_search_source=Entity&hybrid_search_extra=%7B%22sourceType%22%3A%22answer%22%2C%22sourceId%22%3A2301636894%7D)。

**缺点：** 相比Win PE它稍显逊色，由于没有包管理器，你几乎需要从源码编译一切应用，才能在上面安装新软件。不过他是让你体验linux的很经济的一个方法。

## Arch Linux

**优点：** 拥有最庞大的软件库，你几乎可以使用他的包管理器[pacman](https://www.zhihu.com/search?q=pacman&search_source=Entity&hybrid_search_source=Entity&hybrid_search_extra=%7B%22sourceType%22%3A%22answer%22%2C%22sourceId%22%3A2301636894%7D)安装任何东西。它的中文文档非常完善，即使对于我这个debian系用户来说，也不得不常常到arch wiki去查东西。另外，它很有助于你增加对系统底层的了解，定制化程度很高，也没有什么多余的软件。另外，它和[manjaro](https://www.zhihu.com/search?q=manjaro&search_source=Entity&hybrid_search_source=Entity&hybrid_search_extra=%7B%22sourceType%22%3A%22answer%22%2C%22sourceId%22%3A2301636894%7D)一样，可以安装qq。

**缺点：** 他的缺点是定制化程度太高了，安装起来也有一定难度，让很多新手望而却步。在安装完之后，你所面对的是一个黑不溜秋的终端，各种驱动 桌面环境 [应用管理器](https://www.zhihu.com/search?q=%E5%BA%94%E7%94%A8%E7%AE%A1%E7%90%86%E5%99%A8&search_source=Entity&hybrid_search_source=Entity&hybrid_search_extra=%7B%22sourceType%22%3A%22answer%22%2C%22sourceId%22%3A903672359%7D)等等等等，全部需要自己手动敲命令下载，一不小心敲错了，系统也就崩了。换句话说，想做到像宣传图这样的效果，有时要花去你一个多月的时间。常常滚动更新，滚挂了之后很折腾。

## Debian

**优点：** 精简而稳定，它是数万人共同努力的成果。它的deb包高度集中，依赖性问题出现的很少。当然，它也拥有最大的支持社区。

**缺点：** 由于它是完全自由的操作系统，因此没有专业的技术支持。另外它的更新周期很长，软件库里很多软件也显得老旧了。

## Solus

**优点：** 非常简洁快速，几乎所有评论中都提到了它神奇的开机速度。由于它是新兴的发行版本，[设计概念](https://www.zhihu.com/search?q=%E8%AE%BE%E8%AE%A1%E6%A6%82%E5%BF%B5&search_source=Entity&hybrid_search_source=Entity&hybrid_search_extra=%7B%22sourceType%22%3A%22answer%22%2C%22sourceId%22%3A2301636894%7D)也是比较前卫的，不会存在冗余代码的问题。另外，它的包管理器也是全新设计的，安装应用速度非常快。

**缺点：** 所有新的发行版都有这个问题：对驱动的支持不好，有的网友也指出solus会在他们的电脑上崩溃或者运行效率很低。不清楚它的[包管理机制](https://www.zhihu.com/search?q=%E5%8C%85%E7%AE%A1%E7%90%86%E6%9C%BA%E5%88%B6&search_source=Entity&hybrid_search_source=Entity&hybrid_search_extra=%7B%22sourceType%22%3A%22answer%22%2C%22sourceId%22%3A2301636894%7D)，因此我谨慎地假设，它能安装的软件数量也是屈指可数的。

## Ubuntu

Ubuntu 22.04.1 LTS

**优点：** 社区支持非常完善，你几乎可以在ASK UBUNTU社区里询问一切关于linux的问题，大部分问题都能得到热心的解答。另外，ubuntu作为一个成熟的系统，被广泛地应用，软件数量能与Arch匹敌了。

**缺点：** 是个好的操作系统，但不是一个好的个人操作系统。Unity桌面的性能很低，并且不是很稳定，常常卡死。内置的软件大多数没用，你通常要花上一个星期才能把ubuntu打造成适合自己的系统。另外，它也不是很稳定。至少在我使用时，平均每天都要出现三四次内部错误，对使用体验的负面影响比较大。

## Manjaro

**优点：** 由于基于Arch，它获得了惊人数量的软件库。安装很多软件时，你不需要百度，不需要到处找，一个命令就全部ok了。另外，它的易用性也是它极大的优势。相比上面的系统，它在简洁性上完胜。另外更棒的是，它提供了直接可用的QQ！

**缺点：** 在网上找了好久，几乎没发现有对它负面评论。一定要说有缺点的话，可能是arch系的通性：滚动更新的方式容易崩溃…还有似乎并没有自带[中文输入法](https://www.zhihu.com/search?q=%E4%B8%AD%E6%96%87%E8%BE%93%E5%85%A5%E6%B3%95&search_source=Entity&hybrid_search_source=Entity&hybrid_search_extra=%7B%22sourceType%22%3A%22answer%22%2C%22sourceId%22%3A2301636894%7D)…

## Linux Mint

**优点：** Mint最大的特点就是极其符合windows用户的操作习惯，甚至贴心地准备了更新管理器、开始菜单、Office等用户在windows上喜闻乐见的功能。mint是一个真正的开箱即用的发行版本。它完善到你完成安装后甚至不用再添加别的软件，就可以畅快开始使用。相比它的爸爸ubuntu，这个儿子在各个方面都做得更好。

**缺点：** 如此高度集中化的linux版本，自然会引来某些人士关于臃肿的批评。而且，基于[debian](https://www.zhihu.com/search?q=debian&search_source=Entity&hybrid_search_source=Entity&hybrid_search_extra=%7B%22sourceType%22%3A%22answer%22%2C%22sourceId%22%3A2301636894%7D)注定了他的软件库数量不会很大。有时安装一些不常见的软件，甚至需要自己从源码编译。而且，它没有集成wine，这意味着你得很痛苦地下载安装后才能运行exe。

## Ubuntu Kylin

## MX Linux

## 参考文献

https://www.zhihu.com/question/19676224/answers/updated

[Debian与Ubuntu到底有什么不同，应该如何选择？ - 知乎 (zhihu.com)](https://zhuanlan.zhihu.com/p/472294142)

[(8 封私信 / 83 条消息) Debian 和 Ubuntu 作为程序员桌面系统，推荐哪个呢？ - 知乎 (zhihu.com)](https://www.zhihu.com/question/341249505)

[Debian 和 Ubuntu：有什么不同？应该选择哪一个？ | Linux 中国 - 知乎 (zhihu.com)](https://zhuanlan.zhihu.com/p/406291335)
