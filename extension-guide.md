# 插件指南

## 命令Commands

触发vscode中的操作

### 使用命令Using Commands

VS Code 包含大量内置命令，可用于与编辑器交互、控制用户界面或执行后台操作。许多插件还将其核心功能公开为用户和其他扩展可以利用的命令。

#### 以编程方式执行命令Programmatically executing a command

执行命令`vscode.commands.executeCommand`：

```js
import * as vscode from 'vscode';

vscode.commands.executeCommand('editor.action.addCommentLine');
```

执行命令，传入参数并返回结果`vscode.executeDefinitionProvider`：

```js
import * as vscode from 'vscode';

async function printDefinitionsForActiveEditor() {
  const activeEditor = vscode.window.activeTextEditor;
  if (!activeEditor) {
    return;
  }

  const definitions = await vscode.commands.executeCommand<vscode.Location[]>(
    'vscode.executeDefinitionProvider',
    activeEditor.document.uri,
    activeEditor.selection.active
  );

  for (const definition of definitions) {
    console.log(definition);
  }
}
```

### 内置命令Built-in Commands

- vscode.executeDocumentHighlights - 执行文档突出显示提供程序。
- vscode.executeDocumentSymbolProvider - 执行文档符号提供程序。
- vscode.executeFormatDocumentProvider - 执行文档格式提供程序。
- vscode.executeFormatRangeProvider - 执行范围格式提供程序。
- vscode.executeFormatOnTypeProvider - 在类型提供程序上执行格式。
- vscode.executeDefinitionProvider - 执行所有定义提供程序。
- vscode.executeTypeDefinitionProvider - 执行所有类型定义提供程序。
- vscode.executeDeclarationProvider - 执行所有声明提供程序。
- vscode.executeImplementationProvider - 执行所有实施提供者。
- vscode.executeReferenceProvider - 执行所有参考提供者。
- vscode.executeHoverProvider - 执行所有悬停提供程序。
- vscode.executeSelectionRangeProvider - 执行选择范围提供程序。
- vscode.executeWorkspaceSymbolProvider - 执行所有工作区符号提供程序。
- vscode.prepareCallHierarchy - 在文档内的某个位置准备调用层次结构
- vscode.provideIncomingCalls - 计算一个项目的来电
- vscode.provideOutgoingCalls - 计算一个项目的呼出
- vscode.executeDocumentRenameProvider - 执行重命名提供程序。
- vscode.executeLinkProvider - 执行文档链接提供程序。
- vscode.provideDocumentSemanticTokensLegend - 为文档提供语义标记图例
- vscode.provideDocumentSemanticTokens - 为文档提供语义标记
- vscode.provideDocumentRangeSemanticTokensLegend - 为文档范围提供语义标记图例
- vscode.provideDocumentRangeSemanticTokens - 为文档范围提供语义标记
- vscode.executeCompletionItemProvider - 执行完成项提供程序。
- vscode.executeSignatureHelpProvider - 执行签名帮助提供者。
- vscode.executeCodeLensProvider - 执行代码镜头提供程序。
- vscode.executeCodeActionProvider - 执行代码操作提供程序。
- vscode.executeDocumentColorProvider - 执行文档颜色提供程序。
- vscode.executeColorPresentationProvider - 执行颜色演示提供程序。
- vscode.executeInlayHintProvider - 执行镶嵌提示提供者
- vscode.resolveNotebookContentProviders - 解决笔记本内容提供者
- vscode.openWith - 使用特定编辑器打开提供的资源。
- vscode.diff - 在差异编辑器中打开提供的资源以比较它们的内容。
- vscode.removeFromRecentlyOpened - 从最近打开的列表中删除具有给定路径的条目。
- vscode.openIssueReporter - 使用提供的扩展 ID 作为选定源打开问题报告器
- cursorMove - 将光标移动到视图中的逻辑位置
- editorScroll - 在给定方向滚动编辑器
- revealLine - 在给定的逻辑位置显示给定的行
- editor.unfold - 在编辑器中展开内容
- editor.fold - 在编辑器中折叠内容
- editor.action.goToLocations - 从文件中的某个位置转到位置
- editor.action.peekLocations - 从文件中的某个位置查看位置
- workbench.action.quickOpen - 快速访问
- moveActiveEditor - 按选项卡或组移动活动编辑器
- vscode.setEditorLayout - 设置编辑器布局。
- notebook.cell.execute - 执行单元格
- notebook.cell.cancelExecution - 停止单元格执行
- notebook.execute - 执行笔记本
- notebook.cancelExecution - 取消笔记本执行
- notebook.fold - 折叠单元格
- notebook.unfold - 展开细胞
- notebook.selectKernel - 笔记本内核参数
- search.action.openNewEditor- 打开一个新的搜索编辑器。传递的参数可以包括像 ${relativeFileDirname} 这样的变量。
- search.action.openEditor- 打开一个新的搜索编辑器。传递的参数可以包括像 ${relativeFileDirname} 这样的变量。
- search.action.openNewEditorToSide- 打开一个新的搜索编辑器。传递的参数可以包括像 ${relativeFileDirname} 这样的变量。
- vscode.openFolder- 根据 newWindow 参数在当前窗口或新窗口中打开文件夹或工作区。请注意，除非 newWindow 参数设置为 true，否则在同一窗口中打开将关闭当前扩展主机进程并在给定文件夹/工作区上启动一个新进程。
- workbench.extensions.installExtension - 安装给定的扩展
- workbench.extensions.uninstallExtension - 卸载给定的扩展
- workbench.extensions.search - 搜索特定的扩展
- workbench.action.files.newUntitledFile - 新的无标题文件
- workbench.action.findInFiles - 打开搜索视图

#### 命令 URI

### 创建命令

#### 注册命令Registering a command

`vscode.commands.registerCommand`:将命令 ID 绑定到插件中的处理程序函数

```js
import * as vscode from 'vscode';

export function activate(context: vscode.ExtensionContext) {
  const command = 'myExtension.sayHello';

  const commandHandler = (name: string = 'world') => {
    console.log(`Hello ${name}!!!`);
  };

  context.subscriptions.push(vscode.commands.registerCommand(command, commandHandler));
}
```

#### 创建一个面向用户的命令Creating a user facing command

`vscode.commands.registerCommand`仅将命令 ID 绑定到处理程序函数。要在命令面板中公开此命令以便用户可以发现它，您还需要`contribution`在插件的`package.json`:

```json
{
  "contributes": {
    "commands": [
      {
        "command": "myExtension.sayHello",
        "title": "Say Hello"
      }
    ]
  }
}
```
命令的贡献点，是告诉vscode，插件提供了一个给定的命令，让用户在命令面板中调用