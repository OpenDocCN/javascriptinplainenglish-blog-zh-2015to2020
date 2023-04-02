# jQuery 技巧——数组、Ajax、文本框更改和箭头键

> 原文：<https://javascript.plainenglish.io/jquery-tips-arrays-ajax-textbox-changes-and-arrow-keys-670e88adbed?source=collection_archive---------11----------------------->

![](img/a3ff7141553bc86f62edf3343d60c9bc.png)

Photo by [Andy Brunner](https://unsplash.com/@andy_brunner?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

尽管年代久远，jQuery 仍然是操纵 DOM 的流行库。

在本文中，我们将了解一些使用 jQuery 的技巧。

# 使用 jQuery 从数组中删除特定值

jQuery 有一个`grep`方法让我们返回一个新的数组，其中不包含我们想要删除的项。

例如，我们可以写:

```
const arr = [1, 2, 2, 6, 2, 6, 8, 3];
const itemToRemove = 2;const newArr = $.grep(arr, (value) => {
  return value !== itemToRemove;
});
```

我们用一个数组调用`$.grep`来检查。

我们传入一个回调函数来返回一个数组，该数组返回一个布尔表达式来返回匹配项。

`value !== itemToRemove`表示我们返回一个没有`itemToRemove`值的数组。

# 对 jQuery 和 Ajax 使用基本身份验证

使用 jQuery，如果我们设置了请求头，就可以使用基本身份验证。

为此，我们可以写:

```
beforeSend(xhr) {
  const token = btoa(`${username}:${password}`);
  xhr.setRequestHeader ("Authorization", `Basic ${token}`);
},
```

我们从组合在一起的`username`和`password`中创建一个令牌。

然后我们用令牌设置`Authorization`头。

# 用 jQuery 绑定箭头键

我们可以通过使用`keydown`方法来监听箭头键的按下。

例如，我们可以写:

```
const arrow = { left: 37, up: 38, right: 39, down: 40 };$(window).keydown((e) => {
  switch (e.which) {
    case arrow.left:
      //..
      break;
    case arrow.up:
      //..
      break;
    case arrow.right:
      //..
      break;
    case arrow.down:
      //..
      break;
  }
});
```

我们听着窗户上的按键声。

然后我们可以在回调的参数中获取事件对象。

它有`which`属性，带有我们按下的键的键码。

然后我们可以使用一个`switch`语句来查找被按下的键的匹配。

# 使用 jQuery 将 JavaScript 对象转换为数组

我们可以使用`map`方法将 JavaScript 对象转换成数组。

它对对象和数组都有效。

例如，我们可以写:

```
const obj = {
  1: 'foo',
  2: 'bar'
};const array = $.map(obj, (value, index) => {
  return [value];
});console.log(array);
```

我们用我们的`obj`对象作为第一个参数调用`map`。

在第二个参数中，我们传入一个回调，该回调将每个对象条目的`value`作为第一个参数。

`index`是被循环的键，是第二个参数。

我们返回数组中的`value`来返回放入数组中的条目。

它应该包含在一个数组中。

然后我们可以用控制台日志检查`array`值。

# 如何检测文本框的内容已经改变

为了检测文本框的内容是否已经改变，我们可以监听输入事件。

例如，我们可以写:

```
$('#textarea').on('input', () => {
  //...
});
```

我们可以用`bind`做同样的事情:

```
$('#textarea').bind('input', () => {
  //...
});
```

我们还可以监听`propertychange`和`paste`事件来监听输入更改和粘贴文本。

例如，我们可以写:

```
$('#textarea').on('input propertychange paste', () => {
  //...
});
```

# 自动滚动到页面底部

我们可以通过调用`animate` 自动滚动到页面底部。

例如，我们可以写:

```
$('html,body').animate({ scrollTop: document.body.scrollHeight }, "fast");
```

我们用一个物体来称呼`animate`。

该对象有一些滚动选项。

`scrollTop`是我们想要滚动到的 y 坐标。

第二个参数是动画的速度。

# 在 jQuery 中显示加载微调器

我们可以使用`show`和`hide`方法显示和隐藏装载微调器。

例如，我们可以写:

```
const spinner = $('#spinner').hide();
$(document)
  .ajaxStart(() => {
    spinner.show();
  })
  .ajaxStop(() => {
    spinner.hide();
  });
```

`spinner`具有加载微调器元件。

我们一开始就藏起来了。

然后，当 Ajax 请求开始时，我们会显示微调器。

这是通过传入对`ajaxStart`方法的回调，然后在我们的微调器元素上调用`show`来完成的。

同样，我们可以通过在`ajaxStop`方法的回调中调用`spinner.hide()`来隐藏微调器。

![](img/64d004bb603728796981269c80c2f8f7.png)

Photo by [Lisa H](https://unsplash.com/@lh_photography?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 结论

jQuery 有一个`grep`方法，其作用类似于 JavaScript 数组的`filter`方法。

我们可以通过 jQuery 收听箭头键的按压。

`ajaxStart`和`ajaxStop`让我们分别在 Ajax 请求开始和结束时运行代码。

jQuery 的`map`方法适用于对象和数组。

## **简单明了的 JavaScript**

喜欢这篇文章吗？如果是这样，通过 [**订阅解码得到更多类似内容，我们的 YouTube 频道**](https://www.youtube.com/channel/UCtipWUghju290NWcn8jhyAw) **！**