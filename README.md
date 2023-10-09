## Windows平台node安装管理

[nvs安装包下载](https://github.com/jasongin/nvs/releases)

```powershell
# 安装最新版本的 node
nvs add latest
# 安装最新 LTS 版本的 node
nvs add lts
# 使用指定版本的 node
nvs use lts
nvs use 14.21.0
# 永久添加到环境变量
nvs link 14.21.0
```

## Hexo

```powershell
# 安装 hexo
npm install -g hexo-cli
```

## 创建 blog 目录

- 用命令初始化 blog 目录：`hexo init hexo-starter`
- 下载 GitHub 上的仓库：同步完成目录建立和仓库建立
    - fork [hexojs/hexo-starter](https://github.com/hexojs/hexo-starter) 仓库
    - clone 仓库：`git clone https://github.com/zhuyuanxiang/hexo-starter`

## 安装 blog 的辅助包

```powershell
cd hexo-starter
npm install
```

新建完成后，指定文件夹目录下有：

- node_modules: 依赖包
- public：存放生成的页面
- scaffolds：生成文章的一些模板
- source：用来存放你的文章
- themes：主题
- `_config.yml`: 博客的配置文件

## 启动 hexo 服务

```powershell
hexo g
hexo server
```

## 创建 Github Pages

1. 创建 Github 账号
2. 创建 Github 仓库：zhuyuanxiang.github.io
3. 添加 ssh 到 Github

    ```powershell
    # 配置 git
    git config --global user.name "yourname"
    git config --global user.email "youremail"
    # 检测 git 的配置
    git config user.name
    git config user.email
    # 创建 ssh
    ssh-keygen -t rsa -C "youremail"
    # 将 id_rsa.pub 保存到 Github后，检测是否成功
    ssh -T git@github.com
    ```

注：ssh，简单来讲，就是一个秘钥，其中，`id_rsa`是你这台电脑的私人秘钥，不能给别人看的，`id_rsa.pub`是公共秘钥，可以随便给别人看。把这个公钥放在GitHub上，这样当你链接GitHub自己的账户时，它就会根据公钥匹配你的私钥，当能够相互匹配时，才能够顺利的通过git上传你的文件到GitHub上。
而后在GitHub的setting中，找到SSH keys的设置选项，点击`New SSH key`  
把你的`id_rsa.pub`里面的信息复制进去。

## 部署 hexo

```yaml
# _config.yml
deploy:
  type: git
  repo: https://github.com/YourgithubName/YourgithubName.github.io.git
  branch: master
```

```powershell
# 安装 git 发布插件
npm install hexo-deployer-git --save
# 清除历史文件
hexo clean
# 生成静态页面
hexo generator
# 发布
hexo deploy
```

## 安装 hexo 的 theme

```powershell
cd hexo-starter
git clone https://github.com/theme-next/hexo-theme-next themes/next
git clone https://github.com/wujun234/hexo-theme-tree.git  themes/tree
```

删除原始目录下的 `_config.yml` 文件

```powershell
del themes/next/_config.yml
del themes/tree/_config.yml
```

配置根目录下的 `_config.yml` 的 `theme` 为 `next` 或者 `tree`

注：建议 Github 中使用 `tree`，因为使用最简单，Gitee 中使用 `next`，效果最炫。
