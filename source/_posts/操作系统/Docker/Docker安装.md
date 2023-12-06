---
title: Docker安装说明
excerpt: Docker在不同操作系统下的安装流程
categories:
  - 操作系统
tags:
  - 操作系统
  - Docker
  - CUDA
date: 2023-12-06
updated: 2023-12-06
toc: true
typora-root-url: D:\Projects\Github\zhuyuanxiang\hexo_pages\hexo-starter\source\_posts\
---

# 安装

## Ubuntu

1. 卸载旧的安装包：`apt-get remove docker docker-engine docker.io containerd runc`
2. 更新软件包：`sudo apt update && sudo apt upgrade`
3. 安装Docker依赖包：`apt-get install apt-transport-https ca-certificates curl gnupg lsb-release software-properties-common`
4. 添加Aliyun的GPG密钥：`curl -fsSL http://mirrors.aliyun.com/docker-ce/linux/ubuntu/gpg | sudo apt-key add -`
5. 添加Aliyun的软件源：`sudo add-apt-repository "deb [arch=amd64] http://mirrors.aliyun.com/docker-ce/linux/ubuntu $(lsb_release -cs) stable"`
6. 安装Docker：`apt-get install docker-ce docker-ce-cli containerd.io`
7. 配置用户组：`sudo usermod -aG docker $USER`
8. 启动Docker服务：`systemctl start docker`
9. 测试Docker服务：`sudo docker run hello-world`
10. 查看Docker版本：`sudo docker version`
11. 重启Docker服务：`service docker restart`
12. 查看Docker镜像：`sudo docker images`
13. 查看Docker容器：`sudo docker ps`

### 安装GPU

1. 配置安装源：

```shell
curl -fsSL https://nvidia.github.io/libnvidia-container/gpgkey | sudo gpg --dearmor -o /usr/share/keyrings/nvidia-container-toolkit-keyring.gpg \
  && curl -s -L https://nvidia.github.io/libnvidia-container/stable/deb/nvidia-container-toolkit.list | \
    sed 's#deb https://#deb [signed-by=/usr/share/keyrings/nvidia-container-toolkit-keyring.gpg] https://#g' | \
    sudo tee /etc/apt/sources.list.d/nvidia-container-toolkit.list \
  && \
    sudo apt-get update
```

2. 安装NVIDIA Container Toolkit：`sudo apt-get install -y nvidia-container-toolkit`
3. 配置Docker：`sudo nvidia-ctk runtime configure --runtime=docker && sudo systemctl restart docker`
4. 配置containerd：`sudo nvidia-ctk runtime configure --runtime=containerd && sudo systemctl restart containerd`
5. 配置CRI-O：`sudo nvidia-ctk runtime configure --runtime=crio && sudo systemctl restart crio`
6. 测试安装结果：`sudo docker run --rm --runtime=nvidia --gpus all ubuntu nvidia-smi`
7. 运行CUDA样例：`podman run --rm --security-opt=label=disable --device=nvidia.com/gpu=all ubuntu nvidia-smi`
# 参考文献

1. [Installing the NVIDIA Container Toolkit](https://docs.nvidia.com/datacenter/cloud-native/container-toolkit/latest/install-guide.html)
2. [docker使用GPU总结](https://www.cnblogs.com/linhaifeng/p/16108285.html)
3. [Docker中使用GPU](https://zhuanlan.zhihu.com/p/565339942)