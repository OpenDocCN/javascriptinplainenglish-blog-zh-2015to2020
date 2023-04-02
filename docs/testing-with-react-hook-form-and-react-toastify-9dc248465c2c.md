# 使用反作用钩形和反作用刚度进行测试

> 原文：<https://javascript.plainenglish.io/testing-with-react-hook-form-and-react-toastify-9dc248465c2c?source=collection_archive---------10----------------------->

## 出乎意料的困难

![](img/f3945fadc53c7c273d132b77e69116f0.png)

Photo by [Florian Olivo](https://unsplash.com/@florianolv?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) on [Unsplash](https://unsplash.com/s/photos/code?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)

我正在构建这个应用程序，一个披萨外卖应用程序:Midas Pizza — *很酷的名字对不对……不对？……很公平。*这是一个完整的*堆栈式*应用程序，它在前端使用 React 来消费一个节点交付 API(这也是我构建的)；然而，在我一次例行访问`__tests__`文件夹时，我遇到了一个问题；我想不出一种方法来测试我的 react-hook 表单和 react-toastify 实现。所以我恳求谷歌。

我第一次搜索如何测试 react-hook-form，绕了几圈后，找到了一个解决方案，但我不能像我希望的那样容易地找到它，至于 react-toastify 上的后一个查询，我没有找到足够的具体信息来处理；我不得不考虑一下，但最终我想出了一个解决方案，并决定通过这篇文章让它更容易理解。

# 反应挂钩形式

对于那些不知道的人来说，react-hook-form 是一个帮助创建表单的库，并通过它的钩子 API 简化了输入验证。

React-Hook-Form 异步处理它的验证，这在测试时几乎总是会导致意想不到的结果。

> React Hook 形式的所有验证方法都将被视为一个异步函数，所以在 act 中包含异步是很重要的
> 
> *— React Hook 表单文档*

医生提到我们需要“用`async`包裹住你的`act`，但是这是什么意思呢？

首先，我们应该知道`act`函数的目的是让你的组件在测试环境中像在浏览器中一样运行。我们都经历过这样的情况:在导致我们测试的组件重新呈现的操作之后，我们的断言失败了。发生这种情况是因为我们的断言不能与组件的更新 UI 一起工作。`act`函数通过接受(作为回调)导致组件更新的动作来解决这个问题——这些动作统称为*交互单元*——并确保这些动作导致的更新在断言运行之前得到处理并应用到 DOM。

act usage example

> **注意**像 enzyme 和 react-testing-library(上面使用的那个)这样的流行测试库已经将 act 函数嵌入到它们的 API 中，所以上面的例子有点多余。

然而，react hook 表单需要一个额外的步骤，因为如 react-hook-form 文档中的摘录所述，“React Hook 表单中的所有验证方法都将被视为`async`函数”。`act`函数将确保在我们的断言运行之前，测试期间由组件上的*交互单元*引起的 DOM 更新被绘制出来，但是它不会确保更新过程中涉及的任何承诺都被解析。要做到这一点，我们的测试必须是异步的，我们需要`await`这个`act`函数，它也需要一个`async`函数作为它的回调函数。

async test example

总的来说，我们的测试应该是这样的

Async test example with act function

# 对僵硬的反应

我个人非常喜欢这个库，因为它为我的应用程序添加弹出通知带来了简单性。然而，它唯一的警告是它无法被测试。这里的问题是通知(也称为 toast)没有在它应该出现的时候出现在 DOM 上，导致断言经常失败。我一直在努力寻找这个问题的解决方案，直到我发现定时器可以用 jest 来提前，由于这个库利用定时器来促进通知的出现和消失，它创建了，那么提前所有正在运行的定时器实例似乎是一个显而易见的解决方案。

我所做的是将所有正在运行的计时器(包括 toast 使用的计时器)从 toast 应该出现的时刻提前几秒钟，这样就可以在 DOM 中看到它。代码看起来像这样

react -toastify test example

# 概述

*   记住，在测试 react-hook-form 的实现时，使用`act`和异步测试。
*   此外，当使用 react-toastify 时，如果通知(toast)在指定的时刻不可见，则使用 jest 伪计时器从 toast 应该出现的时刻开始快进运行计时器。

此外，当使用 react-toastify 时，如果通知(toast)在给定的时刻不可见，则使用 jest 伪计时器在 toast 被设置为离开页面之前的几秒钟快进任何正在运行的计时器。

感谢阅读👌🚀