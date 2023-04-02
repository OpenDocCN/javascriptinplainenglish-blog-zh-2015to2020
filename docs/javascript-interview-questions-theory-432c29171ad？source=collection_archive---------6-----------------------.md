# JavaScript 面试问题—理论

> 原文：<https://javascript.plainenglish.io/javascript-interview-questions-theory-432c29171ad?source=collection_archive---------6----------------------->

![](img/659a666daa24c9cb86a24653ab460414.png)

Photo by [Wonderlane](https://unsplash.com/@wonderlane?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

为了得到一份前端开发人员的工作，我们需要搞定编码面试。

在本文中，我们将探讨一些关于 JavaScript 的理论问题

# 举出两个对 JavaScript 应用开发者很重要的编程范例。

JavaScript 既支持面向对象编程(OOP ),也支持一些函数式编程概念。

JavaScript 通过原型继承支持 OOP。

JavaScript 类语法只是其原型继承模型之上的语法糖。

JavaScript 中的函数式编程概念包括闭包、一级函数和匿名函数。

# JavaScript 中函数式编程概念用在哪里？

函数式编程概念包括纯函数，即避免副作用的函数，组合函数，以及将函数作为参数传递，因为函数是第一类。

对于函数式编程来说，对象的不变性也很重要，可以避免多个实体改变同一个对象的错误。

# 经典继承和原型继承有什么区别？

具有类继承的语言有类，这些类是创建对象的蓝图。

我们也可以有父类的子类。所以，就有了阶级阶层。

该类的实例是用`new`操作符创建的。

原型继承是对象的实例直接从原型对象继承而来。

实例通常由工厂函数创建。实例可以由许多不同的对象组成。

尽管使用了`new`操作符，JavaScript 仍然使用原型继承。

`new`操作符与构造函数一起使用，构造函数是用于创建对象新实例的工厂函数。

类语法与构造函数相同。尽管使用了类语法，JavaScript 并不使用基于类的继承。

JavaScript 类仍然是构造函数，具有更清晰、更容易理解的语法。

# 函数式编程 vs 面向对象编程的优缺点是什么？

OOP 很容易理解，对象的基本概念也很简单。

因此，很容易解读到其成员的含义。

OOP 使用命令式编程风格，而不是声明式。命令式编程意味着语句被用来改变程序的状态。

声明式编程描述了逻辑，但没有描述控制流。

OOP 依赖于共享状态。对象和行为被放在一起成为一个实体。实体的成员可以在任何。

这可能会导致像竞争条件这样的问题。

函数式编程使用纯函数来避免任何副作用。这消除了多个函数改变同一资源的错误。

与 OOP 相比，函数往往被简化和组合，以获得更多可重用的代码。

函数式编程通常使用声明式风格，这种风格不会一步一步地写出所有内容。

这意味着有更大的重构空间，我们可以更容易地用更有效的算法替换算法。

由于其声明式风格，函数式编程代码更难阅读，因为并非所有内容都是逐行写出的。

函数式编程也比面向对象编程有更多的学术材料，因为面向对象编程因其受欢迎程度而有更多面向初学者的材料。

# 古典传承什么时候是合适的选择？

经典继承只适用于单级继承。多层次的继承总是一种反模式。

多层次混乱难读。

对象组合比继承好。

# 什么时候原型继承是合适的选择？

原型继承很有用，因为它允许组合。

这意味着对象可以具有 has-a、uses-a 或 can-do 关系，这与具有类继承的 is-a 关系相反。

我们可以用原型继承组合来自多个来源的对象，并使用 mixins 或`Object.assign`将多个对象组合成一个。

# “重对象组合轻类继承”是什么意思？

代码应该从更小的功能单元组装成新的对象，而不是从类继承和创建对象分类法。

这意味着，他们可以拥有“能干”、“有”、或“使用”,而不是“是”——一种关系。

通过组合，我们避免了类的层次结构、紧密耦合和严格的分类。

这也使得代码更加灵活。

![](img/32bba34f807399d559f161727f54164b.png)

Photo by [Bernardo Artus](https://unsplash.com/@berniart?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 什么是异步编程，为什么它在 JavaScript 中很重要？

JavaScript 有一个单线程运行时环境。这意味着所有同步代码都将阻塞主执行线程。

长时间运行的网络请求和 I/O 操作将阻止应用程序的其余部分运行。

异步编程意味着引擎在事件循环中运行。阻塞操作在事件循环中排队，而不是立即运行。因此，代码不会阻止其余代码的运行。

当响应就绪时，事件处理程序运行，异步代码的控制流继续。

这样，单个程序线程可以处理许多并发操作。

默认情况下，浏览器 JavaScript 环境和 Node.js 都是异步的。

# 结论

要知道 JavaScript 既是面向对象的语言，也是函数式语言，这一点很重要。

对象是由继承原型对象组成的，而不是有一个严格的类层次结构。类语法对此没有任何影响。

对象是从构造函数中创建的。

JavaScript 是单线程语言，因此并发操作必须异步运行，以防止阻塞主线程。

## **用简单英语写的 JavaScript 笔记**

我们已经推出了三种新的出版物！请关注我们的新出版物:[**AI in Plain English**](https://medium.com/ai-in-plain-english)[**UX in Plain English**](https://medium.com/ux-in-plain-english)[**Python in Plain English**](https://medium.com/python-in-plain-english)**——谢谢，继续学习！**

**我们也一直有兴趣帮助推广高质量的内容。如果您有一篇文章想要提交给我们的任何出版物，请发送电子邮件至[**submissions @ plain English . io**](mailto:submissions@plainenglish.io)**，并附上您的 Medium 用户名，我们会将您添加为作者。另外，请让我们知道您想加入哪个/哪些出版物。****