---
title: "Ubuntu Remote Services"
excerpt: "Ubuntu"
categories:
- OS
tags:
- Ubuntu
- SSHD
- xRDP
- Remote Services
- Remote Connect
date: 2022-04-27
updated: 2022-04-27
toc: true
toc_sticky: true
---

# Remote Connect

## SSHD

### Check Service

Ubuntu 默认不安装 SSH 服务，可以通过命令检查

```bash
ps -e | grep ssh
```

### Install Service

```bash
sudo apt-get install ssh
sudo apt-get install openssh-server
```

### Start Service

```bash
sudo /etc/init.d/ssh start
```

### Config File

```bash
sudo gedit /etc/ssh/sshd_config
```

修改参数 `PermitRootLogin yes`

### Restart Service

```bash
service ssh restart
```

### Add ssh Key

1. 制作密钥对：`ssh-keygen`
2. 在服务器上安装公钥：`cat id_rsa.pub >> ~/.ssh/authorized_keys`
    1. 文件 `id_rsa.pub` 所在目录 `~/.ssh/`
    2. 目录权限：`chmod 700 ~/.ssh`
    3. 文件权限：`chmod 600 authorized_keys`
3. 编辑配置文件：`/etc/ssh/sshd_config`

```text
RSAAuthentication yes
PubkeyAuthentication yes
# 允许 root 用户登录
PermitRootLogin yes
# 禁止密码登录
PasswordAuthentication no
```

4. 重启SSH服务：`service sshd restart`
5. 将私钥文件下载到客户端
  1. VSCode 直接访问 `~/.ssh/`目录

## Vino-Serv

### Install Vino-Serv

在ubuntu 20.04 桌面发行版打开`屏幕共享`功能，步骤如下：

```txt
设置-->共享-->屏幕共享
    打开总开关
    允许连接控制屏幕
    需要密码（自己设置密码，最多8位）
    网络打开
```

### Manage

查看桌面应用服务的状况

查看当前用户占用的共享桌面端口`lsof -i:5900`

```bash
zfb@my-Server:~/build$ lsof -i:5900
COMMAND    PID USER  FD   TYPE   DEVICE SIZE/OFF NODE NAME
vino-serv 4308 zfb   11u  IPv6 12350285      0t0  TCP *:5900 (LISTEN)
vino-serv 4308 zfb   12u  IPv4 12350286      0t0  TCP *:5900 (LISTEN)
```

本用户只能看到自己占用的端口，一般从`5900`开始，一个用户占用一个，只有开启共享的用户才占用。例如，用户`root1`和用户`zfb`都开启了共享，则用户`zfb`执行命令`lsof -i:5900`、`lsof -i:5901`和`lsof -i:5902`，只有一个命令有返回结果，即自己占用的那个端口。但是，使用命令`ss -lnt`可以看到`5900-5902`端口都在被占用，只是无法看到具体进程和用户

## xRDP

支持 Windows 的 远程桌面协议(RDP)

### Install Ubuntu Desktop

如果使用的就是UBUNTU桌面版，则此步忽略。

1. 安装Gnome，它是UBUNTU 20.04 的默认桌面环境
2. 安装Xfce，它是快速、稳定、轻量级桌面环境

```bash
sudo apt update
sudo apt install ubuntu-desktop
```

### Install xRDP

