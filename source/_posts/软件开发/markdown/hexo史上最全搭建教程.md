---
title: hexo史上最全搭建教程
excerpt: 在github page平台上托管我们的博客。这样就可以安心的来写作，又不需要定期维护，而且hexo作为一个快速简洁的博客框架，用它来搭建博客真的非常容易。
categories: 编程
tags:
  - hexo
  - Github
  - Pages
  - 编程
  - 笔记
date: 2023-10-08
updated: 2023-10-09
toc: true
toc_sticky: true
---

[原文链接](https://blog.csdn.net/sinat_37781304/article/details/82729029)

现在市面上的博客很多，如CSDN，博客园，简书等平台，可以直接在上面发表，用户交互做的好，写的文章百度也能搜索的到。缺点是比较不自由，会受到平台的各种限制和恶心的广告。

而自己购买域名和服务器，搭建博客的成本实在是太高了，不光是说这些购买成本，单单是花力气去自己搭这么一个网站，还要定期的维护它，对于我们大多数人来说，实在是没有这样的精力和时间。

那么就有第三种选择，直接在github page平台上托管我们的博客。这样就可以安心的来写作，又不需要定期维护，而且hexo作为一个快速简洁的博客框架，用它来搭建博客真的非常容易。

## 概要

Hexo是一款基于Node.js的静态博客框架，依赖少易于安装使用，可以方便的生成静态网页托管在GitHub和Coding上，是搭建博客的首选框架。大家可以进入[hexo官网](https://hexo.io/zh-cn/)进行详细查看，因为Hexo的创建者是台湾人，对中文的支持很友好，可以选择中文进行查看。

教程分三个部分，

- 第一部分：hexo的初级搭建还有部署到github page上，以及个人域名的绑定。
- 第二部分：hexo的基本配置，更换主题，实现多终端工作，以及在coding page部署实现国内外分流
- 第三部分：hexo添加各种功能，包括搜索的SEO，阅读量统计，访问量统计和评论系统等。

______________________________________________________________________

# 第一部分

hexo的初级搭建还有部署到github page上，以及个人域名的绑定。

## Hexo简介

Hexo是一款基于Node.js的静态博客框架，依赖少易于安装使用，可以方便的生成静态网页托管在GitHub和Coding上，是搭建博客的首选框架。大家可以进入[hexo官网](https://hexo.io/zh-cn/)进行详细查看，因为Hexo的创建者是台湾人，对中文的支持很友好，可以选择中文进行查看。

## Hexo搭建步骤

1. 安装Git
1. 安装Node.js
1. 安装Hexo
1. GitHub创建个人仓库
1. 生成SSH添加到GitHub
1. 将hexo部署到GitHub
1. 设置个人域名
1. 发布文章

### 1. 安装Git

Git是目前世界上最先进的分布式版本控制系统，可以有效、高速的处理从很小到非常大的项目版本管理。也就是用来管理你的hexo博客文章，上传到GitHub的工具。Git非常强大，我觉得建议每个人都去了解一下。廖雪峰老师的Git教程写的非常好，大家可以了解一下。[Git教程](https://www.liaoxuefeng.com/wiki/0013739516305929606dd18361248578c67b8067c8c017b000)

windows：到git官网上下载,[Download git](https://gitforwindows.org/),下载后会有一个Git Bash的命令行工具，以后就用这个工具来使用git。

linux：对linux来说实在是太简单了，因为最早的git就是在linux上编写的，只需要一行代码

```shell
sudo apt-get install git
```

安装好后，用`git --version` 来查看一下版本

### 2. 安装nodejs

Hexo是基于nodeJS编写的，所以需要安装一下nodeJs和里面的npm工具。

windows：[nodejs](https://nodejs.org/en/download/)选择LTS版本就行了。

linux：

```shell
sudo apt-get install nodejs
sudo apt-get install npm
```

安装完后，打开命令行

```shell
node -v
npm -v
```

检查一下有没有安装成功

顺便说一下，windows在git安装完后，就可以直接使用git bash来敲命令行了，不用自带的cmd，cmd有点难用。

### 3. 安装hexo

前面git和nodejs安装好后，就可以安装hexo了，你可以先创建一个文件夹blog，然后`cd`到这个文件夹下（或者在这个文件夹下直接右键git bash打开）。

输入命令

```shell
npm install -g hexo-cli
```

依旧用`hexo -v`查看一下版本

至此就全部安装完了。

接下来初始化一下hexo

```shell
hexo init myblog
```

这个myblog可以自己取什么名字都行，然后

```bash
cd myblog //进入这个myblog文件夹
npm install
```

新建完成后，指定文件夹目录下有：

- node_modules: 依赖包
- public：存放生成的页面
- scaffolds：生成文章的一些模板
- source：用来存放你的文章
- themes：主题
- `_config.yml``: 博客的配置文件

```shell
hexo g
hexo server
```

打开hexo的服务，在浏览器输入localhost:4000就可以看到你生成的博客了。

大概长这样：  ![img](pics/hexo%E5%8F%B2%E4%B8%8A%E6%9C%80%E5%85%A8%E6%90%AD%E5%BB%BA%E6%95%99%E7%A8%8B/84e7c09ee0395f0faac60fdc381cd526.jpeg) 使用ctrl+c可以把服务关掉。

### 4. GitHub创建个人仓库

首先，你先要有一个GitHub账户，去注册一个吧。

注册完登录后，在GitHub.com中看到一个New repository，新建仓库

![img](pics/hexo%E5%8F%B2%E4%B8%8A%E6%9C%80%E5%85%A8%E6%90%AD%E5%BB%BA%E6%95%99%E7%A8%8B/3270f5e69799b107def5e169a34869e4.jpeg)

创建一个和你用户名相同的仓库，[后面加.github.io](http://xn--yfr16an19l.github.io/)，只有这样，将来要部署到GitHub page的时候，才会被识别，[也就是xxxx.github.io](http://xn--xxxx-4m5f354ev5p.github.io/)，其中xxx就是你注册GitHub的用户名。我这里是已经建过了。

![img](pics/hexo%E5%8F%B2%E4%B8%8A%E6%9C%80%E5%85%A8%E6%90%AD%E5%BB%BA%E6%95%99%E7%A8%8B/d002150a2eb02a0358d1c88199e4e726.jpeg)

点击create repository。

### 5. 生成SSH添加到GitHub

回到你的git bash中，

```verilog
git config --global user.name "yourname"
git config --global user.email "youremail"
```

这里的yourname输入你的GitHub用户名，youremail输入你GitHub的邮箱。这样GitHub才能知道你是不是对应它的账户。

可以用以下两条，检查一下你有没有输对

```shell
git config user.name
git config user.email
```

然后创建SSH,一路回车

```bash
ssh-keygen -t rsa -C "youremail"
```

这个时候它会告诉你已经生成了.ssh的文件夹。在你的电脑中找到这个文件夹。

![img](pics/hexo%E5%8F%B2%E4%B8%8A%E6%9C%80%E5%85%A8%E6%90%AD%E5%BB%BA%E6%95%99%E7%A8%8B/9e5dde5c03e38f8a4cbecef0e0131bce.jpeg)

ssh，简单来讲，就是一个秘钥，其中，`id_rsa`是你这台电脑的私人秘钥，不能给别人看的，`id_rsa.pub`是公共秘钥，可以随便给别人看。把这个公钥放在GitHub上，这样当你链接GitHub自己的账户时，它就会根据公钥匹配你的私钥，当能够相互匹配时，才能够顺利的通过git上传你的文件到GitHub上。

而后在GitHub的setting中，找到SSH keys的设置选项，点击`New SSH key`
把你的`id_rsa.pub`里面的信息复制进去。

![img](pics/hexo%E5%8F%B2%E4%B8%8A%E6%9C%80%E5%85%A8%E6%90%AD%E5%BB%BA%E6%95%99%E7%A8%8B/3194ad0a9d04d94c09485122932968f3.jpeg)

在gitbash中，查看是否成功

```css
ssh -T git@github.com
```

### 6. 将hexo部署到GitHub

这一步，我们就可以将hexo和GitHub关联起来，也就是将hexo生成的文章部署到GitHub上，打开站点配置文件 `_config.yml`，翻到最后，修改为
YourgithubName就是你的GitHub账户

```yaml
deploy:
  type: git
  repo: https://github.com/YourgithubName/YourgithubName.github.io.git
  branch: master
```

这个时候需要先安装deploy-git ，也就是部署的命令,这样你才能用命令部署到GitHub。

```shell
npm install hexo-deployer-git --save
```

然后

```shell
hexo clean
hexo generate
hexo deploy
```

其中 `hexo clean`清除了你之前生成的东西，也可以不加。
`hexo generate` 顾名思义，生成静态文章，可以用 `hexo g`缩写
`hexo deploy` 部署文章，可以用`hexo d`缩写

注意deploy时可能要你输入username和password。

得到下图就说明部署成功了，过一会儿就可以在`http://yourname.github.io` 这个网站看到你的博客了！！
![img](pics/hexo%E5%8F%B2%E4%B8%8A%E6%9C%80%E5%85%A8%E6%90%AD%E5%BB%BA%E6%95%99%E7%A8%8B/a4eba29bb7c185169228b596e636422f.jpeg)

### 7. 设置个人域名

现在你的个人网站的地址是 `yourname.github.io`，如果觉得这个网址逼格不太够，这就需要你设置个人域名了。但是需要花钱。

注册一个阿里云账户,在[阿里云](https://wanwang.aliyun.com/?spm=5176.8142029.digitalization.2.e9396d3e46JCc5)上买一个域名，我买的是 `fangzh.top`，各个后缀的价格不太一样，比如最广泛的.com就比较贵，看个人喜好咯。

你需要先去进行实名认证,然后在域名控制台中，看到你购买的域名。

点**解析**进去，添加解析。

![img](pics/hexo%E5%8F%B2%E4%B8%8A%E6%9C%80%E5%85%A8%E6%90%AD%E5%BB%BA%E6%95%99%E7%A8%8B/951e65614a195996cff7519f7fafdec1.jpeg)

其中，192.30.252.153 和 192.30.252.154 是GitHub的服务器地址。
**注意，解析线路选择默认**，不要像我一样选境外。这个境外是后面来做国内外分流用的,在后面的博客中会讲到。记得现在选择**默认**！！

![img](pics/hexo%E5%8F%B2%E4%B8%8A%E6%9C%80%E5%85%A8%E6%90%AD%E5%BB%BA%E6%95%99%E7%A8%8B/6ae646523b70539689b04866baf844c9.jpeg)

登录GitHub，进入之前创建的仓库，点击settings，设置Custom domain，输入你的域名`fangzh.top`

![img](pics/hexo%E5%8F%B2%E4%B8%8A%E6%9C%80%E5%85%A8%E6%90%AD%E5%BB%BA%E6%95%99%E7%A8%8B/598c53e8b59a6b88ad484024879da694.jpeg)

然后在你的博客文件source中创建一个名为CNAME文件，不要后缀。写上你的域名。

![img](pics/hexo%E5%8F%B2%E4%B8%8A%E6%9C%80%E5%85%A8%E6%90%AD%E5%BB%BA%E6%95%99%E7%A8%8B/170e6d6498ede45ec1950599fa74a260.jpeg)

最后，在gitbash中，输入

```undefined
hexo clean
hexo g
hexo d
```

过不了多久，再打开你的浏览器，输入你自己的域名，就可以看到搭建的网站啦！

接下来你就可以正式开始写文章了。

```cpp
hexo new newpapername
```

然后在source/_post中打开markdown文件，就可以开始编辑了。当你写完的时候，再

```undefined
hexo clean
hexo g
hexo d
```

就可以看到更新了。

# 第二部分

hexo的基本配置，更换主题，实现多终端工作，以及在coding page部署实现国内外分流。

## 1. hexo基本配置

在文件根目录下的`_config.yml`，就是整个hexo框架的配置文件了。可以在里面修改大部分的配置。详细可参考[官方的配置](https://hexo.io/zh-cn/docs/configuration)描述。

### 网站

| 参数            | 描述                                                                                                                                   |
| ------------- | ------------------------------------------------------------------------------------------------------------------------------------ |
| `title`       | 网站标题                                                                                                                                 |
| `subtitle`    | 网站副标题                                                                                                                                |
| `description` | 网站描述                                                                                                                                 |
| `author`      | 您的名字                                                                                                                                 |
| `language`    | 网站使用的语言                                                                                                                              |
| `timezone`    | 网站时区。Hexo 默认使用您电脑的时区。[时区列表](https://en.wikipedia.org/wiki/List_of_tz_database_time_zones)。比如说：`America/New_York`, `Japan`, 和 `UTC` 。 |

其中，`description`主要用于SEO，告诉搜索引擎一个关于您站点的简单描述，通常建议在其中包含您网站的关键词。`author`参数用于主题显示文章的作者。

### 网址

| 参数                   | 描述                                                   |
| -------------------- | ---------------------------------------------------- |
| `url`                | 网址                                                   |
| `root`               | 网站根目录                                                |
| `permalink`          | 文章的 [永久链接](https://hexo.io/zh-cn/docs/permalinks) 格式 |
| `permalink_defaults` | 永久链接中各部分的默认值                                         |

在这里，你需要把`url`改成你的网站域名。

permalink，也就是你生成某个文章时的那个链接格式。

比如我新建一个文章叫`temp.md`，那么这个时候他自动生成的地址就是`http://yoursite.com/2018/09/05/temp`。

以下是官方给出的示例，关于链接的变量还有很多，需要的可以去官网上查找 [永久链接](https://hexo.io/zh-cn/docs/permalinks) 。

| 参数                              | 结果                          |
| ------------------------------- | --------------------------- |
| `:year/:month/:day/:title/`     | 2013/07/14/hello-world      |
| `:year-:month-:day-:title.html` | 2013-07-14-hello-world.html |
| `:category/:title`              | foo/bar/hello-world         |

再往下翻，中间这些都默认就好了。

```yaml
theme: landscape

# Deployment
## Docs: https://hexo.io/docs/deployment.html
deploy:
  type: git
  repo: <repository url>
  branch: [branch]

```

`theme`就是选择什么主题，也就是在`theme`这个文件夹下，在官网上有很多个主题，默认给你安装的是`lanscape`这个主题。当你需要更换主题时，在官网上下载，把主题的文件放在`theme`文件夹下，再修改这个参数就可以了。

接下来这个`deploy`就是网站的部署的，`repo`就是仓库(`Repository`)的简写。`branch`选择仓库的哪个分支。这个在之前进行github page部署的时候已经修改过了，不再赘述。而这个在后面进行双平台部署的时候会再次用到。

### Front-matter

Front-matter 是文件最上方以 `---` 分隔的区域，用于指定个别文件的变量，举例来说：

```yaml
title: Hello World
date: 2013/7/13 20:46:25
---
```

下是预先定义的参数，您可在模板中使用这些参数值并加以利用。

| 参数           | 描述         |
| ------------ | ---------- |
| `layout`     | 布局         |
| `title`      | 标题         |
| `date`       | 建立日期       |
| `updated`    | 更新日期       |
| `comments`   | 开启文章的评论功能  |
| `tags`       | 标签（不适用于分页） |
| `categories` | 分类（不适用于分页） |
| `permalink`  | 覆盖文章网址     |

其中，分类和标签需要区别一下，分类具有顺序性和层次性，也就是说 `Foo, Bar` 不等于 `Bar, Foo`；而标签没有顺序和层次。

```yaml
categories: Diary
tags:
  - PS3
  - Games
```

### layout（布局）

当你每一次使用代码

```cpp
hexo new paper
```

它其实默认使用的是`post`这个布局，也就是在`source`文件夹下的`_post`里面。

Hexo 有三种默认布局：`post`、`page` 和 `draft`，它们分别对应不同的路径，而您自定义的其他布局和 `post` 相同，都将储存到 `source/_posts` 文件夹。

| 布局      | 路径               |
| ------- | ---------------- |
| `post`  | `source/_posts`  |
| `page`  | `source`         |
| `draft` | `source/_drafts` |

而new这个命令其实是：

```cpp
hexo new [layout] <title>
```

只不过这个layout默认是post罢了。

#### page

如果你想另起一页，那么可以使用

```cpp
hexo new page board
```

系统会自动给你在source文件夹下创建一个board文件夹，[以及board文件夹中的index.md](http://xn--boardindex-zt2p02eqgs45dlht758bqmyd.md/)，这样你访问的board对应的链接就是`http://xxx.xxx/board`

#### draft

draft是草稿的意思，也就是你如果想写文章，又不希望被看到，那么可以

```cpp
hexo new draft newpage
```

这样会在source/_draft中新建一个newpage.md文件，如果你的草稿文件写的过程中，想要预览一下，那么可以使用

```css
hexo server --draft
```

在本地端口中开启服务预览。

如果你的草稿文件写完了，想要发表到post中，

```undefined
hexo publish draft newpage
```

就会自动把newpage.md发送到post中。

______________________________________________________________________

## 2. 更换主题

到这一步，如果你觉得默认的`landscape`主题不好看，那么可以在官网的主题中，选择你喜欢的一个主题进行修改就可以啦。[点这里](https://hexo.io/themes/)

![img](pics/hexo%E5%8F%B2%E4%B8%8A%E6%9C%80%E5%85%A8%E6%90%AD%E5%BB%BA%E6%95%99%E7%A8%8B/5499f5c70d21f130e51c5568de3549e2.jpeg)

这里有200多个主题可以选。不过最受欢迎的就是那么几个，比如[NexT主题](https://github.com/theme-next/hexo-theme-next)，非常的简洁好看，大多数人都选择这个，关于这个的教程也比较多。不过我选择的是[hueman](https://github.com/ppoffice/hexo-theme-hueman)这个主题，好像是从WordPress移植过来的，展示效果如下：

![img](pics/hexo%E5%8F%B2%E4%B8%8A%E6%9C%80%E5%85%A8%E6%90%AD%E5%BB%BA%E6%95%99%E7%A8%8B/243c5e2ca51f4e8b6f5ea3037b2d667a.jpeg)

不管怎么样，至少是符合我个人的审美。

直接在github链接上下载下来，然后放到`theme`文件夹下就行了，然后再在刚才说的配置文件中把`theme`换成那个主题文件夹的名字，它就会自动在`theme`文件夹中搜索你配置的主题。

而后进入`hueman`这个文件夹，可以看到里面也有一个配置文件`_config.xml`，貌似它默认是`_config.xml.example`，把它复制一份，重命名为`_config.xml`就可以了。这个配置文件是修改你整个主题的配置文件。

### menu（菜单栏）

也就是上面菜单栏上的这些东西。

![img](pics/hexo%E5%8F%B2%E4%B8%8A%E6%9C%80%E5%85%A8%E6%90%AD%E5%BB%BA%E6%95%99%E7%A8%8B/24081eeae96804090f36d58284a1b13a.jpeg)

其中，About这个你是找不到网页的，因为你的文章中没有about这个东西。如果你想要的话，可以执行命令

```cpp
hexo new page about
```

它就会在根目录下`source`文件夹中新建了一个`about`文件夹，[以及index.md](http://xn--index-ok2hl60a.md/)，在index.md中写上你想要写的东西，就可以在网站上展示出来了。

如果你想要自己再自定义一个菜单栏的选项，那么就

```cpp
hexo new page yourdiy
```

然后在主题配置文件的menu菜单栏添加一个 `Yourdiy : /yourdiy`，注意冒号后面要有空格，以及前面的空格要和menu中默认的保持整齐。然后在`languages`文件夹中，找到`zh-CN.yml`，在index中添加`yourdiy: '中文意思'`就可以显示中文了。

### customize(定制)

在这里可以修改你的个人logo，默认是那个hueman，在`source/css/images`文件夹中放入自己要的logo，再改一下`url`的链接名字就可以了。

`favicon`是网站中出现的那个小图标的icon，找一张你喜欢的logo，然后转换成ico格式，放在images文件夹下，配置一下路径就行。

`social_links` ，可以显示你的社交链接，而且是有logo的。

**tips:**

在这里可以添加一个rss功能，也就是那个符号像wifi一样的东西。

### 添加RSS

1. 什么是RSS？

    RSS也就是订阅功能，你可以理解为类似与订阅公众号的功能，来订阅各种博客，杂志等等。

1. 为什么要用RSS？

    就如同订阅公众号一样，你对某个公众号感兴趣，你总不可能一直时不时搜索这个公众号来看它的文章吧。博客也是一样，如果你喜欢某个博主，或者某个平台的内容，你可以通过RSS订阅它们，然后在RSS阅读器上可以实时推送这些消息。现在网上的垃圾消息太多了，如果你每一天都在看这些消息中度过，漫无目的的浏览，只会让你的时间一点一点的流逝，太不值得了。如果你关注的博主每次都发的消息都是精华，而且不是每一天十几条几十条的轰炸你，那么这个博主就值得你的关注，你就可以通过RSS订阅他。

    在我的理解中，如果你不想每天都被那些没有质量的消息轰炸，只想安安静静的关注几个博主，每天看一些有质量的内容也不用太多，那么RSS订阅值得你的拥有。

1. 添加RSS功能

    先安装RSS插件

    ````
        ```shell
        npm i hexo-generator-feed
        ```
    ````

    而后在你整个项目的`_config.yml`中找到Extensions，添加：

    ````
        ```yaml
        # Extensions
        ## Plugins: https://hexo.io/plugins/
        #RSS订阅
        plugin:
        - hexo-generator-feed
        #Feed Atom
        feed:
            type: atom
            path: atom.xml
            limit: 20
        ```
    ````

    这个时候你的RSS链接就是 域名`/atom.xml`了。

    所以，在主题配置文件中的这个`social links`，开启RSS的页面功能，这样你网站上就有那个像wifi一样符号的RSS logo了，注意空格。

    ````
        ```bash
        rss: /atom.xml
        ```
    ````

1. 如何关注RSS？

    首先，你需要一个RSS阅读器，在这里我推荐`inoreader`，宇宙第一RSS阅读器，而且中文支持的挺好。不过它没有PC端的程序，只有网页版，chrome上有插件。在官网上用google账号或者自己注册账号登录，就可以开始你的关注之旅了。

    每次需要关注某个博主时，就点开他的RSS链接，把链接复制到`inoreader`上，就能关注了，当然，如果是比较大众化的很厉害的博主，你直接搜名字也可以的，比如每个人都非常佩服的阮一峰大师，直接在阅读器上搜索`阮一峰`，应该就能出来了。

    我关注的比如，阮一峰的网络日志，月光博客，知乎精选等，都很不错。当然，还有我！！赶快关注我吧！你值得拥有：[http://fangzh.top/atom.xml](http://fangzh.top/atom.xml)

    在安卓端，inoreader也有下载，不过因为国内google是登录不了的，你需要在inoreader官网上把你的密码修改了，然后就可以用账户名和密码登录了。

    在IOS端，没用过，好像是reader 3可以支持inoreader账户，还有个readon也不错，可以去试试。

### widgets(侧边栏)

侧边栏的小标签，如果你想自己增加一个，比如我增加了一个联系方式，那么我把`communication`写在上面，在`zh-CN.yml`中的`sidebar`，添加`communication: '中文'`。

然后在`hueman/layout/widget`中添加一个`communicaiton.ejs`，填入模板：

```js
< %
if (site.posts.length) {
    % >
    <
    div class = "widget-wrap widget-list" >
    <
    h3 class = "widget-title" > < %= __('sidebar.communiation') % > < /h3> <
        div class = "widget" >
        <
        !--这里添加你要写的内容-- >
        <
        /div> <
        /div> <
        %
} % >
```

### search(搜索框)

默认搜索框是不能够用的，

> you need to install `hexo-generator-json-content` before using Insight Search

它已经告诉你了，如果想要使用，就安装这个插件。

### comment(评论系统)

这里的多数都是国外的，基本用不了。这个`valine`好像不错，还能统计文章阅读量，可以自己试一试，[链接](https://valine.js.org/quickstart.html#npm)。

### miscellaneous(其他)

这里我就改了一个`links`，可以添加友链。注意空格要对！不然会报错！

### 总结

整个主题看起来好像很复杂的样子，但是仔细捋一捋其实也比较流畅，

- languages: 顾名思义
- layout：布局文件，其实后期想要修改自定义网站上的东西，添加各种各样的信息，主要是在这里修改，其中`comment`是评论系统，`common`是常规的布局，最常修改的在这里面，比如修改页面`head`和`footer`的内容。
- scripts：js脚本，暂时没什么用
- source：里面放了一些css的样式，以及图片

## 3. git分支进行多终端工作

问题来了，如果你现在在自己的笔记本上写的博客，部署在了网站上，那么你在家里用台式机，或者实验室的台式机，发现你电脑里面没有博客的文件，或者要换电脑了，最后不知道怎么移动文件，怎么办？

在这里我们就可以利用git的分支系统进行多终端工作了，这样每次打开不一样的电脑，只需要进行简单的配置和在github上把文件同步下来，就可以无缝操作了。

### 机制

机制是这样的，由于`hexo d`上传部署到github的其实是hexo编译后的文件，是用来生成网页的，不包含源文件。

![可以看到，并没有source等源文件在内](pics/hexo%E5%8F%B2%E4%B8%8A%E6%9C%80%E5%85%A8%E6%90%AD%E5%BB%BA%E6%95%99%E7%A8%8B/a043c79e9e69631259014b633a308776.jpeg)

也就是上传的是在本地目录里自动生成的`.deploy_git`里面。

其他文件 ，包括我们写在source 里面的，和配置文件，主题文件，都没有上传到github

![img](pics/hexo%E5%8F%B2%E4%B8%8A%E6%9C%80%E5%85%A8%E6%90%AD%E5%BB%BA%E6%95%99%E7%A8%8B/de6cb5a609ae31f7b871075a65fb9d08.jpeg)

所以可以利用git的分支管理，将源文件上传到github的另一个分支即可。

### 上传分支

首先，先在github上新建一个hexo分支，如图：

![img](pics/hexo%E5%8F%B2%E4%B8%8A%E6%9C%80%E5%85%A8%E6%90%AD%E5%BB%BA%E6%95%99%E7%A8%8B/9ef237aa346708b1f641902595664ec4.jpeg)

然后在这个仓库的settings中，选择默认分支为hexo分支（这样每次同步的时候就不用指定分支，比较方便）。

![img](pics/hexo%E5%8F%B2%E4%B8%8A%E6%9C%80%E5%85%A8%E6%90%AD%E5%BB%BA%E6%95%99%E7%A8%8B/379e8bad592c693dc06066fcca13b625.jpeg)

然后在本地的任意目录下，打开git bash，

```shell
git clone git@github.com:ZJUFangzh/ZJUFangzh.github.io.git
```

将其克隆到本地，因为默认分支已经设成了hexo，所以clone时只clone了hexo。

接下来在克隆到本地的`ZJUFangzh.github.io`中，把除了.git 文件夹外的所有文件都删掉

把之前我们写的博客源文件全部复制过来，除了`.deploy_git`。这里应该说一句，复制过来的源文件应该有一个`.gitignore`，用来忽略一些不需要的文件，如果没有的话，自己新建一个，在里面写上如下，表示这些类型文件不需要git：

```x86asm
.DS_Store
Thumbs.db
db.json
*.log
node_modules/
public/
.deploy*/
```

注意，如果你之前克隆过theme中的主题文件，那么应该把主题文件中的`.git`文件夹删掉，因为git不能嵌套上传，最好是显示隐藏文件，检查一下有没有，否则上传的时候会出错，导致你的主题文件无法上传，这样你的配置在别的电脑上就用不了了。

而后

```shell
git add .
git commit –m "add branch"
git push
```

这样就上传完了，可以去你的github上看一看hexo分支有没有上传上去，其中`node_modules`、`public`、`db.json`已经被忽略掉了，没有关系，不需要上传的，因为在别的电脑上需要重新输入命令安装 。

![img](pics/hexo%E5%8F%B2%E4%B8%8A%E6%9C%80%E5%85%A8%E6%90%AD%E5%BB%BA%E6%95%99%E7%A8%8B/0039e2039209460ea6e15deb7e47bdaa.jpeg)

这样就上传完了。

### 更换电脑操作

一样的，跟之前的环境搭建一样，

- 安装git

```csharp
sudo apt-get install git
```

- 设置git全局邮箱和用户名

```verilog
git config --global user.name "yourgithubname"
git config --global user.email "yourgithubemail"
```

- 设置ssh key

```perl
ssh-keygen -t rsa -C "youremail"
#生成后填到github和coding上（有coding平台的话）
#验证是否成功
ssh -T git@github.com
ssh -T git@git.coding.net #(有coding平台的话)
```

- 安装nodejs

```csharp
sudo apt-get install nodejs
sudo apt-get install npm
```

- 安装hexo

```avrasm
sudo npm install hexo-cli -g
```

但是已经不需要初始化了，

直接在任意文件夹下，

```bash
git clone git@………………
```

然后进入克隆到的文件夹：

```lua
cd xxx.github.io
npm install
npm install hexo-deployer-git --save
```

生成，部署：

```undefined
hexo g
hexo d
```

然后就可以开始写你的新博客了

```cpp
hexo new newpage
```

**Tips:**

1. 不要忘了，每次写完最好都把源文件上传一下

    ```shell
    git add .
    git commit –m "xxxx"
    git push
    ```

1. 如果是在已经编辑过的电脑上，已经有clone文件夹了，那么，每次只要和远端同步一下就行了

    ```undefined
    git pull
    ```

## 4. coding page上部署实现国内外分流

之前我们已经把hexo托管在github了，但是github是国外的，而且百度的爬虫是不能够爬取github的，所以如果你希望你做的博客能够在百度引擎上被收录，而且想要更快的访问，那么可以在国内的coding page做一个托管，这样在国内访问就是coding page，国外就走github page。

1. 申请coding账户，新建项目

1. 添加ssh key：跟github一样。

    ```shell
    # 添加后，检查一下是不是添加成功
    ssh -T git@git.coding.net
    ```

1. 修改_config.yml

    ```yaml
    deploy:
        type: git
        repo:
            coding: git@git.coding.net:ZJUFangzh/ZJUFangzh.git,master
            github: git@github.com:ZJUFangzh/ZJUFangzh.github.io.git,master
    ```

1. 部署

    ```shell
    hexo g
    hexo d
    ```

1. 开启coding pages服务，绑定域名

    ![img](pics/hexo%E5%8F%B2%E4%B8%8A%E6%9C%80%E5%85%A8%E6%90%AD%E5%BB%BA%E6%95%99%E7%A8%8B/10a1bbb64e8a644bafb91a2cc772e5a3.jpeg)

1. 阿里云添加解析

    ![img](pics/hexo%E5%8F%B2%E4%B8%8A%E6%9C%80%E5%85%A8%E6%90%AD%E5%BB%BA%E6%95%99%E7%A8%8B/74c4eee3ea9dde8c1cecbaadf05d2f9a.jpeg)

    这个时候就可以把之前github的解析改成境外，把coding的解析设为默认了。

1. 去除coding page的跳转广告

    coding page的一个比较恶心人的地方就是，你只是银牌会员的话，访问会先跳转到一个广告，再到你自己的域名。那么它也给出了消除的办法。右上角切换到coding的旧版界面，默认新版是不行的。然后再来到`pages服务`这里。

    ![img](pics/hexo%E5%8F%B2%E4%B8%8A%E6%9C%80%E5%85%A8%E6%90%AD%E5%BB%BA%E6%95%99%E7%A8%8B/30b3a764555f9c5b36c5a92c786b8719.jpeg)

    只要你在页面上添加一行文字，写`Hosted by Coding Pages`，然后点下面的小勾勾，两个工作日内它就会审核通过了。

    ```xml
    <?xml version="1.0" encoding="utf-8"?>
    <p>
        Hosted by
        <a href="https://pages.coding.me" style="font-weight: bold">
            Coding Pages
        </a>
    </p>

    ```

    我的选择是把这一行代码放在主题文件夹`/layout/common/footer.ejs`里面，也就是本来在页面中看到的页脚部分。

    ![img](pics/hexo%E5%8F%B2%E4%B8%8A%E6%9C%80%E5%85%A8%E6%90%AD%E5%BB%BA%E6%95%99%E7%A8%8B/cd1a623e7c46deaa5bf02c00f0dc53b5.jpeg)

    当然，为了统一，我又在后面加上了and **Github**哈哈，可以不加。

    ```xml
    <?xml version="1.0" encoding="utf-8"?>
    <p>
        <span>
            Hosted by
            <a href="https://pages.coding.me" style="font-weight: bold">
                Coding Pages
            </a>
        </span>
        and
        <span>
            <a href="https://github.com" style="font-weight: bold">
                Github
            </a>
        </span>
    </p>

    ```

    这是最终加上去的代码。

______________________________________________________________________

# 第三部分

hexo添加各种功能，包括搜索的SEO，阅读量统计，访问量统计和评论系统等。

本文参考了: [visugar.com](http://visugar.com/2017/08/01/20170801HexoPlugins/)这里面说的很详细了。

## 1. SEO优化

推广是很麻烦的事情，怎么样别人才能知道我们呢，首先需要让搜索引擎收录你的这个网站，别人才能搜索的到。那么这就需要SEO优化了。

> SEO是由英文Search Engine Optimization缩写而来， 中文意译为“搜索引擎优化”。SEO是指通过站内优化比如网站结构调整、网站内容建设、网站代码优化等以及站外优化。

### 百度seo

刚建站的时候是没有搜索引擎收录我们的网站的。可以在搜索引擎中输入`site:<域名>`

来查看一下。

#### 1. 登录百度站长平台添加网站

登录[百度站长平台](https://ziyuan.baidu.com/linksubmit/index?)，在站点管理中添加你自己的网站。

验证网站有三种方式：文件验证、HTML标签验证、CNAME验证。

第三种方式最简单，只要将它提供给你的那个xxxxx使用CNAME解析到xxx.baidu.com就可以了。也就是登录你的阿里云，把这个解析填进去就OK了。

#### 2. 提交链接

我们需要使用npm自动生成网站的sitemap，然后将生成的sitemap提交到百度和其他搜索引擎

```mipsasm
npm install hexo-generator-sitemap --save
npm install hexo-generator-baidu-sitemap --save
```

这时候你需要在你的根目录下`_config.xml`中看看url有没有改成你自己的：

![img](pics/hexo%E5%8F%B2%E4%B8%8A%E6%9C%80%E5%85%A8%E6%90%AD%E5%BB%BA%E6%95%99%E7%A8%8B/d15a2de073246209dee33dfd74fc7505.jpeg)

重新部署后，就可以在public文件夹下看到生成的sitemap.xml和baidusitemap.xml了。

然后就可以向百度提交你的站点地图了。

这里建议使用自动提交。

![img](pics/hexo%E5%8F%B2%E4%B8%8A%E6%9C%80%E5%85%A8%E6%90%AD%E5%BB%BA%E6%95%99%E7%A8%8B/e9c830bf284910f23b9c8f8505395a3c.jpeg)

自动提交又分为三种：主动推送、自动推送、sitemap。

可以三个一起提交不要紧，我选择的是后两种。

- 自动推送：把百度生成的自动推送代码，放在主题文件`/layout/common/head.ejs`的适当位置，然后验证一下就可以了。
- sitemap：把两个sitemap地址，提交上去，看到状态正常就OK了。

![img](pics/hexo%E5%8F%B2%E4%B8%8A%E6%9C%80%E5%85%A8%E6%90%AD%E5%BB%BA%E6%95%99%E7%A8%8B/bb66cfa619e41b4598a0598f3bbdeb4a.jpeg)

**ps:** 百度收录比较慢，慢慢等个十天半个月再去`site:<域名>`看看有没有被收录。

### google的SEO

流程一样，google更简单，而且收录更快，进入[google站点地图](https://search.google.com/search-console/sitemaps?resource_id=http://fangzh.top/&hl=zh-CN)，提交网站和sitemap.xml，就可以了。

如果你这个域名在google这里出了问题，那你就提交 [yourname.github.io](http://yourname.github.io/)，这个链接，效果是一样的。

不出意外的话一天内google就能收录你的网站了。

![img](pics/hexo%E5%8F%B2%E4%B8%8A%E6%9C%80%E5%85%A8%E6%90%AD%E5%BB%BA%E6%95%99%E7%A8%8B/57a23694869f490d30d4f6fd9dcf7a68.jpeg)

其他的搜索，如搜狗搜索，360搜索，流程是一样的，这里就不再赘述。

## 2. 评论系统

评论系统有很多，但是很多都是墙外的用不了，之前说过这个valine好像集成在hueman和next主题里面了，但是我还没有研究过，我看的是[visugar](http://visugar.com/2017/08/01/20170801HexoPlugins/)这个博主用的来比力评论系统，感觉也还不错。

[来比力官网](https://livere.com/)，注册好后，点击管理页面，在`代码管理`中找到安装代码：

![img](pics/hexo%E5%8F%B2%E4%B8%8A%E6%9C%80%E5%85%A8%E6%90%AD%E5%BB%BA%E6%95%99%E7%A8%8B/4b68111ca1736e5aee54974763fd2ba0.jpeg)

获取安装代码后，在主题的comment下新建一个文件放入刚刚那段代码，再找到article文件，找到如下代码，若没有则直接在footer后面添加即可。livebe即为刚刚所创文件名称。

```csharp
<%- partial('comment/livebe') %>
```

然后可以自己设置一些东西：

![img](pics/hexo%E5%8F%B2%E4%B8%8A%E6%9C%80%E5%85%A8%E6%90%AD%E5%BB%BA%E6%95%99%E7%A8%8B/341c058799572737832bdfb661b883d5.jpeg)

还可以设置评论提醒，这样别人评论你的时候就可以及时知道了。

## 3. 添加百度统计

百度统计可以在后台上看到你网站的访问数，浏览量，浏览链接分布等很重要的信息。所以添加百度统计能更有效的让你掌握你的网站情况。

[百度统计](https://tongji.baidu.com/)，注册一下，这里的账号好像和百度账号不是一起的。

![img](pics/hexo%E5%8F%B2%E4%B8%8A%E6%9C%80%E5%85%A8%E6%90%AD%E5%BB%BA%E6%95%99%E7%A8%8B/78605ca97255b53fd75d90f8b0fc858d.jpeg)

照样把代码复制到`head.ejs`文件中，然后再进行一下安装检查，半小时左右就可以在百度统计里面看到自己的网站信息了。

## 4. 文章阅读量统计leanCloud

[leanCloud](https://leancloud.cn/)，进去后注册一下，进入后创建一个应用：

![img](pics/hexo%E5%8F%B2%E4%B8%8A%E6%9C%80%E5%85%A8%E6%90%AD%E5%BB%BA%E6%95%99%E7%A8%8B/fda4f72f190ff5910ff790b05641096c.jpeg)

在`存储`中创建Class，命名为Counter,

![img](pics/hexo%E5%8F%B2%E4%B8%8A%E6%9C%80%E5%85%A8%E6%90%AD%E5%BB%BA%E6%95%99%E7%A8%8B/16843063bfd1ae6f2e743001bc06067e.jpeg)

然后在设置页面看到你的`应用Key`，在主题的配置文件中：

```yaml
leancloud_visitors:
  enable: true
  app_id: 你的id
  app_key: 你的key
```

在`article.ejs`中适当的位置添加如下，这要看你让文章的阅读量统计显示在哪个地方了，

```bash
阅读数量:<span id="<%= url_for(post.path) %>" class="leancloud_visitors" data-flag-title="<%- post.title %>"></span>次
```

然后在`footer.ejs`的最后，添加：

```xml
<?xml version="1.0" encoding="utf-8"?>
<script src="//cdn1.lncld.net/static/js/2.5.0/av-min.js"/>

```

重新部署后就可以了。

## 5. 引入不蒜子访问量和访问人次统计

不蒜子的添加非常非常方便，[不蒜子](http://busuanzi.ibruce.info/)

在`footer.ejs`中的合适位置，看你要显示在哪个地方，添加：

```xml
<?xml version="1.0" encoding="utf-8"?>
<!--这一段是不蒜子的访问量统计代码-->
<script>
 src="//dn-lbstatics.qbox.me/busuanzi/2.3/busuanzi.pure.mini.js"&gt;
</script>

```

就可以了。

# 全文总结

到这里就基本做完了。其实都是参考别的博主的设置的，不一定仅限于hueman主题，其他主题的设置也是大体相同的，所以如果你希望设置别的主题，那么仔细看一下这个主题的代码结构，也能够把上边的功能添加进去。

多看看别的博主的那些功能，如果有你能找到自己喜欢的功能，那么好好发动搜索技能，很快就能找到怎么做了。加油吧！
