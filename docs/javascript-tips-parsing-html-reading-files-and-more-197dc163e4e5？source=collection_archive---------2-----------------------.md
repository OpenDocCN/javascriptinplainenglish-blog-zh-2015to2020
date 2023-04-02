# JavaScript 技巧——解析 HTML、读取文件等等

> 原文：<https://javascript.plainenglish.io/javascript-tips-parsing-html-reading-files-and-more-197dc163e4e5?source=collection_archive---------2----------------------->

![](img/c4f972cbe288cae1fca129785f640de6.png)

Photo by [Sarah Bedu](https://unsplash.com/@sarahbedu?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

像任何类型的应用程序一样，当我们编写 JavaScript 应用程序时，有一些困难的问题需要解决。

在本文中，我们将研究一些常见 JavaScript 问题的解决方案。

# 用 JavaScript 解析 HTML 字符串

我们可以通过创建一个不附加到 DOM 的`html`元素，用 JavaScript 解析一个 HTML 字符串。

然后我们可以用它来得到我们想要的/

例如，如果我们有:

```
const html = `
<html> <head>
    <title>title</title>
  </head> <body><a href='test'>link</a></body></html>
`
```

然后我们可以写:

```
const el = document.createElement('html');
el.innerHTML = html;
const anchor = el.querySelector('a');
```

我们用`createElement`创建了`html`元素。

然后我们用`html`设置`innerHTML`来填充新元素的字符串中的条目。

然后我们可以使用任何 DOM 方法，如`querySelector`来获取 HTML 中的元素。

还有用于解析 DOM 的`DOMParser`构造函数。

例如，我们可以写:

```
const parser = new DOMParser();
const htmlDoc = parser.parseFromString(html, 'text/htm;');
```

其中`html`是和我们之前一样的字符串。

# 将字符串转换成浮点数

我们可以使用`parseFloat`函数将字符串解析成浮点数。

例如，我们可以写:

```
const value = parseFloat("123.45");
```

我们用它把 123.45 解析成一个浮点数。

如果我们使用不是句点的小数点分隔符，那么我们必须用句点来替换它。

例如，我们可以写:

```
const value = parseFloat("123,45".replace(",", "."));
```

# HTML 中的自定义属性

如果我们想给元素添加自定义属性，那么我们必须给它加上前缀`data-`。

例如，我们写道:

```
<p data-date-changed="2020-01-01">Hello</p>
```

然后我们可以用`getAttribute`得到属性值:

```
const dateChanged = document.querySelector("p").getAttribute("data-date-changed");
```

# 从 URL 获取 YouTube 视频 ID

我们可以通过拆分查询字符串从 URL 中获取 YouTube 视频 ID。

为此，我们可以使用`URLSearchParams`构造函数。

例如，我们可以写:

```
const urlSearchParams = new URLSearchParams(window.location.search.slice(1))
const videoId = urlSearchParams.get('v');
```

第一行用查询字符串创建了`URLSearchParams`实例。

我们使用`lice`从`window.location.search`管柱中取出`?`。

然后我们用查询参数键`v`调用`get`。

而`videoId`将是视频 ID。

# 从目录中的所有文件导入模块

如果我们从每个模块中导出所有内容，我们可以从一个目录中导入所有模块。

所以在一个模块中，我们可以写:

`things/index.js`

```
export * from './moduleA';
export * from './moduleB';
export * from './moduleC';
```

然后我们可以通过编写来导入所有内容:

```
import { ModuleA, ModuleB, ModuleC } from './things';
```

我们在`index.js`中导出每个模块中的所有内容。

然后我们在另一个模块中导入它们。

我们可以对默认导出做同样的事情。

例如，我们可以写:

`things/index.js`

```
export { default as ModuleA } from './moduleA'
export { default as ModuleB } from './moduleB'
export { default as ModuleC } from './moduleC'
```

然后，我们可以通过编写以下内容来导入它们:

```
import { ModuleA, ModuleB, ModuleC } from './things'
```

# innerText 和 innerHTML 的区别

`innerText`和`innerHTML`的区别在于`innerText`只能处理纯文本。

设置为其值的所有内容都将被解释为纯文本。

另一方面，`innerHTML`会将 HTML 文本解释为 HTML，并在屏幕上呈现它们。

# 当从文件输入中选择文件时，解析 C:\fakepath

当我们选择文件输入时，我们无法获得完整的文件路径。

但是，我们可以将文件的内容读入一个字符串。

例如，我们可以写:

```
const input = document.querySelector("input");
input.onchange = () => {
  const fileReader = new FileReader();
  fileReader.readAsDataURL(input.files[0]);
  fileReader.onloadend = (event) => {
    console.log(event.target.result);
  }
}
```

`input`元素以`file`类型输入。

我们可以通过设置一个监听器到`onchange`属性来监视文件的变化。

然后我们创建一个新的`FileReader`实例并调用`readAsDataURL`方法来读取 base64 字符串形式的数据。

当文件被读取时，连接到`fileReader`的`onloadend`监听器将运行。

`event.target.result`有 base64 字符串，里面有文件的完整内容。

![](img/6938199ec3606df31d774e6902a993c5.png)

Photo by [Helena Lopes](https://unsplash.com/@wildlittlethingsphoto?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 结论

我们可以通过将字符串设置为`innerHTML`或使用`DOMParser`来解析 HTML 字符串。

我们可以用`parseFloat`将字符串转换成浮点数。

要在文件被选中时读取其内容，我们可以使用`FileReader`构造函数。

同样，我们可以用`*`从一个模块中导出所有东西。

## 简单英语的 JavaScript

喜欢这篇文章吗？如果有，通过 [**订阅获取更多类似内容解码，我们的 YouTube 频道**](https://www.youtube.com/channel/UCtipWUghju290NWcn8jhyAw) **！**