# 面向对象的 JavaScript——原型

> 原文：<https://javascript.plainenglish.io/object-oriented-javascript-prototypes-de97f43a3594?source=collection_archive---------16----------------------->

![](img/d8a1f0e87c8a0714351ff5d63e15ea46.png)

Photo by [Glenn Carstens-Peters](https://unsplash.com/@glenncarstenspeters?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

JavaScript 部分是面向对象的语言。

要学习 JavaScript，我们必须学习 JavaScript 的面向对象部分。

在本文中，我们将研究对象原型。

# 原型

每个物体都有原型。

对象继承自称为原型的对象。

# 原型属性

原型位于构造函数的`prototpe`属性中。

构造函数具有`prototype`属性，该属性具有通过构造函数实例返回的方法。

例如，如果我们有一个函数:

```
function foo(a, b) {
  return a + b;
}
```

然后我们可以通过书写得到它的原型:

```
console.log(foo.prototype)
```

它有`constructor`属性，这是函数本身。

只要我们不另外指定，这个属性总是可用的。

# 添加原型方法和属性

我们可以向原型添加属性和方法，然后它们将被构造函数的所有实例共享。

例如，我们可以写:

```
function Person(name) {
  this.name = name;
  this.whoAreYou = function() {
    return `I am ${this.name}`;
  };
}
```

然后我们有了`name`和`whoAreYou`实例属性。

这将为每个实例创建不同的属性副本。

但是我们可以在不同的实例之间共享`whoAreYou`，因为它是相同的方法。

我们可以通过将它放在`prototype`属性中来实现:

```
function Person(name) {
  this.name = name;
}Person.prototype.whoAreYou = function() {
  return `I am ${this.name}`;
};
```

`this.name`在不同的实例之间应该是唯一的，所以应该在构造函数中。

但是`whoAreYou`是可以共享的，所以我们把它添加为`prototype`的一个属性。

# 使用原型的方法和属性

我们可以通过实例化构造函数的实例来使用原型的方法和属性。

例如，我们可以写:

```
const jane = new Person('jane');
```

然后我们可以通过写来调用`whoAreYou`:

```
console.log(jane.whoAreYou())
```

然后我们得到:

```
'I am jane'
```

# 自有属性与原型属性

自有属性是在对象本身中定义的属性，而不是继承的属性。

原型属性是从另一个对象继承的属性。

在`Person`构造函数中，我们有`prototype.whoAreYou`方法，这意味着它继承自`Person`构造函数的原型。

当我们获取一个属性或调用一个方法时，JavaScript 引擎会查看原型链中对象的所有属性来获取属性。

我们不能仅仅通过查看调用来区分。

所以`jane.name`看起来像`jane.whoAreYou()`，但是第一个是自己的属性，第二个是原型属性。

我们可以使用`constructor`属性进行检查。

例如，我们可以写:

```
console.log(jane.constructor.prototype);
```

我们在记录的对象中看到了`whoAreYou`方法。

# 用自己的属性覆盖原型的属性

我们可以用自己的属性覆盖原型的属性。

例如，我们可以写:

```
function Person(name) {
  this.name = name;
}Person.prototype.name = 'no name';
```

然后，如果我们创建一个`Person`实例:

```
const jane = new Person('jane');
```

我们看到的`jane`是:

```
{name: "jane"}
```

我们可以用`hasOwnProperty`方法确定一个属性是否是自己的属性。

例如，如果我们写:

```
console.log(jane.hasOwnProperty('name'));
```

我们得到`true`。

我们可以用`delete`操作符删除一个属性。

例如，我们可以写:

```
delete jane.name
```

移除它。

![](img/ac768c00a71c6c4db0578d3de665c9d6.png)

Photo by [Anthony Young](https://unsplash.com/@ayoungh?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 结论

对象的原型是对象继承的对象。

我们可以覆盖它的属性。

喜欢这篇文章吗？如果有，通过 [**订阅获取更多类似内容解码，我们的 YouTube 频道**](https://www.youtube.com/channel/UCtipWUghju290NWcn8jhyAw?sub_confirmation=true) **！**