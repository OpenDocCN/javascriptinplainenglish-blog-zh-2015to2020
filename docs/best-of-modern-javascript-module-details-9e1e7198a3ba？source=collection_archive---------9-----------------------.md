# 现代 JavaScript 精华—模块细节

> 原文：<https://javascript.plainenglish.io/best-of-modern-javascript-module-details-9e1e7198a3ba?source=collection_archive---------9----------------------->

![](img/1f340c80ece3ce9b1bf7c0db5d9009ff.png)

Photo by [Justin Veenema](https://unsplash.com/@justinveenema?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

自 2015 年以来，JavaScript 有了巨大的进步。

现在用起来比以前舒服多了。

在本文中，我们将研究 ES6 模块系统的设计。

# 使用变量来指定我要从哪个模块导入

我们可以用`import`函数指定导入哪个模块。

它接受一个带有模块路径的字符串。

例如，我们可以写:

```
(async () => {
  const foo = await import("./foo");
  //...
})();
```

使用`import`功能导入模块。

它需要一个字符串，所以我们可以传入一个动态生成的字符串。

它返回一个承诺，所以我们使用`await`来获得解析的值。

# 有条件或按需导入模块

使用`import`函数，我们可以有条件地或按需导入一个函数。

例如，我们可以写:

```
(async () => {
  if (Math.random() < 0.5) {
    const foo = await import("./foo");
    //...
  }
})();
```

有条件地导入模块。

# 在 Import 语句中使用变量

我们不能在导入语句中使用变量。

所以我们不能写这样的东西:

```
import foo from 'bar-' + SUFFIX;
```

但是用`import`函数，我们可以写:

```
(async () => {
  if (Math.random() < 0.5) {
    const foo = await import(`bar-${SUFFIX}`);
    //...
  }
})();
```

# 在`import`语句中使用析构

我们不能在`import`语句中使用嵌套析构。

这是有道理的，因为出口只能在最高层进行。

它看起来像析构，但语法不同。

进口是静态的，出口是动态的。

所以我们不能写这样的东西:

```
import { foo: { bar } } from 'some_module';
```

# 指定出口

有了命名导出，我们可以用对象来实现静态结构。

如果我们用一个对象创建一个默认的导出，那么我们就不能静态地分析这个对象。

对象可以有任何属性，并且可以嵌套。

# `eval()`一个模块的代码

我们不能在模块代码上调用`eval`。

模块对于`eval`来说级别太高。

`eval`接受不允许使用`import`或`export`关键字的脚本。

# ES6 模块的优势

ES6 模块有几个好处。

它们包括更紧凑的语法。

静态模块结构也有助于消除死代码、静态检查、优化等。

此外，我们检查循环依赖。

使用标准模块系统，我们消除了多个模块系统的碎片。

使用旧模块系统的一切都将迁移到 ES6 标准模块。

此外，我们不再需要使用对象作为名称空间。

该功能现在由模块提供。

像`Math`和`JSON`这样的对象充当隔离实体的名称空间。

![](img/64c06e01d660580e149ad7f00797032c.png)

Photo by [Berkay Gumustekin](https://unsplash.com/@berkaygumustekin?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 结论

与旧的非标准模块系统相比，ES6 模块为我们提供了许多优势。

此外，它们可以动态导入。

它们允许各种优化。

喜欢这篇文章吗？如果是这样，通过 [**订阅解码获得更多类似内容，我们的 YouTube 频道**](https://www.youtube.com/channel/UCtipWUghju290NWcn8jhyAw) **！**