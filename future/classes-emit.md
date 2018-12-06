# 类事件

## IIFE有什么用

为该类生成的js可能是：

```
function Point(x, y) {
    this.x = x;
    this.y = y;
}
Point.prototype.add = function (point) {
    return new Point(this.x + point.x, this.y + point.y);
};
```

它被包含在立即调用的函数表达式（IIFE, Immediately-Invoked Function Expression）中的原因，即:

```
(function () {

    // BODY

    return Point;
})();
```

与继承有关。它允许TypeScript将基类捕获为变量_super，例如

```
var Point3D = (function (_super) {
    __extends(Point3D, _super);
    function Point3D(x, y, z) {
        _super.call(this, x, y);
        this.z = z;
    }
    Point3D.prototype.add = function (point) {
        var point2D = _super.prototype.add.call(this, point);
        return new Point3D(point2D.x, point2D.y, this.z + point.z);
    };
    return Point3D;
})(Point);
```
请注意，IIFE允许TypeScript轻松捕获_super变量中的基类Point，并在类体中一致地使用。

## __extends

您会注意到，只要继承了一个类，TypeScript也会生成以下函数：
```
var __extends = this.__extends || function (d, b) {
    for (var p in b) if (b.hasOwnProperty(p)) d[p] = b[p];
    function __() { this.constructor = d; }
    __.prototype = b.prototype;
    d.prototype = new __();
};
```

## d.prototype.__proto__ = b.prototype
在辅导了很多人之后我发现以下解释是最简单的。首先，我们将解释__extends中的代码如何等同于简单的d.prototype .__ proto__ = b.prototype，然后为什么这一行本身就很重要。要了解这一切，您需要了解以下事项：

1.__proto__
2.prototype
3.new对此函数内部的影响
4.new 对 prototype 和 __proto__的影响

JavaScript中的所有对象都包含__proto__成员。在旧版浏览器中通常无法访问此成员（有时文档将此神奇属性称为[[prototype]]）。它有一个目标：如果在查找期间在对象上找不到属性（例如obj.property），则会查找obj.__proto__.property。如果仍未找到，则obj.__proto__.__proto__.property直到：找到它或者最新的.__ proto__本身为空。这解释了为什么JavaScript被认为支持开箱即用的原型继承。这在以下示例中显示，您可以在chrome控制台或Node.js中运行它：

```
var foo = {}

// setup on foo as well as foo.__proto__
foo.bar = 123;
foo.__proto__.bar = 456;

console.log(foo.bar); // 123
delete foo.bar; // remove from object
console.log(foo.bar); // 456
delete foo.__proto__.bar; // remove from foo.__proto__
console.log(foo.bar); // undefined
```

很酷，所以你明白__proto__。另一个有用的事实是JavaScript中的所有函数都有一个名为prototype的属性，并且它有一个指向函数的成员构造函数。如下所示：

```
function Foo() { }
console.log(Foo.prototype); // {} i.e. it exists and is not undefined
console.log(Foo.prototype.constructor === Foo); // Has a member called `constructor` pointing back to the function
```

现在让我们看一下new对这个被调用函数内部的影响。基本上，在被调用函数内部将指向将从函数返回的新创建的对象。很容易看出你是否在函数内部改变了一个属性：

```
function Foo() {
    this.bar = 123;
}

// call with the new operator
var newFoo = new Foo();
console.log(newFoo.bar); // 123
```

现在唯一需要知道的是，在函数上调用new会将函数原型分配给函数调用返回的新创建对象的__proto__。以下是您可以运行以完全理解它的代码：

```
function Foo() { }

var foo = new Foo();

console.log(foo.__proto__ === Foo.prototype); // True!
```

仅此而已。现在看看__extends中的以下内容。我冒昧地为这些行编号：

```
1  function __() { this.constructor = d; }
2   __.prototype = b.prototype;
3   d.prototype = new __();
```

反过来读取这个函数第3行的d.prototype = new __ （）实际上意味着d.prototype = { __ proto__：__.prototype}（因为new对原型和__proto__的影响），将它与前一行结合（即第2行__。原型= b.prototype;）你得到d.prototype = {__ proto__：b.prototype}。 但是等等，我们想要d.prototype .__ proto__，即只更改proto并维护旧的d.prototype.constructor。这就是第一行（即函数__（）{this.constructor = d;}）的重要性所在。这里我们将有效地拥有d.prototype = {__ proto ___ ： __。原型，构造函数：d}（因为new对此内部调用函数的影响）。因此，既然我们恢复了d.prototype.constructor，我们唯一真正变异的就是__proto__，因此d.prototype .__ proto__ = b.prototype。

## d.prototype.__proto__ = b.prototype的意义

重要的是它允许您将成员函数添加到子类并从基类继承其他函数。以下简单示例演示了这一点：

```
function Animal() { }
Animal.prototype.walk = function () { console.log('walk') };

function Bird() { }
Bird.prototype.__proto__ = Animal.prototype;
Bird.prototype.fly = function () { console.log('fly') };

var bird = new Bird();
bird.walk();
bird.fly();
```

基本上bird.fly将从bird.__ proto__.fly中查找（记住新的鸟类.__ proto__指向Bird.prototype）和bird.walk（一个继承的成员）将从鸟类中查找.__ proto __ .__ proto __ 。walk （如鸟.__ proto__ == Bird.prototype和bird .__ proto __ .__ proto__ == Animal.prototype）。
