---
title: "Thunderbot 911 黑武士 II"
excerpt: "家庭电脑使用问题记录"
categories:
- 硬件
tags:
- 家庭电脑
- 硬件
- BIOS
updated: 2020-07-11
---
# 家庭电脑

## BIOS

### UEFI

1. 总内存
   * 如果使用「板载显卡」作为主图形适配器，则共享内存占用为 256M
   * 如果使用「PCI Express」作为主图形适配器，则共享内存占用为 64M
2. 显卡配置
   * 「高级→芯片组配置→IGPU 多监视器」，可以打开电脑自带的板载显卡
   * 「高级→芯片组配置→主图形适配器」，可以将板载显卡设置为主图形适配器，从而将更多的游戏显卡的内存留给游戏或者机器学习的计算使用
   * 配置为板载显卡时，必须使用 DVI 进行连接，才会有 BIOS 输出的信号，如果是 HDMI 连接，则可能没有 BIOS 输出的信号，但是配置好后，重启又可以使用 HDMI 输出信号了
   * 配置为 PCI Express ( 2060 ) 显卡时，使用 HDMI 连接，也会有 BIOS 输出的信号

### 机器学习

1. 将「板载显卡」设置为主图形适配器，Windows 10 启动时就会将大部分程序运行在「板载显卡」上
2. 将「RTX 2060」显卡硬件屏蔽后，使用板载显卡运行所有系统程序，在进行「机器学习」的计算时，再将显卡打开，就可以充分使用显卡的资源了
   - 但是计算的时候，好像显存依然无法全部使用
3. BIOS 中设置「快速启动」后，没有进入 BIOS 设置模式，也没法启动 U 盘怎么办？
   - 基于 Windows 10，按住 Shift + 点「重启」，选择「疑难解答」，选择「高级选项」，选择「UEFI 固件设置」，选择「重启」就可以了。
4. Win10 怎样解除 260 个字符路径长度限制？
   - Windows 文件系统 NTFS 的文件名和目录路径有最大长度 即 MAX_PATH 限制最长 260 个字符。
   - Win+R，输入 Gpedit.msc 然后回车
   - 沿着「本地计算机策略>计算机配置>管理模板>系统>文件系统>启用 Win32 长路径
   - 启用 Win32 长路径后，列入清单的 win32 应用程序和 Windows 应用商店应用程序即可在支持每节点 260 个字符标准限制的文件系统上访问超出该限制的路径。启用此设置将导致在进程中可以访问长路径。
5. 如何控制应用程序使用哪个 GPU？
   - Graphics settings page ( Settings > System > Display > Graphics settings )
   - 「应用程序」就点击「浏览」，添加相应的程序，设置「选项」为指定的 GPU
   - 「APP」就直接在下拉菜单中选择
   - 主要目的就是屏蔽其他程序使用 GPU，都留给 深度学习 使用

## SSH Server

1. 在系统设置→可选功能→安装 ( OpenSSH 服务器、OpenSSH 客户端 )
2. 使用管理员权限打开 PowerShell，开启 SSH 服务

   ```powershell
   net start sshd
   ```
3. 使用客户端软件登录

- username: 如果本身使用的是 Email 地址作为用户名称，Windows 10 会自动去掉域名地址和数字作为用户名，准确的名称需要参考「`C:\Users`」目录中的用户名称

  ```powershell
  ssh username@servername
  ssh username@192.168.1.1
  ```

4. 使用管理员权限打开 PowerShell，关闭 SSH 服务

   ```powershell
   net stop sshd
   ```

## Find HotKey

安装 Visual Studio 2019 Community 版，运行 Spy++ 工具软件。

- 点击「监视→日志消息」
- 在「消息选项」窗口中「窗口」标签下，选中「系统中的所有窗口」选项
- 在「消息选项」窗口中「消息」标签下，点击「全部清除」，再在「消息」中选择「WM_HOTKEY」，点击「确定」
- 按发生冲突的快捷键，例如：「Ctrl+Alt+A」，点击出现的新消息，点击鼠标右键，选择「属性」
- 在「消息属性」窗口中，点击「窗口句柄」后的 句柄号，就可以找出发生冲突的线程情况。

参考： 
- [win10 快捷键冲突检测工具与方法](https://blog.csdn.net/zw521cx/article/details/102665663?utm_medium=distribute.pc_relevant_t0.none-task-blog-BlogCommendFromMachineLearnPai2-1.nonecase&depth_1-utm_source=distribute.pc_relevant_t0.none-task-blog-BlogCommendFromMachineLearnPai2-1.nonecase)
- [[Windows/电脑热键冲突检测]]

## PowerShell

### PSSecurityException

在管理员模式的 PowerShell 下执行 `Set-ExecutionPolicy -ExecutionPolicy RemoteSigned` 就可以实现授权了。
