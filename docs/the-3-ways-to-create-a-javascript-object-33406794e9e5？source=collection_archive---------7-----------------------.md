# 创建 JavaScript 对象的三种方法

> 原文：<https://javascript.plainenglish.io/the-3-ways-to-create-a-javascript-object-33406794e9e5?source=collection_archive---------7----------------------->

## 用 JavaScript 创建对象非常容易

当您考虑 JavaScript 语言中的对象时，只需考虑键值对的哈希表。这些值可以是基元或其他对象。用户定义的本机对象在任何时候都是可变的，因此您可以从一个空白对象开始，并在进行过程中向其添加功能。

> 请记住，JavaScript 中的对象是动态的，这意味着它们可以在代码执行过程中的任何时候发生变化。

![](img/6e9bf14cacac38ffd8a7c35903e15277.png)

Photo by [Max Nelson](https://unsplash.com/@maxcodes?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

如何定义一个 JavaScript 对象？有各种各样的方法。但是…

# 在 JavaScript 中定义对象有三种最简单的方法

## 使用对象文字

在 JavaScript 中定义对象最简单的方法是使用对象文字。考虑下面的例子:

*   首先我们开始一个空白的对象

```
// add an empty object
var person = {}
<< {}
```

*   然后我们添加一个属性

```
// add a property
person.name = "Dang Trung Anh"
```

*   最后添加一个方法

```
// add a method
person.getName = function() { return person.name
}
```

在上面的例子中，我们从一个空对象开始，然后向它添加一个属性和一个方法(函数)。我们创建的对象总是可以修改的，除非我们另外指定，我们将在后面详细讨论。

*   我们也可以完全删除属性

```
// delete a property
delete person.name<< true
```

*   并随着我们的进展添加更多的属性和方法

```
person.go = function () { console.log("I'm walking now.")
}
<< I'm walking now.
```

另一方面，作为完整的示例，我们可以在创建时向对象添加功能。

## 另一种方式，我们可以使用内置的构造函数，但它是反模式的

我们使用 Object()构造函数，现在不鼓励使用。

当传递给 Object()构造函数的值是动态的并且直到运行时才知道时，它的这种行为会导致意外的结果。

*   一个数字对象

```
var o = new Object(10); 
console.log(o.constructor === Number); << true
```

*   一个字符串对象

```
var o = new Object(“This is a string”); console.log(o.constructor === String); 
<< true
```

## 除了以上两种方法，我们可以使用自己的自定义构造函数来创建对象，如下例所示

这里有一个低效的地方，如果我们在任何时候调用一个新的人(…)，一个新的 go()函数就会在内存中创建。我们可以使用 prototype 关键字来更好地改进代码。

```
Person.prototype.go = function () { return “I’m walking now.”}
```

在 JavaScript 中创建一个对象没有最好的方法，这取决于你的用例。例如，如果我们只需要一个类似单例的对象，我们使用第一种方法，如果我们想创建几个类似的对象，我们使用最后一种方法。

感谢阅读！不要忘记👏跟着走。