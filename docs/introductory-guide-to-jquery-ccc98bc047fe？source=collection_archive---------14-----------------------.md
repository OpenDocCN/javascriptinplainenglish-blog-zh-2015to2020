# jQuery 入门指南

> 原文：<https://javascript.plainenglish.io/introductory-guide-to-jquery-ccc98bc047fe?source=collection_archive---------14----------------------->

## 用 jQuery 操作 DOM 的入门指南。

[jQuery](https://jquery.com/) 很老了。但还是用了一个 *lot* 。遗留代码、教育、自己的项目、网站，应有尽有。

![](img/b1d3033393fddf1a2491fcddd7cd75e3.png)

Photo by [Greg Rakozy](https://unsplash.com/@grakozy?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) on [Unsplash](https://unsplash.com/s/photos/javascript?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)

## 什么是 jQuery？

> jQuery 是一个快速、小巧、功能丰富的 JavaScript 库。它通过一个跨多种浏览器工作的易于使用的 API，使 HTML 文档遍历和操作、事件处理、动画和 Ajax 变得更加简单。— *引自 jQuery 官网。*

根据官方网站[的说法](https://jquery.com/)这是一个特性丰富的 JavaScript 库，使得处理 DOM (HTML 元素)更加容易。让我们看一个简单的例子。

Simple jQuery code

对

Click listener on a button in plain JS

您可以看到 jQuery 示例有较少的代码(和一个用于导入 jQuery 文件的脚本标记)。这两个例子都只是一个 onClick 侦听器，当单击某个按钮时，它会在控制台中输出一些内容。您可以看到 jQuery 示例以此开始:

```
$("button.add#1")
```

这种符号乍一看似乎很奇怪。但这比你想象的要容易。

## jQuery 符号

jQuery 使用了一种符号，这种符号现在在讨论带有类和 id 的 HTML 元素时经常使用。

我们给$()函数的字符串中的'*按钮'*,就是指一个按钮。在 jQuery 中编写 *$("button")* 将选择 DOM 中的所有按钮。

*’。字符串中的' add'* 表示选择带有类别 *'add '的所有内容。*在这种情况下，它将使用“添加”类的所有按钮。

最后一部分， *'#1'* 指的是赋予按钮的 id。在我们的 jQuery 示例中，我们有三个 id 为 1、2 和 3 的按钮。所以 *'button.add#1'* 的完整字符串会选择 class 为*' add '*id 为 *'1 '的按钮。*

## jQuery 函数

如上所述，这是一个介绍性的指南。因此，我不会重复所有功能。我会告诉你如何使用几个，以及如何自学其余的。

先学最简单直接的函数是() 上的 [**。这是您最有可能使用的函数，因为您可以用它来创建事件侦听器。在上面的例子中，我们将它用作“点击”监听器。您还可以将它与“change”一起使用，以听取通过选择框或其他类似元素所做的更改。**](https://api.jquery.com/on/)

```
// You select an element (".elem") and execute a function when 
//that element gets clicked.$(".elem").on("click", function(){
    // Do something
});
```

你在很多教程里看到的就是 [**fadeIn()**](https://api.jquery.com/fadeint/) 和 [**fadeOut()**](https://api.jquery.com/fadeout/) 的用法。所以我们接下来会检查这些。下面的要点有一个带有一些文本的< p >元素，我们可以使用两个按钮淡入淡出。尝试阅读和理解代码。

您可能已经注意到，我们制作了两个“click”侦听器，它们执行另一行 jQuery，该行 jQuery 选择 fadedText 元素并使用 fadeIn()或 fadeOut()。

这基本上就是你扩展知识面需要知道的全部了。这是我在学校学到的所有东西，很快它就变得有意义了。

# 下一步是什么？

当然，这看起来仍然像是你一无所获。所以继续玩吧。尝试找出如何使淡入淡出更快。了解如何使用[**【addClass()**](https://api.jquery.com/addclass/)等函数切换、添加或删除类。

不要淡入淡出，试着旋转文本，或者让它闪烁。

您可以做的还有很多，但是 on()函数的事件侦听器对我来说是最重要的。

祝你今后学习顺利，玩得开心。