# 功能性 JavaScript——管道和函子

> 原文：<https://javascript.plainenglish.io/functional-javascript-piping-and-functors-b2040605348a?source=collection_archive---------14----------------------->

![](img/7f4a61adaaaeb9a7059a1450145a6a89.png)

Photo by [Crystal Kwok](https://unsplash.com/@spacexuan?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

JavaScript 在一定程度上是一种功能性语言。

要学习 JavaScript，我们必须学习 JavaScript 的功能部分。

在本文中，我们将研究如何用 JavaScript 传递函数和函子。

# 管

我们可以通过创建一个以函数数组作为参数的函数来创建`pipe`函数。

它返回一个接受一个值的函数，我们用这个函数调用`reduce`，用`acc`调用`fn`。

例如，我们可以写:

```
const compose = (...fns) =>
  (value) =>
  fns.reduceRight((acc, fn) => fn(acc), value)
```

唯一的区别是我们使用了`reduceRight`代替`reduce`，所以我们不需要调用`reverse`来应用所有的函数。

# 作文是联想的

组合是关联的，这意味着我们可以重新排列操作的括号

例如:

```
compose(f, compose(g, h)) == compose(compose(f, g), h)
```

返回`true`。

# 函子

函子是一个简单的对象，它在运行 River 每个值以产生一个新对象的同时实现函数`map`。

函子是一个容器，其中包含一些值。

例如，我们可以写:

```
const Container = function(val) {
  this.value = val;
}
```

来创建我们的容器。

然后我们可以使用`new`调用`Container`构造函数:

```
let foo = new Container(3);
```

我们可以创建`Container.of`属性来添加一个容器，让我们返回一个`Container`实例:

```
const Container = function(val) {
  this.value = val;
}Container.of = function(value) {
  return new Container(value);
}
```

我们增加了`of`静态方法来返回一个`Container`实例。

`of`方法只是给了我们一个使用`new`运算符创建`Container`实例的替代方法。

然后我们可以用`of`方法创建一个`Container`实例:

```
const nested = Container.of(Container.of(1));
```

然后我们将看到嵌套的`Container`实例。

# 函子实现称为映射的方法

函子执行`map`方法。

我们可以将其添加到`prototype`属性中，作为实例方法添加:

```
const Container = function(val) {
  this.value = val;
}Container.of = function(value) {
  return new Container(value);
}Container.prototype.map = function(fn) {
  return Container.of(fn(this.value));
}
```

创建`Container`实例后，可以使用`map`方法。

例如，我们可以通过书写来使用它:

```
const squared = Container.of(3).map(a => a ** 2);
```

则`squared`为`Container`实例，且`value`为 9。

我们反复调用`map`重复一次手术。

所以我们可以写道:

```
const squared = Container.of(3)
  .map(square)
  .map(square)
  .map(square);
```

那么`squared`就是一个`Container`实例，其中`value`为 6561。

Functor 只是一个实现了`map`方法的对象。

![](img/db93129c702590ff088023c9f1166a7f.png)

Photo by [Samuel Sianipar](https://unsplash.com/@samthewam24?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 结论

函子是有值的普通对象。

该对象有一个`map`方法。

我们可以通过管道函数来调用多个函数，并获得返回值。

组合是关联的，所以我们可以重新排列操作的括号。

喜欢这篇文章吗？如果有，通过 [**订阅解码获得更多类似内容，我们的 YouTube 频道**](https://www.youtube.com/channel/UCtipWUghju290NWcn8jhyAw?sub_confirmation=true) **！**