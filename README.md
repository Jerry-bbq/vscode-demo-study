# vscode-demo-study

记录学习vscode插件，版本为v1.63

## Extension API

extension api又叫插件api,vscode的每个部分都可以通过插件api进行定制和增强，vscode的许多核心功能都是作为插件构建的，并使用插件api。

[vscode插件市场](https://marketplace.visualstudio.com/vscode)

## 开始

### 第一个插件

安装Yeoman和VS Code Extension Generator:

```bash
npm install -g yo generator-code
```

使用generator搭建一个TS项目:

```bash
yo code
```

```bash
?==========================================================================
We're constantly looking for ways to make yo better! 
May we anonymously report usage statistics to improve the tool over time? 
More info: https://github.com/yeoman/insight & http://yeoman.io
========================================================================== Yes

     _-----_     ╭──────────────────────────╮
    |       |    │   Welcome to the Visual  │
    |--(o)--|    │   Studio Code Extension  │
   `---------´   │        generator!        │
    ( _´U`_ )    ╰──────────────────────────╯
    /___A___\   /
     |  ~  |     
   __'.___.'__   
 ´   `  |° ´ Y ` 

? What type of extension do you want to create? New Extension (TypeScript)
? What's the name of your extension? HelloWorld
? What's the identifier of your extension? helloworld
? What's the description of your extension? this is a demo for extension
? Initialize a git repository? No
? Bundle the source code with webpack? No
? Which package manager to use? yarn
```

调试

- mac按`fn+F5`进行调试，vscode编辑器会新开一个窗口
- 在新窗口中，打开命令面板（`⇧⌘P`）,运行`Hello World`命令
- 会看到Hello World from HelloWorld!显示的通知
- 更改消息，在新窗口中运行`Developer: Reload Window`,再次运行`Hello World`
- 可以在浏览器中直接打断点进行调试

### 解析

文件结构：

```json
.
├── .vscode
│   ├── launch.json     // Config for launching and debugging the extension
│   └── tasks.json      // Config for build task that compiles TypeScript
├── .gitignore          // Ignore build output and node_modules
├── README.md           // Readable description of your extension's functionality
├── src
│   └── extension.ts    // Extension source code
├── package.json        // Extension manifest
├── tsconfig.json       // TypeScript configuration
```

- `launch.json`用于配置 VS Code Debugging
- `tasks.json`用于定义 VS Code任务
- `tsconfig.json`查阅 TypeScript手册
- `package.json`，插件的描述清单

```json
{
  // 激活事件
  "activationEvents": [
        "onCommand:helloworld.helloWorld"
	],
  // 插件入口
	"main": "./out/extension.js",
  // 贡献点  
	"contributes": {
		"commands": [
			{
				"command": "helloworld.helloWorld",
				"title": "Hello World"
			}
		]
	}
}
```
- `extension.ts`，插件的入口文件

```ts
import * as vscode from 'vscode';

export function activate(context: vscode.ExtensionContext) {
	let disposable = vscode.commands.registerCommand('helloworld.helloWorld', () => {
		vscode.window.showInformationMessage('Hello World!');
	});

	context.subscriptions.push(disposable);
}

export function deactivate() {}

```

`Hello World`插件做3件事情：

- 注册激活事件: 因此当用户运行命令时扩展被激活。onCommand onCommand:helloworld.helloWorldHello World
- 使用贡献点使命令在命令面板中可用，并将其绑定到命令 ID 。contributes.commands Hello Worldhelloworld.helloWorld
- 使用VS Code API将函数绑定到注册的命令 ID 。commands.registerCommand helloworld.helloWorld

## 相关文档

- [插件api文档](https://code.visualstudio.com/api)
- [官方代码示例](https://github.com/microsoft/vscode-extension-samples)
- [参考文献](https://liiked.github.io/VS-Code-Extension-Doc-ZH/#/)