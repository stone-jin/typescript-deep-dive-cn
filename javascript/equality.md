## Equality

在JavaScript中要注意的一件事是==和===之间的区别。当JavaScript试图对错误的编程模式具有弹性，==尝试在两个变量之间进行类型强制，例如将字符串转换为数字，以便您可以与数字进行比较，如下所示：

```
console.log(5 == "5"); // true   , TS Error
console.log(5 === "5"); // false , TS Error
```

但是，JavaScript的选择并不总是理想的。例如，在下面的示例中，第一个语句为false，因为“”和“0”都是字符串，显然不相等。然而，在第二种情况下，0和空字符串（“”）都是false的（即表现得像假），因此相对于==是相等的。使用===时，这两个语句都是错误的。

```
console.log("" == "0"); // false
console.log(0 == ""); // true

console.log("" === "0"); // false
console.log(0 === ""); // false
```

```
请注意，string == number和string === number都是TypeScript中的编译时错误，因此您通常不必担心这一点。
```

类似于== vs. ===，有！= vs.！== 所以小知识：总是使用===和！==除了空检查，我们稍后会介绍。

## 结构对比

如果要比较两个对象的结构相等性== / ===是不够的。例如

```
console.log({a:123} == {a:123}); // False
console.log({a:123} === {a:123}); // False
```

要进行此类检查，请使用深度相等的npm包，例如

```
import * as deepEqual from "deep-equal";

console.log(deepEqual({a:123},{a:123})); // True
```

但是，通常你不需要深度检查，所有你真正需要的是检查一些id，例如

```
type IdDisplay = {
  id: string,
  display: string
}
const list: IdDisplay[] = [
  {
    id: 'foo',
    display: 'Foo Select'
  },
  {
    id: 'bar',
    display: 'Bar Select'
  },
]

const fooIndex = list.map(i => i.id).indexOf('foo');
console.log(fooIndex); // 0
```
