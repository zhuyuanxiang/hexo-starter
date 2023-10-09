---
title: VSCode插件开发
excerpt: 一个基本的 VSCode 插件开发流程，已经自己开发插件的动机
categories:
- 编程
tags:
- VSCode
- 插件
- 入门
date: 2020-04-23
updated: 2020-04-23
toc: true
toc_sticky: true
---

# Preface

 ( 这里就是个对 [微软开发文档](https://code.visualstudio.com/api/get-started/your-first-extension) 的翻译，和网上 [其他文档](https://blog.csdn.net/marksinoberg/article/details/89355341) 的总结 )

动机 : 定制化插件，代码中写入自己需要的部分，简化 VSCode 安装了过多的插件，还可以管理自定义插件的代码和版本，也可以发布到 Market

## Initialize Environtent

- 安装 node.js 准备 npm 环境
  - 安装 [Yeoman](http://yeoman.io/) : 用于生成代码框架
  - 安装 [VS Code Extension Generator](https://www.npmjs.com/package/generator-code) : 用于打包代码框架
- 安装 git 环境
- 安装最新版本的 VSCode

## Developing Plugins

### New Project

- 安装需要的开发包

```shell
npm install -g yo generator-code
```

- 生成需要的样本代码 ( 创建代码前，可以创建个 VSCode 目录作为工作目录 )
    -`#`为系统交互的内容，需要输入或者选择
  - 微软推荐的是 ( TypeScript )，个人建议使用 ( [JavaScript](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Guide) )

```shell
yo code

# ? What type of extension do you want to create? New Extension ( JavaScript )
# ? What's the name of your extension? HelloWorld
### 下面的选择全部直接「回车」使用默认值 ###

# ? What's the identifier of your extension? helloworld
# ? What's the description of your extension? LEAVE BLANK
# ? Initialize a git repository? Yes
# ? Which package manager to use? npm
```

- 使用 VSCode 打开项目目录

```shell
code ./helloworld
```

- 按`F5`打开「**Extension Development Host**」窗口，按`Ctrl+Shit+P`进入「Command Palette」，输入「Hello World」执行这个项目
- 在这个新的窗口中会出现「Hello World」的通知，任务完成！

### Modify Code

- 打开`extension.js`，修改代码为

```javascript
vscode.window.showInformationMessage ( 'Hello World from VSCode!' ) ;
```

- 按`Ctrl+Shit+P`进入「Command Palette」，输入「Developer: Reload window」重新载入窗口
- 按`Ctrl+Shit+P`进入「Command Palette」，输入「Hello World」执行这个项目

### Advanced Project

- 修改项目在「Command Palette」中显示的内容
  - 修改`package.json`中的代码

```json
"contributes": {
    "commands": [
    {
        "command": "helloworld.helloWorld",
        "title": "Hello World extension"
    }
    ]
},
```

- 增加一个命令显示当前时间

```javascript
// extend.js
function activate ( context ) {
    // 增加下面的内容
    let disposable_0 = vscode.commands.registerCommand ( 'helloworld.HelloTime', function ( ) {
    var date = new Date ( ) ;
    vscode.window.showInformationMessage ( date.getFullYear ( ) +"-"+date.getMonth ( ) +"-"+date.getDate ( ) +" "+date.getHours ( ) +":"+date.getMinutes ( ) ) ;
 } ) ;
    context.subscriptions.push ( disposable_0 ) ;
}
```

```javascript
// package.json
"activationEvents": [
    // 增加下面的内容
    "onCommand: helloworld.HelloTime"
],
"contributes": {
    "commands": [
        // 增加下面的内容
        {
        "command": "helloworld.HelloTime",
        "title": "Hello Time"
        }
    ]
    },

```

- 替换`vscode.window.showInformationMessage`使用其他的 [VS Code API](https://code.visualstudio.com/api/references/vscode-api) 显示信息。

```javascript
// extend.js
function activate ( context ) {
    // 增加下面的内容
    let disposable_1 = vscode.commands.registerCommand ( 'helloworld.HelloErrorWorld', function ( ) {
        vscode.window.showErrorMessage ( 'Hello World from VSCode!' ) ;
    } ) ;
    context.subscriptions.push ( disposable_1 ) ;
}
```

```javascript
// package.json
"activationEvents": [
    // 增加下面的内容
    "onCommand: helloworld.HelloErrorWorld"
],
"contributes": {
    "commands": [
        // 增加下面的内容
        {
            "command": "helloworld.HelloErrorWorld",
            "title": "Hello Error World"
        }
    ]
    },

```

### Debug Code

- VSCode 可以直接设置断点进行调试

注 : 工具的作用是有限的，长期的开发经验才是关键

### Code Architecture

```javascript
.
├── .vscode
│   ├── launch.json     // 载入和调试插件的配置文件
│   └── tasks.json      // 编译 TypeScript 的配置文件 ( JavaScript 不生成这个文件 )
├── .gitignore          // git 管理时忽略掉 编译输出 和 node 模块
├── README.md           // 描述插件功能的文件
├── src
│   └── extension.ts    // 插件的源代码
├── package.json        // 插件的 manifest
├── tsconfig.json       // TypeScript 的配置文件 ( JavaScript 不生成这个文件 )
```

- 插件做了三件事
  - 注册命令 [`onCommand`](https://code.visualstudio.com/api/references/activation-events#onCommand)  [**Activation Event**](https://code.visualstudio.com/api/references/activation-events) :`onCommand: extension.helloWorld`, 保证运行`Hello World`命令时激活插件
  - 使用属性 [`contributes.commands`](https://code.visualstudio.com/api/references/contribution-points#contributes.commands)  [**Contribution Point**](https://code.visualstudio.com/api/references/contribution-points) 保证命令`Hello World`出现在 Command Palette, 并且绑定 command ID`extension.helloWorld`.
  - 使用属性 [`commands.registerCommand`](https://code.visualstudio.com/api/references/vscode-api#commands.registerCommand)  [**VS Code API**](https://code.visualstudio.com/api/references/vscode-api) 将函数绑定到注册过的 command ID`extension.helloWorld`.

- VS Code 编写插件时需要理解的三个概念
  - 激活事件 ( Activation Events ) : 插件上的事件可以被激活
  - 贡献点 ( Contribution Points ) : 静态声明在插件的 manifest 文件`package.json`文件，用于扩展 VS Code.
  - VS Code API: 插件的代码中可以调用的一组 JavaScript APIs

- 插件的 manifest 文件
    -`name`和`publisher`: VS Code 使用`.`作为插件的惟一 ID. 例子: Hello World 的 ID`vscode-samples.helloworld-sample`. VS Code 使用这个 ID 去惟一确定开发的插件
    -`main`: 插件的入口点
    -`activationEvents`和`contributes`: [Activation Events](https://code.visualstudio.com/api/references/activation-events) 和 [Contribution Points](https://code.visualstudio.com/api/references/contribution-points) .
    -`engines.vscode`: 定义插件需要依赖的 VS Code API 的最低版本。

- 插件的 extension.js 文件
  - 言论扣的两个主要函数 :`activate`and`deactivate`.
        -`activate`: 当插件的 **注册事件** 被激活时时被执行的内容
        -`deactivate`: 当插件的 **注册事件** 去激活时需要清理的内容

## Deploying Plugins

### Packing Plugins

- 安装 vsce

```shell
npm install -g vsce
```

- 使用 vsce

```shell
# 进入插件的安装目录
$ cd myExtension
$ vsce package
# myExtension.vsix generated
# 打包生成后缀为 vsix 的文件
$ vsce publish
# <publisherID>.myExtension published to VS Code MarketPlace
# 发布打包后的文件到 Market，也可以直接在 VSCode 上安装这个插件
```

### Deploying in Market

- 去 [Market](https://marketplace.visualstudio.com/VSCode) 注册用户
- 去 [Publisher](https://marketplace.visualstudio.com/manage) 发布自己编写的插件

注 1：发布后，市场反应有点慢，别着急。特别是更新版本的时候，更需要等一等。

注 2：欢迎大家测试我的 Pangu-Markdown-VScode

### Common Errors

- Error: Missing publisher name. Learn more: [https://code.visualstudio.com...](https://code.visualstudio.com/docs/extensions/publish-extension#_publishing-extensions)
  - 在 package.json 中将刚刚创建好的发布账号配置进去"publisher":"your name"
- Error: Make sure to edit the README.md file before you publish your extension
  - 删除 README.MD 文件中开头的一段文字 ( This is the README for your extension "hello world". After writing up a brief description, we recommend including the following sections. )
- A『repository』field is missing from the 'package.json' manifest file .Do you want to continue? [y/n]
  - 提供没有配置仓库，可以不管，直接确认
