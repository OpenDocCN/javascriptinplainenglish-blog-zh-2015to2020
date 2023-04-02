# jQuery 技巧——就绪状态、大小和 Ajax

> 原文：<https://javascript.plainenglish.io/jquery-tips-ready-state-sizes-and-ajax-a9141c15a034?source=collection_archive---------5----------------------->

![](img/8343f166bd2927778aa585c0c77599ff.png)

Photo by [Globe City Guide 🌎](https://unsplash.com/@globecityguide?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

尽管年代久远，jQuery 仍然是操纵 DOM 的流行库。

在本文中，我们将了解一些使用 jQuery 的技巧。

# $(文档)。没有 jQuery 的现成等效项

我们可以创建自己的函数，通过检查应用程序的加载状态来实现类似于`$(document).ready`的功能。

例如，我们可以写:

```
function ready(callback){  
  if (document.readyState !== 'loading') {
    callback();
  } else if (document.addEventListener)  {
    document.addEventListener('DOMContentLoaded', callback);
  } else {
    document.attachEvent('onreadystatechange', () => {
      if (document.readyState === 'complete') {
        callback();
      }
    });
  }  
}
```

我们通过比较文档的`readyState`和`'loading'`状态来检查它。

如果不是，那么 DOM 被加载，我们可以调用我们的`callback`。

如果不可用，那么我们为`DOMContentLoaded`添加一个事件监听器，它被加载，这意味着事件被触发，然后我们调用我们的`callback`。

否则，我们用`attachEvent`来观察`readystatechange`事件。这个只在 IE 8 或更早版本上使用，所以我们可能不需要这个。

# 获取屏幕、当前网页和浏览器窗口的大小

我们可以通过使用`height`和`width`方法得到屏幕的大小。

例如，我们可以写:

```
$(window).height();
$(window).width();
```

分别获取视口的高度和宽度。

同样，我们可以用`document`代替`window`来得到`document`的高度和宽度。

所以我们可以写:

```
$(document).height();
$(document).width();
```

# 使用 jQuery 中止 Ajax 请求

jQuery 返回的 XHR 对象有`abort`方法让我们取消请求。

例如，我们可以写:

```
const xhr = $.ajax({
  type: "POST",
  url: "some.php",
  data: "first_name=james&last_name=smith",
  success: (res) => {
    alert(res);
  }
});xhr.abort()
```

我们得到由`$.ajax`返回的`xhr`对象，然后我们对它调用`abort`来取消它。

# 使用 jQuery 获取元素的 ID

我们可以用 jQuery 通过使用`attr`方法获得一个元素的 ID。

例如，我们可以写:

```
$('.test').attr('id')
```

我们用类`test`获得第一个元素的 ID。

同样，我们可以通过调用`get`或使用括号来获取 DOM 对象，从而通过 DOM 对象获取它。

例如，我们可以写:

```
$('.test').get(0).id;
```

或者:

```
$('.test')[0].id;
```

或者我们可以使用`prop`方法编写:

```
$('.test').prop('id')
```

# 使用 jQuery 将选项作为 JavaScript 对象添加到 Select from 中

我们可以通过使用`append`方法将`options`对象作为`select`的子对象添加到选择下拉列表中。

例如，我们可以写:

```
for (const o of options) {
  $('#mySelect')
    .append(
      $("<option></option>")
        .attr("value", o)
        .text(o)
    ); 
}
```

我们用`$`创建了`option`元素，并向选项元素添加了一个值属性和文本。

# 管理 jQuery Ajax 调用后的重定向请求

通过在`success`回调中进行重定向，Wed 可以在 jQuery Ajax 调用后管理重定向请求。

例如，我们可以写:

```
$.ajax({
  type: "POST",
  url: reqUrl,
  data: reqBody,
  dataType: "json",
  success: (data) => {
    window.location.href = data.redirect;
  }
});
```

我们用`ajax`发出 POST 请求。

我们通过传入 URL 和 body 来做到这一点。

我们将`dataType`设置为 JSON，用 JSON 有效负载发出请求。

然后在`success`回调中，我们从响应数据中获取重定向 URL，它在第一个参数中。

我们通过将它设置为`window.location.href`来进行重定向。

# 使用 jQuery 更改超链接的 href

我们可以用`attr`方法改变超链接的`href`。

例如，我们可以写:

```
$("a[href='http://www.google.com/']").attr('href', 'http://www.example.com/')
```

我们将`href`设置为`http://www.google.com`的链接替换为`http://www.example.com`。

`attr`将属性名作为第一个参数，将值作为第二个参数。

![](img/701c9249b350a2e142795c37c654fee8.png)

Photo by [Lukas L](https://unsplash.com/@lukaslinden?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 结论

我们可以动态地改变链接的 href。

此外，我们可以获得视窗或文档的宽度和高度。

Ajax 请求可以被中止。

我们可以动态添加下拉选项。

# 简单英语的 JavaScript

喜欢这篇文章吗？如果有，通过 [**订阅解码，我们的 YouTube 频道**](https://www.youtube.com/channel/UCtipWUghju290NWcn8jhyAw) **获取更多类似内容！**