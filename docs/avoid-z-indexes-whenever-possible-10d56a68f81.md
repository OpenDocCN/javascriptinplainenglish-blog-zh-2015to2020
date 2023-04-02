# 尽可能避免 z 索引

> 原文：<https://javascript.plainenglish.io/avoid-z-indexes-whenever-possible-10d56a68f81?source=collection_archive---------9----------------------->

当我第一次听说`z-index` css 属性时，它听起来像是一个很有帮助且天真的概念。但是在使用了几年之后，我会宣布它们是 web 开发中价值数十亿美元的错误。但是让我更详细地解释一下。

# 一些历史

回到过去，如果你想确保你的一些 HTML 内容显示在你的其他内容之上，使用`z-index`是可行的方法。主要的用例是任何类型的覆盖图或对话框，试图引起用户的注意。通常这些组件也不允许点击它们之外的任何地方，因此用户必须小心处理它们。由于这是一种非常激进的获得关注的方式，这些元素在网站上很少使用。也许在整个网站上有一个单独的覆盖图来让你的用户注册你的时事通讯(并不是说我是这些的忠实粉丝…)或者一个覆盖图来展示一个图片库。

由于用例非常有限，处理它们并不是什么大事。这种覆盖的实现可能如下所示:

```
**<!DOCTYPE html>**
<html>
    <head>
        <style>
            **.overlay** {
                **width**: 300px;
                **height**: 200px;
                **background**: lightgray;
                **position**: absolute;
                **z-index**: 10;
                **top**: 30px;
                **left**: 100px;
                **font-size**: 0.5em;
            }
        </style>
    </head>
    <body>
        <h1>My headline</h1>
        <h2>My subheadline</h2> <div class="overlay">
            <h2>My overlay content</h2>
        </div>
    </body>
</html>
```

