# 使用 JavaScript 检测广告拦截器

> 原文：<https://javascript.plainenglish.io/detecting-an-ad-blocker-using-javascript-d488b794e5c7?source=collection_archive---------0----------------------->

## 只用几行 JavaScript 代码就能增加你的博客或网站的收入

![](img/ce6bace39c25fd4821062df28bff4b98.png)

Photo by [Fotis Fotopoulos](https://unsplash.com/@ffstop?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) on [Unsplash](https://unsplash.com/s/photos/programming?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)

出版商和抵制展示广告的人之间的斗争已经持续了一段时间，而且还远没有结束。用户无法使显示反广告拦截消息的博客或网站看不到广告拦截器。相反，出版商没有办法完全并始终如一地让他们的网站免受广告拦截器的攻击。

由于广告拦截扩展，出版商需要找到新的方法来弥补收入损失，要么通过实施特定的广告再插入应用程序，要么通过安装付费墙。

# 什么是广告拦截器？

广告拦截器是一种浏览器扩展，通过阻止特定脚本和 DOM 元素来禁用某些网页中的广告。基本上，它们是一个巨大的黑名单，列出了哪些文件不应该被加载，或者哪些域不应该加载文件。

当你打开一个网站时，你的广告拦截器会查看是否有任何列入黑名单的 JavaScript 文件(针对谷歌的广告- `adsbygoogle.js`)，它会阻止这些脚本加载，因此，广告将不会显示。查看热门黑名单— [easylist.txt](https://easylist-downloads.adblockplus.org/easylist.txt)

## 如何检测广告拦截器？

通过实现一个小诱饵脚本是知道用户有广告拦截器的最简单的方法。如果你放置一个带有 **ads** 类名的 div 元素，那么广告拦截器会阻止它的渲染。我们可以通过使用另一个具有独特标识符的 div 来利用这一点。

**HTML**

```
<div id="notify">
    <div class="ads">
    </div>
</div>
```

**CSS**

```
.ads {
   width: 1px;
}
```

我们可以使用 JQuery 编写一个脚本来检查元素的宽度

**JavaScript**

```
$(document).ready(function(){
    if($("#notify").width() > *0*) {
        alert('No AdBlocker');
    } else {
        alert('AdBlocker Detected');
    }
});
```

# 流行的广告拦截脚本

## [检测 Adblock](https://www.detectadblock.com/)

所有阻止列表都包含对“ads.js”的引用，因为这是与提供广告相关的 JavaScript 文件的通用名称。了解这一点后，将下面创建隐藏 div 的 JavaScript 代码保存到一个名为“ads.js”的文件中，并将其放在网站的根目录下。

```
var e=document.createElement('div');
e.id='ECKckuBYwZaP';
e.style.display='none';
document.body.appendChild(e);
```

我们需要检查是否加载了“ads.js”。将 JavaScript 放在 HTML 源代码中的

```
<script src="/ads.js" type="text/javascript"></script>
<script type="text/javascript">

if(document.getElementById('ECKckuBYwZaP')){
  alert('Blocking Ads: No');
} else {
  alert('Blocking Ads: Yes');
}

</script>
```

## [F**k AdBlock](https://github.com/sitexw/FuckAdBlock)

F**k AdBlock 是由社区开发的反广告拦截脚本，也称为 BlockAdBlock。非常简单有效。你可以去他们的[网站](https://github.com/sitexw/BlockAdBlock)，参考代码和示例实现。

F**kAdBlock 用的诱饵是这样的:

```
baitClass:   'pub_300x250 pub_300x250m pub_728x90 text-ad textAd text_ad text_ads text-ads text-ad-links'
```

这意味着它将通过参考由 [IAB](https://www.iab.com/) (互动广告局)推荐的标准名称和普通广告图像尺寸来激活广告拦截软件

# 替代方法

这是一个你可以借助 Google Adsense ( `adsbygoogle.js` ) JavaScript 文件进行编码的方法。

我使用 HEAD 作为请求类型，因为它比 GET 小得多，也快得多。您可以将请求 URL 更改为任何已知的广告 JavaScript 并进行检查。

我希望你会发现它们有用！如果你有自己的建议，请在下面的评论中分享。

## **简明英语团队的笔记**

你知道我们有四种出版物吗？给他们一个 follow 来表达爱意:[**JavaScript in Plain English**](https://medium.com/javascript-in-plain-english)[**AI in Plain English**](https://medium.com/ai-in-plain-english)[**UX in Plain English**](https://medium.com/ux-in-plain-english)[**Python in Plain English**](https://medium.com/python-in-plain-english)**—谢谢，继续学习！**

**此外，我们总是有兴趣帮助推广好的内容。如果您有一篇文章想要提交给我们的任何出版物，请发送电子邮件至[**submissions @ plain English . io**](mailto:submissions@plainenglish.io)**，附上您的媒体用户名和您感兴趣的内容，我们将会与您联系！****