# 数字类型

每当您使用任何编程语言处理数字时，您都需要了解语言如何处理数字的特性。以下是您应该了解的有关JavaScript中数字的一些重要信息。

## 核心类型

JavaScript只有一种数字类型。它是一个双精度64位数。下面我们讨论它的局限性以及推荐的解决方案。

## 十进制

对于熟悉其他语言的双精度/浮点数的人，您会知道二进制浮点数不能正确映射到十进制数。 JavaScript内置数字的一个简单（和着名）示例如下所示：

```
console.log(.1 + .2); // 0.30000000000000004
```

```
对于真十进制数学，请使用下面提到的big.js
```

## 整数

由内置数字类型表示的整数限制是Number.MAX_SAFE_INTEGER和Number.MIN_SAFE_INTEGER。

```
console.log({max: Number.MAX_SAFE_INTEGER, min: Number.MIN_SAFE_INTEGER});
// {max: 9007199254740991, min: -9007199254740991}
```

在这种情况下，安全性指的是该值不能是舍入误差的结果。 不安全的值远离这些安全值+1 / -1，任何加/减量都会使结果四舍五入。

```
console.log(Number.MAX_SAFE_INTEGER + 1 === Number.MAX_SAFE_INTEGER + 2); // true!
console.log(Number.MIN_SAFE_INTEGER - 1 === Number.MIN_SAFE_INTEGER - 2); // true!

console.log(Number.MAX_SAFE_INTEGER);      // 9007199254740991
console.log(Number.MAX_SAFE_INTEGER + 1);  // 9007199254740992 - Correct
console.log(Number.MAX_SAFE_INTEGER + 2);  // 9007199254740992 - Rounded!
console.log(Number.MAX_SAFE_INTEGER + 3);  // 9007199254740994 - Rounded - correct by luck
console.log(Number.MAX_SAFE_INTEGER + 4);  // 9007199254740996 - Rounded!
```

要检查安全性，您可以使用ES6 Number.isSafeInteger：

```
// Safe value
console.log(Number.isSafeInteger(Number.MAX_SAFE_INTEGER)); // true

// Unsafe value
console.log(Number.isSafeInteger(Number.MAX_SAFE_INTEGER + 1)); // false

// Because it might have been rounded to it due to overflow
console.log(Number.isSafeInteger(Number.MAX_SAFE_INTEGER + 10)); // false
```

```
JavaScript最终将获得BigInt支持。现在，如果你想要任意精度整数数学使用下面提到的big.js。
```

## big.js

无论何时使用数学进行财务计算（例如GST计算，带有美分的金钱，加法等），都要使用像big.js这样的库来设计 
- 完美的十进制数学 
- 安全的超出整数值

安装很简单：

```
npm install big.js @types/big.js
```

快速使用示例：

```
import { Big } from 'big.js';

export const foo = new Big('111.11111111111111111111');
export const bar = foo.plus(new Big('0.00000000000000000001'));

// To get a number:
const x: number = Number(bar.toString()); // Loses the precision
```

```
不要将此库用于用于UI /性能密集目的的数学，例如图表，画布绘图等。
```

## NaN

当某个数字计算无法用有效数字表示时，JavaScript会返回一个特殊的NaN值。一个典型的例子是虚数：

```
console.log(Math.sqrt(-1)); // NaN
```

注意：等式检查不适用于NaN值。请改用Number.isNaN：

```
// Don't do this
console.log(NaN === NaN); // false!!

// Do this
console.log(Number.isNaN(NaN)); // true
```

## 无穷

Number中可表示的值的外部边界可用作静态Number.MAX_VALUE和-Number.MAX_VALUE值。

```
console.log(Number.MAX_VALUE);  // 1.7976931348623157e+308
console.log(-Number.MAX_VALUE); // -1.7976931348623157e+308
```

不改变精度的范围之外的值被限制在这些限制范围内，例如：

```
console.log(Number.MAX_VALUE + 1 == Number.MAX_VALUE);   // true!
console.log(-Number.MAX_VALUE - 1 == -Number.MAX_VALUE); // true!
```

超出精度更改范围的值将解析为特殊值Infinity / -Infinity，例如

```
console.log(Number.MAX_VALUE + 10**1000);  // Infinity
console.log(-Number.MAX_VALUE - 10**1000); // -Infinity
```

当然，这些特殊的无穷大值也会出现需要它的算术，例如：

```
console.log( 1 / 0); // Infinity
console.log(-1 / 0); // -Infinity
```

您可以手动使用这些Infinity值，也可以使用Number类的静态成员，如下所示：

```
console.log(Number.POSITIVE_INFINITY === Infinity);  // true
console.log(Number.NEGATIVE_INFINITY === -Infinity); // true
```

幸运的是，比较运算符（</>）可靠地在无穷大值上工作：

```
console.log( Infinity >  1); // true
console.log(-Infinity < -1); // true
```

## 无穷

Number中可表示的最小非零值可用作静态Number.MIN_VALUE

```
console.log(Number.MIN_VALUE);  // 5e-324
```

小于MIN_VALUE（“下溢值”）的值将转换为0。

```
console.log(Number.MIN_VALUE / 10);  // 0
```

```
进一步的直觉：就像大于Number.MAX_VALUE的值被限制为INFINITY一样，小于Number.MIN_VALUE的值会被限制为0。
```