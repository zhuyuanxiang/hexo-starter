---
title: "小企业的NAS932+的安装过程"
categories:
- NAS
tags:
- NAS
date: 2023-04-13
updated: 2023-04-13
toc: true
toc_sticky: true
---

## 套件中心

- Active Insight：加速技术支持
- Advanced Media Extensions：支持 HEVC 文件
- Audio Station：音频客户端，移动应用程序DS Audio
- Cloud Sync
- DNS Server：域名服务器
- Docker
- Download Station：下载客户端
- File Station：文件夹客户端
- Hybrid Share
- LDAP Server：轻量组路径服务器
- Mail Station：EMail客户端
- Note Station：笔记本客户端
- OAuth Service
- SAN Manager
- SMB Service：SMB服务
- SSO 服务器
- Secure SignIn 服务
- Synology Calendar：日历组件
- Synology Drive Server：云盘服务器
- Synology Mail Server：EMail服务器
- Synology Office：文件夹中 Office 办公套件
- Synology Photos：图片客户端
- Synology 应用程序服务
- Universal Search：搜索索引
- Universal Viewer：通过索引搜索结果
- Video Station：视频客户端
- Web Station：Web客户端
- WebDAV Server
- 媒体服务器：用于支持流媒体
- 存储空间分析：分析存储的使用情况
- 文本编辑器：文件夹中编辑文本
- 文档查看器：可以查看Office和PDF类的文件
- 日志中心：日志分析工具

## DNS Server

区域→新增→主要区域→正向区域

- 域名：company_domain.org
- 主 DNS 服务器：192.168.1.2
- 限制区域传输：false

双击新记录，原始的两条记录不动，输入

- 新增A类型
  - 名称：
  - IP：192.168.1.2
- 新增A类型
  - 名称：mail
  - IP：192.168.1.2
  - 注：其他类似（ldap, pop3, smtp, www）

解析

- 启用解析服务：true
  - 启用转发器
  - 转发器1：192.168.1.1
  - 转发器2：114.114.114.114

## LDAP Server

设置→服务器→启动LDAP服务器：true

- 作为Provider服务器
  - FQDN：company_domain.org
  - root的密码
- 备份和还原
  - 备份→设置→启动计划管理备份→每天00:00
  - 目的地：backup
  - 备份版本数量上限：10
- 管理用户
  - 新增：先输入每个用户的名称、描述、电子邮件
- 管理群组
  - 新增：从高层开始建立群组
  - 编辑群组成员：低群组直接包括高群组成员

注：默认密码（`12345678`）

## Mail

### Synology Mail Server

- SMTP
  - 账户类型：LDAP用户
  - 启用SMTP：true
  - 启用SMTP认证：true
  - 主机名（FQDN）：company_domain.org
  - 端口：25
  - 邮件大小上限：100
  - 启用SMTP-SSL端口：465
  - 启用SMTP-TLS端口：587
- IMAP/POP3
  - 启用POP3：true
  - 启用POP3 SSL/TSL：true
  - 启用IMAP：true
  - 启用IMAP SSL/TSL：true
- 垃圾邮件：nil
- 别名
  - 创建
  - 别名：root
  - 用户账号：zhuyuanxiang@company_domain.org

## 控制面板

### 共享文件夹

- 数据备份
- 图书馆
- 论文集
- 音乐库
- 视频库
- 照片库
- 软件库
- 公司产品发布包
- 业务一组
  - 常规
  - 对“网上邻居”隐藏共享文件夹：true
  - 对没有权限的用户隐藏子文件夹和文件：true
  - 启用回收站：true
  - 高级
  - 启用数据总和检查码以实现高级数据完整性：true
- 业务二组
- 市场部
- 行政部
- 财务部
- 运营部
- 销售部
- 销售部经理

### 文件服务

SMB：

- 启用SMB服务：true
  - 工作群组：WORKGROUP
  - 对没有权限的用户隐藏共享文件夹
  - 启动传输日志

