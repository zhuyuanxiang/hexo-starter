---
title: Ubuntu 16使用指南
excerpt: Ubuntu 16使用指南
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

# 总览

## 安装

## 升级

从 16.04 升级到 18.04

```shell
sudo apt-get update
sudo apt-get upgrade
sudo apt dist-upgrade
snap install hello-world
sudo apt-get install update-manager-core
sudo do-release-upgrade
sudo apt-get autoclean
sudo apt-get autoremove
```

## 安装软件包

```shell
apt-get update
apt-get install iproute2  # ip
apt-get install net-tools #ifconfig, netstat
apt-get install inetutils-ing  # ping
apt-get install sudo
procps  # ps
apt-get install vim
```

## 更换国内源

`sudo vim /etc/apt/sources.list`

```shell
#中科大源
deb https://mirrors.ustc.edu.cn/ubuntu/ bionic main restricted universe multiverse
deb https://mirrors.ustc.edu.cn/ubuntu/ bionic-updates main restricted universe multiverse
deb https://mirrors.ustc.edu.cn/ubuntu/ bionic-backports main restricted universe multiverse
deb https://mirrors.ustc.edu.cn/ubuntu/ bionic-security main restricted universe multiverse
deb https://mirrors.ustc.edu.cn/ubuntu/ bionic-proposed main restricted universe multiverse
deb-src https://mirrors.ustc.edu.cn/ubuntu/ bionic main restricted universe multiverse
deb-src https://mirrors.ustc.edu.cn/ubuntu/ bionic-updates main restricted universe multiverse
deb-src https://mirrors.ustc.edu.cn/ubuntu/ bionic-backports main restricted universe multiverse
deb-src https://mirrors.ustc.edu.cn/ubuntu/ bionic-security main restricted universe multiverse
deb-src https://mirrors.ustc.edu.cn/ubuntu/ bionic-proposed main restricted universe multiverse

#阿里云源
deb http://mirrors.aliyun.com/ubuntu/ jammy main restricted universe multiverse
deb http://mirrors.aliyun.com/ubuntu/ jammy-security main restricted universe multiverse
deb http://mirrors.aliyun.com/ubuntu/ jammy-updates main restricted universe multiverse
deb http://mirrors.aliyun.com/ubuntu/ jammy-proposed main restricted universe multiverse
deb http://mirrors.aliyun.com/ubuntu/ jammy-backports main restricted universe multiverse
deb-src http://mirrors.aliyun.com/ubuntu/ jammy main restricted universe multiverse
deb-src http://mirrors.aliyun.com/ubuntu/ jammy-security main restricted universe multiverse
deb-src http://mirrors.aliyun.com/ubuntu/ jammy-updates main restricted universe multiverse
deb-src http://mirrors.aliyun.com/ubuntu/ jammy-proposed main restricted universe multiverse
deb-src http://mirrors.aliyun.com/ubuntu/ jammy-backports main restricted universe multiverse

deb http://mirrors.aliyun.com/ubuntu/ jammy main restricted
deb http://mirrors.aliyun.com/ubuntu/ jammy universe
deb http://mirrors.aliyun.com/ubuntu/ jammy multiverse
deb http://mirrors.aliyun.com/ubuntu/ jammy-updates main restricted
deb http://mirrors.aliyun.com/ubuntu/ jammy-updates universe
deb http://mirrors.aliyun.com/ubuntu/ jammy-updates multiverse
deb http://mirrors.aliyun.com/ubuntu/ jammy-backports main restricted universe multiverse
deb http://mirrors.aliyun.com/ubuntu/ jammy-security main restricted
deb http://mirrors.aliyun.com/ubuntu/ jammy-security universe
deb http://mirrors.aliyun.com/ubuntu/ jammy-security multiverse

deb http://mirrors.aliyun.com/ubuntu/ bionic main restricted universe multiverse
deb http://mirrors.aliyun.com/ubuntu/ bionic-security main restricted universe multiverse
deb http://mirrors.aliyun.com/ubuntu/ bionic-updates main restricted universe multiverse
deb http://mirrors.aliyun.com/ubuntu/ bionic-proposed main restricted universe multiverse
deb http://mirrors.aliyun.com/ubuntu/ bionic-backports main restricted universe multiverse
deb-src http://mirrors.aliyun.com/ubuntu/ bionic main restricted universe multiverse
deb-src http://mirrors.aliyun.com/ubuntu/ bionic-security main restricted universe multiverse
deb-src http://mirrors.aliyun.com/ubuntu/ bionic-updates main restricted universe multiverse
deb-src http://mirrors.aliyun.com/ubuntu/ bionic-proposed main restricted universe multiverse
deb-src http://mirrors.aliyun.com/ubuntu/ bionic-backports main restricted universe multiverse

#清华源
deb https://mirrors.tuna.tsinghua.edu.cn/ubuntu/ bionic main restricted universe multiverse
deb https://mirrors.tuna.tsinghua.edu.cn/ubuntu/ bionic-updates main restricted universe multiverse
deb https://mirrors.tuna.tsinghua.edu.cn/ubuntu/ bionic-backports main restricted universe multiverse
deb https://mirrors.tuna.tsinghua.edu.cn/ubuntu/ bionic-security main restricted universe multiverse
deb https://mirrors.tuna.tsinghua.edu.cn/ubuntu/ bionic-proposed main restricted universe multiverse
deb-src https://mirrors.tuna.tsinghua.edu.cn/ubuntu/ bionic main restricted universe multiverse
deb-src https://mirrors.tuna.tsinghua.edu.cn/ubuntu/ bionic-updates main restricted universe multiverse
deb-src https://mirrors.tuna.tsinghua.edu.cn/ubuntu/ bionic-backports main restricted universe multiverse
deb-src https://mirrors.tuna.tsinghua.edu.cn/ubuntu/ bionic-security main restricted universe multiverse
deb-src https://mirrors.tuna.tsinghua.edu.cn/ubuntu/ bionic-proposed main restricted universe multiverse
```

更换完成后：

```shell
sudo apt-get update
sudo apt upgrade
```

## 问题

1. # [ESM 401 Unauthorized error on Ubuntu 16.04](https://askubuntu.com/questions/1371293/esm-401-unauthorized-error-on-ubuntu-16-04)

```shell
sudo apt-get purge ubuntu-advantage-tools --autoremove
```

## 参考资料

1. [upgrade - ESM 401 Unauthorized error on Ubuntu 16.04 - Ask Ubuntu](https://askubuntu.com/questions/1371293/esm-401-unauthorized-error-on-ubuntu-16-04)
1. [我的ubuntu连vi都没有？？那在命令行怎么编辑文件？？ - 知乎 (zhihu.com)](https://zhuanlan.zhihu.com/p/67066631)[我的ubuntu连vi都没有？？那在命令行怎么编辑文件？？ - 知乎 (zhihu.com)](https://zhuanlan.zhihu.com/p/67066631)
1. [(61条消息) 简单解决Docker中的镜像没有vim的问题（同时解决apt-get下载很慢的问题，原始的echo写入国内的源）_docker nginx 缺少vim_一只有点酸的程序员的博客-CSDN博客](https://blog.csdn.net/weixin_44598727/article/details/108300731)
1. [(61条消息) Ubuntu apt-get彻底卸载软件包_apt彻底卸载_享学IT的博客-CSDN博客](https://blog.csdn.net/get_set/article/details/51276609)[(61条消息) Ubuntu apt-get彻底卸载软件包_apt彻底卸载_享学IT的博客-CSDN博客](https://blog.csdn.net/get_set/article/details/51276609)
