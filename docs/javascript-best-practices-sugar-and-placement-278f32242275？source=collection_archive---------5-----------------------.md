# JavaScript 最佳实践— Sugar 和 Placement

> 原文：<https://javascript.plainenglish.io/javascript-best-practices-sugar-and-placement-278f32242275?source=collection_archive---------5----------------------->

![](img/3ac5890271e4603431f1b4a5d1a63077.png)

Photo by [Todd Quackenbush](https://unsplash.com/@toddquackenbush?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

像任何类型的应用程序一样，JavaScript 应用程序也必须写得很好。

否则，我们以后会遇到各种各样的问题。

在本文中，我们将研究一些在编写 JavaScript 代码时应该遵循的最佳实践。

# 句法糖

JavaScript 最近的许多变化都是对现有语法的修饰。

有了它们，我们的生活会更轻松。

我们可以在现有代码中使用几个插入式替换。

# `let and const`

我们可以使用`let`和`const`来声明块范围的变量。

它们只有在定义后才可用。

例如，我们可以写:

```
let x = 1;
const y = 2;
```

宣布它们。

那么我们只有在声明之后才使用它们。

`const`变量不能被重新赋值给一个新值，但是如果它是一个对象，我们可以改变它里面的东西。

# 限制函数的范围

我们可以用箭头函数来限制函数的范围。

例如，不要写:

```
function foo(){}
```

它甚至在定义之前就可以在任何地方被调用。

我们写道:

```
const foo = () => {}
```

如果我们不需要函数绑定到自己的`this`，那么我们肯定应该使用箭头函数。

它们也不能在被定义之前被调用。

# 班级

传统的构造函数已经被类所取代。

所以与其写:

```
function Animal(name){
  this.name = name;
}Animal.prototype.speak = function (){
  console.log(this.name);
}
```

我们写道:

```
class Animal {
  constructor(name) {
    this.name = name;
  }
  speak() {
    console.log(this.name);
  }
};
```

一切都在一个地方，它类似于实际的类，即使它只是语法上的糖。

# 命名变量

我们应该用我们能理解的方式命名变量。

例如，不写:

```
let d ;
let elapsed;
```

我们写道:

```
let daysSinceUpdate
```

它们还应该易于发音，这样我们在处理它们时就不会有困难。

所以我们可以这样写:

```
let firstName, lastName
```

# 书写功能

我们应该创建只做一件事的函数。

所以我们应该写这样的函数:

```
const add = (a, b) => a + b;
```

只会增加。

# 使用长的描述性名称

我们上面的名字是描述性的。

他们交流他们的意图。

例如，不要写:

```
function inv(user) {
  //...
}
```

我们写道:

```
function inviteUser(user) {
  //...
}
```

# 避免长参数列表

我们应该在代码中避免长的参数列表。

如果我们需要很多参数，我们应该把它们放在一个对象中。

我们的争论越少，我们就越容易处理。

例如，不写:

```
function getRegisteredUsers(fields, include, fromDate, toDate) { 
  //...
}
```

我们写道:

```
function getRegisteredUsers({ fields, include, fromDate, toDate }) { 
  //...
}
```

我们有一个对象参数，而不是多个参数。

这样，我们仍然有它们的名字，但是我们可以以任何顺序检索它们。

# 减少副作用

我们不应该在代码中有不必要的副作用。

例如，不写:

```
function addItemToCart (item, quantity = 1) {
  cart.set(item.id, quantity)
  return cart
}
```

我们写道:

```
function addItemToCart(cart, item, quantity = 1) {
  const cartCopy = { ...cart };
  cartCopy.set(item.id, quantity)
  return cartCopy;
}
```

我们没有访问函数之外的`cart`，而是复制了一个副本，然后按照我们想要的方式操作它并返回副本。

# 根据递减规则组织文件中的函数

高层次的功能应该在上面，低层次的功能应该在下面。

这样，它们的阅读顺序是合乎逻辑的。

例如，不写:

```
function getFullName(user) {
  return `${user.firstName} ${user.lastName}`
}function renderEmail(user) {
  // ...
  const fullName = getFullName(user)
  return `Dear ${fullName}, ...`
}
```

我们写道:

```
function renderEmail(user) {
  // ...
  const fullName = getFullName(user)
  return `Dear ${fullName}, ...`
}function getFullName(user) {
  return `${user.firstName} ${user.lastName}`
}
```

`getFullName`是`renderEmail`的助手，所以应该在它下面。

![](img/c5d243a40abeb62e636dfdd2e99b1b50.png)

Photo by [Jeremy Bishop](https://unsplash.com/@jeremybishop?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 结论

我们可以使用一些句法糖来使我们的生活更容易。

此外，我们应该更仔细地放置我们的代码，以便以后更容易地使用它。

## **用简单英语写的 JavaScript**

喜欢这篇文章吗？如果有，通过 [**订阅获取更多类似内容解码，我们的 YouTube 频道**](https://www.youtube.com/channel/UCtipWUghju290NWcn8jhyAw) **！**