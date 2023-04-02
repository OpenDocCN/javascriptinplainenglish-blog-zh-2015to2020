# 在 React 中有条件地呈现组件的 7 种简单方法

> 原文：<https://javascript.plainenglish.io/7-simple-ways-to-conditionally-render-components-in-react-a3170d0cd9e0?source=collection_archive---------4----------------------->

![](img/f9f70bfb856de03d29987206ffd03d47.png)

Photo by [Charles Deluvio](https://unsplash.com/@charlesdeluvio?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

为用户类型 A 显示红色用户名，为用户类型 B 显示蓝色用户名，或者只为登录用户显示仪表板。根据特定条件显示不同的 UI 是每个 React 项目中非常常见的任务。

你怎么能这样做？我打赌你心里有你的解决方法。但是你想知道其他的方法吗？下面我给你看一些。

# 最基本的假设

最简单的方法是使用 if。

假设您不希望因为空数据或类似的原因而显示一块内容。使用**如果**是你首先能想到的。例如，如果用户不存在，则返回 null。

或者如果用户还没有登录，给他显示一个登录按钮，否则什么也不做。

# 如果…否则…

另一个通常的用法是 if… else…

你有一份名单。如果列表至少有一项，您希望显示呈现列表。如果列表的大小为零，简单地显示一个文本表明没有数据。

# 第三的

我喜欢短代码。无论如何，我倾向于使用短代码，它产生的结果与看起来专业但冗余的代码一样。干净简洁。

看看这个:

让我们把它转换成更令人满意的代码:

上面的两个代码片段做了同样的事情——如果用户登录，就向他显示真实数据，否则显示为匿名。但是第二个看起来更矮更漂亮。

你看，多简洁啊。

# &&运算符

你学到了你可以用第一节中的经典 if 或者漂亮的三元组来渲染一个元素或者什么都不渲染。你认为完成这项任务没有捷径。如果是这种情况，你可能不知道&& operator。

假设您希望在用户尚未登录的情况下显示一个登录按钮元素。

在以下情况下，您可以使用:

或三元:

或者& amp；

&&在这种情况下彻底打败三元。

# 开关盒

在有许多条件用户界面的情况下，应该考虑切换大小写。

在上面的例子中，我们将相应的元素呈现给用户的类型。

这种情况下也可以使用 **if/else** ，但是**开关情况**更简洁。

# 立即调用函数表达式

自调用函数是在 JSX 中执行 JavaScript 代码的一种方式。

这意味着你可以直接在 JSX 内部编写条件逻辑。有关更多详细信息，请查看下面的示例:

为了使它更简洁，只需使用箭头函数:

# 增强型 JSX

有些库允许你在不直接使用 JavaScript 的情况下向 JSX 添加条件逻辑。 [JSX 控制语句](https://github.com/AlexGilleran/jsx-control-statements)就是其中之一。请参见下面的示例:

它类似于**开关盒**，但更像组件。

# 现在，你更喜欢用哪一个？

我发现自己通常坚持 if/else 方式。我们解读的方式——*如果<条件>那就做点什么，否则做点别的*，就像我们说话的方式一样自然。这就是我的编码风格。

你会坚持和我一样的人在一起，还是有标准来选择最适合你的情况。

# 用简单英语写的便条

你知道我们有四份出版物和一个 YouTube 频道吗？你可以在我们的主页 [**plainenglish.io**](https://plainenglish.io/) 找到所有这些内容——关注我们的出版物并 [**订阅我们的 YouTube 频道**](https://www.youtube.com/channel/UCtipWUghju290NWcn8jhyAw) **来表达你的爱吧！**

# 进一步阅读

[](https://medium.com/javascript-in-plain-english/9-great-javascript-extensions-for-visual-studio-code-to-speed-up-your-development-8b3275248718) [## Visual Studio 代码的 9 大 JavaScript 扩展加速您的开发

### 谁想更快更容易地编码？

medium.com](https://medium.com/javascript-in-plain-english/9-great-javascript-extensions-for-visual-studio-code-to-speed-up-your-development-8b3275248718) [](https://medium.com/javascript-in-plain-english/boost-your-javascript-skills-with-these-5-websites-8559a498ca33) [## 通过这 5 个网站提升你的 JavaScript 技能

### 您将掌握 Javascript 的基础知识。

medium.com](https://medium.com/javascript-in-plain-english/boost-your-javascript-skills-with-these-5-websites-8559a498ca33)