---
title: VSCode插件开发日记--1环境配置
date: 2022-01-03 18:54:25
tags: 
- TypeScript
- VScode
---

## 写在前面

​	在插件商店中找了很久，能听歌了，能写知乎了，甚至可以去交代码了。但是在kaggle的比赛中并没有相关的插件，所以开始着手写自己的第一个插件，去适应相关的数据竞赛的环境。

<!--more-->

## 前期准备

​	在VScode的文档页有对插件开发的相关介绍，需要自己的电脑已经安装了Node.js 和 Git

​	之后运行

```
npm install -g yo generator-code

yo code

? What type of extension do you want to create? New Extension (JavaScript)
? What's the name of your extension? Kaggle-Tools
? What's the identifier of your extension? kaggle-tools
? What's the description of your extension?
? Enable JavaScript type checking in 'jsconfig.json'? No
? Initialize a git repository? Yes
? Which package manager to use? npm
```

之后，进入到编辑器中 ，按F5，会在一个新的本地插件开发窗口打开

![image-20220103195211834](/Users/l_collection/Library/Application Support/typora-user-images/image-20220103195211834.png)

在运行窗口中 `cmd+shift+p` 调出命令面板，输入Hello World 右下角弹出相应的信息反馈。



## 后续开发 

之后可以尝试改变右下角的消息弹出内容

	1. 改变showInformationMessage()中的内容在extension.js文件中（我是以JavaScript格式生成的）
	1. 再次运行开发窗口，重复运行一边上述的操作，发现弹出信息发生变化



## Debug

VScode在插件开发的过程中可以进行断点调试



## 项目文件夹结构

```
.
├── .gitignore                  //配置不需要加入版本管理的文件
├── .vscode                     // VS Code的整合
│   ├── launch.json
│   ├── settings.json
│   └── tasks.json
├── .vscodeignore                //配置不需要加入最终发布到拓展中的文件
├── README.md
├── src                         // 源文件
│   └── extension.ts            // 如果我们使用js来开发拓展，则该文件的后缀为.js
├── test                        // test文件夹
│   ├── extension.test.ts       // 如果我们使用js来开发拓展，则该文件的后缀为.js
│   └── index.ts                // 如果我们使用js来开发拓展，则该文件的后缀为.js
├── node_modules
│   ├── vscode                  // vscode对typescript的语言支持。
│   └── typescript              // TypeScript的编译器
├── out                         // 编译之后的输出文件夹(只有TypeScript需要，JS无需)
│   ├── src
│   |   ├── extension.js
│   |   └── extension.js.map
│   └── test
│       ├── extension.test.js
│       ├── extension.test.js.map
│       ├── index.js
│       └── index.js.map
├── package.json                // 该拓展的资源配置文件
├── tsconfig.json               // 
├── typings                     // 类型定义文件夹
│   ├── node.d.ts               // 和Node.js关联的类型定义
│   └── vscode-typings.d.ts     // 和VS Code关联的类型定义
└── vsc-extension-quickstart.md 
```

### package.json

```json
   {
    "name": "sample",              //插件扩展名称（对应创建项目时候的输入）
    "displayName": "sample",
    "description": "blog sample",  //插件扩展的描述（对应创建项目时候的输入）
    "version": "0.0.1",
    "publisher": "caipeiyu",       //发布时候的一个名称（对应创建项目时候的输入）
    "engines": {
        "vscode": "^0.10.10"
    },
    "categories": [
        "Other"
    ],
    "activationEvents": [          //这是我们要理解的地方，是触发插件执行一些代码的配置
        "onCommand:extension.sayHello" //这种是通过输入命令来触发执行的
    ],
    "main": "./out/src/extension",  //这个是配置TypeScript编译成js的输出目录
    "contributes": {
        "commands": [{             //title 和 command是一个对应关系的
            "command": "extension.sayHello", //这个是对应上面那个命令触发的，在代码里面也要用到
            "title": "Hello World"   //这个是我们在vscode里面输入的命令
        }]
    },
    "scripts": {                     //是在发布打包，或者其他运行时候，要执行的一些脚本命令
        "vscode:prepublish": "node ./node_modules/vscode/bin/compile",
        "compile": "node ./node_modules/vscode/bin/compile -watch -p ./",
        "postinstall": "node ./node_modules/vscode/bin/install"
    },
    "devDependencies": {           //这是开发的依赖包，如果有其他的依赖包，并要打包的话，需要把dev去掉
        "typescript": "^1.8.5",
        "vscode": "^0.11.0"
    }
   }

```

### extension.ts

```typescript
'use strict';
// The module 'vscode' contains the VS Code extensibility API
// Import the module and reference it with the alias vscode in your code below
import * as vscode from 'vscode';

// this method is called when your extension is activated
// your extension is activated the very first time the command is executed
export function activate(context: vscode.ExtensionContext) {

    // Use the console to output diagnostic information (console.log) and errors (console.error)
    // This line of code will only be executed once when your extension is activated
    console.log('Congratulations, your extension "sample" is now active!');

    // The command has been defined in the package.json file
    // Now provide the implementation of the command with  registerCommand
    // The commandId parameter must match the command field in package.json
    let disposable = vscode.commands.registerCommand('extension.sayHello', () => {
        //只看这个地方'extension.sayHello'和 package.json 里面的 "onCommand:extension.sayHello" 是一个对应关系
        // The code you place here will be executed every time your command is executed

        // Display a message box to the user
        vscode.window.showInformationMessage('Hello World!');
    });

    context.subscriptions.push(disposable);
    }

    // this method is called when your extension is deactivated
    export function deactivate() {
    }

```



当我们需要多增加一个命令叫做Hello Sample的，需要在package.json的文件中添加相关配置

```json
...
"activationEvents": [
			"onCommand:extension.sayHello",
			"onCommand:extension.saySample"
],
"contributes": [
			"commands": [{
						"command": "extension.sayHello",
						"title": "Hello World"
				},{
						"command": "extension.saySample",
						"title": "Hello Sample"
				}]
],
...
```

添加完成配置后，在extension.ts文件中注册命令事件

```typescript
let disposable = vscode.commands.refisterCommand('extension.sayHello', () => {
		vscode.window.showInformationMessage('Hello World!');
});

context.subscriptions.push(dispossable);

let saySample = vscode.commands.registerCommand('extension.saySample',() => {
		vscode.windows.showInformationMessage('This is a new sample command!');
});
context.subscriptions.push(saySample);
```



## 打包与发布 

在编写完成插件后，需要将它打包压缩之后才能发布到官网中。

`npm install -g vsce` 一个打包工具

cd到项目的目录下 

`vsce public`

发布成功后可以在vscode中用 `ext install`来安装这个插件。这种方法需要去配置一个token，还会过期等巴拉巴拉的问题



### 正确简便的发布方式

```
vsce package

Executing prepublish script 'node ./node_modules/vscode/bin/compile'...

Created: /sample/sample-0.0.1.vsix
```

执行完成后，在执行一个`script 'node ./node_moudles/vscode/bin/compile'`这个命令是在`package.json`

然后再配置

```json
"scripts": {
    "vscode:prepublish": "node ./node_modules/vscode/bin/compile",
    "compile": "node ./node_modules/vscode/bin/compile -watch -p ./",
    "postinstall": "node ./node_modules/vscode/bin/install"
},
```

执行之后会生成一个sample-0.0.1.vsix，这个就是打包好的插件安装包，直接拖到vscode的窗口上，就会提示安装成功重启vscode，重启之后就能使用相关命令了，在插件的目录下也多了相应的sample的目录

