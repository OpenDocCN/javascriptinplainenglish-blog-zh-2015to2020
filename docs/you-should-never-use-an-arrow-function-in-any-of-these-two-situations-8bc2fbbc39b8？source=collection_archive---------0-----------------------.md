# 避免箭头功能的两种情况

> 原文：<https://javascript.plainenglish.io/you-should-never-use-an-arrow-function-in-any-of-these-two-situations-8bc2fbbc39b8?source=collection_archive---------0----------------------->

## JavaScript 中箭头函数的良好实践

![](img/62949496b794d913a3933e9117560afe.png)

Photo by [Brayden Law](https://www.pexels.com/@braydenlaw?utm_content=attributionCopyText&utm_medium=referral&utm_source=pexels) from [Pexels](https://www.pexels.com/photo/photo-of-woman-sitting-on-rooftop-1738975/?utm_content=attributionCopyText&utm_medium=referral&utm_source=pexels)

箭头函数表达式很棒，它们是函数中最小的语法，但是当它们用在这两个比你想象的要频繁得多的地方时，我被咬了一口。

# 测试框架

编写比测试框架超时阈值更长的测试用例是很常见的，这就是为什么测试框架允许您指定测试超时的原因；例如，Mocha 允许您灵活地提供自己的每次测试超时。

将自己置于以下简化的场景中:

```
describe('Your suite', () => {
  it('Your test', (done) => {
    setTimeout(done, 2005);
    assert.ok(true); // default timeout for Mocha is 2000ms
  });
});
```

![](img/b02482c548c3dd7c6f4d9718f3f66bcf.png)

这个测试将返回:`Error: Timeout of 2000ms exceeded.`，Mocha 允许您做的是设置`this.timeout`来控制测试用例超时阈值，就像这样:

```
describe('Your suite', () => {
  it('Your test', (done) => {
    this.timeout(3000); // set timeout threshold
    setTimeout(done, 2005);
    assert.ok(true); // default timeout for Mocha is 2000ms
  });
});
```

这样测试就通过了:

![](img/cf3f15db9b9956d69b8a5781f8951f33.png)

`TypeError: this.timeout is not a function`等等，什么？

像 Mocha 这样的测试框架依赖于 bind，通过`this`，即:`this.skip` `this.timeout(1000)`传递配置，但是 Arrow 函数表达式不能是 bind，这意味着我们需要编写一个函数表达式。

这是一个如此普遍的痛点，以至于[摩卡文档有一个章节](https://mochajs.org/#arrow-functions)阻止你使用箭头函数。

> 不鼓励将[箭头函数](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Functions/Arrow_functions)(又名“lambdas”)传递给 Mocha。Lambdas 词汇绑定`*this*`并且不能访问 Mocha 上下文。

让我们用关键字`function`再试一次:

```
describe('Your suite', () => {
  it('Your test', function (done) { // <- function keyword
    this.timeout(3000); // set timeout threshold
    setTimeout(done, 2005);
    assert.ok(true); // and it finally passes the test!! 👏
  });
});
```

![](img/0ffaf7f0a6d2366b135ed97570f31f42.png)

值得庆幸的是，并非所有的框架都是一样的，其他测试框架将超时阈值作为第三个参数，省去了您记住这个微小但令人讨厌的细微差别的麻烦。

我给你的建议是:用函数表达式编写所有的测试用例函数，如果你处于类似的情况，这带来三个主要的好处:

1.  你忘记了这个案例，你已经有太多的领域边缘案例了，为什么还要增加一个额外的编程边缘案例呢？
2.  你的代码更加统一，所有的测试用例都会写成函数表达式，而不是只写那些需要`this`的；统一的好处超过少写 4 个字符的好处。
3.  当你需要这些特性时，避免改变语法，然后再考虑第二点。

# **预计吊装位置**

你可能用过这种模式，顺便说一下，这很有趣:

```
function bigFn(arg) {
  const quickFn = (...) => (...); // keep the one line outside for clean code
  const goodName = quickFn(...);
}
```

那很好，不是吗？它允许你命名所有这些一行程序，并给你的代码带来更多的意义，+1 来清理代码，你做得很好，我是认真的💛。

我要向你展示的是当我们严重依赖这种模式而不是使用函数表达式时会发生什么，你可能自己也经历过。

当编写 JavaScript 时，我们可能会如此习惯于提升，以至于我们不再注意它拯救我们的所有情况，当它不这样做时，可能需要一段时间才能理解发生了什么。如果你想知道什么是提升，我将提供一个快速的定义:

> **提升**是代码执行前将所有声明移动到作用域顶部的默认行为。

提升允许你编写和使用函数表达式，而不用考虑什么先来，这是 JavaScript 最好的部分之一。

看一下下面的场景:

```
function bigFn(arg) {
  // ....
  const goodName2 = quickFn2(args); // quickFn2 is not defined at this point 😕
  // ....
  const quickFn2 = (...) => (...);
}
```

用这种模式声明的函数对于一行程序来说很好，但是它们失去了 JavaScript 的部分动力。

碰巧用`const`关键字声明的变量确实被提升了，但是它们没有被初始化，当它们的词法绑定在运行时被评估时，它们将被初始化，英语:

JavaScript 引擎必须计算赋值操作符(`=`)的右边，并将其赋值给左边，这样引用才被认为是初始化的并且可用。这只会在运行时发生，按顺序，最终迫使你在声明变量之后使用用`const`关键字声明的变量，否则，你会得到一个引用错误，因为你正在访问一个未初始化的变量。

**这种模式在遗留代码、意大利面条式代码或大文件中尤其危险**，这些代码倾向于使用在任何时间任何地方声明的函数，严重依赖于 JavaScript 函数性质，你将增加另一个可能出错的东西。

在这些情况下的一个经验法则是:坚持使用你可以在任何地方使用的普通函数，不要担心在运行时可能发生的引用错误，这种错误在非常罕见的情况下，在最烦人的时候，比如星期五下午离开办公室之前。

严重依赖这种模式会将您的代码库带回到顺序编程，迫使您在顶层定义所有函数以便以后使用。

JavaScript 已经努力让你使用在文件中任何地方定义的函数表达式，为什么还要担心函数初始化呢？不要因为在合适或有疑问的时候使用好的旧函数表达式而不使用箭头函数，就认为自己是个自食其果的人。

# 奖金

## 关于箭头函数表达式的主体

箭头函数表达式不需要有主体，它们可以直接计算表达式并返回它的值，而不需要`return`关键字，所以你可以写这样的东西:

```
const sum = (a, b) => a + b;
```

雄伟，不是吗？有效的代码。直到它不存在。最糟糕的是，它可能会失败。实际上，我们在这里谈论的是 JavaScript，一切都可能失败。

当某些东西不能相加时(双关语)，您需要在函数体中放置工具，它变成这样:

```
const sum = (a, b) => { // <- we need to add body
  console.log(a, b); // Please, consider that production is 1000x times more complex
  return a + b; // <- we need to use the return keyword
}
```

这里发生的情况是，我们现在处于一种需要我们重构语法的情况，从立即返回一个表达式变成一系列语句，使用`return`关键字返回一个表达式。

在您完成调试并最终修复错误之后，您将删除主体并再次创建一个一行程序。**这个过程重复了很多次**。当有人忘记返回到一行程序时，它甚至可能是您 repo 中的一个持续差异。

如果你最终调试了一个单行的 Arrow 函数表达式:**保留 body** ，不要再费心去移除它，你所经历的将会再次发生，由你或你的一个队友来完成，为什么要浪费时间和为此制造分歧。它可能看起来紧凑而时尚，但是如果不断地添加和移除身体会降低你的团队的生产力，那就不要这样做。

如果您的 Arrow 函数是一个复杂的表达式，容易受到调试工具的影响，不要偷懒，只写整个函数体，也不要过于主动，在调试后删除函数体，这样您就可以不用担心，为您的团队节省那些不断的更改。

## 箭头函数表达式的完美位置

**箭头函数表达式的最佳位置是不可重用的特定位置函数**，比如`array.map` `array.filter`的谓词。

每个工具都可能被滥用，带来更多的伤害，你发现过其他不好的箭头函数表达式吗？请在下面留下评论👇

## 无耻的插头

我希望我的旅程能对你有所帮助。如果你喜欢这篇文章，也许你也会喜欢其他的:

1.  [由测试用例叙述:Redux 动作重试](https://medium.com/javascript-in-plain-english/narrated-by-test-cases-redux-action-retry-b2e004c4c823)。
2.  [为我改变 JavaScript 的 6 个工具](https://medium.com/javascript-in-plain-english/the-6tools-that-changed-javascript-for-me-3ee1faf40585)。
3.  [管理层如何限制你作为开发人员的潜力](https://medium.com/better-programming/how-management-is-limiting-your-potential-as-a-developer-abb46f18e097)。

如果你喜欢这篇文章，请[订阅我的时事通讯](http://eepurl.com/hg7AeP)，在 [Instagram @codingedgar](https://www.instagram.com/codingedgar/) 上关注我，我会在那里发布关于编程概念的漂亮图片，在 [Twitter @codingedgar](https://twitter.com/codingedgar) 上关注我，我会在那里谈论……事情。测试你的代码，刷牙，留下一些掌声和美好的评论💬

## 用简单英语写的 JavaScript 的注释

我们已经推出了三种新的出版物！请关注我们的新出版物: [**AI in Plain English**](https://medium.com/ai-in-plain-english) ，[**UX in Plain English**](https://medium.com/ux-in-plain-english)，[**Python in Plain English**](https://medium.com/python-in-plain-english)**——谢谢，继续学习！**

**我们也一直有兴趣帮助推广高质量的内容。如果您有一篇文章想要提交给我们的任何出版物，请发送电子邮件至[**submissions @ plain English . io**](mailto:submissions@plainenglish.io)**，使用您的 Medium 用户名，我们会将您添加为作者。另外，请让我们知道您想加入哪个/哪些出版物。****