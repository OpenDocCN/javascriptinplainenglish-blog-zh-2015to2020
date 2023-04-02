# 用普通 JavaScript 构建一个简单的架子鼓

> 原文：<https://javascript.plainenglish.io/building-a-simple-drum-kit-with-vanilla-javascript-e391e2f4e2ab?source=collection_archive---------3----------------------->

## 让我们用普通的 JavaScript 构建一个架子鼓

![](img/1c058133b5a5abf11c3587a706761545.png)

Photo by [Carlos Coronado](https://unsplash.com/@carloscoronadodr?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 介绍

**架子鼓**是你可以在浏览器中用 JavaScript 构建的令人兴奋的东西之一。上个月，我制作了一个简单的架子鼓，这是我在 100 天代码挑战中尝试过的项目之一。所以如果你是初学者，这是一个很好的测试你技能的项目。

在尝试这个项目之前，你需要对 **HTML** 、 **CSS** 和 **JavaScript** 有基本的了解。让我们开始吧。

# 项目演示

Drum Kit.

正如你在上面的例子中看到的，这个项目非常简单。你按下键盘上的键，然后你会听到一首架子鼓歌曲。我们还增加了一个简单的按键动画。

# 让我们从 HTML 开始

首先，我们将把所有东西放在一个 div 中，并使用一个类`container`，然后我们将把 div 放在包含字母的方块中。对于每个 div，我们将添加一个 ID 来表示 div 中每个字母的键码。当我们讲到 JavaScript 部分时，你就会知道了。在容器 div 之后，我们将把我们所有的架子鼓歌曲与一个 ID 放在一起，这个 ID 代表我们将要按下的字母的键码，以便播放特定的音频。

看看下面的例子:

The HTML Structure.

# 让我们来设计我们的元素

所以现在，我们将使用 CSS 样式化我们的元素。您可以阅读下面的代码来查看我们的样式表。

Styling Our Elements with CSS.

# JavaScript 部分

现在，这是令人兴奋的部分，将使我们的项目功能。在我们的 JavaScript 逻辑中，我们使用`querySelectorAll()`选择了容器 div 中的所有音频元素和 div，因为我们将使用方法`forEach()`来处理它们。

之后，我们添加了一个事件监听器，它将接受两个参数:事件`keydown`和回调函数，回调函数也将事件对象作为参数。

这里有一个例子:

```
window.addEventListener('keydown', (**e**) =>{
     console.log(e.**keyCode);
}** // When you press a letter on your keyboard, it will print the keycode of that letter in the console.
```

这就是为什么在我们的 HTML 中，我们为每个 div 和每个音频添加了 ID(keyCode)。当您按下与音频元素具有相同 ID 的字母时，它将播放该特定的音频元素。

你可以仔细阅读下面的代码来理解我们的逻辑:

Our JavaScript Code.

现在恭喜你，你已经用普通的 JavaScript 轻松地创建了一个简单的架子鼓项目。

# 结论

成为一名优秀的开发人员的最好方法是实践你所学到的东西。熟能生巧。构建一个简单的架子鼓将帮助你练习和提高你的编码技能。

感谢您阅读本文，希望您觉得有用。

# 更多阅读

[](https://medium.com/javascript-in-plain-english/5-amazing-front-end-development-tools-that-you-should-know-7372dc377d7) [## 你应该知道的 5 个惊人的前端开发工具

### 每个开发人员都应该知道的有用的前端开发工具

medium.com](https://medium.com/javascript-in-plain-english/5-amazing-front-end-development-tools-that-you-should-know-7372dc377d7)