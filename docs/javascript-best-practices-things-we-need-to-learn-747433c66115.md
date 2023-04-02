# JavaScript 最佳实践——我们需要学习的东西

> 原文：<https://javascript.plainenglish.io/javascript-best-practices-things-we-need-to-learn-747433c66115?source=collection_archive---------8----------------------->

![](img/a4d70134b08210c67ba4bbe278b3ea85.png)

Photo by [Kalen Emsley](https://unsplash.com/@kalenemsley?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

像任何类型的应用程序一样，JavaScript 应用程序也必须写得很好。

否则，我们以后会遇到各种各样的问题。

在本文中，我们将研究一些在编写 JavaScript 代码时应该遵循的最佳实践。

# 了解范围

我们应该学习变量和函数的范围。

有了`let`和`const`变量，变量作用域应该很容易。

它们只能在街区内访问，所以没有歧义。

此外，我们应该知道模块可以访问什么。

只有出口的东西才能进口。

棘手的是用`function`关键字声明的`var`变量和函数。

`var`函数是否限定了作用域，因此它在整个函数中都可用。

`function`可以悬挂报关单。

像这样的功能:

```
function foo(){
  //...
}
```

可以在代码中定义之前调用。

# 使用最快的方式循环变量

循环变量的最快方法是。

所以我们写道:

```
let arrayLength = array.length;
for(let i = 0 ; i < arrayLength; i++) {
   let val = array[i];
}
```

循环浏览项目。

然而，为了方便起见，for-of 循环和`forEach`也是在不牺牲太多性能的情况下遍历项目的好方法。

如果我们需要映射数据，我们使用`map`。

如果我们需要返回一个新的数组，其中的数据符合给定的条件，我们使用`filter`。

还有一个`while`循环来遍历数据。

# 不要做太多的嵌套

嵌套太多，不好读。

这也意味着它们很难修改和调试。

例如，我们不应该有这样的东西:

```
firstFunction(args, () => {
  secondFunction(args, () => {
    thirdFunction(args, () => {
      //...
    });
  });
});
```

嵌套越深，就越难处理，所以我们应该避免这样的事情。

# 小心大教堂

如果我们正在创建客户端 JavaScript 应用程序，那么我们需要操作 DOM。

了解如何在 DOM 中添加、编辑和删除元素是一个好主意。

除了元素节点之外，还有许多类型的节点，比如文本和注释节点。

事件传播也是需要考虑的事情。

# 使用最好的库和框架

顶级客户端 JavaScript 框架与一些额外的库 Angular 和 Vue 相互作用。

React 与 React 路由器可以视为一个框架。

也有苗条的，这是上升和未来。

这些概念在它们之间是可以转换的，所以如果我们学习一个，我们可以学习另一个，

它们都有基于组件的架构，所以它们是相似的。

此外，还有许多为他们建立的库。

# 具有高效的配置和转换

我们应该把所有的配置和翻译放在一个地方，这样我们就可以在任何地方使用它们。

这样，如果我们需要更改，我们只需在一个地方更改它们。

# 减少对变量和属性的访问

如果我们不需要访问变量，那么我们应该避免它。

例如，对于 For 循环，我们可以将数组长度缓存到 for 循环之外的变量中。

而不是写:

```
let names = ['mary', 'james', 'may'];
for (let i = 0; i < names.length; i++) {
  greet(names[i]);
}
```

我们写道:

```
let names = ['mary', 'james', 'may'];
let length = names.length;
for (let i = 0; i < length; i++) {
  greet(names[i]);
}
```

# 不要相信任何数据

我们不应该相信任何地方的任何数据。

因此，我们应该验证任何输入值，并检查来自 API 等其他来源的数据。

这样，我们只处理我们需要的数据，避免恶意数据带来的任何安全问题。

# 使用箭头功能

箭头功能很棒。

如果我们没有创建构造函数，可以使用它。

如果我们不需要它绑定到自己的`this`，那么我们也可以使用它。

例如，我们可以写:

```
const foo = () => console.log('hello');
```

因为它没有绑定到自己的`this`，所以它更短，也更容易混淆。

# 使用模板文字

模板文字比串联好得多，因为它更容易阅读。

我们可以在其中加入任何表达式。

例如，不要写:

```
'hello '  + name;
```

我们写道:

```
`hello ${name}`;
```

模板文字总是用反斜线而不是单引号或双引号来分隔。

![](img/3820d2d48599977238b5d23bb94aba6d.png)

Photo by [Jeremy Bishop](https://unsplash.com/@jeremybishop?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 结论

我们应该使用 JavaScript 的最新特性，比如模板文字和箭头函数。

此外，我们还得学习望远镜和`this`。

## 简单英语的 JavaScript

喜欢这篇文章吗？如果有，通过 [**订阅获取更多类似内容解码，我们的 YouTube 频道**](https://www.youtube.com/channel/UCtipWUghju290NWcn8jhyAw) **！**