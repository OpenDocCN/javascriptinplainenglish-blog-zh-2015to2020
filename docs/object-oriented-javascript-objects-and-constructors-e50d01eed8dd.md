# 面向对象的 JavaScript —对象和构造函数

> 原文：<https://javascript.plainenglish.io/object-oriented-javascript-objects-and-constructors-e50d01eed8dd?source=collection_archive---------8----------------------->

![](img/b977eb9c86762e910c9cdc09a4273c42.png)

Photo by [Kelly Sikkema](https://unsplash.com/@kellysikkema?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

JavaScript 部分是面向对象的语言。

要学习 JavaScript，我们必须学习 JavaScript 的面向对象部分。

在本文中，我们将研究对象和构造函数。

# 访问对象的属性

我们可以使用方括号符号或点符号来访问对象的属性。

要使用方括号符号，我们可以这样写:

```
dog['name']
```

这适用于所有属性名，不管它们是否是有效的标识符。

我们还可以使用它们来访问属性，方法是用字符串或符号动态地传递属性名。

可以通过书写来使用点符号:

```
dog.name
```

这更短，但只适用于有效的标识符。

一个对象可以包含另一个对象。

例如，我们可以写:

```
const book = {
  name: 'javascript basics',
  published: 2020,
  author: {
    firstName: 'jane',
    lastName: 'smith'
  }
};
```

`author`属性具有`firstName`和`lastName`属性。

我们可以通过编写以下内容来获得嵌套属性:

```
book['author']['firstName']
```

或者

```
book.author.firstName
```

我们可以混合方括号和点符号。

所以我们可以写:

```
book['author'].firstName
```

# 调用对象的方法

我们可以像调用其他函数一样调用一个方法。

例如，如果我们有以下对象:

```
const dog = {
  name: 'james',
  gender: 'male',
  speak() {
    console.log('woof');
  }
};
```

然后我们可以通过编写调用`speak`方法:

```
dog.speak()
```

# 改变属性/方法

我们可以通过赋值来改变属性。

例如，我们可以写:

```
dog.name = 'jane';
dog.gender = 'female';
dog.speak = function() {
  console.log('she barked');
}
```

我们可以用`delete`操作符从对象中删除一个属性:

```
delete dog.name
```

然后当我们试图得到`dog.name`时，我们得到`undefined`。

# 使用这个值

一个对象有自己的`this`值。

我们可以在对象的方法中使用它。

例如，我们可以写:

```
const dog = {
  name: 'james',
  sayName() {
    return this.name;
  }
};
```

我们返回`this.name`，应该是`'james'`。

因为`this`是`sayName`方法内的`dog`对象。

# 构造函数

我们可以创建构造函数来创建具有固定结构的对象。

例如，我们可以写:

```
function Person(name, occupation) {
  this.name = name;
  this.occupation = occupation;
  this.whoAreYou = function() {
    return `${this.name} ${this.occupation}`
  };
}
```

我们有实例属性`name`、`occupation`和`this.whoAreYou`实例方法。

当我们用构造函数创建一个新对象时，它们都被返回。

然后我们可以使用`new`操作符来创建一个新的`Person`实例:

```
const jane = new Person('jane', 'writer');
```

设置为返回的`Person`实例的`this` os 的值。

我们应该将构造函数的第一个字母大写，这样我们就可以将它们与其他函数区分开来。

我们不应该在没有操作符的情况下调用构造函数。

所以我们不写:

```
const jane = Person('jane', 'writer');
```

这样,`this`的值不会被设置为返回的`Person`实例。

# 全局对象

浏览器中的全局对象是`window`对象。

我们可以用顶层的`var`向它添加属性:

```
var a = 1;
```

那么`window.a`就是 1。

调用不带`new`的构造函数会返回`undefined`。

所以如果我们有:

```
const jane = Person('jane', 'writer');
```

那么`jane`就是`undefined`。

内置全局函数是全局对象的属性。

所以`parseInt`和`window.parseInt`一样。

![](img/b53b11d224a43feaa239e00fe736c554.png)

Photo by [Rushabh Nishar](https://unsplash.com/@rush_nishar?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 结论

我们可以通过两种方式访问对象属性。

此外，我们可以创建构造函数来创建具有固定结构的对象。

![](img/787be6c671be8d345dc786dad8729ce5.png)

[Subscribe to Decoded, our official YouTube channel!](https://www.youtube.com/channel/UCtipWUghju290NWcn8jhyAw)