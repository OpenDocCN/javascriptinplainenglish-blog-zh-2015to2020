# 常见 JS 问题的解决方案—剪贴板、时间和请求

> 原文：<https://javascript.plainenglish.io/solutions-to-common-js-problems-clipboard-time-and-requests-a79094462109?source=collection_archive---------14----------------------->

![](img/098b4cd93460cd95f016f55b11f1956c.png)

Photo by [Aron Visuals](https://unsplash.com/@aronvisuals?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

一些 JavaScript 问题经常出现。经常出现的问题包括 DOM 问题、字符串格式、数组等等。

在本文中，我们看看如何用 JavaScript 解决我们在 web 开发中遇到的常见问题。

# **如何得到两个日期的差值(天数)？**

我们可以将`Date`对象转换成时间戳，然后减去时间戳，再转换成天数。

为此，我们创建以下函数:

```
const diffDays = (dateInitial, dateFinal) =>
  (dateFinal - dateInitial) / (1000 * 3600 * 24);
```

上面的代码通过首先将日期转换成时间戳来减去日期。有了返回的以毫秒为单位的整数差值，我们可以用它除以`1000 * 3600 * 24`得到以天为单位的差值。

当我们这样称呼它时:

```
console.log(diffDays(new Date('2018-12-13'), new Date('2019-12-22')));
```

我们得到 374。

# **如何向给定的 URL 发出 GET 请求？**

我们可以使用 Fetch API 来发出 GET 请求。

例如，我们可以如下使用它来从 JSON 占位符 API 获取一个 post:

```
(async () => {
  const res = await fetch('https://jsonplaceholder.typicode.com/posts/1')
  const post = await res.json();
  console.log(post);
})();
```

然后我们会得到这样的结果:

```
{
  "userId": 1,
  "id": 1,
  "title": "sunt aut facere repellat provident occaecati excepturi optio reprehenderit",
  "body": "quia et suscipit\nsuscipit recusandae consequuntur expedita et cum\nreprehenderit molestiae ut ut quas totam\nnostrum rerum est autem sunt rem eveniet architecto"
}
```

从控制台日志输出。

# 如何向给定的 URL 发出 POST 请求？

我们也可以用 Fetch API 来做这件事。

为此，我们可以编写以下代码:

```
(async () => {
  const data = {
    title: 'title',
    body: 'body'
  }
  const res = await fetch('https://jsonplaceholder.typicode.com/posts', {
    method: 'POST',
    headers: {
      'Content-Type': 'application/json',
    },
    body: JSON.stringify(data),
  })
  const post = await res.json();
  console.log(post);
})();
```

在上面的代码中，我们用请求方法传入了一个额外的对象，这个对象就是`'POST'`。另外，我们的请求头在`headers`属性中，请求体在字符串化的`body`属性中。

# **如何将字符串复制到剪贴板？**

我们可以通过创建一个 textarea 元素将一个字符串复制到剪贴板，用我们的字符串填充它，然后我们可以用`document.execCommand`方法运行`'copy'`命令。

为此，我们编写以下函数:

```
const copyStringToClipboard = (str) => {
  const el = document.createElement('textarea');
  el.value = str;
  el.setAttribute('readonly', '');
  el.style = {
    position: 'absolute',
    left: '-9999px'
  };
  document.body.appendChild(el);
  el.select();
  document.execCommand('copy');
  document.body.removeChild(el);
}copyStringToClipboard('foo')
```

在上面的代码中，我们将 textarea 的值设置为`str`，这样我们就可以在运行`document.execCommand(‘copy’)`时复制`str`的值。

此外，我们将元素移出屏幕，这样用户就看不到它了。

# **如何发现页面的浏览器选项卡是否被聚焦？**

我们使用`document.hidden`属性来查看它是否被聚焦，所以我们可以直接否定它来检查它是否被聚焦。

![](img/bc6e6d528aa8b976b45b5bcb5de93651.png)

Photo by [Nick Karvounis](https://unsplash.com/@nickkarvounis?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# **如何得到给定毫秒数的可读格式？**

我们可以通过下面的函数来实现:

```
const formatDuration = ms => {
  const time = {
    day: Math.floor(ms / (1000 * 60 * 60 * 24)),
    hour: Math.floor(ms / 60 * 60 * 24) % 24,
    minute: Math.floor(ms / 1000 * 60) % 60,
    second: Math.floor(ms / 1000) % 60,
    millisecond: Math.floor(ms) % 1000
  };
  return Object.entries(time)
    .filter(val => val[1] !== 0)
    .map(([key, val]) => `${val} ${key}(s)`)
    .join(', ')
};
```

我们所做的只是将毫秒数除以一天、小时、分钟、秒和毫秒中的毫秒数。

然后，我们删除所有对象条目中所有为零的 1。

最后，我们将它们全部连接在一起。

那么我们可以通过如下调用来尝试一下:

```
console.log(formatDuration(3432545574))
```

我们得到:

```
39 day(s), 34 minute(s), 5 second(s), 574 millisecond(s)
```

# **等待后如何调用提供的函数(以毫秒计)？**

我们可以使用`setTimeout`函数在给定的时间后运行一些东西。

例如，我们可以做一个`delay`函数如下:

```
const delay = (fn, wait, ...args) => setTimeout(fn, wait, ...args);
```

然后我们可以通过运行来调用它:

```
delay(
  (text) => {
    console.log(text);
  },
  1000,
  'foo'
);
```

然后我们看到`'foo'`在一秒钟后被记录，因为我们有 1000 毫秒的延迟，我们将`'foo'`传递给`setTimeout`的第三个参数，该参数被传递给`setTimeout`的回调参数。

# 结论

我们可以使用`setTimeout`运行延迟给定时间的功能。

属性检查一个标签是否隐藏。

`document.execCommand('copy')`将从文本区复制文本。

我们可以通过划分正确的数量来格式化时间戳，以获得天数、小时数、分钟数和秒数。

Fetch API 对于发出 HTTP 请求很有用。

## **用简单英语写的 JavaScript 笔记**

我们已经推出了三种新的出版物！请关注我们的新出版物，表达对它们的喜爱:[**AI in Plain English**](https://medium.com/ai-in-plain-english)，[**UX in Plain English**](https://medium.com/ux-in-plain-english)，[**Python in Plain English**](https://medium.com/python-in-plain-english)**——谢谢，继续学习！**

**我们也一直有兴趣帮助推广高质量的内容。如果您有一篇文章想要提交给我们的任何出版物，请发送电子邮件至[**submissions @ plain English . io**](mailto:submissions@plainenglish.io)**，并附上您的 Medium 用户名，我们会将您添加为作者。另外，请让我们知道您想加入哪个/哪些出版物。****