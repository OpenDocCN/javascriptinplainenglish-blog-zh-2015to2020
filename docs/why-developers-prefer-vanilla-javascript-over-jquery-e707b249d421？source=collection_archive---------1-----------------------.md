# 为什么开发人员更喜欢普通的 JavaScript 而不是 jQuery

> 原文：<https://javascript.plainenglish.io/why-developers-prefer-vanilla-javascript-over-jquery-e707b249d421?source=collection_archive---------1----------------------->

![](img/39f17194d194cffc6841653adf0b038e.png)

Icon by [Loris Grillet](https://dribbble.com/shots/762276-Vanilla-Ice-Cream-icon#shot-description)

## 在过去的几年里，业界已经形成了对不依赖外部库的普通 JavaScript 的偏好。

jQuery 是一个轻量级且易于使用的 JavaScript 库，有助于用很少的代码行创建复杂的功能。jQuery 编码比同等的普通 JS 代码更短，也更简单。那么，为什么许多开发人员远离这个库，而转向普通或普通的 JavaScript 呢？

在这个故事中，我们将经历:

*   JavaScript 和 jQuery 的比较:`fading an element`
*   jQuery 最重要的优点和缺点
*   用 jQuery 提交 AJAX 表单
*   用 JavaScript 提交 AJAX 表单

# 淡化 JavaScript 和 jQuery 中的元素

jQuery 是一个高度抽象的库，使用起来很容易，但却忽略了各种学习机会。让我们看一个比较普通 Javascript 和 jQuery 的小例子:**淡化一个元素**。

首先是普通的 JavaScript。

```
**function** **fadeIn**(el) { // Vanilla JavaScript
  el.style.opacity = 0; **var** last = +**new** Date();
  **var** tick = **function**() {
    el.style.opacity = +el.style.opacity + (**new** Date() - last) / 400;
    last = +**new** Date(); **if** (+el.style.opacity < 1) {
      (window.requestAnimationFrame && requestAnimationFrame(tick)) || setTimeout(tick, 16);
    }
  }; tick();
}fadeIn(el); // calling the above function
```

相对于 jQuery 等价物。

```
$(el).fadeIn();
```

正如这里举例说明的那样，jQuery 库包含一些预先编写的函数，用于经常使用的操作，比如 DOM 操作、动画或异步执行(AJAX)。

# jQuery 的优点和缺点

## 优势

*   最好的部分是 jQuery 是浏览器灵活的。这意味着 jQuery 兼容市场上的所有浏览器，因此开发者不必担心用户可能使用的浏览器。然而，现在大多数浏览器都支持 JS 中的相同功能([来源](https://caniuse.com/#search=jquery))。
*   几行代码和大量的抽象，让那些不知道如何使用普通 JavaScript 的人更容易使用。现在，在我的网络开发之初，我不理解 JavaScript，也许现在仍然不理解。我开始使用 jQuery 的原因是因为它更加用户友好，尤其是对于像我这样的新手。

## 不足之处

*   人们发现普通 JS 比 jQuery 快得多，这取决于你所比较的操作，它可以比 jQuery 快 10 到 25 倍。
*   高度的抽象意味着你不能学到尽可能多的东西。这意味着，您可能会陷入一个使用 jQuery 的新手开发人员的无限循环中，因此一直是新手。
*   该库需要包含在每个 HTML 网页中。

如果你想知道关于你是否应该学习/使用 jQuery 这个问题的答案，请看[这里的顶部答案](https://hashnode.com/post/why-do-many-web-developers-hate-jquery-ciibz8fp801g9j3xtgx19utpe)。接下来的两个代码片段向您展示了如何使用 jQuery 和 Vanilla JS 进行同样的异步表单提交。

# 用 jQuery 提交 AJAX 表单

# 用普通 JS 提交 AJAX 表单

最后，我将留给你这个，[http://youmightnotneedjquery.com/](http://youmightnotneedjquery.com/)，一个详细介绍流行的 jQuery 函数的所有普通 JavaScript 替代品的网站。也看看这个恶搞，展示了[香草 JS 库](http://vanilla-js.com/)的所有强大选项。(剧透预警！没有库，只是普通的 JavaScript)

# **简明英语团队的笔记**

你知道我们有四份出版物和一个 YouTube 频道吗？你可以在我们的主页 [**plainenglish.io**](https://plainenglish.io/) 找到所有这些内容——关注我们的出版物并 [**订阅我们的 YouTube 频道**](https://www.youtube.com/channel/UCtipWUghju290NWcn8jhyAw) **来表达你的爱吧！**