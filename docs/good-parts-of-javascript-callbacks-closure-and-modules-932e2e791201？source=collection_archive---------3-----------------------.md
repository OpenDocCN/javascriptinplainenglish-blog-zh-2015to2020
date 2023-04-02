# JavaScript 的优秀部分——回调、闭包和模块

> 原文：<https://javascript.plainenglish.io/good-parts-of-javascript-callbacks-closure-and-modules-932e2e791201?source=collection_archive---------3----------------------->

![](img/90aca2d67ff07958ffb6b22aa61bba0e.png)

Photo by [Samuel Scrimshaw](https://unsplash.com/@samscrim?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

JavaScript 是世界上最流行的编程语言之一。它可以做很多事情，并且有一些领先于许多其他语言的功能。

在本文中，我们将探讨在 JavaScript 应用程序中定义和使用回调、闭包和模块的方法。

# 复试

函数使得处理不连续事件变得更加容易。

例如，我们可以使用它们在固定时间后运行一些代码。

回调是传递给另一个函数并被同步或异步调用的函数。

同步回调调用的一个例子是我们传递给数组方法的回调。

例如，我们可以写:

```
const arr = [1, 2, 3].map(a => a ** 3);
```

函数`a => a ** 3`在`map`一被调用就被同步调用。

回调也可以是异步的。异步回调在函数被调用一段时间后才会运行。

异步回调的一个例子是我们传递给`setTimeout`函数的回调。

例如，我们可以写:

```
setTimeout(() => {
  console.log('foo');
}, 100)
```

上面的代码在 100ms 后调用回调。

由于 JavaScript 程序在单个线程中运行，异步回调被用在很多地方，所以我们尽可能异步地运行代码，以避免阻塞程序线程。

# 组件

在 JavaScript 拥有官方模块之前，我们通常将私有代码放在闭包里，以避免暴露给公众。

例如，我们可以写:

```
const module = (() => {
  let value = 0;
  return {
    getValue() {
      return value;
    }
  }
})()
```

在上面的代码中，我们创建了一个返回对象的生命。

该对象具有返回私有的`value`值的`getValue`方法。

我们不能访问`value`，但是可以调用`getValue()`。

然而，现在我们有了模块，我们可以用它们来代替。

例如，我们可以定义如下:

`module.js`

```
export const foo = 1;
const bar = 2;
export const getBar = () => bar;
```

`index.js`

```
import { getBar } from "./module";
console.log(getBar());
```

我们在`module.js`中导出`foo`和`getBar`，这样我们就可以在`index.js`中使用它们。

`getBar`可以在`index.js`中调用。

我们保持了隐私，但是我们认为如果我们想的话，我们可以揭露他们。

同样，我们可以将一个模块中的所有东西作为默认导出。

例如，我们可以写:

`module.js`

```
const bar = 2;export default {
  getBar: () => bar
};
```

对于默认导出，我们导出一个东西，并通过编写来导入它:

```
import module from "./module";
console.log(module.getBar());
```

如果我们导入默认导出，我们不需要花括号。

# 串联

我们可以在方法中链接返回`this`的方法。

例如，我们可以写:

```
const box = {
  setHeight(height) {
    this.height = height;
    return this;
  },
  setWidth(width) {
    this.width = width;
    return this;
  },
  setLength(length) {
    this.length = length;
    return this;
  },
}
```

那么当我们调用如下方法时:

```
box.setHeight(100).setWidth(200).setLength(150);
```

我们得到:

```
{
  "height": 100,
  "width": 200,
  "length": 150
}
```

正如我们看到的，如果我们返回`this`，那么我们可以按照我们希望的方式设置更新`this`并返回它。

然后我们可以在一个对象中链接这些方法。

这让我们可以制作富有表现力的界面。

我们可以在每个方法中做一点点，把所有的小动作串联起来。

![](img/50ef95d69d589790f65ab35f683a4196.png)

Photo by [chuttersnap](https://unsplash.com/@chuttersnap?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 咖喱菜肴

库里函数允许我们通过组合一个函数和一个自变量来产生一个新的函数。

我们可以创建一个`curry`函数来返回一个应用了一些参数的函数，让我们通过编写以下代码来应用其余的参数:

```
const curry = (fn, ...args) => {
  return (...moreArgs) => {
    return fn.apply(null, [...args, ...moreArgs]);
  };
}
```

在上面的代码中，我们有一个`curry`函数，它带有一个函数`fn`和一些参数`args`。

在其中，我们返回一个函数，该函数返回应用了两个函数的所有参数的`fn`。

这样，我们首先应用`args`中的参数，然后将`moreArgs`中的参数应用到`fn`。

现在如果我们这样称呼它:

```
const add = (a, b, c) => a + b + c;const curried = curry(add, 1);
const result = curried(2, 3);
```

我们首先在`curried`中得到一个将 1 应用于`add`的函数。

然后我们通过将 2 和 3 应用于`curried`得到最终的总和。

因此，`result`为 6。

# 结论

JavaScript 中经常使用回调。可以同步或异步调用。

我们可以用返回一个对象的正式模块或函数来定义模块。

Currying 函数让我们返回一个部分应用了参数的函数，以后可以应用更多的参数。

## 简单英语的 JavaScript

喜欢这篇文章吗？如果有，通过 [**订阅我们的 YouTube 频道**](https://www.youtube.com/channel/UCtipWUghju290NWcn8jhyAw) **获取更多类似内容！**