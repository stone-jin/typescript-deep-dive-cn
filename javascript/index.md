## 你的JavaScript是TypeScript

在JavaScript编译器的一些语法中有（并将继续）许多竞争对手。 TypeScript与它们的不同之处在于您的JavaScript是TypeScript。这是一个图表：

![](https://raw.githubusercontent.com/basarat/typescript-book/master/images/venn.png)

但是，它确实意味着你需要学习JavaScript（好消息是你只需要学习JavaScript）。 TypeScript只是标准化您提供有关JavaScript的良好文档的所有方法。 

- 给你一个新的语法无法帮助修复错误（跟CoffeeScript一样）。
- 创建一种新语言让您离运行时更接近，社区（跟Dart一样)。

TypeScript只是带有文档的JavaScript。

## 让JavaScript变得更好

TypeScript将尝试保护您免受从未运行的JavaScript部分的影响（因此您不需要记住这些内容）：

```
[] + []; // JavaScript will give you "" (which makes little sense), TypeScript will error

//
// other things that are nonsensical in JavaScript
// - don't give a runtime error (making debugging hard)
// - but TypeScript will give a compile time error (making debugging unnecessary)
//
{} + []; // JS : 0, TS Error
[] + {}; // JS : "[object Object]", TS Error
{} + {}; // JS : NaN or [object Object][object Object] depending upon browser, TS Error
"hello" - 1; // JS : NaN, TS Error

function add(a,b) {
  return
    a + b; // JS : undefined, TS Error 'unreachable code detected'
}
```

基本上TypeScript是带校验的 JavaScript。只是比没有类型信息的其他语言做得更好。

## 你仍然需要学习JavaScript

那就是说TypeScript非常务实，因为你确实编写了JavaScript，因此有一些关于JavaScript的东西你仍然需要知道才能不被猝不及防。我们接下来再讨论它们。

```
注意：TypeScript是JavaScript的超集。只是编译器/ IDE实际可以使用的文档;）
```

