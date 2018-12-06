## 引用

除了文字之外，JavaScript中的任何对象（包括函数，数组，正则表达式等）都是引用。这意味着以下内容:


## 引用传递是可变的

```
var foo = {};
var bar = foo; // bar is a reference to the same object

foo.baz = 123;
console.log(bar.baz); // 123
```

## 引用是相等的

```
var foo = {};
var bar = foo; // bar is a reference
var baz = {}; // baz is a *new object* distinct from `foo`

console.log(foo === bar); // true
console.log(foo === baz); // false
```