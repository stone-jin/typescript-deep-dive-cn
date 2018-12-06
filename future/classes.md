# 类

将JavaScript中的类作为第一类项, 重要的原因是：
1. 类提供了有用的结构抽象 
2. 为开发人员提供一种一致的方式来使用类而不是每个框架（emberjs，reactjs等）提供他们自己的版本。3. 面向对象的开发人员已经了解了类。

最后JavaScript开发人员可以拥有 class 这个特性。这里我们有一个名为Point的基本类：

```
class Point {
    x: number;
    y: number;
    constructor(x: number, y: number) {
        this.x = x;
        this.y = y;
    }
    add(point: Point) {
        return new Point(this.x + point.x, this.y + point.y);
    }
}

var p1 = new Point(0, 10);
var p2 = new Point(10, 20);
var p3 = p1.add(p2); // {x:10,y:30}
```

该类在ES5生成时JavaScript：
```
var Point = (function () {
    function Point(x, y) {
        this.x = x;
        this.y = y;
    }
    Point.prototype.add = function (point) {
        return new Point(this.x + point.x, this.y + point.y);
    };
    return Point;
})();
```

这是一个相当惯用的传统JavaScript类模式，现在作为第一类语言构造。

## 继承

TypeScript中的类（与其他语言一样）使用extends关键字支持单继承，如下所示：

```
class Point3D extends Point {
    z: number;
    constructor(x: number, y: number, z: number) {
        super(x, y);
        this.z = z;
    }
    add(point: Point3D) {
        var point2D = super.add(point);
        return new Point3D(point2D.x, point2D.y, this.z + point.z);
    }
}
```

如果你的类中有一个构造函数，那么你必须从构造函数中调用父构造函数（TypeScript会指出这一点）。这可以确保设置它的东西得到设置。接下来是对super的调用，你可以在构造函数中添加你想要做的任何其他东西（这里我们添加另一个成员z）。 请注意，您可以轻松覆盖父成员函数（这里我们覆盖add），并且仍然使用成员中超类的功能（使用super这个语法）。

## 静态

TypeScript类支持由类的所有实例共享的静态属性。放置（和访问）它们的自然位置在类本身上，这就是TypeScript所做的：

```
class Something {
    static instances = 0;
    constructor() {
        Something.instances++;
    }
}

var s1 = new Something();
var s2 = new Something();
console.log(Something.instances); // 2
```

您可以拥有静态成员以及静态函数。

## 访问修饰符

TypeScript支持访问修饰符public，private和protected，它们决定了类成员的可访问性，如下所示：

| 可获取性 | public |  protected | private |
| --- | --- | --- | --- |
| class | yes | yes | yes |
| class children | yes | yes | no |
| class instances | yes | no | no | 

如果未指定访问修饰符，则它是隐式公共的，因为它符合JavaScript的方便性。 请注意，在运行时（在生成的JS中），这些没有任何意义，但如果您错误地使用它们，则会给出编译时错误。每个示例如下所示：

```
class FooBase {
    public x: number;
    private y: number;
    protected z: number;
}

// EFFECT ON INSTANCES
var foo = new FooBase();
foo.x; // okay
foo.y; // ERROR : private
foo.z; // ERROR : protected

// EFFECT ON CHILD CLASSES
class FooChild extends FooBase {
    constructor() {
      super();
        this.x; // okay
        this.y; // ERROR: private
        this.z; // okay
    }
}
```

与往常一样，这些修饰符适用于成员属性和成员函数。

## 抽象

abstract可以被认为是一个访问修饰符。我们单独呈现它，因为与前面提到的修饰符相反，它可以在类以及类的任何成员上。具有抽象修饰符主要意味着不能直接调用此类功能，并且子类必须提供该功能。 抽象类不能直接实例化。相反，用户必须创建一些继承自抽象类的类。 无法直接访问抽象成员，子类必须提供功能。

## 构造函数是可选的

该类不需要具有构造函数。例如以下是完全正常的。

```
class Foo {}
var foo = new Foo();
```

## 使用构造函数定义

在一个类中有一个成员并按如下所示进行初始化：

```
class Foo {
    x: number;
    constructor(x:number) {
        this.x = x;
    }
}
```

是一种常见的模式，TypeScript提供了一种速记，您可以在其中为成员添加访问修饰符前缀，并在类上自动声明并从构造函数中复制。所以前面的例子可以重写为（注意公共x：数字）：

```
class Foo {
    constructor(public x:number) {
    }
}
```

## 属性初始化器

这是TypeScript支持的一个漂亮的功能（实际上来自ES7）。您可以在类构造函数之外初始化类的任何成员，对提供默认值很有用（notice members = []）

```
class Foo {
    members = [];  // Initialize directly
    add(x) {
        this.members.push(x);
    }
}
```