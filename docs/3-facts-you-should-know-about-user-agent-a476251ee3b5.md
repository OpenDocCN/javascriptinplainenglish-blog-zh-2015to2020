# 关于用户代理你不知道的是

> 原文：<https://javascript.plainenglish.io/3-facts-you-should-know-about-user-agent-a476251ee3b5?source=collection_archive---------3----------------------->

## 每个开发人员都应该知道的关于用户代理的 3 个事实

![](img/97be14a5dca7357fa55963698ded40aa.png)

在花半天时间深入研究用户代理问题之前，我希望知道一些事实。我将分享我在这篇文章中学到的东西。

首先，让我们定义什么是用户代理。

> 用户代理(UA)是包含在 HTTP 头中的字符串，用于浏览器检测:识别访问用户的设备/平台，并可用于确定要返回的适当内容。

## 用户代理一团糟

事实是，用户代理充满了谎言。在很多情况下，[一个浏览器用用户代理](https://webaim.org/blog/user-agent-string-history/)伪装成另一个浏览器。在用户代理的历史上，浏览器经常伪装成另一个浏览器，因为一些网站基于像 Mozilla 这样的浏览器类型提供内容(框架等)。伪装成“Mozilla”是用框架接收内容的最简单方法。

所以现在每个浏览器都伪装成另一个类似 Mozilla 的浏览器。一个例子就是 Chrome 自称 *M* `*ozilla/5.0 (Windows; U; Windows NT 5.1; en-US) AppleWebKit/525.13 (KHTML, like Gecko) Chrome/0.2.149.27 Safari/525.13*` *。*

不幸的是，几乎所有的浏览器都这样做，这使得用户代理完全一团糟。

## 使用 UA 的浏览器检测不可靠

用户代理通常用于浏览器检测，通常包括在 UA 字符串中搜索特定的字符串或模式。下面是一个简单的例子。

```
if (window.navigator.userAgent.indexOf('MSIE') > 0){
    alert('IE is not supported');
}
```

这是一个快速简单的解决方案，但也有代价。一个问题是用户代理字符串在新版本中可能会改变。然后，脚本需要更新。如果 UA 浏览器检测的目的是使用/不允许某个 web 功能，找到每个浏览器用户代理是不实际的，因为有太多的变化。

一个更好的解决方案是使用像 [Modernizr](https://modernizr.com/) 这样的工具进行特征检测。

## 用户代理将被谷歌弃用

2020 年，谷歌宣布[它将“反对并冻结”用户代理](http://User Agent will be deprecated)。

> “除了这些隐私问题，用户代理嗅探是兼容性问题的一个丰富来源，特别是对少数民族浏览器来说，导致浏览器对自己撒谎，网站(包括谷歌属性)[在一些浏览器中被破坏](https://www.google.com/url?q=https%3A%2F%2Fwww.onmsft.com%2Fnews%2Fgoogle-has-apparently-blocked-its-stadia-cloud-gaming-service-on-the-chromium-based-microsoft-edge&sa=D&sntz=1&usg=AFQjCNGLbwpKABVCN-FuD6HvibnwdHJ-5g)没有好的理由，”[负责 Chrome 浏览器的谷歌工程师 Yoav Weiss](https://groups.google.com/a/chromium.org/forum/#!msg/blink-dev/-2JIRNMWJ7s/yHe4tQNLCgAJ) 说。

谷歌的第一步是用 Chrome 81 弃用`navigator.userAgent`方法，然后随着 2020 年 6 月 Chrome 83 的发布，每次更新都会冻结用户代理字符串。Chrome 85 将统一跨设备的用户代理。

谷歌取代用户代理的是[客户端提示](https://developers.google.com/web/fundamentals/performance/optimizing-content-efficiency/client-hints)。

围绕谷歌的举动有很多争论和怀疑，但在苹果和微软的支持下，用户代理的逐步淘汰似乎是不可避免的。

## 结论

以下摘自 [MDN Web](https://developer.mozilla.org/en-US/docs/Web/HTTP/Browser_detection_using_the_user_agent) 的摘录很好地总结了本文的论点。

> 使用用户代理嗅探很少是个好主意。您几乎总能找到更好、更广泛兼容的方法来解决您的问题！

*如果您还不是 Medium 的付费会员，* [***您可以访问此链接***](https://sunnysun-5694.medium.com/membership) *。你可以无限制地阅读媒体上的所有报道。我会收你一部分会员费作为介绍费。*