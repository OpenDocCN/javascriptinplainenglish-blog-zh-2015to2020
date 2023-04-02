# 编写更健壮代码的 JavaScript 最佳实践——避免坏代码

> 原文：<https://javascript.plainenglish.io/javascript-best-practices-for-writing-more-robust-code-avoiding-bad-code-e21a2249d4a9?source=collection_archive---------9----------------------->

![](img/4928881d3d2e6a17aee5794a51be44a4.png)

Photo by [BBH Singapore](https://unsplash.com/@bbh_singapore?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

JavaScript 是一种简单易学的编程语言。编写运行并执行某些操作的程序很容易。然而，很难考虑所有的用例并编写健壮的 JavaScript 代码。

在本文中，我们将研究一些糟糕的代码，如果我们想编写更健壮的 JavaScript 代码，就应该避免这些代码。

# 避免大型模块

大型模块不好，因为它们有太多的成员，这使得它们难以跟踪和调试。

较长的代码也很难阅读。

模块应该很小。例如，我们可以有一个只做数学运算的模块。

我们可以这样写:

```
export const add = (a, b) => a + b;
export const subtract = (a, b) => a - b;
```

上面的代码简短易读。

# 避免具有多重职责的模块

多重职责的模块可能做得太多了。

这违反了单一责任原则，这是不好的，因为在一段代码中有许多不同的事情会令人困惑。

具有多重职责的模块让许多人感到困惑，因为它们做了太多的事情。让一个模块做多件事情是不合理的。

例如，如果我们有:

```
export const add = (a, b) => a + b;
export const foo = () => console.log("foo");
```

那就不好了，因为我们在一个模块中有做多种事情的函数。

我们有一个将 2 个数相加的`add`函数，和另一个记录`'foo'`的函数。

这是没有意义的，因为它做多件事。这使得使用它变得更加困难，因为我们必须查看它来了解这个模块是做什么的。

如果模块只做一件事，并且模块名表明确实如此，那么人们就不必太费力就能发现一个模块在做什么。

# 避免紧密耦合

紧密耦合是不好的，因为当我们需要更改代码时，很容易破坏代码，这是不可避免的。

为了避免紧密耦合，我们应该避免将模块中的太多成员暴露给外部。

当两个模块紧密耦合时，这意味着两个类必须一起改变。

另一方面，模块的松散耦合意味着它们大多是独立的。

其他可能紧密耦合的实体包括类和函数。例如，如果我们有以下代码:

`index.js`

```
import { greet } from "./module";
export class Person {
  constructor(name) {
    this.name = name;
  } greet(greeting) {
    greet(`${greeting} ${this.name}`);
  }
}
```

`module.js`

```
import { Person } from "./index";
export const greet = greeting =>
  console.log(`${greeting} ${new Person("foo")}`);
```

在上面的代码中，我们在`index.js`之外添加了`greet`函数，并在`greet`函数中导入了`Person`。在`index.js`中，我们有从`module.js`导入`greet`函数的`Person`类。

这种依赖结构耦合得太紧，因为`Person`依赖于`greet`,`greet`依赖于`Person`。

当一个改变时，我们不得不担心它是否会破坏另一个，这是不好的，因为它会使我们的代码更加脆弱，并在我们试图做出改变时减慢我们的速度。

对他们来说最好是独立的。如果我们不需要他们依赖对方，那么我们应该消除这种依赖。

例如，我们可以编写以下内容:

`index.js`

```
class Person {
  constructor(name) {
    this.name = name;
  } greet(greeting) {
    console.log(`${greeting} ${this.name}`);
  }
}
```

`module.js`

```
export const greet = greeting => console.log(greeting);
```

在上面的代码中，我们通过消除依赖项的导入和引用来保持它们的独立性。

这要干净得多，我们不必担心破坏项目所在的模块代码之外的东西，因为它们彼此不依赖。

![](img/3fd327b0a9dfa53ab7847d16c7c59432.png)

Photo by [DESIGNECOLOGIST](https://unsplash.com/@designecologist?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 避免神奇的数字

幻数是出现在多个地方的数字，其含义无法解释。它们经常被用作常量。

因为它们被用作常量，所以应该用命名的常量替换。

例如，代替编写以下内容:

```
const foo = 1;
const bar = 1;
const baz = 1;
```

我们应该使用一个命名的常量来代替 1。例如，我们可以编写以下内容:

```
const CONSTANT = 1;
const foo = CONSTANT;
const bar = CONSTANT;
const baz = CONSTANT;
```

这样，我们知道 1 是常数，并赋予它意义。JavaScript 中的常量名通常是大写的，以表明它是一个常量。

# 结论

要编写健壮的 JavaScript，我们应该创建只做一件事的小模块。

应该消除实体之间的紧密耦合。最后，应该避免幻数，应该用常数代替。

## **用简单英语写的 JavaScript 笔记**

我们已经推出了三种新的出版物！请关注我们的新出版物:[**AI in Plain English**](https://medium.com/ai-in-plain-english)[**UX in Plain English**](https://medium.com/ux-in-plain-english)[**Python in Plain English**](https://medium.com/python-in-plain-english)**——谢谢，继续学习！**

**我们也一直有兴趣帮助推广高质量的内容。如果您有一篇文章想要提交给我们的任何出版物，请发送电子邮件至[**submissions @ plain English . io**](mailto:submissions@plainenglish.io)**，并附上您的 Medium 用户名，我们会将您添加为作者。另外，请让我们知道您想加入哪个/哪些出版物。****