# 成为前端开发人员的基本 JavaScript 概念

> 原文：<https://javascript.plainenglish.io/basic-javascript-concepts-for-becoming-a-front-end-developer-73ef17c5d663?source=collection_archive---------6----------------------->

![](img/db1c1f65a1cd9a71cc6e11544ae94e3b.png)

Photo by [Louis Hansel @shotsoflouis](https://unsplash.com/@louishansel?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

如果我们想成为一名前端开发人员，那么学习 JavaScript 是必须的。

在本文中，我们将研究 JavaScript 概念，学习如何成为一名高效的前端开发人员。

# 值与参考变量

我们知道了原始值和参考变量之间的区别。

在 JavaScript 中，原始值是:

*   用线串
*   比金茨
*   数字
*   标志
*   布尔型
*   不明确的
*   空

其他都是参考变量。即使原始值是通过像`new Boolean(true)`这样的构造函数创建的，那些也是引用值。

数组也是引用变量。

例如，以下是原始变量:

```
let x = 1;
let str = 'foo';
```

这些是参考变量:

```
let obj = {
  a: 1
}
let str = new String('foo');
let arr = [1, 2, 3];
```

原始值通过值传递给函数，但是对象通过引用传递给函数。

这意味着作为参数传递的对象在参数内部传递时可以被更改。

但是原始值在传递给函数之前会被复制。

此外，对象是可变的，即使它被赋给了一个`const`常量。

所以我们可以写:

```
const obj = {
  a: 1
}
obj.a = 2;
```

我们得到`obj.a`是 2。

# 关闭

闭包在 JavaScript 中非常重要，因为它让我们可以对变量进行私有访问。

在模块出现之前，这是以私有方式访问变量的唯一方式。

例如，闭包如下所示:

```
const greeter = (greeting) => {
  return (name) => {
    console.log(`${greeting}, ${name}`);
  }
}
```

上面的代码返回一个接受`name`参数的函数，并且可以私有访问`greeting`参数，因为参数可以在函数内部访问。

# 解构

析构对于将数组条目和对象属性直接赋给变量非常有用。

例如，我们可以将它用于数组，如下所示:

```
const [foo, bar] = [1, 2, 3];
```

上面的代码也将 1`foo`和 2`bar`赋值，没有任何额外的操作。

它只是将右侧每个位置的任何条目分配给左侧的相同条目。

对于对象，我们可以写:

```
const {
  a,
  b
} = {
  a: 1,
  b: 2,
  c: 3
};
```

1 被分配给`a`，2 被分配给`b`，因为匹配是通过嵌套级别和属性名完成的。

因为`a`和`b`在顶层，并且在两端都可用，所以它们被赋值。

只要级别和属性名称匹配，析构也适用于嵌套对象。

# 扩展语法

spread 语法是前端开发人员需要了解的另一个很好的语法。

它允许我们浅层复制对象和数组。浅层复制意味着只复制顶层，而其余部分仍然引用原始对象。

此外，在调用函数时，我们可以将数组转换为参数列表。

例如，我们可以将以下内容写入浅拷贝对象:

```
const arr = [1, 2, 3];
const copy = [...arr];
```

我们将`arr`的条目复制到`copy`数组中。

我们可以对物体做同样的事情:

```
const obj = {
  a: 1,
  b: 2
}
const copy = {
  ...obj
};
```

我们也可以调用一个带有参数列表的函数，如下所示:

```
const arr = [3.5, 7, 8];
const max = Math.max(...arr);
```

上面的代码将以逗号分隔的参数列表的形式传播`arr`。

![](img/bece5030e200c45ca66a5f8487179927.png)

Photo by [Toa Heftiba](https://unsplash.com/@heftiba?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# Rest 语法

我们可以用它将具有相应参数的变量转换成一个数组。

如果我们有额外的参数，我们没有参数，但我们想访问它们，我们可以用 rest 语法。

例如，我们可以写:

```
const add = (a, ...rest) => a + rest.reduce((a, b) => a + b, 0)
```

除了存储在`rest`中的第一个参数之外，上面的代码没有任何其他参数。

所以如果我们这样称呼它:

```
add(1, 2, 3)
```

我们得到`[2, 3]`作为`rest`的值。

这就是为什么我们可以在它上面调用`reduce`来添加那些其他的数字。

# 结论

前端开发人员应该知道的这些重要的缩写和概念。这将使许多编程任务变得更容易。

当我们传播道具和当他们被接收时破坏道具时，使用传播和休息操作符。

此外，当我们在`useState`钩子中更新数组时，我们可能必须使用 spread 操作符。

我们不能混淆原始值和对象，因为它们的行为不同。

# **用简单英语写的便条**

你知道我们有四份出版物和一个 YouTube 频道吗？你可以在我们的主页[**plain English . io**](https://plainenglish.io/)找到所有这些内容——关注我们的出版物并 [**订阅我们的 YouTube 频道**](https://www.youtube.com/channel/UCtipWUghju290NWcn8jhyAw) **来表达你的爱吧！**