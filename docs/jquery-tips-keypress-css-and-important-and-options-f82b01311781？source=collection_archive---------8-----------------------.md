# jQuery 提示—按键、CSS 和重要以及选项

> 原文：<https://javascript.plainenglish.io/jquery-tips-keypress-css-and-important-and-options-f82b01311781?source=collection_archive---------8----------------------->

![](img/24fb6515c7a4bce6e99753c24e649cce.png)

Photo by [FLOUFFY](https://unsplash.com/@theflouffy?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

尽管年代久远，jQuery 仍然是操纵 DOM 的流行库。

在本文中，我们将了解一些使用 jQuery 的技巧。

# 如何申请！重要用途。css()？

我们可以以各种方式应用`!important`。

应用它的一种方法是为一个类创建我们自己的 CSS 规则，然后将该类应用于我们的元素。

例如，我们可以写:

```
.important { width: 100px !important; }
```

使用`!important`指令添加 CSS 属性。

然后我们调用`addClass`来添加这个类。

例如，我们可以写:

```
$('#foo').addClass('important');
```

我们将`important`类应用于 ID 为`foo`的元素。

同样，我们可以调用`attr`将`style`属性值添加到元素中。

例如，我们可以写:

```
$('#foo').attr('style', 'width: 100px !important');
```

我们用自己的规则设置了`style`属性，用 CSS 规则设置了`!important`指令。

同样，我们可以传入`'cssText'`作为`css`的第一个参数来应用带有`!important`的 CSS 规则。

例如，我们可以写:

```
$('#foo').css('cssText', 'width: 100px !important');
```

以`'cssText'`作为第一个参数，我们可以应用`!important`。

# 使用 jQuery 检测键盘上的 Enter 键

我们可以通过在 jQuery 时监听 keypress 事件来检测 Enter 键的按下。

例如，我们可以写:

```
$(document).on('keypress', (e) => {
  if (e.which === 13) {
    console.log('enter is pressed');
  }
});
```

我们得到用`which`属性按下的键。

13 是回车键的代码，所以我们可以用它来对比。

还有一些浏览器中可能有的`keycode`属性。

所以我们可以写:

```
$(document).on('keypress', (e) => {
  if (e.which === 13 || e.keyCode === 13) {
    console.log('enter is pressed');
  }
});
```

一个更现代的选择，可能是最好的选择，是使用`key`属性。

例如，我们可以写:

```
$(document).keypress((event) => {
  if (event.key === "Enter") {
    // ...
  }
});
```

`key`属性包含被按下的键的文本，而不是数字代码。

这比其他选择更清楚。

# jQuery Ajax 错误处理和显示自定义异常消息

我们可以在`error`处理程序中得到错误。

例如，我们可以写:

```
$.ajax({
  type: "post", 
  url: "/login",
  success: data, text) => {
    //...
  },
  error: (request, status, error) => {
    console.log(request.responseText);
  }
});
```

我们用`ajax`方法发出请求。

`type`有请求类型。

`url`是网址。

`success`是成功处理程序。

`error`是错误处理程序。

`request`具有带有请求响应文本的`responseText`属性。

`status`有状态码。

`error`发出请求时出错。

错误仅在发出请求时出现网络错误。

# 在 jQuery 的选择元素中选择特定选项

在 select 元素中选择一个特定的选项，我们可以得到具有`value`属性的选项。

例如，我们可以写:

```
$('select option[value="foo"]')
```

我们获得了将`value`属性设置为`foo`的选项。

如果我们想通过索引得到选项，可以使用`eq`伪元素。

我们可以写道:

```
$('select option:eq(1)')
```

此外，我们还可以使用`contains`伪元素通过文本得到一个选项元素。

例如，我们可以写:

```
$('select option:contains("foo")')
```

我们使用`contains`伪元素来搜索具有给定文本的元素。

我们可以调用`filter`返回给定文本的选项元素。

例如，我们可以写:

```
$('select option')
  .filter((e, i) => { 
    return $(e).text() === "foo"
  })
```

我们得到了选项元素。

在回调中，我们有参数`e`，它是 select 元素的选项元素对象。

`text`方法返回选项元素的文本。

为了让给定值的项目被选中，我们可以通过各种方式来完成。

例如，我们可以写:

```
$('select option[value="foo"]').attr("selected", "selected");
```

我们将`selected`属性添加到`option`元素中。

为了让我们的生活更简单，我们可以使用`val`方法来设置 select 元素的值。

例如，我们可以写:

```
$('select').val('foo');
```

我们将该值设置为`foo`以选择值为`foo`的选项元素。

![](img/63941132854ec5f61529f4569bc3c144.png)

Photo by [Charlie Green](https://unsplash.com/@charliegreen998?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 结论

有很多方法可以将`!important`指令应用到我们的 CSS 中。

还有很多方法可以选择带有值或文本的选项元素。

我们可以通过听按键事件来得到按键。

# 简单英语中的 JavaScript

喜欢这篇文章吗？如果是这样，通过 [**订阅解码得到更多类似内容，我们的 YouTube 频道**](https://www.youtube.com/channel/UCtipWUghju290NWcn8jhyAw) **！**