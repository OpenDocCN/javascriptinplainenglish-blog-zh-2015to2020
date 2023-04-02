# jQuery 不再有用了。原因如下。

> 原文：<https://javascript.plainenglish.io/jquery-will-die-soon-heres-why-976a6b8105e1?source=collection_archive---------0----------------------->

## 我们还需要 jQuery 来编写 web 页面的 DOM 脚本吗？

这不是 Angular，React 或 Vue 的出现，这是一个事实。开发人员知道前端编程已经发展成为一件严肃的事情，非常严肃，这就是为什么我们需要一个像我之前所说的现代 JS 框架来更好地实现 OOP、设计模式、性能改进、代码管理和可重用性，但它甚至比这更深入。我将使用关键的案例研究来解释为什么 jQuery 会日复一日地变得毫无用处，并且在几个月后会变得毫无意义。

![](img/88726e44a7c2c2068c17cc1f1fdac7a4.png)

Photo by [Jan Tinneberg](https://unsplash.com/@craft_ear?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

## 不可能:jQuery 太受欢迎了

那是真的，绝对是真的。jQuery 仍然是当今最流行的 JS 框架，有大量的网页集成了它。此外，jQuery 确实增加了它的受欢迎程度，因为它大规模集成到了 Wordpress 主题和类似的平台中，使它成为了迄今为止未被讨论过的顶级 Javascript 框架。jQuery 持续了很长时间，甚至可能超过 10 年，并且年复一年地流行起来。这就是为什么每个前端开发人员在 jQuery 死后至少几年内仍然使用它。无论如何，即使开发人员仍然使用 jQuery 来编写新功能和解决实际网页的 bug，我们也绝对需要切换到“必要的”而不是“旧的和必要的”！我知道并遇到了仍在使用 Java 6 的开发人员，但这并不意味着 Java 6 对编写新软件没有用处。

## 不可能:jQuery 是动画的，对我来说太酷了！

我记得当 jQuery 发布的时候，我有一个朋友看着它的动画，他几乎呆住了…我也是。jQuery 常用的方法像 show()、hide()、slideUp()、slideDown()在 UX 统治了几年，甚至 Google 也在它的第一个版本和 AdSense 中使用 jQuery。今天我们有更好的东西。首先，由于 jQuery 的性能和效率低下，Google 转而使用 GreenSock。如果你检查那些使用动画的最酷网站背后的代码，没有一个网站使用 jQuery。更重要的是，你可以使用简单的 CSS 过渡获得简单有效的动画。这就是为什么这种实现毕竟是一种岔路口:如果你需要基本的动画，使用 CSS 过渡，或者如果你想要真正快速和流畅的动画，使用专用框架。事实上，jQuery 并没有那么酷，它只是一个先驱。

## 不可能:jQuery 选择器是基础

jQuery 流行且易于使用的关键因素之一是能够使用 CSS 选择器选择 DOM 元素。它们被认为比 document.getElementById()和其他普通方法好得多，只是因为它们简单而有效。但是……你知道 document.querySelectors 吗？IE 9+和所有常见的浏览器都支持它们，它只是 Javascript。在大约一年或更短的时间内，它将被认为是选择 DOM 元素的参考方法。再见！

## 不可能:jQuery 简化了 AJAX

jQuery 受欢迎的另一个好处是能够用简化的语法产生 XHR 请求。此外，美元。ajax 方法用$更加简化了。邮政和美元。获取方法。jQuery 不仅简化了编码 XHR 请求的方式，还使得它们可以在 IE 7 及以后的所有浏览器上工作。今天，无论如何，你可以使用一个新的 XMLHttpRequest()让 HTTP 请求在 IE 10+上工作。

## 不可能:jQuery 集成在 Bootstrap 和其他 UI 框架中

Bootstrap 总是利用 jQuery 来使所有的 UI 交互组件工作:下拉菜单、折叠、旋转...多亏了 jQuery，它们才能正常工作。与版本 3 相比，今天 Bootstrap 4 放弃了对旧浏览器的支持，引入 Flex 作为它的基本网格系统，而不是列宽百分比。你知道吗？Boostrap 5 还在开发中，**它将把 jQuery 从它的需求中去掉**，这意味着所有这些 UI 组件将更多地使用现代 Javascript 而不是老一套的 jQuery！

# 结论:方式。jQuery 几乎没用…

您看到了吗，这些年来使 jQuery 流行的所有主要因素如今都被使用普通 Javascript 或专用库的替代方法所取代。考虑到对 IE 9 和其他流行浏览器旧版本的支持，我们可能还需要几个月的时间来考虑它们是否被弃用。

但有一件事是肯定的。在不到一年的时间里，jQuery 对于新项目来说将是完全无用的，即使它将会得到更多的支持。毕竟，随着 Angular、Vue 和 React 的出现，jQuery 正在成为一个长期约会的朋友，一个你恨不得永远离开和抛弃的朋友。