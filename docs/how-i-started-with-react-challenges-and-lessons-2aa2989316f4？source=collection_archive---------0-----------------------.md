# 我如何开始 React —挑战和教训

> 原文：<https://javascript.plainenglish.io/how-i-started-with-react-challenges-and-lessons-2aa2989316f4?source=collection_archive---------0----------------------->

![](img/bad17d3184e800cead7d07ce5e9def24.png)

Photo by [Fabian Grohs](https://unsplash.com/photos/dC6Pb2JdAqs?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) on [Unsplash](https://unsplash.com/search/photos/programming-react?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)

# 动机

我目前在罗马尼亚的一家外包公司担任前端工程师，我一直对 Javascript 框架感兴趣，从 Angular 1 开始，但我从未使用任何框架做过严肃的项目，去年我决定我应该追求这个兴趣。

我开始学习 Angular 2(我认为现在是 Angular 7)，但是冗长的语法对我来说没有任何意义，而且 React 在编程论坛上看起来也很酷，所以我继续学习。

在与一些更有经验的同事进行了几次讨论后，我为自己画了一个路线图，我在这里展示它，希望有人能从中找到一些价值。

# 路标

* *注意:每个标题都附有一个课程链接*

1.  [ecmascript 6](https://frontendmasters.com/courses/es6-right-parts/)
2.  [**React+Redux**](https://frontendmasters.com/courses/react/)
3.  [**更多的 React 和 Redux**](https://www.udemy.com/react-redux/)

# 挑战和教训

当然，它的每一部分都伴随着一些挑战，我不得不花更多的时间去理解它的某些方面。

ES6 引入了类，这可以被认为是 JS 中函数之上的语法糖，但是它们之间有一些微妙的区别，主要与类和函数的原型继承有关。(更多详情[此处](https://stackoverflow.com/a/47671709))

说到原型，我在凯尔·辛普森的书中找到了关于这一点和其他与 JS 相关的奇怪现象的最好和最有力的解释， [*你不知道的 JS*](https://github.com/getify/You-Dont-Know-JS) 系列*。*

这些已经成为我一次又一次去的地方，每当我觉得我不能很好地理解一个概念时，这是我会建议任何人去做的。

JSX 起初觉得有很多工作要写，从 JS 返回 HTML 代码有点奇怪，但在习惯了语法之后，这就变得很自然了。

然后就是功能组件和类组件的区别，这迫使我去理解 React 组件中本地状态的概念。这也带来了一个问题:

> 我们应该在组件状态下保存哪些数据？

关于局部状态，需要知道的重要一点是，当状态值改变时，它会触发重新渲染。这种状态也可以作为道具传递给子组件。

自然，我必须学习一种更好的方法来管理这种状态，所以我偶然发现了 Redux。在通读了文档并做了一些例子后，我仍然发现它的概念很难理解，主要是因为我不知道它能解决什么问题，而且我觉得如果我以前使用过其他状态管理工具(例如 Flux)，对我来说会更容易。

## 重复主要概念

*   **动作—** 一个对象，它最低限度地描述了状态树中需要改变的内容
*   **Reducer** —这是一个纯粹的函数，它基于上一个状态树和被调度的动作计算下一个状态树

纯函数就是在给定输入的情况下总是返回相同输出的函数。

进一步帮助我的是 Redux 的 3 个原则，这是我从 Dan Abramov 的课程中学到的。

[](https://egghead.io/courses/getting-started-with-redux) [## Redux 入门

### 在这个全面的教程，丹阿布拉莫夫 Redux 的创造者-将教你如何管理你的反应状态…

蛋头](https://egghead.io/courses/getting-started-with-redux) 

## Redux 的 3 个原则

1.  应用程序中的所有变化，包括数据和 UI 状态，都包含在一个名为 state 的对象中。
2.  状态树是只读的。您不能对其进行修改或写入。任何时候你想改变状态，你需要调度和行动，这是一个普通的 JS 对象描述的变化。
3.  为了描述状态突变，你必须编写一个函数，它接受应用程序的前一个状态，即正在调度的动作，并返回应用程序的下一个状态。这个函数必须是纯函数，称为缩减函数。

当然，还有更多的可以重复，比如中间件和传奇，但是上面的概念帮助我为进一步理解这是如何工作的打下了基础。

# 测试

最后但同样重要的是，每个好的项目都有单元测试。我正在开发的这个程序使用 Jest + Enzyme，加上一些测试设置文件来简化工作。

你可以在这里看文档[，在这里](https://jestjs.io/)看文档[。](https://airbnb.io/enzyme/)

> 我在这里学到的一个教训是测试功能，而不是实现。

不管实现的方式如何，测试组件是否按照它们应该的方式运行是非常重要的，但是当然，您的同事应该通过代码评审注释帮助您走上正确的轨道。

我希望你在学习道路上取得成功，如果你觉得有什么我应该扩展的，给我留言！