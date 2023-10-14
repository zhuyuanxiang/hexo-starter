---
title: Conda学习关键点总结
excerpt: conda 的国内源、常用命令、问题解决
categories:
  - 编程
tags:
  - conda
  - Python
  - 编程
  - 机器学习
  - 数据科学
date: 2019-01-01
updated: 2022-04-21
toc: true
toc_sticky: true
---

# conda

## Channels

### change pip channels

寻找`pip.conf`的位置：`pip -v config list`，然后添加下面的内容

```json
[global]
# 清华源
index-url = https://pypi.tuna.tsinghua.edu.cn/simple
# 阿里源
index-url = https://mirrors.aliyun.com/pypi/simple
```

1. 临时使用别的源：`pip install -i https://pypi.tuna.tsinghua.edu.cn/simple $PKG_NAME`
1. 设为默认安装源：`pip config set global.index-url https://pypi.tuna.tsinghua.edu.cn/simple`

### change conda channels

修改用户目录下的 `.condarc` 文件即可，详情参考[镜像使用帮助](https://mirror.tuna.tsinghua.edu.cn/help/anaconda/)

#### TsingHua

清华源

```json
channels:
  - defaults
show_channel_urls: true
channel_alias: https://mirrors.tuna.tsinghua.edu.cn/anaconda
default_channels:
  - https://mirrors.tuna.tsinghua.edu.cn/anaconda/pkgs/main
  - https://mirrors.tuna.tsinghua.edu.cn/anaconda/pkgs/free
  - https://mirrors.tuna.tsinghua.edu.cn/anaconda/pkgs/r
  - https://mirrors.tuna.tsinghua.edu.cn/anaconda/pkgs/pro
  - https://mirrors.tuna.tsinghua.edu.cn/anaconda/pkgs/msys2
custom_channels:
  conda-forge: https://mirrors.tuna.tsinghua.edu.cn/anaconda/cloud
  msys2: https://mirrors.tuna.tsinghua.edu.cn/anaconda/cloud
  bioconda: https://mirrors.tuna.tsinghua.edu.cn/anaconda/cloud
  menpo: https://mirrors.tuna.tsinghua.edu.cn/anaconda/cloud
  pytorch: https://mirrors.tuna.tsinghua.edu.cn/anaconda/cloud
  simpleitk: https://mirrors.tuna.tsinghua.edu.cn/anaconda/cloud
```

#### USTC

已经关闭，打开时自动转向清华源。

#### SJTUG

上海交通大学

```json
channels:
  - defaults
show_channel_urls: true
channel_alias: https://mirrors.sjtug.sjtu.edu.cn/anaconda
default_channels:
  - https://mirrors.sjtug.sjtu.edu.cn/anaconda/pkgs/main
  - https://mirrors.sjtug.sjtu.edu.cn/anaconda/pkgs/free
  - https://mirrors.sjtug.sjtu.edu.cn/anaconda/pkgs/r
  - https://mirrors.sjtug.sjtu.edu.cn/anaconda/pkgs/pro
  - https://mirrors.sjtug.sjtu.edu.cn/anaconda/pkgs/msys2
custom_channels:
  conda-forge: https://mirrors.sjtug.sjtu.edu.cn/anaconda/cloud
  msys2: https://mirrors.sjtug.sjtu.edu.cn/anaconda/cloud
  bioconda: https://mirrors.sjtug.sjtu.edu.cn/anaconda/cloud
  menpo: https://mirrors.sjtug.sjtu.edu.cn/anaconda/cloud
  pytorch: https://mirrors.sjtug.sjtu.edu.cn/anaconda/cloud
  simpleitk: https://mirrors.sjtug.sjtu.edu.cn/anaconda/cloud
```

## PIP

### Install PIP

Python 安装完成后，自带 `pip`。

### requirements.txt

#### pip freeze

列出需要的包：`pip freeze`

生成 `requirements.txt` 文件：`pip freeze > requirements.txt`

安装 `requirements.txt` 文件：`pip install -r requirements.txt`

在 env1 中生成，在 env2 中安装：

```shell
env1/bin/pip freeze > requirements.txt
env2/bin/pip install -r requirements.txt
```

#### pipreqs

只会导出当前项目运行所依赖的包，环境中多余的库就不会导出。

安装模块：`pip install pipreqs`

生成文件：`pipreqs ./ --encoding utf-8` 会在当前目录下生成文件

##### Error

问题：UnicodeDecodeError: 'gbk' codec can't decode byte 0x80 in position 966: illegal multibyte sequence

解决：指定编码格式：`pipreqs ./ --encoding=utf8`

## Anaconda

### Install Anaconda

使用[清华源](https://mirrors.tuna.tsinghua.edu.cn/anaconda/archive/)去下载安装最新版的安装文件。

#### Ubuntu

不要使用 sudo 去安装，避免安装到 root 目录中，不方便配置和使用

```shell
sh Anaconda3-5.3.1-Linux-x86_64.sh

# miniconda
wget https://repo.anaconda.com/miniconda/Miniconda3-latest-Linux-x86_64.sh
bash Miniconda3-latest-Linux-x86_64.sh
# 一直按回车然后输入yes
please answer 'yes' or 'no':
>>> yes

# 选择安装路径, 文件名前加点号表示隐藏文件
Miniconda3 will now be installed into this location:
>>> ~/.miniconda3

# 添加配置信息到 ~/.bashrc文件
Do you wish the installer to initialize Miniconda3 by running conda init? [yes|no]
[no] >>> yes

# 重新启动 shell
source ~/.bashrc
```

打开 anaconda 的图形化界面（Navigator）

```shell
anaconda-navigator
```

#### Windows 10

安装文件：Anaconda3-5.3.1-Windows-x86_64.exe，也建议安装给当前用户使用。

## environment

基于`anaconda python`的环境导出到`environment.yml`，再通过这个文件在其它电脑上创建`anaconda python`环境。

- 导出环境到 `environment.yml` 文件

```bash
linux: conda env export | grep -v "^prefix: " > environment.yml
windows: conda env export | findstr /v "^prefix: " > environment.yml
```

上面命令导出时通过grep删除了“prefix:“ ，但是实际好像通过文件创建的anaconda python环境，prefix选项好像是不起作用的。

- 基于 `environment.yml` 创建环境

```bash
conda env create -f environment.yml
```

上面命令会安装在 conda 默认的环境路径 `envs`，如果要指定其它安装路径，使用-p选项

```bash
conda env create -f environment.yml -p /home/user/anaconda3/envs/env_name
```

- 导出的 `environment.yml` 中不包含编译依赖

```bash
conda env export --no-builds -f environment.yml
```

- 导出的 `environment.yml` 中只从构建历史中提取显示定义（即我构建的过程，主要工具包，依赖项不导出）

```bash
conda env export --from-history -f environment.yml
```

## conda vs. pip vs. virtualenv

[conda vs. pip vs. virtualenv commands](https://conda.io/projects/conda/en/latest/commands.html#id2)

- Pip 是包管理器
- virtualenv 是环境管理器
- conda 既是包管理器，也是环境管理器

注：[conda general commands help](https://conda.io/projects/conda/en/latest/commands.html)

### base commands

| 任务        | conda                                                                          | Pip                  |
| --------- | ------------------------------------------------------------------------------ | -------------------- |
| 创建需求文件    | `conda list --export`                                                          | `pip freeze`         |
| 安装其他管理器   | `conda install pip`                                                            | `pip install conda`  |
| 安装 Python | `conda install python=x.x`                                                     | X                    |
| 更新 Python | `conda update python`[^2]                                                      | X                    |
| 查看版本      | `conda -V`                                                                     | `pip -V`             |
| 查看帮助      | conda -h`或者`conda --help                                                       | pip -h`或者`pip --help |
| 清除缓存      | [conda clean](https://conda.io/projects/conda/en/latest/commands/clean.html)   |                      |
| 配置参数      | [conda config](https://conda.io/projects/conda/en/latest/commands/config.html) |                      |

### environment commands

| 任务    | conda                                                                      | Virtualenv                                            |
| ----- | -------------------------------------------------------------------------- | ----------------------------------------------------- |
| 创建环境  | conda create -n env_name python=3.6                                        | `cd $ENV_BASE_DIR; virtualenv $ENVIRONMENT_NAME`      |
| 删除环境  | conda remove -n env_name --all                                             | X                                                     |
| 复制环境  | conda create -n dest_env_name --clone src_env_name                         | X                                                     |
| 重命名环境 | 可以通过复制环境完成重命名                                                              | X                                                     |
| 激活环境  | conda activate env_name                                                    | `source $ENV_BASE_DIR/$ENVIRONMENT_NAME/bin/activate` |
| 去活环境  | conda deactivate env_name                                                  | `deactivate`                                          |
| 环境信息  | [conda info](https://conda.io/projects/conda/en/latest/commands/info.html) |                                                       |
| 环境列表  | conda env list                                                             | Install virtualenv wrapper, then `lsvirtualenv`       |

### package commands

| 任务                 | conda 包管理器命令和环境管理器命令                                                                      | Pip 包管理器命令                     |
| ------------------ | ----------------------------------------------------------------------------------------- | ------------------------------ |
| 网络安装包              | [conda install pkg_name](https://conda.io/projects/conda/en/latest/commands/install.html) | `pip install pkg_name`         |
| 磁盘安装包              | `conda install pkg_location`                                                              |                                |
| 指定源安装包             | `conda install pkg_name -c $URL`                                                          | `pip install pkg_name -i $URL` |
| 安装包到指定环境           | `conda install -n env_name pkg_name`                                                      | X                              |
| 安装包到指定用户目录(解决权限问题) | `pip install pkg_name --user`                                                             |                                |
| 删除当前环境包            | [`conda remove pkg_name`](https://conda.io/projects/conda/en/latest/commands/remove.html) | `pip uninstall $PKG_NAME`      |
| 删除指定环境包            | `conda remove -n env_name pkg_name`                                                       | X                              |
| 更新当前环境包            | [`conda update pkg_name`](https://conda.io/projects/conda/en/latest/commands/update.html) | `pip install -U pkg_name`      |
| 更新指定环境包            | `conda update -n env_name pkg_name`                                                       | X                              |
| 更新包管理器             | `conda update conda`                                                                      | `pip install -U pip`           |
| 搜索有效包              | [`conda search pkg_name`](https://conda.io/projects/conda/en/latest/commands/search.html) | `pip search pkg_name`          |
| 当前环境安装包列表          | [conda list](https://conda.io/projects/conda/en/latest/commands/list.html)                | `pip list`                     |
| 指定环境安装包列表          | `conda list -n $ENV_NAME`                                                                 |                                |
| conda底层包工具(试验)     | [`conda package`](https://conda.io/projects/conda/en/latest/commands/package.html)        |                                |

- - Windows: `activate`
    - Linux and macOS: `source activate`
-

## Examples

### Manage Environment

#### VSCode activate powershell conda env

在`setting.json`文件中加入下面两行配置

```json
"terminal.integrated.shell.windows": "C:\\Windows\\System32\\WindowsPowerShell\\v1.0\\powershell.exe",
"terminal.integrated.shellArgs.windows":[ "-ExecutionPolicy" ,"ByPass", "-NoExit", "-Command","& 'C:\\ProgramData\\Anaconda3\\shell\\condabin\\conda-hook.ps1'; conda activate 'C:\\ProgramData\\Anaconda3' "]
```

#### Win10 activate conda env

Q: Windows 10 无法激活 conda 的环境时，即输入 `conda activate env_name`，结果还是在 `base` 环境下，就是激活失败。

A: `Win+X` 进入 `PowerShell` 的管理员模式，输入 `conda init powershell`，关闭当前的 `PowerShell`窗口，再次进入发现可以激活了。

C: VSCode 的 Remote Developement 的 Terminal 环境下的 `PowerShell` 无法激活问题也可以照此操作。

参考文献：

- [解决Win10 PowerShell无法激活Anaconda环境的问题](https://www.cnblogs.com/dereen/p/ps_conda_env.html)
- [How to activate different anaconda environment from powershell](https://stackoverflow.com/questions/47800794/how-to-activate-different-anaconda-environment-from-powershell?rq=1)
- [Activation script initialization](https://www.anaconda.com/conda-4-6-release/)
- [Implementation of Conda's activate/deactivate functions in PowerShell.](https://github.com/BCSharp/PScondaEnvs)
- [Anaconda在PowerShell中无法使用activate激活环境](https://blog.csdn.net/kdongyi/article/details/81905494)

## Errors

### Can't locate DLLs

无法定位动态链接库问题

进入 "Anaconda\DLLS" 目录，查看出了问题的 DLL 文件的日期；

再进入 "Anaconda\Library\bin" 目录，比较两者日期，如果日期不同步就选择一个进行同步，然后重新启动 Anaconda 就可以了。

Import ssl_错误，也可以采用同样的方式处理。删除多余的 DLL 文件就可以了。

### SSLError

1. 找不到 SSL 模块，因此需要将 SSL 模块所在的文件夹路径加入环境变量 ( "D: \Program Files\Anaconda3\Library\bin" )
1. 找不到 DLL 文件，需要将缺失的文件拷入目录中 ( "D: \Program Files\Anaconda3\Library\bin" )，或者拷入到环境所在的目录中 ( "D: \Program Files\Anaconda3\env\your-env\bin" )

### UnicodeDecodeError

错误内容：

```shell
File "C:\Users\ygpfr\Anaconda3\envs\tf-gpu\lib\site-packages\pyreadline\lineeditor\history.py", line 82, in read_history_filey_file for line in open(filename, 'r'):
UnicodeDecodeError: 'gbk' codec can't decode byte 0xae in position 451: illegal multibyte sequence
```

pyreadline 的代码在 GBK 环境下存在问题，找到 `Anaconda3\Lib\site-packages\pyreadline\lineeditor\history.py` 中 82 行，把原来的 `open(filename, ‘r’)` 改为 `open(filename, ‘r’,encoding=‘utf-8’)` 即可。

## Git Bash 中使用 Conda

1. 找到 conda 的安装目录，进入 `etc/profild.d` 文件夹，找到 `conda.sh` 文件
1. 右键，进入 `Git Bash Here`
1. 输入 `echo ". ${PWD}/conda.sh" >> ~/.bashrc`
    1. 如果路径中有空格就输入 `echo ". '${PWD}'/conda.sh >> ~/.bashrc`
1. 关闭窗口，重新打开 Git Bash
    1. 第一次打开时因为缺少 `.bash_profile` 会报错，关掉再打开就好了

参考文献：

- [Setting Up Conda in Git Bash](https://discuss.codecademy.com/t/setting-up-conda-in-git-bash/534473)
- [在 Git Bash 中使用 Anaconda Conda](https://blog.csdn.net/xovee/article/details/126949439)

[^2]: `conda update python` 只更新当前系列中的最新版本。例如：Python 2.x 更新为最新版本的 2.x, Python 3.x 更新为最新版本的 3.x.
