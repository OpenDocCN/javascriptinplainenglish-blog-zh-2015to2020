# JavaScript 反模式—命名和文档

> 原文：<https://javascript.plainenglish.io/javascript-antipatterns-naming-and-documentation-db3c0cba200b?source=collection_archive---------6----------------------->

![](img/d1416aa0740d66a5f60961bebf118a0b.png)

Photo by [Obi Onyeador](https://unsplash.com/@thenewmalcolm?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

JavaScript 让我们可以做很多事情。它的语法有时过于宽容。

在本文中，我们将研究一些在格式化 JavaScript 代码时应该避免的反模式，包括命名和文档。

# 命名规格

JavaScript 代码有一些标准的命名约定。

我们应该遵循它，这样我们就可以在一个项目中保持一致性。

然而，我们可以稍微调整它们以适应我们自己的风格，只要它不与其他人的风格冲突。

# 大写构造函数和类

构造函数名为 PascalCase。

每个单词的第一个字母大写。

例如，我们可以定义如下:

```
function FooConstructor() {...}
```

或者:

```
class MyClass {...}
```

其他功能有 camelCase。

# 分隔单词

大多数标识符通常用 camelCase 编写，除了我们上面提到的构造函数。

例如，我们可以通过编写`let firstName;`来定义变量。

一个函数可以写成`calculateArea()`。

常量全部用大写字母书写，单词之间用下划线隔开。

例如，我们可以写`const MAX_DONUTS = 1;`

属性名通常也用 camelCase 编写，所以我们得到`person.firstName = 'bob';`

# 其他命名模式

有些名字不遵循这些惯例。

例如，在`Number`对象中有大写的常量属性。

例如，`Number.MAX_SAFE_INTEGER`是一个具有大写属性名的常量。

这表明它是常数。

我们也可能有以下划线开头的属性来表示它是私有的。

由于在构造函数和类中没有私有变量，我们可能不得不这样做来表明我们不应该直接访问它。

例如，我们可以写:

```
class Person {
  constructor(name) {
    this._name = name;
  }
}
```

我们可以用方法做同样的事情。例如，我们可以写:

```
class Person {
  constructor(name) {
    this._name = name;
  } _greet() {
    //...
  }
}
```

同样，我们可以对对象做同样的事情:

```
const person = {
  getName() {
    return `${this._getFirst()} ${this._getLast()}`;
  },
  _getFirst() {
    // ...
  },
  _getLast() {
    // ...
  }
};
```

`_getFirst`和`_getLast`是私有的，如名称开头的下划线所示。

所以我们不应该直接打电话给他们，即使我们可以。

或者，我们可以对私有成员和受保护成员使用以下约定:

*   尾随下划线表示私有
*   前导下划线表示受保护，2 表示私有

我们可以根据自己的喜好进行调整。

# 写评论

我们应该为不明显的代码编写注释。

这意味着我们应该在每个变量或语句上写注释。

我们应该只解释代码中不清楚的东西。

此外，我们可以讨论我们做出的任何决定，如果我们需要证明它们是正确的。

# 编写 API 文档

API 文档是必不可少的。如果我们让外部用户使用我们的 API，那么我们需要记录他们。

尽管这可能很无聊，没有回报，但我们必须这样做，这样每个人都知道如何使用我们的 API。

我们通常可以写一些注释，用 JSDoc 之类的东西把它们转换成 API 文档。

# 供阅读的写作

我们必须写好我们的文档，这样它才能对阅读它的读者有用。

否则，我们就是在浪费时间。我们文档的读者会感到沮丧。

信息应该准确，结构应该清晰，易于理解。

我们可能要经过多份草稿才能把它们弄对。

编写 API 文档也提供了一个重新审视我们代码的机会，所以我们可以看看代码，如果需要的话，可以在那时修改它。

我们应该假设其他人会读它。否则，我们一开始就不用写了。

![](img/8228b5c80aa233ec31897134a1488a6a.png)

Photo by [Kelly Sikkema](https://unsplash.com/@kellysikkema?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 同行评审

同行评审也改进了我们的代码。审阅者可以提供改进建议。

它还让我们看到其他人的风格，并了解我们从代码中错过了什么。

最后，评审帮助我们写出清晰的代码，至少因为我们知道别人会阅读它。

源代码控制系统也是必不可少的。它不仅能帮助我们轻松撤销不好的改变。

它还让人们在我们签入代码时可以看到我们的代码。

# 结论

我们应该坚持一些命名惯例。比如变量用 camelCase，常量用大写。

写文档也很重要，因为我们需要告诉我如何使用我们的东西。

同行评议也很好，因为我们可以从其他人那里学习。

# **用简单英语写的便条**

你知道我们有四份出版物和一个 YouTube 频道吗？你可以在我们的主页 [**plainenglish.io**](https://plainenglish.io/) 上找到所有这些——关注我们的出版物并 [**订阅我们的 YouTube 频道**](https://www.youtube.com/channel/UCtipWUghju290NWcn8jhyAw) **来表达你的爱吧！**