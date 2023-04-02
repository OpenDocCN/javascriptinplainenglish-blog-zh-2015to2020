# JavaScript 最佳实践—隐私

> 原文：<https://javascript.plainenglish.io/javascript-best-practices-privacy-bfef099274a7?source=collection_archive---------8----------------------->

![](img/0c6d82ef2ba22231e355838cf3b69ac6.png)

Photo by [Dayne Topkin](https://unsplash.com/@dtopkin1?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

JavaScript 让我们可以做很多事情。它的语法有时过于宽容。

在本文中，我们将研究如何创建和使用对象，包括保持事物的私有性。

# 隐私失败

当我们在一个应该是私有的函数中直接返回对象时，我们可以直接从外部修改它们。

这是因为变量是通过引用传递的。

例如，如果我们有:

```
function Foo() {
  const bar = {
    a: 1,
    b: 2
  }; // public function
  this.getBar = function() {
    return bar;
  }
}
```

那么`bar`是私有的，但是因为它是由`getBar`返回的，所以我们可以从外部访问它。

我们也可以修改它。

例如，如果我们写:

```
const foo = new Foo();
bar = foo.getBar();
console.log(foo.getBar());
bar.a = 2;
console.log(foo.getBar());
```

然后我们会看到`bar`现在是`{a: 2, b: 2}`。

这并不好，因为我们不想从外部改变`bar`。

因此，我们应该在返回对象或将其放入模块之前克隆它。

我们可以用 spread 操作符做一个浅层克隆。

我们可以使用`export`关键字来导出模块中的对象。

这样，我们要么改变对象的克隆，要么在试图直接修改它时得到一个错误。

# 对象文字和隐私

如果我们希望对象数据是私有的，那么我们必须将它包装在一个函数中，然后返回它的一部分。

例如，我们可以写:

```
(() => {
  const name = 'foo'; return {
    getName() {
      return name;
    }
  }
})()
```

在上面的例子中，`getName`有特权访问`name`常量。

我们在函数定义后立即运行它，所以如果我们将返回的对象赋给一个变量或常量，就可以调用它。

同样，我们可以将对象赋给一个生命之外的变量，如下所示:

```
let obj;
(() => {
  const name = 'foo'; obj = {
    getName() {
      return name;
    }
  }
})()
```

现在我们可以在生活结束后调用`obj.getName`。

# 原型和隐私

如果我们不想在一个构造函数的每个实例中有单独的方法实例，那么我们必须把它们放在原型中。

这样，所有实例方法都是从构造函数的原型继承的。

例如，我们可以写:

```
function Foo() {}Foo.prototype.hello = function() {
  //...
}
```

然而，由于构造函数没有存储私有数据的地方，我们不得不努力寻找一个存储私有数据的地方。

例如，我们可以写:

```
function Foo() {}Foo.prototype = (() => {
  const foo = "foo";
  return {
    getFoo() {
      return foo;
    }
  };
})();
```

在上面的代码中，我们将`foo`作为私有变量。

我们用`getFoo`函数返回一个对象。

因此，当我们调用`getFoo`时如下:

```
const foo = new Foo();
console.log(foo.getFoo());
```

我们记录了`'foo'`而没有公开`foo`常量本身。

这是类语法现在没有的一点。

没有好的方法可以用类语法添加私有变量，所以这是构造函数语法的一个优点。

# 将私有函数公开为公共方法

如果我们想将一些私有函数作为公共方法公开，我们可以这样做。

我们可以使用启示模式来揭示我们选择的一些私有成员。

例如，我们可以写:

```
const obj = (() => {
  const foo = () => {
    //...
  } const bar = () => {
    //...
  }
  const baz = () => {
    //...
  } return {
    foo,
    bar
  }
})();
```

我们在下面的对象中返回了`foo`和`bar`，所以我们可以用`obj.foo()`和`obj.bar()`来调用它们。

这对于存储我们不想暴露给外界的帮助函数是很方便的。

我们可以冻结`obj`对象以防止意外修改。

![](img/65dc1763f4a5783406745fd9cdd3d898.png)

Photo by [Tim Mossholder](https://unsplash.com/@timmossholder?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 模块模式

在模块模式中，我们将私有和特权方法和名称空间合二为一。

例如，我们可以通过编写以下代码来定义自己的模块:

```
const APP = {
  utilities: {}
}APP.utilities.math = (() => {
  return {
    add(a, b) {
      // ...
    },
    subtract(a, b) {
      // ...
    }
  };
}());
```

上面的代码定义了一个名为`APP.utilities.math`的模块。

它有对外可用的`add`和`subtract`方法。

我们可以随时打电话给他们:

```
APP.utilities.math.add((1, 2)
```

或者:

```
APP.utilities.math.subtract(1, 2)
```

我们不必返回返回对象中的所有成员。

所以我们可以保留一些隐私。

当我们的 JavaScript 项目中没有模块时，这仍然很有用。

# 结论

当我们的项目中没有模块时，我们可以创建自己的模块。

为了防止意外修改函数中返回的对象，我们可以复制或冻结它们。

我们也可以通过给返回一个对象的`prototype`分配一个生命来在构造函数中拥有私有成员。

## 简单英语的 JavaScript

通过 [**订阅我们的 YouTube 频道**](https://www.youtube.com/channel/UCtipWUghju290NWcn8jhyAw) **来表达爱意吧！**