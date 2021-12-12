# 插件功能概述

[插件功能概述](https://code.visualstudio.com/api/extension-capabilities/overview)

## 通用功能Common Capabilities

- 注册命令、配置、键绑定或上下文菜单项。
- 存储工作区或全局数据。
- 显示通知消息。
- 使用 Quick Pick 收集用户输入。
- 打开系统文件选择器，让用户选择文件或文件夹。
- 使用 Progress API 指示长时间运行的操作

### 命令Command

插件可以：

- 使用`vscode.commands`API注册和执行命令。

```js
vscode.commands.registerCommand('', () => {...})
```

- 使用`contributes.commands`贡献点使命令面板中的命令可用

```json
{
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

### 配置Configuration

插件可以通过`contributes.configuration`贡献点贡献特定于扩展的设置，并使用`workspace.getConfigurationAPI`读取它们。

### 按键绑定Keybinding

插件可以添加自定义键绑定。在`contributes.keybindings`和键绑定主题中阅读更多内容。

### 上下文菜单Context Menu

插件可以注册自定义上下文菜单项，这些项将在右键单击时显示在 VS Code UI 的不同部分。在`contributes.menus`贡献点阅读更多信息。

### 数据存储Data Storage

有四种存储数据的选项：

- `ExtensionContext.workspaceState`：您可以在其中编写键/值对的工作区存储。VS Code 管理存储并在再次打开同一个工作区时将其恢复。
- `ExtensionContext.globalState`：一个全局存储，您可以在其中编写键/值对。VS Code 管理存储并将在每次扩展激活时恢复它。您可以通过使用`setKeysForSyncon`方法设置同步键来选择性地同步全局存储中的键/值对`globalState`。
- `ExtensionContext.storagePath`：工作区特定的存储路径，指向您的扩展程序具有读/写访问权限的本地目录。如果您需要存储只能从当前工作区访问的大文件，这是一个不错的选择。
- `ExtensionContext.globalStoragePath`：指向本地目录的全局存储路径，您的扩展具有读/写访问权限。如果您需要存储可从所有工作区访问的大文件，这是一个不错的选择。

### 显示通知Display Notifications

- window.showInformationMessage
- window.showWarningMessage
- window.showErrorMessage

### 快速选择Quick Pick

使用`vscode.QuickPick`API，您可以轻松收集用户输入或让用户从多个选项中进行选择。该QuickInput示例说明了API。

### 文件选择器File Picker

插件可以使用`vscode.window.showOpenDialog`API 打开系统文件选择器并选择文件或文件夹。

### 输出通道Output Channel

输出面板显示 的集合`OutputChannel`，非常适合用于记录目的。通过`window.createOutputChannel`API轻松利用它。

### 进度 APIProgress API

您可以使用`vscode.Progress`API 向用户报告进度更新。

可以使用以下`ProgressLocation`选项在不同位置显示进度：

- 在通知区域
- 在源代码管理视图中
- VS Code 窗口中的一般进度

## 主题化Theming

在 Visual Studio Code 中，有三种类型的主题：

- 颜色主题：从 UI 组件标识符和文本标记标识符到颜色的映射。颜色主题允许您将喜欢的颜色应用于 VS Code UI 组件和编辑器中的文本。
- 文件图标主题：从文件类型/文件名到图像的映射。文件图标显示在 VS Code UI 中的位置，例如文件资源管理器、快速打开列表和编辑器选项卡。
- 产品图标主题：一组在整个 UI 中使用的图标，从侧栏、活动栏、状态栏到编辑器字形边距

## 插件工作台Extending Workbench

工作台包括如下部分：

- 标题栏Title Bar
- 活动栏Activity Bar
- 侧边栏Side Bar
- 控制板Panel
- 编辑组Editor Group
- 状态栏Status Bar

### 视图容器Views Container

通过`contributes.viewsContainers`贡献点，您可以添加显示在五个内置视图容器旁边的新视图容器。在树视图主题中了解更多信息。

### 树视图Tree View
使用`contributes.views`贡献点，您可以添加显示在任何视图容器中的新视图。在树视图主题中了解更多信息。

### 网页浏览Webview

Webviews 是使用 HTML/CSS/JavaScript 构建的高度可定制的视图。它们显示在编辑器组区域中的文本编辑器旁边。在Webview 指南 中阅读有关 Webview 的更多信息。

### 状态栏项目Status Bar Item

插件可以创建`StatusBarItem`在状态栏中显示的自定义。状态栏项目可以显示文本和图标并在点击事件上运行命令。

- 显示文本和图标
- 单击时运行命令