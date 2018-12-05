## 快速入门Typescript

Typescript编译成Javascript。Javascript是您实际要执行的（在浏览器或服务器上）。所以你需要准备一下东西
- Typescript编译器（OSS在源代码和NPM上可以获取）
- 一个Typescript编辑器（你可以使用notepad，如果你想要使用的话。但是我使用vscode并使用了我写的一个插件。同时还有很多许多IDE也支持它）

## Typescript版本
我们将在本书中介绍很多可能与版本号无关的内容，而不是使用稳定的Typescript编辑器。我通常建议人们使用最近的版本，因为编辑器测试套件只会随着时间的推移捕获更多的错误。

你可以通过命令行安装，例如:

```
npm install -g typescript@next
```

现在命令行tsc也是最近和最棒的。各种IDE也支持它，例如:

- 你可以通过创建已下内容.vscode/settings.json来要求vscode使用此版本

```
{
  "typescript.tsdk": "./node_modules/typescript/lib"
}
```

## 获取源代码

本书的源代码可以在书籍github存储库[https://github.com/basarat/typescript-book/tree/master/code](https://github.com/basarat/typescript-book/tree/master/code),大多数代码实例都可以复制到vscode中，您可以按原样使用它们。对于需要额外设置的代码示例（例如npm模块），我们会在呈现代码之前将您连接到代码实例。例如:

this/will/be/the/link/to/the/code.ts

```
// This will be the code under discussion
```

通过开发设置，让我们跳转到Typescript语法。