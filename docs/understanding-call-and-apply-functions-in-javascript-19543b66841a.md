# 理解 JavaScript 中的调用和应用函数

> 原文：<https://javascript.plainenglish.io/understanding-call-and-apply-functions-in-javascript-19543b66841a?source=collection_archive---------9----------------------->

## 用 JavaScript 调用和应用实例

![](img/49070bc1c84100b12787a0c2dee7a0e2.png)

Image created with ❤️️ By [Mehdi Aoussiad](https://mehdiouss315.medium.com/).

# 介绍

在 JavaScript 中，所有的函数都是对象方法。通过调用和应用，您可以编写一个可用于不同对象的方法。

在本文中，我们将学习 JavaScript 中的调用和应用函数。让我们开始吧。

# 呼叫方法

方法`call()`是一个预定义的 JavaScript 方法。它可以用来调用一个拥有者对象作为参数的方法。

此方法允许一个对象使用属于另一个对象的方法。

这里有一个例子，我们调用了`person`的方法 **fullName** ，在对象`intro`中使用它:

```
var person = {
  **fullName**: function() {
    return this.firstName + " " + this.lastName; 
// the keyword   'this' refers to the owner(object person).
  }
}
var intro = {
  firstName:"John",
  lastName: "Doe"
}
person.fullName.**call**(**intro**);  // Will return "John Doe"
```

如你所见，方法`call()`允许我们在对象`intro`中使用方法`fullName`。

# 带参数的调用方法

方法`call()`接受传递给它的参数。如果我们想在另一个对象中调用的方法也有参数，这将非常有用。

看看下面的例子:

```
var person = {
  fullName: function(**city, country**) {
    return this.firstName + " " + this.lastName + "," + city + "," + country;
  }
}
var intro = {
  firstName:"John",
  lastName: "Doe"
}
person.fullName.**call(intro, "New York", "USA")**; // Returns: John Doe,New York,USA
```

方法`fullName`中有参数。因此，我们能够通过向`call`函数添加参数来使用它们(参数)。

# 该应用方法

方法`apply()`非常类似于`call()`方法。唯一的区别是`call`单独接受参数，而方法`apply`接受数组形式的参数。

如果您想使用数组而不是参数列表，apply 方法非常方便。

这里有一个例子:

```
var person = {
  fullName: function(city, country) {
    return this.firstName + " " + this.lastName + "," + city + "," + country;
  }
}
var intro = {
  firstName:"John",
  lastName: "Doe"
}
**person.fullName.apply(intro, ["London", "UK"]);** // Returns: John Doe,London,UK
```

看看下面的例子，我们在一个数组上模拟了一个方法`Max`。

```
**Math.max.apply(null, [1,2,3]**); // Will return 3
```

因为 JavaScript 数组没有 max 方法，所以我们应用了`Math.max()`方法。

# 结论

Apply 和 call 是 JavaScript 中非常有用的函数。您可以使用它们来编写可用于不同对象的方法。这可以在没有类继承或构造函数的情况下完成。

感谢您阅读本文，希望您觉得有用。

# 更多阅读

[](https://medium.com/javascript-in-plain-english/5-ways-to-write-a-clean-javascript-using-the-spread-operator-f03a3d5f6340) [## 使用 Spread 运算符编写简洁 JavaScript 的 5 种方法

### 带有实际例子的扩展算子

medium.com](https://medium.com/javascript-in-plain-english/5-ways-to-write-a-clean-javascript-using-the-spread-operator-f03a3d5f6340)