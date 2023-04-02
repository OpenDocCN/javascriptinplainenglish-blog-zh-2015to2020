# JavaScript 提示—侦听器、排序和嵌套对象

> 原文：<https://javascript.plainenglish.io/javascript-tips-listeners-sorting-and-nested-objects-a81a9755fe65?source=collection_archive---------11----------------------->

![](img/6270e2e939d808ccdae49af6155fd7b2.png)

Photo by [Martin Schmidli](https://unsplash.com/@martin_schmidli?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

像任何类型的应用程序一样，当我们编写 JavaScript 应用程序时，有一些困难的问题需要解决。

在本文中，我们将研究一些常见 JavaScript 问题的解决方案。

# addEventListener vs onclick

`addEventListener`和`onclick`是等价的。

然而，我们可以使用`addEventListener`来监听来自多个 DOM 元素的事件。

例如，如果我们有:

```
element.addEventListener('click', () => { 
  //...
}, false);
```

然后我们可以听听`element`的`click`事件。

我们也可以写:

```
element.onclick = () =>
  //...
}
```

做同样的事情。

我们可以使用`event`对象来监听多个事件。

例如，我们可以写:

```
element.addEventListener('click', (event) => { 
  //...
}, false);
```

然后我们可以使用`event`对象从`element`的子元素中获取事件数据，并检查动作。

# 从字符串中去除所有非数字字符

我们可以用`replace`方法从字符串中去掉所有非数字字符。

例如，我们可以写:

```
const s = "123 abc".replace(/[^\d.-]/g, ''); 
```

我们使用`^\d`来查找所有非数字字符。

然后我们添加`g`标签来搜索字符串中模式的所有实例。

# 如何按日期属性对数组排序

要按`Date`属性对数组进行排序，我们可以使用`sort`方法。

例如，我们可以写:

```
array.sort((a, b) => {
  return new Date(b.date) - new Date(a.date);
});
```

我们将日期字符串转换成`Date`实例，然后减去它们得到排序顺序。

我们可以这样做，因为减法操作符将把`Date`实例转换成时间戳，时间戳是整数。

# 按值对对象属性进行排序

为了按值对对象属性进行排序，我们可以用`sort`方法按值对键进行排序。

例如，我们可以写:

```
const list = {"baz": 200, "me": 75, "foo": 116, "bar": 15};
keysSorted = Object.keys(list).sort((a, b) => {
  return list[a] - list[b];
})
console.log(keysSorted);
```

我们有了`list`对象。

我们用一个数组`Object.keys`来获取密钥。

然后我们可以在返回的数组上调用`sort`。

# Moment.js 转换为日期对象

我们可以使用`toDate`方法将力矩对象转换成`Date`对象。

例如，我们可以写:

```
moment().toDate();
```

# 测试嵌套 JavaScript 对象键是否存在

我们可以用递归函数来检查一个键。

例如，我们可以写:

```
const checkNested = (obj, level,  ...rest) => {
  if (obj === undefined) {
     return false;
  }

  if (rest.length === 0 && obj.hasOwnProperty(level)) {
    return true;
  }  
  return checkNested(obj[level], ...rest)
}
```

我们检查`obj`是否是`undefined`，如果是则返回`false`，因为在我们到达路径的终点之前是`undefined`。

否则，我们检查是否没有剩余的参数，并检查带有键`level`的属性是否存在。

如果存在，那么我们返回`true`。

否则，我们通过移动到较低的级别再次调用`checkNested`函数。

所以我们可以这样称呼它:

```
checkNested(foo, 'level1', 'level2', 'bar'); 
```

检查`foo.level1.level2.bar`是否存在。

为了使我们的生活更容易，我们也可以使用`?.`可选链接操作符:

```
const value = obj?.level1?.level2?.level3
```

如果不存在任何中间层，它将返回`undefined`。

# 用 Javascript 播放音频

我们可以使用音频元素的`play`方法用 JavaScript 播放音频。

例如，我们可以写:

```
const audio = new Audio('audio.mp3');
audio.play();
```

或者:

```
document.getElementById('audioPlayer').play();
```

# 用 JavaScript 定义一个类

我们可以用两种方式定义一个类。

我们可以创建一个构造函数或者使用类语法。

例如，我们可以写:

```
function Person(name) {
  this.name = name;
}
```

创建一个`Person`构造函数。

然后，为了添加方法，我们编写:

```
Person.prototype.greet = function() {
  console.log(`hello ${this.name}`);
}
```

我们可以使用类语法来做同样的事情:

```
class Person {
  constructor(name) {
    this.name = name;
  } greet() {
    console.log(`hello ${ this.name }`);
  }
}
```

我们在类中定义实例方法，而不是给`prototype`添加一个属性。

它更干净，静态分析可以检查它的语法。

对于来自基于类的语言的人来说也更容易理解。

两种方式都是一样的。

![](img/4a27f23ecf1e336066b8ed51512e0c49.png)

Photo by [Andrea Lightfoot](https://unsplash.com/@andreaelphotography?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 结论

我们可以用多种方式定义类。

此外，我们可以通过使用可选的链接操作符或我们自己的函数来测试嵌套对象键的存在。

有各种各样的方法来分类。

## 简单英语的 JavaScript

你知道我们有四份出版物和一个 YouTube 频道吗？在[**plain English . io**](https://plainenglish.io/)和 [**找到它们订阅解码，我们的 YouTube 频道**](https://www.youtube.com/channel/UCtipWUghju290NWcn8jhyAw) **！**