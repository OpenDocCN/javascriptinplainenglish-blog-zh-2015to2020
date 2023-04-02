# 向静态站点添加动态功能

> 原文：<https://javascript.plainenglish.io/adding-dynamic-features-to-a-static-site-cf56ff7b163a?source=collection_archive---------9----------------------->

## 如何使用 JavaScript 来活跃你的内容

![](img/eb6b4ab374e88d560de3fbeea1b610ee.png)

Photo by [Joseph Chan](https://unsplash.com/@yulokchan?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

在文章 [The Power of Static](https://medium.com/swlh/the-power-of-static-20e4f7768c3d) 中，我解释了将内容发布为“静态站点”的一些好处:一个简单的 HTML 文件集合，而不是一个更复杂的站点，位于脚本语言之上。我提到的一个缺点是缺乏灵活性，尽管我简单地提到过:

> 随着 JavaScript 的日益流行，一旦静态页面被交付给用户的浏览器，就可以选择在静态页面上“分层”个性化。

现在，我想进一步探讨这个概念。

[*所有的例子内容都可以从这个对应的 GitHub 库中获得。*](https://github.com/bobbykjack/adding-dynamic-features-to-a-static-site)

# 服务器与客户端

对于一个静态网站，我们可以把我们的服务器——计算机和软件本身——想象成一个愚蠢的仆人，接受最基本的命令，代表我们取东西。然而，*我们仍然*聪明，能够做大多数事情，即使是一个聪明的仆人也能做到，而且我们要他们取的*物品*不仅仅是一个简单的内容。

是什么提供了这种力量？JavaScript。JavaScript 在整个过程的客户端阶段运行，换句话说，就是读者所在的浏览器/计算机。客户是“我们”，在前面的类比中是娇生惯养的雇主。我们最终将不得不为自己做一些工作，但这通常是一种优势，因为:

*   这项工作是专为我们进行的。实际上，服务器可能被要求处理许多许多的客户端，每个客户端都需要一些宝贵的时间——在规模上，这可能会大大降低速度。
*   这项工作定义明确，范围有限:我们只能使用浏览器基础设施，而服务器几乎有无限的机会。这个机会可能很强大，但它往往太强大了。

# 拥有您的数据

是时候快速转移注意力了。你对自己的数据被发送到服务器，与他人的数据结合在一起，被操纵，可能与第三方共享，并反馈给你供你欣赏有多高兴？毫无疑问，这种数据传输有很大的优势，但有时，也许不是每个博客或新闻网站都应该收集所有这些东西，这难道不是感觉吗？

作为一个网站所有者，你可能想考虑告诉你的读者“这是我要说的，用它做你想做的事情”的价值——本质上，这是我们在使用静态网站时可以采用的方法，由 JavaScript 增强。

# 已经够理论了

![](img/db22cbedbcc541b71464687838da3a8b.png)

Photo by [Aron Visuals](https://unsplash.com/@aronvisuals?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

是时候举些例子了。让我们从一件小事开始:将时间插入到文档中。

## 服务器端

我们可以使用 PHP 脚本轻松插入服务器请求页面的时间:

```
<!doctype html>
<html lang="en">
<head>
    <meta charset="utf-8" />
    <title>Example one</title>
</head>
<body>
    <h1>Hello</h1>
    <p>The time is <b>**<?php echo date('H:i a'); ?>**</b>.</p>
</body>
</html>
```

这里不用太担心实现，重要的是要记住，PHP 代码是在服务器端执行的，时间是在交付给客户端(浏览器)之前动态插入页面的。每次我们刷新页面，服务器将页面发送给我们，我们将得到一个可能不同的文档。

## 客户端

现在让我们考虑一个 JavaScript 替代方案:

```
<!doctype html>
<html lang="en">
<head>
    <meta charset="utf-8" />
    <title>Example one</title>
</head>
<body>
    <h1>Hello</h1>
    <p>The time is <b>**<script>document.write(****new Date().toLocaleTimeString()****);</script>**</b>.</p>
</body>
</html>
```

从某种意义上来说，我们的 JavaScript 替代品比它的 PHP 等效物更加动态；就我们客户而言，它是动态的。我们总是会收到相同的页面，但每次看到它时，我们都会做少量的工作，最终得到不同的结果。服务器已经告诉我们要做什么，但是实际上要由我们来做。

当然，JavaScript 方法还有另一个好处:这个内容可以不断地改变，而不仅仅是在 URL 被请求的时候。如果我们不是告诉读者“这是页面加载的时间”，而是告诉他们“这是时间*现在*”，我们的 JavaScript 版本是唯一明智的方法:

```
<!doctype html>
<html lang="en">
<head>
    <meta charset="utf-8" />
    <title>Example one</title>
</head>
<body>
    <h1>Hello</h1>
    <p>The time is <b id="thetime"></b>.</p>
 **<script>
    setInterval(function() {
        document.querySelector("#thetime").textContent = new Date().toLocaleTimeString();
    }, 1000);
    </script>**
</body>
</html>
```

每秒(1000 毫秒)，匿名函数更新`b`元素的文本(通过它的`id`定位它)并将其设置为当前时间。

# 一个更有用的例子

![](img/889e5b19f059a63aab314e6f12cf29ca.png)

Photo by [Denisse Leon](https://unsplash.com/@denisseleon?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

告诉时间当然很好，但是一个与现实世界更相关的例子呢？最后，我将展示一个在客户端管理书签的小脚本。这种脚本可以添加到任何静态站点，并且无需服务器端设置、软件安装或数据库配置就能立即提供有用的功能。这是一个简单的例子，但是它应该让你思考这种方法的可能性。

许多网站提供某种“书签”功能。就在媒体上，你可以为文章做书签，以便以后阅读。亚马逊允许你将商品添加到愿望清单中，以备将来参考。这两者目前都是使用服务器端技术实现的，但是在每种情况下，客户端替代方案都是完全可行的。

剧本`bookmarks.js`[在 GitHub](https://github.com/bobbykjack/adding-dynamic-features-to-a-static-site/blob/master/bookmarks.js) 上有。我建议下载它，并将其包含在您可以访问的静态站点的几个页面中(最好是在测试或开发环境中！).在页面的任何地方用一个`script`元素来引用`bookmarks.js`。

如果您现在无法下载，至少可以在 GitHub 上查看该脚本，并在接下来的讨论中参考它。

## 方法的描述

该脚本的工作方式如下:

*   `localStorage`用于跟踪一组页面(特别是 URL 的路径部分)。这是一个半永久性的数据存储——可以把它看作是 cookies 的一个增强的替代品。
*   只有字符串数据可以保存在 localStorage 中，但是 JSON 允许我们使用`stringify`和`parse`方法将任意 JavaScript 数据转换成字符串，反之亦然。
*   DOM 用于操作浏览器中的页面。请记住，我们的方法是从服务器获取静态站点内容，并在客户端对其进行增强。
*   当一个页面被查看时，它的 URL 被检查。如果是主页*，会在页面顶部插入一个书签列表。否则，根据页面是否已经被书签标记，注入“添加”或“移除”链接。

* *请注意，您页面的 URL 必须是/即位于您域名的顶级或“根”位置*

# 最后的话

![](img/675ca18bb4bba1897dc4cf650d30152f.png)

Photo by [Markus Winkler](https://unsplash.com/@markuswinkler?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

想象一下同样的方法，集成到您自己的静态站点中。你可以在你主页的边栏或者一个专门的“书签”页面上显示书签列表；这取决于你。需要注意的重要一点是，一个站点可以是动态的，即使它的内容是以静态形式从服务器传送过来的。

真的没有必要再将书签数据从客户机发送到服务器，除非有一些实实在在的好处。将所有东西都放在客户端可以让我们的网站保持静态(快速、易于维护等)。)但也尊重用户的隐私；我们不关心他们选择哪些页面或产品作为书签，所以我们从来不询问这些细节。从我们读者的角度来看，这也是一种非常快速的体验，因为一切都是在他们的浏览器中即时发生的。

下次当你考虑一个特性时，考虑一下服务器参与进来的必要性；如果你能把它从等式中去掉，一个完全静态但功能齐全的站点就在你的掌握之中了！