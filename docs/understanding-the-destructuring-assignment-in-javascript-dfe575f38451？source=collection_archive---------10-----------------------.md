# 理解 JavaScript 中的析构赋值

> 原文：<https://javascript.plainenglish.io/understanding-the-destructuring-assignment-in-javascript-dfe575f38451?source=collection_archive---------10----------------------->

## 通过实例了解 JavaScript 中的析构

![](img/f1d3a277c934e587fe22dd8004c29b19.png)

Photo by [Nicole Wolf](https://unsplash.com/@joeel56?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 介绍

析构赋值是 ES6 中引入的一种特殊语法，用于灵活地赋值直接来自对象的值。它是一个 JavaScript 表达式，可以将数组中的值或对象中的属性解包到不同的变量中。析构使得以一种简单的方式编写更干净的 JavaScript 代码变得更加容易。因此，您的代码将易于阅读和维护。

在本文中，我们将学习 JavaScript 中的析构赋值。让我们开始吧。

![](img/a291d1948cacc1dcf687abc52f1595f2.png)

Image created with ❤️️ By author.

# 使用析构从对象中提取值

我们可以使用析构赋值从对象中提取值，但是首先，让我们看看在 ES5 中我们是如何做的。

这里有一个例子:

```
const user = { name: 'John Doe', age: 34 };

const name = user.name; //Prints: name = 'John Doe'
const age = user.age; //Prints: age = 34
```

下面是一个使用 ES6 析构语法的等效例子:

```
**const { name, age } = user;**
//Prints: name = 'John Doe', age = 34
```

如你所见，`name`和`age`变量将被创建，并被赋予来自`user`对象的各自的值。使用 ES6 析构要简单和干净得多。您可以从对象中提取任意多或任意少的值。

# 从对象分配变量

析构赋值允许我们从对象中赋值变量。

下面是一个如何在赋值中给新变量名的例子:

```
const { **name: userName, age: userAge** } = user;// userName = 'John Doe', userAge = 34
```

你可以把它理解为“获取`user.name`的值，并把它赋给一个名为`userName`的新变量”。

我们也可以使用 ES6 析构来分配来自**嵌套对象的变量。**我们来看看`johnDoe`下面的嵌套对象，试着从中分配变量。

这里有一个例子:

```
const user = {
  **johnDoe**: { 
    age: 34,
    email: 'johnDoe@gmail.com'
  }
};
```

下面是如何提取对象属性的值并将它们分配给同名变量:

```
const { **johnDoe:** **{ age, email }**} = user;
```

通过使用析构赋值，你可以随心所欲地处理嵌套对象。

# 从数组中分配变量

ES6 使得析构数组像析构对象一样容易。看看下面的例子:

```
const [a, b] = [1, 2, 3, 4, 5, 6];
console.log(a, b); // 1, 2
```

我们还可以通过使用逗号来访问数组中任意索引处的值，以达到所需的索引。

这里有一个例子:

```
const [a, b**,,,** c] = [1, 2, 3, 4, 5, 6];
console.log(a, b, c); // 1, 2, 5
```

如你所见，这是一种从数组中分配变量的简单方法。

# 将对象作为函数的参数传递

在某些情况下，您可以在函数参数本身中析构对象。

看看下面的例子:

```
const profileUpdate = (**{ name, age, nationality, location }**) => { /* do something with these fields */
}
```

当`profileData`被传递给上述函数时，这些值被从函数参数中析构出来，以便在函数中使用。

您可以通过下面的示例实现同样的目的:

```
const profileUpdate = (profileData) => {
  const **{ name, age, nationality, location }** = profileData;
  // do something with these variables
}
```

这有效地析构了发送到函数中的对象。

# 结论

正如您所看到的，JavaScript 中的析构赋值是您应该知道的重要且有用的 ES6 特性之一。当使用像 React or 或 Vue 这样的框架时，您会非常需要它。

谢谢你阅读这篇文章，我希望你觉得它有用。如果是这样，通过 [**订阅解码得到更多类似内容，我们的 YouTube 频道**](https://www.youtube.com/channel/UCtipWUghju290NWcn8jhyAw?sub_confirmation=true) **！**

# 更多阅读

[](https://medium.com/javascript-in-plain-english/5-amazing-front-end-development-tools-that-you-should-know-7372dc377d7) [## 您应该了解的 5 种令人惊叹的前端开发工具

### 每个开发人员都应该知道有用的前端开发工具

medium.com](https://medium.com/javascript-in-plain-english/5-amazing-front-end-development-tools-that-you-should-know-7372dc377d7)