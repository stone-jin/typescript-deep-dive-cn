# 为什么要使用Typescript

Typescript主要有两个主要目标:

- 为Javascript提供可选的类型系统。
- 提供从未来Javascript版本到当前Javascript引擎。

以下是对这些目标的渴望。

## Typescript类型系统

您可能想知道“为什么要向JavaScript添加类型？” 

类型已证明能够提高代码质量和可理解性。大型团队（谷歌，微软，Facebook）不断得出这一结论。特别： 

- 在进行重构时，类型可以提高您的敏捷性。编译器捕获错误比在运行时失败更好。
- 类型是您可以拥有的最佳文档形式之一。函数签名是一个定义，函数体是证明。

然而，类型有一种不必要的隆重的方式。 TypeScript非常讲究保持入门门槛尽可能低。这是如何做：

### 您的JavaScript是TypeScript
TypeScript为JavaScript代码提供编译时类型安全性。鉴于它的名字，这并不奇怪。最棒的是这些类型是完全可选的。您的JavaScript代码.js文件可以重命名为.ts文件，TypeScript仍然会返回与原始JavaScript文件等效的有效.js文件。 TypeScript是有意并严格的，它是JavaScript的超集，具有可选的类型检查。

### 类型可以是隐含的
TypeScript将尝试尽可能多地推断出类型信息，以便在代码开发过程中以最小的生产成本为您提供类型安全性。例如，在下面的示例中，TypeScript将知道foo的类型是number，如下，并将在第二行显示错误，如下所示：

```
var foo = 123;
foo = '456'; // Error: cannot assign `string` to `number`

// Is foo a number or a string?
```

这种类型的推断很有动力。如果您执行此示例中显示的内容，则在其余代码中，您无法确定foo是数字还是字符串。这些问题经常出现在大型多文件代码库中。我们稍后将深入探讨类型推断规则。

### 类型可以是显式的

正如我们之前提到的，TypeScript将尽可能安全地推断出它。但是，您可以使用注释： 

1. 帮助编译器，更重要的是为下一个必须阅读代码的开发人员记录内容（可能是未来的你！）。
2. 强制执行编译器看到的内容，就是您认为应该看到的内容。这就是您对代码的理解与代码的算法分析（由编译器完成）相匹配。 

TypeScript使用在其他可选带注释的语言（例如ActionScript和F＃）中流行的后缀类型注释。

```
var foo: number = 123;
```

所以，如果你做错了什么，编译器就会出错，例如：

```
var foo: number = '123'; // Error: cannot assign a `string` to a `number`
```

我们将在后面的章节中讨论TypeScript支持的所有注释语法的所有细节。

### 类型是结构性的
在某些语言（特别是强制类型的语言）中，静态类型会导致不必要的仪式，因为即使您知道代码可以正常工作，语言语义也会迫使您复制内容。这就是为什么像C＃的automapper这样的东西对于C＃来说至关重要。在TypeScript中，因为我们真的希望它对于具有最小认知过载的JavaScript开发人员来说很容易，所以类型是结构性的。这意味着typescript是一流的语言构造。请考虑以下示例。函数iTakePoint2D将接受包含它所需的所有东西（x和y）的任何东西：

```
interface Point2D {
    x: number;
    y: number;
}
interface Point3D {
    x: number;
    y: number;
    z: number;
}
var point2D: Point2D = { x: 0, y: 10 }
var point3D: Point3D = { x: 0, y: 10, z: 20 }
function iTakePoint2D(point: Point2D) { /* do something */ }

iTakePoint2D(point2D); // exact match okay
iTakePoint2D(point3D); // extra information okay
iTakePoint2D({ x: 0 }); // Error: missing information `y`
```

### 类型错误不会阻止JavaScript发出

为了便于您将JavaScript代码迁移到TypeScript，即使存在编译错误，默认情况下TypeScript也会尽可能地发出有效的JavaScript。例如

```
var foo = 123;
foo = '456'; // Error: cannot assign a `string` to a `number`
```

会发报错在下面的js中.

```
var foo = 123;
foo = '456';
```

因此，您可以将JavaScript代码逐步升级为TypeScript。这与其他语言编译器的工作方式有很大不同，也是转向TypeScript的另一个原因。

### 类型可以是环境

TypeScript的主要设计目标是使您能够安全，轻松地使用TypeScript中的现有JavaScript库。 TypeScript通过声明来做到这一点。 TypeScript为您提供了一个平滑过渡，表明您希望在声明中投入不多的努力，您将更多的努力放在您获得的类型安全+代码智能上。请注意，大多数流行的JavaScript库的定义已经由DefinitelyTyped社区为您编写，因此对于大多数用途： 

1. 定义文件已存在。 
2. 或者至少，您已经拥有大量经过深思熟虑的TypeScript声明模板列表 

作为如何编写自己的声明文件的快速示例，请看jquery的一个简单示例。默认情况下（正如预期的良好JS代码）TypeScript要求您在使用变量之前声明（即在某处使用var）

```
$('.awesome').show(); // Error: cannot find name `$`
```

作为一个快速修复，你可以告诉TypeScript确实存在一个名为$的东西：

```
declare var $: any;
$('.awesome').show(); // Okay!
```

如果您愿意，可以基于此基本定义构建并提供更多信息以帮助保护您免受错误的影响：

```
declare var $: {
    (selector:string): any;
};
$('.awesome').show(); // Okay!
$(123).show(); // Error: selector needs to be a string
```

稍后我们将详细讨论为现有JavaScript创建TypeScript定义的详细信息，一旦您了解了有关TypeScript的更多信息（例如接口和任何内容之类的东西）。

### 未来的JavaScript =>现在

TypeScript提供了许多针对当前JavaScript引擎（仅支持ES5等）在ES6中规划的功能。 TypeScript团队正在积极地添加这些功能，这个列表只会随着时间的推移而变大，我们将在自己的章节中介绍。但就像这里的一个标本是一个类的例子：

```
class Point {
    constructor(public x: number, public y: number) {
    }
    add(point: Point) {
        return new Point(this.x + point.x, this.y + point.y);
    }
}

var p1 = new Point(0, 10);
var p2 = new Point(10, 20);
var p3 = p1.add(p2); // { x: 10, y: 30 }
```

和可爱的箭头函数功能：

```
var inc = x => x+1;
```

## 结论

在本节中，我们为您提供了TypeScript的动机和设计目标。通过这种方式，我们可以深入了解TypeScript的细节。