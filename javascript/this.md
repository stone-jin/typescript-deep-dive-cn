## this

函数内对此关键字的任何访问实际上都是由函数实际调用的方式控制的。它通常被称为“调用上下文”。

这是一个例子:

```
function foo() {
  console.log(this);
}

foo(); // logs out the global e.g. `window` in browsers
let bar = {
  foo
}
bar.foo(); // Logs out `bar` as `foo` was called on `bar`
```

所以请注意你对this的使用。如果要在类中与调用上下文断开连接，请使用箭头函数，稍后再详细说明。