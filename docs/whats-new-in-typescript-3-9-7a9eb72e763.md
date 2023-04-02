# TypeScript 3.9 有什么新功能？

> 原文：<https://javascript.plainenglish.io/whats-new-in-typescript-3-9-7a9eb72e763?source=collection_archive---------9----------------------->

![](img/bd339761b1c30cd0e653f45fc460b5a3.png)

Photo by [JOHN TOWNER](https://unsplash.com/@heytowner?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

TypeScript 是一种扩展 JavaScript 的语言，使编写 JavaScript 应用程序变得更容易。

TypeScript 3.9 带来了我们都能从中受益的新特性。

在本文中，我们将研究 TypeScript 3.9 的新特性。

# 推断承诺的实现结果

接受一组承诺，并解析出所有承诺的实现值。

它们会在一个数组里。

TypeScript 在推断结果类型时存在问题。

例如，如果我们有:

```
interface Person {
  speak(): void
}interface Dog {
  bark(): void
}async function speak(person: Promise<Person>, dog: Promise<Dog | undefined>) {
  const [person, dog] = await Promise.all([person, dog]);
  person.speak();
}
```

那么我们将得到“对象可能未定义”的错误，即使`Person`总是被定义为由类型参数指定的。

由于`dog`的承诺，它推断出了错误的类型。

有了 TypeScript 3.9，这个问题就解决了。

# 速度改进

TypeScript 3.9 提高了编译速度。

`import`陈述通常需要 5 到 10 秒钟。

现在快多了。

Material-UI 的编译时间减少了 40%。

# //@ ts-expect-错误注释

新的`// @ts-expect-error`注释非常方便，因为我们可以用它来抑制测试中可能出现的打字错误。

例如，如果您正在测试此功能:

```
const doSomething = (foo: string, bar: string) => {
  assert(typeof foo === "string");
  assert(typeof bar === "string");
  //...
}
```

然后我们可能想传入一个不是字符串的东西来测试它。

如果我们使用这个注释标签，现在就有可能了。

我们可以写:

```
// @ts-expect-error
doSomething(123, 'abc');
```

并且 TypeScript 编译器不会忽略这个错误。

# 条件表达式中未调用的函数检查

TypeScript 3.7 引入了未调用函数检查，以便在我们忘记调用函数时报告错误。

例如，如果我们有:

```
function shouldRun(): boolean {
    // ...
}if (shouldRun) {
  //...
}
```

那么我们会得到一个错误，因为应该调用`shouldRun`来返回一个布尔值，而不是在没有调用它的情况下写入。

因为`shouldRun`是真的，如果不调用它，检查就没有用。

该检查只适用于`if`语句。

现在它还检查三元表达式。

例如，如果我们有:

```
shouldRun ? 'foo' : 'bar'
```

我们也会得到一个错误。

# JavaScript 中的 CommonJS 自动导入

使用 TypeScript 3.9，TypeScript 可以从`require`导入中检测类型，以便导入 CommonJS 模块。

所以像这样的事情:

```
const { readFile } = require("fs");
```

可以由 TypeScript 编译器检查。

# 在代码操作中保留换行符

TypeScript 重构和快速修复现在可以在添加新行时保留它们。

例如，e 如果我们有:

```
for (let i = 0; i <= 1000; i++) {
  let cube = i ** 3; console.log(cube);
}
```

那么在自动修复和重构之后，循环体中的新行将被保留。

# 缺失返回表达式的快速修复

如果我们忘记了一个`return`，TypeScript 现在会为我们修复它。

例如，如果我们有:

```
const f = () => { 100 }
```

然后去掉大括号，或者添加一个 return 关键字。

# 解析可选链接和非空断言的差异

如果我们在同一个表达式中有`?.`和`!.`操作符。

例如，如果我们有:

```
a.foo?.bar!.baz
```

然后它被解释为

```
(a.foo?.bar).baz
```

在 TypeScript 3.9 中，整个表达式的解释不带`!`。

但是我们可以保持旧的行为方式:

```
(a.foo?.bar)!.baz
```

# 对交叉点和可选属性进行更严格的检查

如果我们有如下的交叉类型:

```
interface A {
  a: string;
}interface B {
  b: string;
}interface C {
  a?: boolean;
  b: string;
}declare let x: A & B;
declare let y: C;y = x;
```

由于 TypeScript 3.9 检查可选和必需属性的类型，因此我们不能再将`y`分配给`x`。

因为它们不匹配，所以现在认为它们不兼容。

# 吸气剂/沉降剂不再可计数

在 JavaScript 中，我们不能枚举获取者和设置者。

对于 TypeScript 3.9，这与常规的 ECMAScript 规范是一致的。

![](img/4f7c3c3fe2e6258112fed7e58998fd70.png)

Photo by [chuttersnap](https://unsplash.com/@chuttersnap?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 结论

TypeScript 3.9 修正了许多错误，使编译更快。

这些事情会给我们带来更少的挫折和更高的生产力，从而使我们受益。

承诺和类型检查的改进对我们都有很大帮助。

## **简单英语 JavaScript**

喜欢这篇文章吗？如果是这样，通过 [**订阅我们的 YouTube 频道**](https://www.youtube.com/channel/UCtipWUghju290NWcn8jhyAw) **获取更多类似内容！**