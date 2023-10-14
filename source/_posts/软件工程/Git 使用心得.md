---
title: Git 使用心得
categories:
  - 版本管理
tags:
  - Git
date: 2019-01-08
updated: 2019-01-08
toc: true
---

## Git terms

- ACK: acknowledgement. // 我确认了或者我接受了, 我承认了
- AFAIK: As Far As I know. // 据我所知
- CC: Carbon copy // 一般代表抄送别人的意思
- IIRC: if I recall correctly. // 如果我没记错
- LGTM: Looks good to me. // Riview 完别人的 PR ，没有问题
- NACK/NAK: negative acknowledgement. // 我不同意
- PR: Pull Request. // 如果给其它项目提交合并代码的请求时，就说会提交一个 PR
- PTAL: Please take a look. // 帮我看下，一般都是请别人 review 自己的 PR
- RFC: request for comments. // 我觉得这个想法很好, 我们来一起讨论下
- SGTM: Sounds Good To Me. // 同 LGTM
- TBR: To Be Reviewed. // 这些代码需要审核
- TL; DR: Too Long; Didn't Read. // 太长了，懒得看
- WIP:  Work in progress, do not merge yet. // 开发中

## Q&A

使用git出现的 `"receive.denyCurrentBranch" Configuration variable to "refuse"` 问题及解决

1. 使用 Git Bash，进入项目的目录，然后输入 `git config receive.denyCurrentBranch ignore`，即可
1. 在仓库建立之前，使用 `git --bare init --shared`，然后建立仓库即可

## GitHub Q&A

使用 ipaddress.com 查找下面 3 个网址对应的 IP 地址：

```txt
github.com

assets-cdn.github.com

github.global.ssl.fastly.net
```

修改 hosts 文件，刷新 dns 缓存。

使用 ping.chinaz.com，查找最快的节点 IP 进行访问。

## Git commands

- Undo a commit & redo

```bash
git add .
git commit -m "有问题的提交"
git log
git reset HEAD~
git log
git add .
git commit -c ORIG_HEAD
```

git reset q&a: https://stackoverflow.com/questions/927358/how-do-i-undo-the-most-recent-local-commits-in-git/927386#927386