*你可以通过* [*这个 codepen*](https://codepen.io/danrot/pen/RwPzNeK) *来查看结果。*

当然，我在这里说的是 HTML5 之前的时代，那时你必须将`text/css`作为一种类型添加到你的`style`标签中，并使用一个更复杂的`DOCTYPE`，我现在懒得去查了。实际上，上面的例子甚至不需要`z-index`定义，但在这种情况下它仍然被大量使用，因为一些其他组件可能已经应用了`z-index`。

但是重点仍然是:在你的网站上放一个这样的覆盖图没什么大不了的，因为使用`z-index`的其他元素的数量是可以管理的..但是有趣的事情发生了:网络变得越来越互动。公司决定使用网络作为他们以前作为桌面软件构建的应用程序的平台。像这样的软件比 CSS 最初设计的网站甚至一些文档要复杂得多。这就是事情开始变得混乱的时候。直到今天，我看到 CSS 包含如下属性:

```
**.overlay** {
    **z-index**: 99999 **!important**;
}
```

**基本上，它只是把更高的数字放在你的 z-index 上，以确保你的覆盖层上没有其他东西。至少直到你想在覆盖层上放些东西，在这种情况下，你会得到一个更高的数字。我想你可以想象这不是很好的扩展。**

所以从那时起`z-index`可能是我最着迷的 css 属性。理解这一概念似乎难以置信的困难。最大的问题之一可能是这些值相互比较，如果有大量的值，很容易变得太复杂而难以管理。我做了一些关于如何驯服这种复杂性的研究，偶然发现了一些有趣的方法。

# 从游戏开发中汲取灵感

在一篇 CSS-Tricks 文章中，我遇到了一种有趣的方法。他们使用了一个在游戏开发中非常流行的想法，即将所有使用过的`z-index`值放在一个文件中。因为不是所有的浏览器([看着你，IE11](https://caniuse.com/#search=variables) )都支持新的 [CSS 自定义属性](https://developer.mozilla.org/en-US/docs/Web/CSS/--*)你通常不得不使用像 SCSS 这样的预处理器来实现它。

```
$zindexHeader: 1000;
$zindexOverlay: 10000;
```

这种方法有两个有趣的地方:

1.  在数字之间留一些空间便于以后添加更多的`z-index`值。
2.  定义所有的值可以让开发人员知道所有的`z-index`值。

然而，为了完成这项工作，你必须确保你的代码库中没有`z-index`属性，也就是没有使用这个文件的变量。[这篇文章还包含一个注释](https://css-tricks.com/handling-z-index/#comment-1580226)，它是这种方法的一个很好的变体。它不使用数字来定义值，而是使用 SCSS 列表和一个从列表中检索给定值索引的函数。这样做的好处是，您甚至不必再考虑任何数字，插入一个新值就像在列表中正确放置它一样简单。这听起来很棒，对吧？

# 介绍堆叠上下文

问题是，即使你严格遵守了这条规则，也可能会出现意想不到的结果。这是因为 HTML 规范中定义了[堆栈上下文](https://developer.mozilla.org/en-US/docs/Web/CSS/CSS_Positioning/Understanding_z_index/The_stacking_context)。浏览器将只在定义元素的堆栈上下文中应用`z-index`值。让我们看一个例子来更容易理解它的意思。

```
**<!DOCTYPE html>**
<html>
    <head>
        <style>
            **.box-1,** **.box-2,** **.box-3** {
                **position**: absolute;
                **width**: 100px;
                **height**: 100px;
            } **.box-1** {
                **top**: 20px;
                **left**: 20px;
                **z-index**: 1;
                **background-color**: blue;
            } **.box-2** {
                **top**: 20px;
                **left**: 20px;
                **z-index**: 3;
                **background-color**: red;
            } **.box-3** {
                **top**: 60px;
                **left**: 60px;
                **z-index**: 2;
                **background-color**: green;
            }
        </style>
    </head>
    <body>
        <div class="box-1">
            <div class="box-2"></div>
        </div>
        <div class="box-3"></div>
    </body>
</html>
```

如果你在没有任何堆栈上下文知识的情况下阅读这篇文章，你可能会说带有类`box-2`的红盒子应该出现在最前面。如果你是这样想的，你可以看看[这个密码本](https://codepen.io/danrot/pen/JjdQogG)来证明事实并非如此。

这种行为的原因是蓝盒子也有一个`z-index`值。它的值`1`比绿框的值`2`低，所以浏览器确保蓝框——包括其内容——将停留在绿框的下方。所以红框的`3`的`z-index`值只会和蓝框的其他子元素进行比较。菲利普·沃尔顿对此做了很好的详细解释。这多少有点道理，因为当你只比较`box-1`和`box-3`时，这可能就是你想要的。为这两个元素编写规则的开发人员可能想确保`box-3`在`box-1`之上，独立于它们的子元素。

不幸的是，这很可能与实现覆盖的开发人员所期望的行为相矛盾。我假设人们添加如此高的`z-index`值的原因是他们想要 100%确定元素总是可见的。如果元素是堆叠上下文的一部分，那么最大值没有帮助。但是更糟糕的是:当一个元素附加了一个`z-index`时，不仅引入了一个新的堆栈上下文，还引入了一些其他属性(参见 MDN 上的[完整列表)。因此，即使您构建的任何东西目前都在工作，其他开发人员也可能会在没有意识到的情况下，通过将这些属性添加到您元素的任何祖先来破坏它(我不能责怪他们，他们可能甚至不知道子树中有这样的元素)。](https://developer.mozilla.org/en-US/docs/Web/CSS/CSS_Positioning/Understanding_z_index/The_stacking_context)

**TL；** `**z-index**`和**博士叠加的语境非常复杂。**

这些问题甚至可能不会出现在使用普通 JavaScript 编写的网站上。只要将覆盖图放在结束的`body`标签之前，它们就会出现在其他所有东西的上面。至少在没有其他`z-index`设置的情况下是这样，因为浏览器会将 HTML 源代码中出现较晚的元素呈现在出现较早的元素之上。这部分听起来很简单，对吗？

问题是，随着库认可组件中的思想(例如 [React](https://reactjs.org/) )，这并不容易实现。这是因为组件树深处的某个组件可能想要呈现一个覆盖图，该覆盖图应该出现在所有其他元素的顶部，而不管它位于源代码中的什么位置。让我们假设您的应用程序具有如下结构:

*   应用
*   App -> `Header`
*   App -> `Form`
*   `Form`->-`Input`
*   `Form`->-
*   `Form` - >提交按钮
*   `Form` - >确认对话框

我猜对于`Header`和`Form`组件来说，拥有一些`z-index`值并不罕见，以确保`Header`将显示在`Form`组件的前面。如果表单现在呈现一个对话框以确认，例如存储所提供的信息，如果组件结构在 DOM 中以相同的方式呈现，则不可能在`Header`前显示该对话框。

但是让我们假设应用程序中没有使用其他的`z-index`——或者任何创建新堆栈上下文的属性。即使这样，您也会遇到问题，因为在 React 中，您可能希望实现一个可以在多个地方重用的单个`Overlay`组件。如果您要显示多个，那么将正确的一个显示在另一个前面可能也很棘手。这是因为`Overlay`分量总是具有相同的`z-index`值。如果你依赖于`z-index`来做这种事情，你可能需要传递一个`z-index`道具给你的 React 组件。这感觉就像我们在做一个完整的循环，回到我们开始的地方:**试图找到一个比其他人都高的数字。**不过还好博文还没写完。

# 从这里去哪里？

我觉得到现在为止我已经咆哮了很多，但我认为理解是什么导致了我接下来要解释的决定是很重要的。当我在很短的时间内开发 [Sulu 的](https://sulu.io/)管理界面时，我面临着两个不同的`z-index`问题。所以我决定在这方面多下点功夫。我从经典的 [*JavaScript:好的部分*](http://shop.oreilly.com/product/9780596517748.do) 中获得了灵感。我读这本书已经有一段时间了，我知道 JavaScript 改变了很多，这本书里的一些建议已经过时了，但我仍然喜欢它的总体思想。摆脱引起麻烦的东西。所以这正是我所做的:[我从 Sulu 的代码库中移除了(几乎)所有的](https://github.com/sulu/sulu/pull/5138) `[z-index](https://github.com/sulu/sulu/pull/5138)` [值。可能看起来有点激进，但我相信从长远来看这是值得的。](https://github.com/sulu/sulu/pull/5138)

你可能认为你的特殊用例需要`z-index`才能让一切正常工作，但是我们正在构建一个相对复杂的单页面应用程序，我可以去掉所有的`z-index`属性。我认为我所做的可以分为两个不同的任务。

首先，我不得不说，仅仅通过在 DOM 中正确地排序元素，就可以避免这么多属性，这真是令人难以置信。你想确保某种元素出现在图片的顶部来编辑图片吗？试着把这些元素放在你的 HTML 源文件中的图片之后，这样就可以了！对此完全不需要`z-index`，并且您将避免上述任何原因可能会破坏您的应用程序。我认为这是在试图避免错误时最重要的认识。

使用这个建议，只有一个元素不是那么容易转换的:在我们的管理界面中带有总是可见的工具栏的标题。问题是它出现在最顶端，在 HTML 中把它放在首位是实现这一点的一种方法。但是如果标题在前面，我就必须添加一个`z-index`让它出现在后面的内容前面。然后，我试图移动容器末尾的标题，但是它出现在了容器的底部(我不想开始使用`position: absolute`或类似的东西，因为它有自己的问题，可能会填充一个单独的博客)。有一小段时间，我认为这没关系，但后来我意识到标题有一个方框阴影，隐藏在 HTML 中标题下面的元素后面。

```
**<!DOCTYPE html>**
<html>
    <head>
        <style>
            ***** {
                **margin**: 0;
                **padding**: 0;
            } header {
                **height**: 20vh;
                **background-color**: steelblue;
                **box-shadow**: 0 1vh 1vh teal;
            } main {
                **height**: 80vh;
                **background-color**: ivory;
            }
        </style>
    </head>
    <body>
        <header></header>
        <main></main>
    </body>
</html>
```

*本*[*codepen*](https://codepen.io/danrot/pen/abOgvEG)*显示的是蓝绿色* `*box-shadow*` *的表头不可见。非常诱人地用了一个* `*z-index*` *…*

我想到的解决方案是将头部放在容器的末尾，并对容器元素使用`flex-direction: column-reverse`。这使得 header 元素出现在其他元素的顶部，因为它稍后出现在源代码中，但仍然显示在屏幕的顶部，因为 flexbox 颠倒了元素的顺序。我尝试过用`order` CSS 属性做类似的事情，但是没有任何运气。我不完全确定这种方法对可访问性有什么影响，但是我想用`z-index`值来纠缠也没有多大帮助。

```
**<!DOCTYPE html>**
<html>
    <head>
        <style>
            ***** {
                **margin**: 0;
                **padding**: 0;
            } body {
                **display**: flex;
                **flex-direction**: column-reverse;
            } header {
                **height**: 20vh;
                **background-color**: steelblue;
                **box-shadow**: 0 1vh 1vh teal;
            } main {
                **height**: 80vh;
                **background-color**: ivory;
            }
        </style>
    </head>
    <body>
        <main>
        </main>
        <header>
        </header>
    </body>
</html>
```

*这个*[*codepen*](https://codepen.io/danrot/pen/yLNdYjX)*展示了如何改变元素的顺序，使用* `*column-reverse*` *也可以做到，而不需要* `*z-index*` *。*

现在剩下的唯一未决问题是像覆盖图这样的组件如何适应这个等式，特别是如果组件树深处的组件想要呈现它。实际上 [React 有一个名为 portals](https://reactjs.org/docs/portals.html) 的内置概念来帮助解决这个问题。门户允许脱离父组件的 DOM。这也解决了覆盖图的其他问题，例如，如果父容器的`overflow`设置为`hidden`，那么就不可能显示父容器之外的任何内容。我们经常使用门户，并在 body 标签的末尾添加内容。由于元素现在呈现在`body`标签的最末端——我们没有任何其他的`z-index`集合——设置一个`z-index`是完全没有必要的！我遇到的唯一缺点是 DOM 中的顺序似乎依赖于门户代码被调用的时间。这导致了一个令人困惑的情况，我很快就解决了，而且没有再产生任何问题。仍然感觉是值得知道的事情，可能有助于调试类似的情况。

最后一点:我提到我只移除了几乎所有的`z-index`。有些仍然是必要的，但只是因为一些第三方库使用了`z-index`。解决方案是在这些库的父容器上自己设置一个`z-index` of `0`。这引入了新的堆叠上下文，确保这些库的这些元素不会显示在我们的覆盖图之前。但是“不依赖于`z-index`”仍然在我评估库的标准列表中。

最后我想说:**每当你要加一个**`**z-index**`**——如果真的没有更好的解决办法，就好好想想。**

## **用简单英语写的 JavaScript 笔记**

我们已经推出了三种新的出版物！请关注我们的新出版物:[**AI in Plain English**](https://medium.com/ai-in-plain-english)[**UX in Plain English**](https://medium.com/ux-in-plain-english)[**Python in Plain English**](https://medium.com/python-in-plain-english)**——谢谢，继续学习！**

**我们也一直有兴趣帮助推广高质量的内容。如果您有一篇文章想要提交给我们的任何出版物，请发送电子邮件至[**submissions @ plain English . io**](mailto:submissions@plainenglish.io)**，使用您的 Medium 用户名，我们会将您添加为作者。另外，请让我们知道您想加入哪个/哪些出版物。****