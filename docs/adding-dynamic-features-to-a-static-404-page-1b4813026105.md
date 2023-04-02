# 向静态 404 页面添加动态功能

> 原文：<https://javascript.plainenglish.io/adding-dynamic-features-to-a-static-404-page-1b4813026105?source=collection_archive---------16----------------------->

## 使用 JavaScript 超越“未找到”

![](img/a38cea6d199b687c14f01403a20d48d3.png)

Photo by [Kelly Sikkema](https://unsplash.com/@kellysikkema?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

我最近写了一篇关于[给静态网站](https://medium.com/javascript-in-plain-english/adding-dynamic-features-to-a-static-site-cf56ff7b163a)添加动态特性的文章，现在我想举另一个例子:你网站的 404 页面。

通常，404 页面应该是静态的，即使是在后台运行脚本语言的动态生成的站点上。这是一个错误页面，您不希望花费太多资源来应对最坏的情况。然而，我们甚至没有在静态站点上执行巧妙的服务器端处理的奢侈，所以 404 应该只是一些表明“未找到”的文本，对吗？不对。

# 基本原则

首先，您的 404 页面应该仍然发送 404 错误，而不是 200 (OK)或任何重定向状态代码(3xx)，这一点很重要。通常，正如 GitHub Pages 的情况一样，您的静态主机会默认启用这一功能，但这值得检查。在 GitHub 页面上运行意味着我只需要上传一个路径为`/404.html`的文件，我的自定义 404 页面就准备好了。

一旦你有了一个基本的 404 页面，是时候考虑一下你可以在上面添加什么样的客户端功能了。我将在这里展示两种可能性:

1.  一些试图诊断问题的解释性文字
2.  可能是预期目标的可选页面列表

像往常一样，我已经将这些例子的代码[上传到 GitHub 库](https://github.com/bobbykjack/dynamic-static-404)。

# 诊断

为了识别坏链接的来源，我们将查看`[Document.referrer](https://developer.mozilla.org/en-US/docs/Web/API/Document/referrer)`属性，它告诉我们读者来自的页面的 URL，前提是他们来自链接到我们的页面。

```
var referrer = document.referrer;
```

如果你的 404 页面实现正确——就像在 GitHub 页面上一样——这将按预期工作，即使 404 页面本身并不是链接的实际目标。如果 referrer 包含一个值，我们可以将其转换为一个 [URL](https://developer.mozilla.org/en-US/docs/Web/API/URL) 对象，以便进一步检查:

```
if (ref) {
    ref = new URL(ref);

    if (ref.hostname == curr.hostname) {
        // case 1: internal link
    } else {
        // case 2: external link
    }
} else {
    // case 3: insufficient info
}
```

注意，我们只是通过查看`hostname`来确定这个推荐人是我们自己的网站还是其他人的网站；这有点过于简单，但是对于我们的目的来说，这是一个合适的“站点”定义。

*请注意，一些浏览器可能不会在推荐人中发送外部的完整 URL 如果没有，他们至少应该发送完整的主机名，这比什么都没有好。*

这里另一个有趣的部分是我们的`mailto:`链接。这包括`subject`和`body`参数，全部都包含在`mailto_link`函数中:

```
function mailto_link(to, subject, body) {
    var a = document.createElement("a"),
        mail_href = "mailto:" + to
            + "?subject=" + encodeURIComponent(subject)
            + "&body=" + encodeURIComponent(body); a.setAttribute("href", mail_href);
    a.appendChild(document.createTextNode("send me an email"));
    return a;
}
```

产生的`Element`被注入到我们的最终页面中，当我们调用这个函数时，我们传递给它一些文本，包括引用者和原始 URL，例如:

```
body = "I spotted a broken link to your site on the following "
    + "URL:\n\n" + document.referrer + "\n\n"
    + "It was a link to:\n\n" + window.location;p.appendChild(mailto_link(to, subject, body);
```

这让我们的访问者有机会点击一个链接，然后立即通过电子邮件向我们发送一份“报告”,以最少的大惊小怪。你可能会在一些网站的 404 页面上看到类似“请联系我们并告诉我们问题”这样的内容，但是如果我们能代表读者完成大部分工作，那就更好了。

# 建议的替代方案

我们将在 404 页面上包含的另一个功能是可选页面列表。这在服务器端脚本语言中似乎是“最好”的，页面存储在数据库中，但是对于我们的静态站点，我们可以利用已经可用的页面列表:[我们的站点地图](https://medium.com/@bobbyjack/generating-a-sitemap-for-a-static-site-356fa3c26074)。

如前所述，站点地图是一个 XML 文档，包含我们站点的页面列表，与我们的站点内容一起托管，这使得它非常适合客户端处理。要开始使用它，使用 [XMLHttpRequest 类](https://developer.mozilla.org/en-US/docs/Web/API/XMLHttpRequest)从服务器请求它:

```
var request = new XMLHttpRequest();
request.addEventListener("load", process_sitemap);
request.open("GET", "/sitemap.xml");
request.send();
```

在第二行中，`process_sitemap`是一个回调函数的名称，一旦响应被返回，这个函数就代表我们被调用。第三行指定了我们要获取的 URL:`/sitemap.xml`。

在回调函数中，我们可以访问 sitemap `[Document](https://developer.mozilla.org/en-US/docs/Web/API/Document)`并像处理网页一样处理它；特别是，我们可以使用 CSS 选择器使用`[querySelectorAll()](https://developer.mozilla.org/en-US/docs/Web/API/Document/querySelectorAll)`方法获取站点地图的位置(URL ):

```
function process_sitemap() {
    var sitemap_doc = this.responseXML,
        urls = sitemap_doc.querySelectorAll("urlset > url > loc");
    ...
}
```

一旦 URL 列表可用，我们就可以使用任何我们认为合适的方法来选择一个接近原始请求 URL 的集合。在我的例子中，我使用第三方函数(`getEditDistance`)来衡量 URL 的相似程度，使用 [Levenshtein 距离算法](https://en.wikibooks.org/wiki/Algorithm_Implementation/Strings/Levenshtein_distance#JavaScript)。该算法的细节超出了本文的范围；重要的一点是，我们获取了自己的站点地图，并在客户端处理它，而不必求助于在数据库或服务器的文件系统中查找。

# 例子

这里有一个链接到我自己的网站的 URL 的例子，我在这里使用了这种技术。URL 不存在，所以您应该会看到 404 页面，并在顶部添加了动态功能。

喜欢这篇文章吗？如果有，通过 [**订阅解码获得更多类似内容，我们的 YouTube 频道**](https://www.youtube.com/channel/UCtipWUghju290NWcn8jhyAw) **！**