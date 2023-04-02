# 您应该避免的 8 个常见 JavaScript 错误

> 原文：<https://javascript.plainenglish.io/8-common-javascript-mistakes-that-you-should-avoid-bbbfe1fb9539?source=collection_archive---------0----------------------->

## 几乎每个开发人员在职业生涯中都犯过哪些错误？

![](img/d915328845b2ad914bffe878969c4466.png)

Photo by [Varvara Grabova](https://unsplash.com/@santabarbara77?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

JavaScript 是我职业生涯中迄今为止学到的最灵活的语言。然而，灵活性是一件好事吗？不完全是。这种灵活性在某种程度上会让我们感到困惑。

在我的日常工作中，我经常看到同样的错误被一遍又一遍地犯着，为了让你不陷入同样的陷阱，我将它们列在下面。

让我们开始吧。

# 1.块级作用域

看看下面的例子:

```
function justLog(inside) {
  if (inside) {
    var message = ‘this is a message’;
    console.log(‘Inside block:’, message);
  }

  console.log(‘Outside block:’, message);
}justLog(true);
```

你认为屏幕上会打印出什么结果？

第二个 **console.log** 记录变量**消息**，该消息在它定义的块之外。你可能认为结果会是:

```
Inside block: this is a message
error: Uncaught ReferenceError: message is not defined
```

但事实并非如此。由于 **var** 关键字的能力，正确的结果是:

```
Inside block: this is a message
Outside block: this is a message
```

很困惑，不是吗？

我不再使用 **var** 关键字了。相反，我使用 **let** 。在我看来，**让**比 **var** 更有意义。并且，你可以通过使用 **let** 来避免一些常见的错误。

# 2.错误地使用“this”

在 JavaScript 中，关键字 **this** 有时会令人困惑。当你在没有清楚理解上下文的情况下使用这个时，它很可能不是你所期待的这个。

尤其是当你用**这个**来回调或者闭包的时候。看一看:

```
var Game = {
  level: 1,
  start: function() {
    console.log(‘Start level ‘ + this.level);
  },
  finish: function() {
    setTimeout(function() {
      console.log(‘Finish level ‘ + this.level);
    }, 5);
  }
}Game.start(); // Start level 1
Game.finish(); // Finish level undefined
```

在调用 **Game.finish()** 之后，我们得到了**未定义的**作为**本级**的结果。为什么？因为我们在 **setTimeout** 函数中使用了**这个**，它在**窗口**对象**的上下文中。在那个上下文中没有变量**级别**。**

您可以使用的解决方案之一是将对**和**的引用保存在一个变量中，如下所示:

```
Game.finish = function() {
  self = this;
  setTimeout(function() {
    console.log(‘Finish level ‘ + self.level);
  }, 5);
}
```

# 3.不要使用严格模式

使用严格模式应该是经验法则。我不知道为什么有些开发人员不把这个微小但强大的声明放在文件的开头。

**‘用严’**有助于去除无声的错误。

**‘使用严格’**使调试更容易。

**‘使用严格’**保护你的代码。

**‘使用严格’**防止意外赋值。

别忘了用它。

# 4.创建意外的全局变量

无论您是初级还是高级开发人员，有时都会面临内存泄漏。这是一个实际问题。

常见的漏洞是当您定义一个意外的全局变量时。例如:

```
function sayHello() {
  message = ‘Hello’;
  console.log(message);
}
```

变量**消息**定义时没有关键字 **let** 或 **var** 。因此，在全局对象内部将创建一个变量**消息**。在大多数情况下，它是**窗口**对象。结果，创建了一个意外的全局变量，它没有被垃圾收集器清除。

要解决这个问题，只需在每个 JavaScript 文件的开头添加*‘use strict’*。它将防止意外的全局变量。

# 5.在一个功能中处理太多任务

有些开发人员喜欢在一个函数中放很多东西。如果你是他们中的一员，马上停止。

真正的开发人员应该将长函数分解成小函数，并切中要点。编写这样的函数有一些好处:

*   它们更容易维护。
*   它们更容易阅读和理解。
*   通过将公共操作转移到单独的功能中来提高代码的可重用性。
*   调试一小段代码比调试一段复杂的长代码更容易。

请记住，如果在一个函数中执行了多个任务，请考虑将其分成更小的任务。

# 6.平等

JavaScript 中我们有三种类型的等号运算符: **=** 、 **==** 、**= =**。第一个用于赋值，其余的用于比较。

JavaScript 是一种灵活的语言。在其他语言中，不能将赋值运算符' = '与 **if** 语句一起使用。在 JavaScript 中，你可以。这就是困惑发生的时候。

例如:

```
if (sum = 2) {
  // Do something
}
```

这里没有错误语法。如果上面的的结果总是真的。虽然您打算将变量 **sum** 与 **2** 进行比较，但使用错误的运算符会导致意想不到的结果。

尽管为了方便起见，您可以在 if 语句中使用' = '，但是不要这样做。在比较的时候，总是用“==”或者“===”。

# 7.不匹配的引号和花括号

当把一个字符串赋给一个变量时，你输入了开始的引号，然后输入了字符串值，不知怎么的你忘记了结束的引号。避免这种错误的最好方法是同时键入左引号和右引号，然后将值放在它们之间。

例如:

```
let message = ‘’;
```

相同的实现应该应用于花括号:

```
function sayHello() {}
```

请记住，如果您要打开一个元素，您需要尽快关闭它。

# 8.未定义 vs 空

下面的代码会有什么问题？

```
if (person !== null && typeof person !== ‘undefined’) {
  // Do something
}
```

那就是如果没有定义 **person** object，就会出错。你应该改变条件的顺序。在使用一个对象之前，总是先检查它是否被定义。

```
if (typeof person !== ‘undefined’ && person !== null) {
  // Do something
}
```

# 结论

JavaScript 是一种奇怪的语言。我们经常犯的错误有时也很奇怪。记住上面的错误，避免进一步的问题。

当然，在你的职业生涯中没有办法避免所有的错误。作为失败者，你犯的错误越多，你就越有经验。

希望你喜欢这个故事！

如果你能在下面的评论中提到你作为 JavaScript 开发人员所犯的常见错误以及你是如何处理它们的，那就太好了。

# 进一步阅读

[](https://medium.com/javascript-in-plain-english/11-javascript-concepts-every-web-developer-should-know-to-take-their-skills-to-the-next-level-37ef6693111a) [## 每个 Web 开发人员都应该知道的 11 个 JavaScript 概念，让他们的技能更上一层楼

### 不了解这些概念，就无法掌握 JavaScript。

medium.com](https://medium.com/javascript-in-plain-english/11-javascript-concepts-every-web-developer-should-know-to-take-their-skills-to-the-next-level-37ef6693111a) ![](img/787be6c671be8d345dc786dad8729ce5.png)

[Subscribe to Decoded, our official YouTube channel!](https://www.youtube.com/channel/UCtipWUghju290NWcn8jhyAw)