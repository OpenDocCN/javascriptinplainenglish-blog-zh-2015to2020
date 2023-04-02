# 如何在获取 API 完成页面加载之前显示加载器

> 原文：<https://javascript.plainenglish.io/adding-loader-to-your-deployed-projects-d8f389e8c928?source=collection_archive---------1----------------------->

![](img/075a448bb9cab1beb7b6811c4587fd5c.png)

is it loading or …

我最近分享了我完成的香草 JavaScript 和 Rails 项目。这是 Rails 后端的 Heroku 和前端的 github.io 最新部署的。

然而，我的朋友对我的项目感到困惑，因为除了几个按钮之外，什么也没有加载。我不得不告诉她，她需要等待大约 30 秒到 5 分钟，这取决于该应用程序最近被获取的时间。这是一个我需要解决的问题。如果我的用户没有得到通知，没有耐心等待数据加载，该怎么办？甚至没有迹象表明数据加载会很慢。

我进行了头脑风暴和研究，找到了解决这个问题的简单方法——添加一个加载器，直到后端数据被完全获取。我想在本文中与您分享如何实现加载器。

![](img/09a53addb7d170c2a2d8caba5a127716.png)

loading cat meme

![](img/0fa6a71e92a0fa90143837fede3eac6e.png)

This is the finished product with the clock emoji loader

create parent-child div in index.html

首先，我们需要创建一个父 div 和一个子 div——在 index.html 的**中总共有两个 div 标签，我们希望在这里定位加载程序。最好将 div 标签放在主体内部。我们将调用父 div、 *preload* 和子 div、 *emoji* 的类。**

index.js

然后，我们在 **index.js** 中编写 div 标签的逻辑。首先，让我们创建两个变量，这样我们就可以轻松地选择 HTML 中的元素。一个，选择名为*的 div 类 preload* ，另一个，选择*表情符号*。

我们还将创建一个名为*表情符号*的数组，其中我们将单个时钟表情符号作为字符串值作为单个元素进行定位。 [setInterval](https://developer.mozilla.org/en-US/docs/Web/API/WindowOrWorkerGlobalScope/setInterval) 逻辑将在每次调用之间以固定的时间延迟调用该函数。在第 7 行，我们将时间间隔声明为 125 毫秒。您可以根据自己的意愿更改延迟时间。

我们将使用单一责任原则并编写两个助手函数，*loade moji*和 *init* 。首先，我们调用第 19 行的函数 *init* 。位于第 16 行的 init 函数将*表情符号*数组作为函数 *loadEmojis* 的参数。所以我们遍历第 9 行，它利用了 setInterval 逻辑。

> e moji . innertext = arr[math . floor(math . random()* arr . length)]

对于第 11 行，我们来分解一下。 *emoji.innerText* 是第 3 行， *emoji* div。我们将用*表情符号*数组中的一个时钟表情符号元素替换表情符号 div。

[Math.random()](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Math/random) 函数返回一个介于 0 到小于 1 之间的浮点伪随机数，例如 0.742。然后我们将随机数乘以*数组长度*，然后使用 [Math.floor()](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Math/floor) 返回小于或等于给定数字的最大整数，因此它是小于*数组长度*且大于 0 的任何数字。这种逻辑将让时钟表情符号不断变化，所以它看起来像是在加载。

![](img/86727a72b7f5abe9c76b43a2fbf0ab98.png)

恭喜你！现在页面上有了加载器！但是等等。这不是我们想要的。当从后端获取数据时，我们需要停止加载程序。下面是解决这个问题的一行代码。

loader stopper inside of the fetch

我们将获取后端数据并解析它。当获取数据时，我们将停止加载程序，因此我们更改显示逻辑的位置在第 6 行。

> document.querySelector("。preload "). style . display = " none "//停止加载。

如果你没有获取，你可以在调用了 *init* 函数之后，把这一行放到 *init()的下面。*只需将 style.display 重新指定为“无”即可停止加载程序。

干得好！我们希望您现在感觉已经很好地掌握了如何实现加载器以及如何停止它。现在，您的用户被告知，如果有加载程序，数据仍在加载。

[*更多内容尽在 plainenglish.io*](http://plainenglish.io/)