AFP：

- 启用AFP服务：true

NFS：

- 启用NFS服务：true

FTP：

- 启用FTP服务（无加密）：true
- 启用FTP SSL/TSL 加密服务（FTPS）：true
- 启用SFTP服务：true

rsync：

高级设置：

- 启用文件快速克隆：true
- 启用 Bonjour 服务：true
- 启用 Windows 网络发现：true

### 网络

常规：

- 服务器名称：DSM-COMPANY-GIT
- 默认网关：192.168.1.1
- 连接性：在HTTP响应中启用“Server”标题
  - 自定义“Server”标题：company_domain

### 终端机和SNMP

- 启用Telnet功能：true
- 启用SSH功能：true
  - 端口：22

### 登录门户

- DSM
  - 自动将DSM桌面的HTTP连接重定向到HTTPS：true
- 应用程序
  - audio
  - calendar
  - download
  - file
  - note
  - sso
  - drive
  - photo
  - video

### 区域设置

- 日期格式：YYYY-MM-dd

### 通知设置

- 启用电子邮件通知
  - SMTP服务器：192.168.1.2
  - SMTP端口：25

### 硬件和电源

- 常规
  - 修复电源问题后自动重新启动：true
  - 静音模式：true
- 硬盘休眠
  - 休眠：1小时
  - 休眠后耗电量降到最低：true
- UPS
  - 启用UPS支持：true
  - 直到电量不足

### Synology账户

zhuyuanxiang@gmail.com

### 应用程序权限

- Download Station: 只能赋予本地用户和本地组

注：其他应用“默认权限→默认授予所有用户此权限”

### 索引服务

加入新的索引文件夹

## Docker

- 映像：
  - gitlab/gitlab-ce:latest
  - centos:latest
- 容器：
  - gitlab-ce-bridge

### gitlab-ce-bridge

网络→使用已经选择的网络（bridge）→通过Web Station 启用网页门户→端口设置（8022:22，8443:443）→存储空间设置：

```txt
/docker/gitlab/etc /etc/gitlab
/docker/gitlab/log /var/log/gitlab
/docker/gitlab/opt /var/opt/gitlab
```

```shell
> ssh admin@192.168.1.2

$ cd /volume1/docker/gitlab/etc
$ sudo vim gitlab.rb

external_url 'http://192.168.1.2/gitlab'

gitlab_rails['smtp_enable'] = true
gitlab_rails['smtp_address'] = "192.168.1.2"
gitlab_rails['smtp_port'] = 25

gitlab_rails['ldap_enabled'] = true

gitlab_rails['ldap_servers'] = YAML.load <<-'EOS'
  main: # 'main' is the GitLab 'provider ID' of this LDAP server
    label: 'LDAP'
    host: '192.168.1.2'
    port: 389
    base: 'dc=company_domain,dc=org'
    uid: 'uid'
    bind_dn: 'uid=admin,cn=users,dc=company_domain,dc=org'
    password: '12345678'
EOS

$ gitlab-ctl reconfigure

$ gitlab-ctl restart

$ gitlab-rake gitlab:ldap:check
```

## Web Station

### 网页服务门户

用户定义的门户

- 服务：Docker gitlab-ce-bridge 80
- 别名：gitlab

## Cloud Sync

云供应商→百度云（登录账号）

- 本地文件夹：`software/download/baiduyun'
- 远程文件夹：根目录（百度云中的“我的应用数据→Cloud Sync”）→计划设置（周一到周六，08:00~20:00）
- 同步方向：双向
- 启用高级一致性检测：true

## Synology Drive Server

团队文件夹：打开才可以在云盘中团队共享中看到

## Synology Chat Server

需要从Synology网站去下载，然后安装，仓库里面找不到。

## Hosts 修改

登录SSH

```powershell
$ sudo -i
vi /etc/hosts


```
