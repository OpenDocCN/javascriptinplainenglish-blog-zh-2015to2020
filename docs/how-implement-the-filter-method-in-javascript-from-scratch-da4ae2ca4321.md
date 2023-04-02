# 如何在 JavaScript 中从头开始实现 filter 方法

> 原文：<https://javascript.plainenglish.io/how-implement-the-filter-method-in-javascript-from-scratch-da4ae2ca4321?source=collection_archive---------3----------------------->

你可能听说过 JavaScript 的内置函数。如果不是，那就没问题。在本文中，我将向您展示如何从头开始实现您自己的`filter`方法。

一路上，你会学到一些关于原型继承、一等公民的功能和`this`关键字的知识。

![](img/3bdf40d299a206dd16ee10a82cf36aab.png)

Photo by [Nathan Dumlao](https://unsplash.com/@nate_dumlao?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

在 JS 中，`filter`方法被应用于一个数组。它将一个函数作为谓词，并返回一个新数组，其中包含通过该谓词的所有值。

`filter`方法是数组对象中的内置函数，也就是说，你不必创建它。它已经存在，可供您使用。但是，如果您想用新的属性扩展现有的对象，您可以很容易地做到，因为 JS 是一种动态语言(意味着您可以在运行时添加新的属性),只需使用点符号，例如

```
let myObject = {};
console.log(myObject.foo); // undefined
myObject.foo = "a";
console.log(myObject.foo); // a
```

但是让我们说，你不想只扩展到一个具体的对象，而是扩展到对象的整个超类型，例如一个数组？

在像 C++或 Java 这样的语言中，你可以扩展这个类。但是，在 JS 中没有类的概念，只有我们所说的`prototype`。注意在 ES6 中引入了`class`类型，但它只是一个抽象层。底下你还有`prototype`。但是什么是`prototype`？

每个对象都有一个隐藏的属性叫做`prototpye`。`property`本身是一个对象，也有一个`prototype` 属性等等。这被称为“原型链”。原型链的末端是`null`对象。他是唯一没有`prototype`属性的对象。但是我们怎么处理财产呢？

因为原型链中的所有对象(也包括数组)都将继承`prototype`属性中的所有内容，所以我们可以将我们的自定义函数扩展到`prototype`，并且可以预期每个具体对象都将拥有我们的自定义函数:

```
Array.prototype.helloWorld = () => console.log("All arrays will have this new property");
const arr = [];
console.log(arr.helloWorld()); // All arrays will have this new property
```

有了`prototype`，我们现在知道如何将一个属性扩展到一个对象的所有超类型。让我们实现我们的自定义过滤器功能。我们将我们的自定义过滤函数命名为`fooFilter`

因为 JS 也是一种函数式编程语言，它可以把其他函数作为参数。这个属性在 JS 语言中被称为一等公民。这意味着函数被当作任何其他变量对待，也就是说，你可以把它作为参数传递，你可以把它赋给一个变量，或者一个函数也可以返回其他函数。

```
Array.prototype.fooFilter = function(checkElement) {
  const returnArr = [];
  // ... loop through all elements in the array 
  return returnArr;
};
```

函数`checkElement`检查数组中的每一项。如果它满足某个条件，即如果函数返回`true`(实际上是`truthy`)，那么它将把元素放入一个(新的)数组。最后它会返回这个数组。但是我们如何访问这个元素呢？

您可以通过`this`关键字访问这些元素。因为在 JS 中`this`指的是调用`this`所在函数的对象。

```
Array.prototype.fooFilter = function(checkElement) {
  const returnArr = [];
  for (let i = 0; i < this.length; i++) {
    if (checkElement(this[i])) {
      returnArr.push(this[i]);
    }
  }
  return returnArr;
};
```

假设我们有一个数字数组`arr`,想要过滤所有偶数:

```
const arr = [1,3,5,6,9,11,26,8,35,10,7,42,88];
console.log(arr.fooFilter(x => x % 2 === 0)); // [6,26,8,10,42,88]
```

就是这样！我们刚刚实现了自己的`filter`函数。你有什么问题吗？只是让我知道。