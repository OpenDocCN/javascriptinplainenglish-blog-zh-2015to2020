# 最佳现代 JavaScript——捕捉类和模块基础

> 原文：<https://javascript.plainenglish.io/best-of-modern-javascript-catches-for-classes-and-module-basics-8d09153383a6?source=collection_archive---------8----------------------->

![](img/39dba4590de16d696b6c385c59070c9d.png)

Photo by [National Cancer Institute](https://unsplash.com/@nci?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

自 2015 年以来，JavaScript 有了巨大的进步。

现在用起来比以前舒服多了。

在本文中，我们将看看如何用 JavaScript 定义类。

# 单一遗传

我们只能用`extends`关键字继承一个类。

然而，我们可以从现有的类中生成一个新的类，并继承它。

这是因为`extends`接受一个返回构造函数的表达式。

# 类别锁定

如果我们想实例化一个类，我们被迫在 ES6 中使用`new`关键字。

这意味着从一个类切换到一个工厂函数将意味着我们必须从现有代码中移除`new`关键字。

然而，我们可以通过用`constructor`返回我们自己的对象来覆盖构造函数返回的内容。

模块系统和类语法也使得重构 JavaScript 代码比以前容易得多。

# 类不能作为函数调用

类不能作为函数调用，即使它们是底层的函数。

这为将来添加用类处理函数调用的方法提供了选择。

# 给定一个参数数组实例化一个类

我们可以让我们的类构造函数用 rest 语法接受一个参数数组。

例如，我们可以写:

```
class Foo {
  constructor(...args) {
    //...
  }
}
```

然后，我们可以通过运行以下命令来实例化它:

```
new Foo(...args);
```

其中`args`是一个参数数组。

我们使用 spread 操作符将参数作为参数传播到`args`数组中。

然后我们可以随心所欲地使用它们。

此外，我们可以使用`Reflect.construct`方法创建一个带有参数数组的类实例。

例如，我们可以写:

```
`const foo = Reflect.construct(Foo, ['foo', 'bar']);
```

我们将类或构造函数作为第一个参数传入，将构造函数的一组参数作为第二个参数传入。

# 模块

JavaScript 在 ES6 之前没有本机模块系统。

然而，有许多模块系统被实现为库。

ES6 模块可以在浏览器和 Node.js 中访问。

在浏览器中，我们添加一个类型属性设置为`module`的脚本标签来导入一个模块。

默认情况下，模块处于严格模式。

顶层值 os `this`是模块的本地值。

模块是异步执行的。

还提供了`import`关键字来导入模块项目。

程序导入也是可用的。

`import`函数返回一个承诺，该承诺用模块的内容解析为一个对象。

模块的文件扩展名仍然是`.js`。

这与旧式脚本不同。

除非另外指定，否则脚本是同步运行的。

并且它们默认为非严格模式。

但是，它们可以异步导入。

每个模块都是一段代码，一旦加载就会运行。

在一个模块中，可能有各种类型的声明，如函数、类、对象等。

一个模块也可以从其他模块导入东西。

它们可以用相对路径如`'./foo/bar'`或绝对路径如`'/foo/bar'`导入。

模块是单例的，所以模块的所有导入都是相同的。

![](img/382d6627a61fb3b9c5c83a335760b4ca.png)

Photo by [www.raubfisch24.de](https://unsplash.com/@raubfisch24de?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 结论

类不能作为函数调用。

我们可以用一组参数来实例化它们。

模块有助于将代码分成更小的块。

喜欢这篇文章吗？如果有，通过 [**订阅获取更多类似内容解码，我们的 YouTube 频道**](https://www.youtube.com/channel/UCtipWUghju290NWcn8jhyAw) **！**