# 可靠性

JavaScript有一个truthy概念，即在某些位置评估为true的东西（例如，if条件 和 布尔&& ||运算符）。以下是JavaScript中的真实内容。一个例子是除0之外的任何数字，例如

```
if (123) { // Will be treated like `true`
  console.log('Any number other than 0 is truthy');
}
```

不真实的东西叫做 falsy(不对的)。

 这是一个方便的表供您参考。

 | 变量类型 | 什么时候是否 | 什么时候是是 |
 |------|-----------|------|
 | boolean | false | true |
 | string | ''（空字符串） | 其他类型的字符串 |
 | number | 0 NaN | 其他数字 |
 | null | always | never |
 | undefined | always | never|
 | 其他任何其他的对象如 {}, [] | never | always |
 
 ## 明确

 ```
!! 语法
 ```

 通常，有必要明确的是，意图是将值视为布尔值并将其转换为真正的布尔值（true | false之一）。您可以通过在其前面添加!!来轻松地将值转换为true布尔值！例如！FOO。只是 ！使用了两次。首先 ！将变量（在本例中为foo）转换为布尔值但反转逻辑（truthy - ！> false，falsy - ！> true）。第二个再次切换它以匹配原始对象的性质（例如truthy - ！> false - ！> true）。 在许多地方使用这种模式是很常见的，例如

 ```
 // Direct variables
const hasName = !!name;

// As members of objects
const someObj = {
  hasName: !!name
}

// e.g. in ReactJS JSX
{!!someName && <div>{someName}</div>}
 ```