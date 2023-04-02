# 可维护的 JavaScript——我们不拥有的对象

> 原文：<https://javascript.plainenglish.io/maintainable-javascript-objects-we-dont-own-65b2231e76ae?source=collection_archive---------9----------------------->

![](img/7990e30e562b5aa72d8bcebfcd59ee29.png)

Photo by [Oliver Hale](https://unsplash.com/@4themorningshoot?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

如果你想继续使用代码，创建可维护的 JavaScript 代码是很重要的。

在本文中，我们将通过研究如何处理我们不拥有的对象来了解创建可维护的 JavaScript 代码的基础。

# 我们拥有的物品

我们拥有的东西就是我们创造的东西。

创建对象的代码可能不是我们写的，但是对象是我们创建的。

这意味着我们不拥有自己的对象，比如本地对象`Object`、`Date`等。

我们也没有 DOM 对象、内置的全局对象或库对象。

这些都是项目环境的一部分。

这些应被视为只读。

我们不应该覆盖这些对象的方法。

我们不应该在这些对象中添加或删除现有的方法。

这是因为我们很容易做别人没想到的事情。

这将导致混乱，并浪费时间跟踪意外的代码。

# 不要覆盖方法

重写我们不拥有的对象的方法是 JavaScript 中最糟糕的做法之一。

在脚本中，很容易覆盖现有的方法。

例如，我们可以写:

```
document.getElementById = () => {
  return null;
}
```

那么每个人都会困惑为什么`document.getElementById`总是回来`null`。

在脚本中，没有什么可以阻止我们覆盖 DOM 方法。

我们也可以覆盖任何库代码中的任何其他属性。

例如，有人可能会编写这样的代码:

```
document._originalGetElementById = document.getElementById;
document.getElementById = (id) => {
  if (id === "window") {
    return window;
  } else {
    return document._originalGetElementById(id);
  }
};
```

这也是不好的，因为它改变了标准库方法的工作方式。

这也和其他事情一样会带来混乱。

新的代码被称为“T4”是“T5”，但是原来的代码被用于其他任何东西。

这同样糟糕，因为`getElementById`有时和我们预期的一样有效，有时也是如此。

因此，我们永远不应该重写任何方法，这样我们就可以让它们像预期的那样工作。

# 不添加新方法

在 JavaScript 中向现有对象添加新方法也很容易。

我们只需要给现有的对象分配一个函数，使其成为一个方法。

它允许我们修改各种对象。

例如:可以在`document`中增加方法:

```
document.foo = () => {
  console.log('hello');
};
```

我们也可以为`Array``prototytpe`添加方法:

```
Array.prototype.foo = () => {
  console.log('hello');
};
```

他们都很坏。

没有什么能阻止我们向它们添加方法。

就像覆盖方法一样，我们使库对象的行为不同于我们所期望的。

此外，我们添加的方法也可能被添加到标准库中。

然后我们有两种方法做不同的事情，我们用我们的版本覆盖它。

即使是细微的差异也可能导致许多混乱。

![](img/f24826ea26217e98847d16dd8284e4a9.png)

Photo by [Michael D Beckwith](https://unsplash.com/@michaeldbeckwith?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 结论

我们不应该添加或覆盖任何内置或库对象的方法。

这是因为我们会对为什么代码的工作方式与我们预期的不同感到困惑。

![](img/787be6c671be8d345dc786dad8729ce5.png)

[Subscribe to Decoded, our official YouTube channel!](https://www.youtube.com/channel/UCtipWUghju290NWcn8jhyAw)