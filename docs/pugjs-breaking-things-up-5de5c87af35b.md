# 用 PugJS 分解代码

> 原文：<https://javascript.plainenglish.io/pugjs-breaking-things-up-5de5c87af35b?source=collection_archive---------5----------------------->

## 如何将你的单个 HTML 页面分解成有用的和可重复使用的部分

![](img/a1d760ef32188cc47246bd023b28766a.png)

Photo by [Greg Rakozy](https://unsplash.com/@grakozy?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) on [Unsplash](https://unsplash.com/s/photos/html?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)

嗨，这是我正在做的 PugJS 系列的第 2 部分，如果你还没有看过的话，可以看看第 1 部分[【这里】](https://medium.com/@Unknown_minimalist/pugjs-replacing-html-541508001af)。

不，今天不是一个关于我如何退出 PugJS 的悲伤分手故事。它是把你的单个文件 HTML 页面分解成有用的和可重用的部分。

我们可以使用**包含**来实现这一点

```
include components/head.pug
```

使用它，你可以在你的组件目录中有一个头文件，它包含你的信息，或者一个脚本文件，你可以在其中加载你所有的 JavaScript 文件。然后您可以简单地包含它们，而不是每次都编写代码。

另一个有用的用途是能够在每一页上创建相同的页眉、页脚或其他元素，并且能够将它们包含到所有文件中，而不必一次又一次地编写它们。您还可以在页面中包含多个文件，如下所示:

```
doctype html
html
    include components/head.pug
    body
        include components/header.pug div
            p Some content here include components/footer.pug
        include components/scripts.pug
```

在这个代码片段中，我们创建了一个 html 页面，它导入了标题、页眉、页脚和脚本。帕格文件。

*>但是我一定要为每一页写周围的 html 和 body 标签吗？*

不，你没有。PugJS 还有一个有用的特性，可以使用 **extends** 和 **block** 关键字进行继承。使用它，您可以定义一个布局文件，并使用另一个文件用您的内容简单地扩展它。一个简单的方法是使用与上面代码片段相同的内容:
(粗体文本便于阅读)

布局. pug:

```
doctype html
html
    include components/head.pug
    body
        include components/header.pug div
            **block content** include components/footer.pug
        **block scripts** include components/scripts.pug
```

帕格:

```
**extends layout****block content**
    p Here is some content**block scripts**
    script(src='/yourPackage.js')
```

对于这么少的代码来说，这似乎有些违背直觉，但是相信我，一旦一个文件变得足够大，这将是一个巨大的帮助。

如您所见，您也可以拥有多个块，并像我们在这里对脚本所做的那样向它们添加块。

这是本系列的第 2 部分。稍后我很可能会重点介绍脚本、循环和混合。

我希望这对你有所帮助，请继续关注第 3 部分…