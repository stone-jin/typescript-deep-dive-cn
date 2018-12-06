## 闭包

JavaScript最好的东西是闭包。 JavaScript中的函数可以访问外部作用域中定义的任何变量。最好通过示例解释闭包：

```
function outerFunction(arg) {
    var variableInOuterFunction = arg;

    function bar() {
        console.log(variableInOuterFunction); // Access a variable from the outer scope
    }

    // Call the local function to demonstrate that it has access to arg
    bar();
}

outerFunction("hello closure"); // logs hello closure!
```

您可以看到内部函数可以从外部作用域访问变量（variableInOuterFunction）。外部函数中的变量已被内部函数关闭（或绑定）。这就是闭包。这个概念本身很简单，非常直观。 现在很棒的部分：即使在返回外部函数之后，内部函数也可以从外部作用域访问变量。这是因为变量仍然绑定在内部函数中，而不依赖于外部函数。让我们看一个例子：

```
function outerFunction(arg) {
    var variableInOuterFunction = arg;
    return function() {
        console.log(variableInOuterFunction);
    }
}

var innerFunction = outerFunction("hello closure!");

// Note the outerFunction has returned
innerFunction(); // logs hello closure!
```

## 它之所以令人敬畏的原因

它允许您轻松地组合对象，例如揭示模块模式：

```
function createCounter() {
    let val = 0;
    return {
        increment() { val++ },
        getVal() { return val }
    }
}

let counter = createCounter();
counter.increment();
console.log(counter.getVal()); // 1
counter.increment();
console.log(counter.getVal()); // 2
```

在较高的层次上，它也可以使像Node.js这样的东西成为可能（不要担心，如果它现在没有点击你的大脑。它最终会🌹）：

```
// Pseudo code to explain the concept
server.on(function handler(req, res) {
    loadData(req.id).then(function(data) {
        // the `res` has been closed over and is available
        res.send(data);
    })
});
```