[Xrdp 包](https://wiki.archlinux.org/index.php/Xrdp)：支持微软 Remote Desktop Protocol(RDP)。

```bash
# 安装脚本
sudo apt-get install xrdp
```

### Start xRDP

```bash
sudo systemctl status xrdp
```

### add xRDP user

默认情况下，xRDP 使用 `/etc/ssl/private/ssl-cert-snakeoil.key`，它仅仅对`ssl-cert`用户组成员可读，为了能够登录远程桌面，运行下面的命令，将用户`xrdp`添加到用户组中

```bash
sudo useradd xrdp ssl-cert	# 系统报错不用管
sudo systemctl restart xrdp
```

### Config xRDP

xRDP 的配置文件在`/etc/xrdp`。使用默认的X Window 桌面环境（Gnome或者XFCE）。主要的配置文件`xrdp.ini`被分成不同的段，允许设置全局配置（如：安全、监听地址、创建不同的 xRDP 会话等）。

#### config xRDP Desktop

编辑`startwm.sh`中可修改启动桌面。

#### config xRDP Firewall

xRDP 守护程序在所有的网络接口上监听端口`3389`，如果在服务器上有防火墙需要打开这个端口。例如：允许从某类IP地址访问服务器，执行命令

```bash
sudo ufw allow from 192.168.33.0/24 to any prot 3389
```

如果允许从任何地方访问

```bash
sudo ufw allow 3389
```

增加安全，考虑 xRDP 仅仅监听 `localhost`，并且创建一个 SSH 隧道，将本地机器的 `3389` 端口到远程服务器的同样端口之间进行流量加密。

#### config multi xRDP Desktop

如果想开启多个桌面，可以使用[安装 xrdp 软件包](https://c-nergy.be/blog/?p=14888)，此时可以通过 3389 商品利用 rdp 协议登录 XOrg 桌面，多个用户都可以登录，并且可以同时登录，但是物理机器需要保留在登录界面。

注：同一个用户，本地和远程同时只能登录一个。

xrdp 的配置文件：/etc/xrdp/xrdp.ini

```config
[xrdp8]
name=Vino-Session
lib=libvnc.so
username=ask
password=ask
ip=127.0.0.1
port=5900
```

## VNCServer

### Install VNCServer

```bash
#更新
apt update
#安装桌面环境
sudo apt install xfce4 xfce4-goodies
#安装vncserver
apt install tightvncserver
#启动vncserver
vncserver
```

### Config VNCServer

```bash
#关闭vncserver ：1窗口
vncserver -kill :1
#备份原来的配置文件
mv ~/.vnc/xstartup ~/.vnc/xstartup.bak
#编辑新的配置文件
vim ~/.vnc/xstartup
```

在xstartup文件中更新为如下内容：

```bash
#!/bin/bash
xrdb $HOME/.Xresources
gnome-sesseion &
```

然后设置该文档的权限

```bash
chmod u+x ~/.vnc/xstartup
```

## vino-server vs. xRDP vs. vnc-server

服务器比较

|      | vino-server                      | xRDP                                     | vnc-server                          |
| ---- | -------------------------------- | ---------------------------------------- | ----------------------------------- |
| 优点 | 系统自带，配置简单               | 配置简单，可用windows自带的远程桌面登录  | 配置简单,但不一定能成功连接远程桌面 |
| 缺点 | 重启后必须登录个人桌面后才可启动 | root用户登陆不容易出错，普通用户容易出错 | 出错不好解决                        |

注：建议`vino-server`+`Xrdp`。因为`Xrdp`容易出错，出错后使用`vino-server`登录上去修补一下。`vino-server`使用过程中比较消耗资源。

## Client

### remmina

打开软件`remmina`，选择`VNC`协议，输入要连接的机器的地址`192.168.10.11:5900`（附带端口），然后会提示输入密码，这里要输入`5900`端口对应的那个用户的密码（共享桌面密码，不是用户密码），即可成功看到桌面。

### vnc viewer

下载[vnc viewer](https://www.realvnc.com/en/connect/download/viewer/windows/)并安装，打开软件，根据提示输入ip地址`192.168.10.11:5900`，如果提示加密策略不一致。那么需要回到开启桌面共享的ubuntu机器上，输入以下命令关闭加密即可
`gsettings set org.gnome.Vino require-encryption false`
或者安装`dconf-editor`工具进行配置，输入以下命令即可安装：
`sudo apt-get install dconf-editor`
然后桌面搜索`dconf-editor`打开，依次展开`org->gnome->desktop->remote-access`，然后取消`require-encryption`的勾选即可

### mstsc

先安装 xrdp 协议，登录时使用 mstsc 即可，但是登录后会出现黑屏现象，而且会跟 vnc viewr 登录冲突。

注1：黑屏问题的解决方案，编辑 `/etc/xrdp/startwm.sh`，添加：

```shell
unset DBUS_SESSION_BUS_ADDRESS
unset XDG_RUNTIME_DIR
```

重启服务 `sudo systemctl restart xrdp.service`

注2：Gnome 的原始桌面没有左侧的任务栏，也没有最小化按钮。安装 `sudo apt install gnome-shell-extension-prefs`，运行 `Extensions` ，打开设置按钮；再依据下列顺序执行操作即可得到原生桌面的效果：

```
vim ~/.xsessionrc	# 添加配置文件

export GNOME_SHELL_SESSION_MODE=ubuntu
export XDG_CURRENT_DESKTOP=ubuntu:GNOME
export XDG_CONFIG_DIRS=/etc/xdg/xdg-ubuntu:/etc/xdg

sudo systemctl restart xrdp.service	# 重启服务
```



## TigerVNC

Ubuntu 20.0 以后，使用 tigervnc 代替 vnc4server 包。

注：我没有使用这个服务，有需要时可以查询其他人的说明。

### Config

```bash1
# 查看屏幕支持的分辨率
xrandr
# 修改为自己需要的分辨率
xrandr --output HDMI-1-2 --mode 1280x720
```

注：

1.可以使用 vnc viewer 自带的适应方式去适应不同的分辨率
2.可以使用 Ubuntu 的桌面右键菜单中的「显示设置」，直接调整分辨率
3.可以对 终端 窗口进行显示比例修改
4.配置 GPU ： `sudo nvidia-settings` 选择 Ubuntu 使用哪个显卡

## Errors

### Authentication Required

- 需要授权来创建色彩管理设备 / Authentication is required to create a color managed device
- 需要授权来移除色彩管理设备 / Authentication is required to remove a color managed device
- 刷新系统软件源需要认证 / Authentication is required to refresh the system repositories

#### Authentication is required to create a color managed device

创建文件 `/etc/polkit-1/localauthority/50-local.d/45-allow-colord.pkla` 并写入内容：

```ini
[Allow Colord all Users]
Identity=unix-user:*
Action=org.freedesktop.color-manager.create-device;org.freedesktop.color-manager.create-profile;org.freedesktop.color-manager.delete-device;org.freedesktop.color-manager.delete-profile;org.freedesktop.color-manager.modify-device;org.freedesktop.color-manager.modify-profile
ResultAny=no
ResultInactive=no
ResultActive=yes
```

#### Authentication is required to refresh the system repositories

创建文件 `/etc/polkit-1/localauthority/50-local.d/46-allow-packagekit.pkla` 并写入内容：

```ini
[Allow Refresh Repository all Users]
Identity=unix-user:*
Action=org.freedesktop.packagekit.system-sources-refresh
ResultAny=no
ResultInactive=no
ResultActive=yes
```

### Others

1. VNC连接提示不支持安全类型

    关闭远程连接加密请求

    ```bash
    sudo apt install dconf-editor
    ```

    进入并且关闭“org->gnome->desktop->remote access->require encryption”

2. SSH登录的系统无法使用vnc viewer登录

    必须在服务器跟前登录桌面后才可以使用。

    解决方法：

    1. 查看当前的桌面管理器

        ```bash
        cat /etc/X11/default-display-manager
        ```

    2. 编辑文件中指定的桌面管理器，例如：/usr/sbin/gdm3，就使用下列指令更改

        ```bash
        sudo vim /etc/gdm3/custom.conf
        ```

        修改下面三行内容

        ```bash
        WaylandEnable=false
        AutomaticLoginEnable=true
        AutomaticLogin=user1  # 这个是自己的用户名
        ```

## References

[xrdp 远程登录需要输入很多次密码](https://blog.csdn.net/wu_weijie/article/details/108481456)

[xrdp完美实现Windows远程访问Ubuntu](https://www.cnblogs.com/xuliangxing/p/7560723.html)

[完美方案——解决XRDP连接黑屏，以及桌面优化！](https://zhuanlan.zhihu.com/p/519648451)

[gnome - (xRDP) Desktop looks different when connecting remotely - Ask Ubuntu](https://askubuntu.com/questions/1233088/xrdp-desktop-looks-different-when-connecting-remotely)

