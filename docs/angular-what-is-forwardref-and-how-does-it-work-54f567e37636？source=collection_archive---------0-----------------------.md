# Angular:什么是 forwardRef，它是如何工作的？

> 原文：<https://javascript.plainenglish.io/angular-what-is-forwardref-and-how-does-it-work-54f567e37636?source=collection_archive---------0----------------------->

![](img/070b9e648c616bd7c7a51a1465518efd.png)

Photo by [Steinar Engeland](https://unsplash.com/@steinart?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) on [Unsplash](https://unsplash.com/s/photos/arrow?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)

Angular 是一个完整的开发框架，它为你能想到的大多数问题提供了解决方案。甚至可能是你想不到的。`forwardRef`解决的问题很可能是其中之一。

# 不现实但简单的例子

让一个组件向服务请求一些数据是一种非常常见的情况。这个服务本身需要另一个服务的帮助，这种情况也不罕见。在一个经典的 Angular 项目中，组件和服务位于不同的文件中。但是没有任何东西禁止在一个文件中有多个类，所以我们可以这样做:

如您所见，服务`Test1Service`首先被定义，但是注入了第二个被声明的`Test2Service`。这不会导致任何林挺或编译错误，您可以用`ng serve`启动您的应用程序。

但是，在浏览器中打开应用程序时，您没有看到预期的“Hello ”,而是在控制台中看到一个错误:

```
Uncaught ReferenceError: Cannot access 'Test2Service' before initialization
```

显然，在`Test1Service`中使用`Test2Service`，而`Test2Service`是在`Test1Service`下物理定义的，确实会产生问题。

*为什么会编译？为什么 Angular 显然知道这项服务，但却没有初始化它？*

这个错误没有告诉我们`Test2Service`没有被定义，它清楚地告诉我们它还没有被初始化。是因为`Test2Service`已经被**吊起**。该错误是在初始化之前试图使用用`let`或`const`定义的类或变量时收到的典型错误。JavaScript 引擎在执行前扫描了脚本，并创建了执行上下文。变量和类已经存在，但没有初始化。创建`Test1Service`时，`Test2Service`仍未初始化，但我们知道它存在。因此出现了这个错误。你可以查看[这篇文章](https://medium.com/javascript-in-plain-english/javascript-interview-question-what-is-hoisting-e02c1f95d5a4)，快速了解提升和 ES6 变量和类的情况。

我们现在可以将`Test2Service`移动到文件的顶部，这样就可以解决问题了。但是由于我们在一个不现实的例子中，我们将使用`forwardRef`。

`forwardRef`，正如 Angular 文档所说，允许你引用尚未定义的参考。它是一个以函数为参数的函数，这个函数本身返回我们要引用的类，这里是`Test2Service`。当我们试图创建一个`Test1Service`的实例时，Angular 将解析`Test2Service`，并且能够创建实例。

*值得注意的一点:带棱角的 8(或据我所知更低)，* `*Test1Service*` *不应该是* `*providedIn: 'root'*` *而是必须在* `*AppModule*` *中提供，否则我们有错误。Angular 9 不再是这种情况。*

# 它是如何工作的？

从技术上讲，问题来自于角度依赖性注入。在一个基本的普通 JS 文件中，您可以毫无问题地完成如下工作:

在定义`TestClass2`之前，`TestClass1`实际上使用了`TestClass2`，但是当我们在脚本末尾实例化`TestClass1`时，由于闭包的奇迹，我们在控制台中看到了`1`。类似地，如果我们放弃在`Test1Service`中注入`Test2Service`,而是简单地手工实例化，这将会起作用:

这是因为在运行时，`Test1Service`实际被实例化时，`Test2Service`已经被定义了。

在`forwardRef`中，我们将令牌(类名)的解析与运行时不同。`forwardRef`功能本身并没有多大作用:

它接受作为参数传递的函数(我们的`() => Test2Service`)，设置属性`__forward_ref__`并返回函数。它几乎是一个标识函数，只是将该函数标记为需要“ref forwarded”。

当创建一个`Test1Service`的实例时，Angular 会尝试解析依赖关系。如果一个参数是标有`__forward_ref__`的函数，它就调用它:

只有这样，才会创建一个实例并将其添加到提供程序中，这才是最重要的。

# 现实生活中的例子

作为最后的手段。通常可以通过为每个服务创建一个文件来避免这种情况。还有一些情况是避免不了的，否则就不存在了。例如，如果你想为角度反应式表单创建你自己的表单控件，你需要为你的组件提供一个 [ControlValueAccessor](https://angular.io/api/forms/ControlValueAccessor) ，它就是…本身。在 decorator 中直接引用组件会导致与我们的例子中相同的错误，所以您需要使用`forwardRef`:

这是众多例子中的一个，用来展示它在现实生活中的应用。

## **用简单英语写的 JavaScript 笔记**

我们已经推出了三种新的出版物！请关注我们的新出版物: [**AI in Plain English**](https://medium.com/ai-in-plain-english) ，[**UX in Plain English**](https://medium.com/ux-in-plain-english)，[**Python in Plain English**](https://medium.com/python-in-plain-english)**——谢谢，继续学习！**

**我们也一直有兴趣帮助推广高质量的内容。如果您有一篇文章想要提交给我们的任何出版物，请发送电子邮件至[**submissions @ plain English . io**](mailto:submissions@plainenglish.io)**，使用您的 Medium 用户名，我们会将您添加为作者。另外，请让我们知道您想加入哪个/哪些出版物。****