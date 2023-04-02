# TypeScript 最佳实践—文字类型和承诺

> 原文：<https://javascript.plainenglish.io/typescript-best-practices-literal-types-and-promises-dca5440e7e8?source=collection_archive---------7----------------------->

![](img/f4ece86b503d857d633bd915db00b090.png)

Photo by [rigel](https://unsplash.com/@rigels?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

为了使代码易于阅读和维护，我们应该遵循一些最佳实践。

在这篇文章中，我们将看看我们应该遵循的一些最佳实践，以使每个人的生活更轻松。

# 没有包含计算键表达式的删除表达式

使用`delete`删除计算的键表达式是一个坏主意。

不安全，也无法优化。

所以与其写:

```
delete foo[bar];
```

我们可以让这个物体保持原样。

# 没有空块

我们的代码中不应该有空块。

他们没用。

例如，我们应该删除如下代码:

```
function foo(){}
```

# 没有浮动承诺

浮动承诺是不存储或返回任何数据的承诺。

因此，它实际上没有做任何事情。

例如，如果我们有:

```
Promise.resolve(1);
```

那我们应该做点什么。

未处理的承诺会导致意想不到的行为，因为它们在不确定的时间内给我们一个值。

# 不要对数组使用 for-in 循环

我们不应该在数组中使用 for-in 循环。它应该和普通物体一起使用。

迭代的顺序没有保证。

所以不用 for-in 这样的:

```
for (const i in array){
  console.log(array[i]);
}
```

我们可以写:

```
for (const a of array){
  console.log(a);
}
```

对于数组项，我们用`of`和`a`替换了`in`。

现在，我们可以轻松地按顺序遍历它们。

# 没有推断的空对象类型

我们不应该有空的对象类型，因为我们应该有一个更具体的变量、参数和返回类型。

而不是写:

```
const obj: {} = {};
```

我们写道:

```
const obj: { foo: number } = { foo: 2 };
```

# 不，这个无效

我们不应该在类或对象文字之外使用`this`。

这只会引起混乱，因为`this`应该是一个类实例。

例如，我们可以写:

```
const obj = {
  a: 1,
  b() {
    console.log(this.a)
  }
}
```

或者:

```
class Foo {
  constructor() {
    this.a = 1;
  } bar() {
    console.log(this.a)
  }
}
```

# 无空关键字

我们应该只对所有空值使用`undefined`，而不是混合`null`或`undefined`。

它们的作用相同，所以我们不需要两者。

`undefined`也有类型`undefined`所以更容易检查。

因此，只要在我们的代码中处处使用`undefined`即可。

# 没有空的和未定义的联合类型

用`null`和`undefined`创建类型是多余的。

这是因为我们的变量、参数或返回值只能有`null`或`undefined`或我们指定的任何其他值。

我们应该使类型更具限制性，这样我们就可以真正利用 TypeScript 的类型检查功能。

# 没有对象文字类型断言

具有对象文字类型的类型断言将隐藏具有过多属性的错误。

即使对象缺少一些必需的字段，数据类型断言也将覆盖没有断言的任何类型。

因此，我们应该在不强制类型为对象文字结构的情况下处理它。

所以我们不应该写:

```
const foo = {} as { bar: number };
```

我们写道:

```
const foo: Foo = {};
```

其中`Foo`是接口、类型或类型别名。

# 布尔型无承诺

我们不应该使用一个`await`表达式作为布尔表达式。

`await`只对未来给予实际价值的承诺。

这不是我们想要检查的实际值。

因此，我们应该将`await` ed 值赋给一个新变量，这样我们就可以用实际值进行检查。

例如，不写:

```
async function bar(personPromise: Promise<Person> ) {
  if (await personPromise) {
    console.log("person retrieved")
  }
}
```

我们写道:

```
async function bar(personPromise: Promise<Person> ) {
  const person = await personPromise; 
  if (person) {
    console.log("person retrieved");
  }
}
```

# 没有回报等待

我们不应该在同一条线上使用`return`和`await`。

它只是在解决承诺之前增加了额外的时间，但是无论是否有`await`承诺都会被返回。

例如，不写:

```
async function bar() {
  return await aPromise;
}
```

我们写道:

```
async function bar() {
  return aPromise;
}
```

![](img/5549dfeb5e972af0e370d32345167483.png)

Photo by [Richard Burlton](https://unsplash.com/@richardworks?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 结论

`delete`运算符不应与计算表达式一起使用。

for-in 循环不应与数组一起使用。

`this`应该只用于对象文字和类。

文字类型断言不应该用于强制对象的结构。

`await`只能在`async`功能中的某些地方使用。

# 简单英语的 JavaScript

喜欢这篇文章吗？如果有，通过 [**订阅解码，我们的 YouTube 频道**](https://www.youtube.com/channel/UCtipWUghju290NWcn8jhyAw) **获取更多类似内容！**