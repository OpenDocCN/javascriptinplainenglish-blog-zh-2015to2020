# 更好的 JavaScript——方法和这个

> 原文：<https://javascript.plainenglish.io/better-javascript-methods-and-this-51bf5892ad01?source=collection_archive---------12----------------------->

![](img/774f9e9daf81e0a3d2b9ef708a4c749c.png)

Photo by [ACK15](https://unsplash.com/@ack15?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

像任何类型的应用程序一样，JavaScript 应用程序也必须写得很好。

否则，我们以后会遇到各种各样的问题。

在本文中，我们将研究改进 JavaScript 代码的方法。

# 在原型上存储方法

我们应该将方法存储在构造函数的原型中。

这样，我们只存储一个方法的副本，每个实例都从原型继承。

例如，不写:

```
function Person(name) {
  this.name = name; this.toString = function() {
    return `Person: ${this.name}`;
  };
}
```

我们写道:

```
function Person(name) {
  this.name = name;
}Person.prototype.toString = function() {
  return `Person: ${this.name}`;
};
```

从表面上看，它们是一样的。

如果我们用`new`操作符创建一个新的`Person`实例，无论如何我们都会得到`toString`方法。

但在表面之下，第二种方式更有效率。

# 使用闭包来存储私有数据

JavaScript 可以在函数中包含函数。

函数内部的数据对外部是不可访问的。

所以我们可以把私有数据放在函数里面。

例如，如果我们有:

```
(() => {
  let x = 1; function add(y) {
    return x + y;
  }
})();
```

箭头函数中有`add`函数。

我们得到了`add`函数中的`x`值。

这样，`x`就不会对外可用了。

# 在实例对象上存储实例状态

我们只在实例对象上存储实例状态。

例如，我们不应该有这样的代码:

```
function Tree() {}Tree.prototype = {
  children: [],
  addChild(x) {
    this.children.push(x);
  }
};
```

相反，我们应该写:

```
function Tree() {
  this.children = [];
}Tree.prototype = {  
  addChild(x) {
    this.children.push(x);
  }
};
```

原型是`Tree`继承而来的，所以我们不应该把状态放在那里，因为它被`Tree`的所有实例共享。

相反，我们应该向构造函数添加状态，这样它们就不会在实例中共享。

可变数据在共享时总是有问题。

使用类语法可以避免这个错误。

所以我们可以写:

```
class Tree {
  constructor() {
    this.children = [];
  } addChild(x) {
    this.children.push(x);
  }
}
```

这样，就没有办法创建多个实例共享的状态。

# 此的隐式绑定

`this`是在不同位置可以有不同值的东西。

如果它在顶层，那么`this`是全局对象，如果严格模式是关闭的，而`undefined`是打开的。

如果在原型的方法中，则为构造函数。

而在一个类中，那么`this`就是类本身。

所以如果我们有:

```
function Tree() {
  this.children = [];
}Tree.prototype = {  
  addChild(x) {
    this.children.push(x);
  }
};
```

那么`this`就是`Tree`构造器。

如果我们有传统函数的回调，那么`this`就是回调本身。

所以我们要清楚它们的值，这样才不会引用错`this`的值。

箭头函数不绑定到它们自己的值`this`，所以它们非常适合回调。

![](img/2f742e906a58f7099f5bf5e5d3fd64ba.png)

Photo by [Zachary Keimig](https://unsplash.com/@zacharykeimig?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 结论

我们应该小心放置构造函数状态的位置。

此外，`this`的值可以根据上下文而改变。

喜欢这篇文章吗？如果有，通过 [**订阅获取更多类似内容解码，我们的 YouTube 频道**](https://www.youtube.com/channel/UCtipWUghju290NWcn8jhyAw) **！**