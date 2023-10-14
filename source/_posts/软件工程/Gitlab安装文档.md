---
title: Gitlab安装文档
excerpt: 在 Ubuntu 上安装 Gitlab 的流程
categories:
  - 版本管理
tags:
  - Ubuntu
  - Gitlab
  - Shell
date: 2022-09-26
updated: 2022-04-18
toc: true
toc_sticky: true
---

# 总览

## 安装

## 重启

安装软件包

```shell
gitlab-ctl reconfigure
gitlab-ctl restart
```

## 更换国内源

`sudo vim /etc/apt/sources.list`

```shell
# ToDo
```

更换完成后：

```shell
# ToDo
```

## 问题

### Gitlab Bundle CPU 消耗过大问题

修改配置文件： `gitlab.rb`

```json
# /etc/gitlab/gitlab.rb
# 关闭电子邮件相关功能
gitlab_rails['smtp_enable'] = false
gitlab_rails['gitlab_email_enabled'] = false
gitlab_rails['incoming_email_enabled'] = false

# Terraform
gitlab_rails['terraform_state_enabled'] = false

# Usage Statistics
gitlab_rails['usage_ping_enabled'] = false
gitlab_rails['sentry_enabled'] = false
grafana['reporting_enabled'] = false

# 关闭容器仓库功能
gitlab_rails['gitlab_default_projects_features_container_registry'] = false
gitlab_rails['registry_enabled'] = false
registry['enable'] = false
registry_nginx['enable'] = false

# 包仓库
gitlab_rails['packages_enabled'] = false
gitlab_rails['dependency_proxy_enabled'] = false

# GitLab KAS
gitlab_kas['enable'] = false
gitlab_rails['gitlab_kas_enabled'] = false

# Mattermost
mattermost['enable'] = false
mattermost_nginx['enable'] = false

# Kerberos
gitlab_rails['kerberos_enabled'] = false
sentinel['enable'] = false

# GitLab Pages
gitlab_pages['enable'] = false
pages_nginx['enable'] = false

# 禁用 PUMA 集群模式
puma['worker_processes'] = 0
puma['min_threads'] = 1
puma['max_threads'] = 2

# 降低后台守护进程并发数
sidekiq['max_concurrency'] = 5

gitlab_ci['gitlab_ci_all_broken_builds'] = false
gitlab_ci['gitlab_ci_add_pusher'] = false

# 关闭监控
prometheus_monitoring['enable'] = false
alertmanager['enable'] = false
node_exporter['enable'] = false
redis_exporter['enable'] = false
postgres_exporter['enable'] = false
pgbouncer_exporter['enable'] = false
gitlab_exporter['enable'] = false
grafana['enable'] = false
sidekiq['metrics_enabled'] = false
```

## 参考资料

1. [Upgrading GitLab | GitLab](https://docs.gitlab.com/ee/update/)
1. [Gitlab备份、迁移、恢复和升级 - SSgeek - 博客园 (cnblogs.com)](https://www.cnblogs.com/ssgeek/p/9392104.html)
1. [记一次 GitLab 的迁移过程 - 程序员自由之路 - 博客园 (cnblogs.com)](https://www.cnblogs.com/54chensongxia/p/14964465.html)
1. [(61条消息) 关于Gitlab10迁移用户_gitlab迁移用户_moxiaomomo的博客-CSDN博客](https://blog.csdn.net/moxiaomomo/article/details/83412173)
1. [(61条消息) 项目源代码迁移到另一个gitlab的方法(保留原来的提交记录)_gitlab迁移项目_拒绝冗余的博客-CSDN博客](https://blog.csdn.net/qq_42887496/article/details/124304243)
1. [Download and install GitLab | GitLab](https://about.gitlab.com/install/#ubuntu)
1. [GitLab 14 轻量化运行方案](https://soulteary.com/2021/07/14/gitlab-14-lightweight-operation-solution.html)
