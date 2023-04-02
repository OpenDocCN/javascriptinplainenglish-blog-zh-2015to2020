# 有 4 种方法可以消除 JavaScript 中干净代码的肮脏副作用

> 原文：<https://javascript.plainenglish.io/4-ways-you-can-get-rid-of-dirty-side-effects-for-cleaner-code-in-javascript-e93c50ff2355?source=collection_archive---------6----------------------->

## 从长远来看，副作用造成了巨大的精神负担

![](img/3ad2f948242e7aaf2bb8a0552940d46d.png)

Photo by [Pierre Châtel-Innocenti](https://unsplash.com/@chatelp?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

分析显示，开发者平均每 1000 行代码会产生 70 个 bug。因此，他将 75%的时间花在调试上。太可悲了！

bug 的产生有多种方式。制造副作用就是其中之一。

有人说副作用是邪恶的，有人说不是。

我在第一组。副作用应该被认为是邪恶的。我们应该瞄准副作用免费代码。

你可以用以下四种方法来实现你的目标。

# 1.严格使用；

**略加利用严格；**到你文件的开头。这个特殊的字符串将打开代码验证，并防止您在没有首先声明变量的情况下使用变量。

# 2.环

请看下面的例子:

```
var names = [‘amy’, ‘james’, ‘anna’, ‘joey’];for (var i = 0; i < names.length; i++) {
  console.log(names[i]);
}
```

在这个例子中，什么会引起副作用？

请注意索引变量 **i** ，它不仅在 **for loop** 范围内可用，而且在外部范围内也可用。

如果将变量 **i** 的值记录在 for 循环的正下方，则应为 4。

```
for (var i = 0; i < names.length; i++) {
  console.log(names[i]);
}console.log(i); // 4
```

为了避免这种副作用，我们目前有两个选择。

**选项 1:** 使用 **forEach**

```
names.forEach(name => {
  console.log(name);
});
```

使用 **forEach** ，我们不再需要定义索引变量。所以，没有更多的副作用。

**选项 2:** 比起 **var** 更喜欢使用 **let**

ES6 附带了很多有用的特性。**让**关键词就是其中之一。

当你用 **let** 定义一个变量时，它只在当前范围内可用。

```
for (let i = 0; i < names.length; i++) {
  console.log(names[i]);
}console.log(i); // error: Uncaught ReferenceError: i is not defined
```

如图所示，如果在定义的范围之外使用 **i** ，则会导致错误。

# 3.立即调用函数表达式(IIFE)

IIFE 是一个 JavaScript 函数，一旦定义就运行。

IIEF 我发现的一个好处是，你可以把变量隐藏在函数内部，避免它们泄露出去，这样就不会发生副作用。

```
const Calculation = (function () {
  const PI = 3.14;
  let mode = 1;

  return {
    sum: function (a, b) {
      return a + b;
    }, changeMode: function (newMode) {
      this.mode = newMode;
    }, printMode: function () {
      console.log(this.mode);
    }
  };
})();let sum = Calculation.sum(2, 6); // 8
Calculation.changeMode(4);
Calculation.printMode(); // 4
```

如果要修改变量，唯一的方法是通过返回的 IIEF 函数更改值，这将避免副作用。

# 4.纯函数

[纯函数](https://medium.com/javascript-in-plain-english/principles-of-functional-programming-in-javascript-that-will-make-your-coding-life-easier-ed1416d2b726)是用相同的输入返回相同的输出的函数。

也就是说，一个纯函数只依赖于它自己的参数，不会改变它作用域之外的变量，所以不会发生副作用。

以下面这个不纯函数为例:

```
let person = {
  name: ‘Amy’,
  age: 28
};function change(person) {
  let someone = person;
  someone.name = ‘James’; return someone;
}let someone = change(person);
console.log(someone.name) // James
console.log(person.name); // James
```

上面的函数是不纯的，因为它改变了变量 **person** 的值，这个变量在函数之外。那是副作用。

为了防止副作用，我们需要将该功能转换为纯功能:

```
let person = {
  name: ‘Amy’,
  age: 28
};function change(person) {
  let someone = {...person}
  someone.name = ‘James’;

  return someone;
}let someone = change(person);
console.log(someone.name) // James
console.log(person.name); // Amy
```

现在，它是纯净的。

大多数时候，为了减少副作用，最好为不太令人惊讶的错误创建纯函数。

我们不需要把所有的函数都做纯。我们可以根据需要混合纯函数和不纯函数，因为不是所有不纯函数都是不好的。有时我们需要它们为特定的目的服务。

# 结论

想象你的代码库是没有副作用的。如果有任何问题，您可以更快地进行调查。您将轻松快速地编写单元测试。更多的单元测试意味着你增加了你的应用程序没有错误的几率。

希望你喜欢这篇文章。

你有其他方法防止副作用吗？如果有，请在下面的评论中分享它们。

# 进一步阅读

[](https://medium.com/javascript-in-plain-english/9-tips-for-writing-scalable-javascript-code-e6bcfc791882) [## 编写可伸缩 JavaScript 代码的 9 个技巧

### 您应该从一开始就准备好扩展您的项目

medium.com](https://medium.com/javascript-in-plain-english/9-tips-for-writing-scalable-javascript-code-e6bcfc791882)