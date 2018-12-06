# Null对比Undefined

JavaScript（以及扩展名为TypeScript）有两种底层类型：null和undefined。它们意味着不同的东西：

- 尚未初始化的：undefined。 
- 无法使用的对象：null。

## 检查是否相等

事实是你需要同时处理两者。只需检查==检查。

```
/// Imagine you are doing `foo.bar == undefined` where bar can be one of:
console.log(undefined == undefined); // true
console.log(null == undefined); // true

// You don't have to worry about falsy values making through this check
console.log(0 == undefined); // false
console.log('' == undefined); // false
console.log(false == undefined); // false
```

建议== null检查undefined或null。你通常不想区分这两者。

```
function foo(arg: string | null | undefined) {
  if (arg != null) {
    // arg must be a string as `!=` rules out both null and undefined. 
  }
}
```

一个例外，我们接下来讨论的根级未定义值。

## 注意root level undefined

记住我怎么说你应该使用== null。你当然会这样做（因为我只是说了^）。不要将它用于根级别的东西。在严格模式下，如果你使用foo和foo是未定义的，你会得到一个ReferenceError异常，整个调用堆栈都会展开。

```
你应该使用严格模式......事实上，如果你使用模块，TS编译器会为你插入...更多关于本书后面的内容，所以你不必明白它:)
```

因此，要检查是否在全局级别定义了变量，通常使用typeof：

```
if (typeof someglobal !== 'undefined') {
  // someglobal is now safe to use
  console.log(someglobal);
}
```

## 限制显式使用undefined

因为TypeScript让您有机会将值与值分开记录，而不是像以下那样：

```
function foo(){
  // if Something
  return {a:1,b:2};
  // else
  return {a:1,b:undefined};
}
```

你应该使用类型注释：

```
function foo():{a:number,b?:number}{
  // if Something
  return {a:1,b:2};
  // else
  return {a:1};
}
```

## Node 格式的回调
Node 格式的回调函数(例如(err,somethingElse)=>{ /* something */ }) )， 如果没有错误，通常调用err设置为null。你通常只是使用truthy检查：

```
fs.readFile('someFile', 'utf8', (err,data) => {
  if (err) {
    // do something
  } else {
    // no error
  }
});
```

在创建自己的API时，可以在这种情况下使用null来保持一致性。对于你自己的API的所有诚意，你应该看看promises，在这种情况下，你实际上不需要打扰缺少错误值（你用.then与.catch处理它们）。

## 不要使用undefined作为表示有效性的手段

例如，像这样的可怕函数:

```
function toInt(str:string) {
  return str ? parseInt(str) : undefined;
}
```

可以这样写得更好：

```
function toInt(str: string): { valid: boolean, int?: number } {
  const int = parseInt(str);
  if (isNaN(int)) {
    return { valid: false };
  }
  else {
    return { valid: true, int };
  }
}
```

## 最后的想法

TypeScript团队不使用null：TypeScript编码指南并没有引起任何问题。 Douglas Crockford认为null是一个坏主意，我们都应该使用undefined。 但是，NodeJS样式代码库使用null作为标准的Error参数，因为它表示Something当前不可用。我个人并不在意区分这两者，因为大多数项目都使用具有不同意见的库，并且只排除了== null。
