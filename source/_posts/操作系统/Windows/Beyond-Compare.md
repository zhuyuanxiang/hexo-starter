---
title: Beyond Compare Evaluate 方法
excerpt: Beyond Compare 4 30天试用到期的解决办法
categories:
  - 工具
tags:
  - Beyond Compare
  - 文件比较
  - 目录比较
date: 2020-09-13
updated: 2020-09-13
toc: true
toc_sticky: true
---

# Evaluate 版本

## 删除 DLL 文件

删除安装目录下的BCUnrar.dll文件

## 修改注册表

1. regedit
1. HKEY_CURRENT_USER\Software\Scooter Software\Beyond Compare 4
1. Delete CacheID

## Schedule Delete

1. 定义 BAT 文件，内容：`reg delete "HKEY_CURRENT_USER\Software\Scooter Software\Beyond Compare 4" /v CacheID /f`
1. 我的电脑-->管理-->任务计划程序-->创建任务-->配置定时任务基本信息
1. 将这个BAT文件定义为每4周执行一次
