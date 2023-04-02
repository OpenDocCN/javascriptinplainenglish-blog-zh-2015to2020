# JavaScript 技巧—下载文件、性能测试等等

> 原文：<https://javascript.plainenglish.io/javascript-tips-download-files-performance-test-and-more-4ceaad3f383a?source=collection_archive---------5----------------------->

![](img/f16339e89df5942515f9412fe9bdc96f.png)

Photo by [Kirill Petropavlov](https://unsplash.com/@kp89_?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

像任何类型的应用程序一样，当我们编写 JavaScript 应用程序时，有一些困难的问题需要解决。

在本文中，我们将研究一些常见 JavaScript 问题的解决方案。

# 使用 Javascript 下载文件

我们可以用 JavaScript 下载一个文件。

为此，我们可以使用 Fetch API。

我们可以这样称呼它:

```
fetch('https://www.w3.org/TR/PNG/iso_8859-1.txt')
  .then(resp => resp.blob())
  .then(blob => {
    const url = window.URL.createObjectURL(blob);
    const a = document.createElement('a');
    a.style.display = 'none';
    a.href = url;
    a.download = 'sample.txt';
    document.body.appendChild(a);
    a.click();
    window.URL.revokeObjectURL(url);
  })
  .catch(() => alert('oh no!'));
```

我们调用`fetch`对文件发出 GET 请求。

然后我们调用`blob`把它变成二进制对象。

接下来，我们调用`createObjectURL`从 blob 创建一个 URL。

然后我们创建一个`a`元素，用对象 URL 作为`a`元素的 URL 来创建一个不可见的链接。

然后我们可以调用`click`来下载文件。

最后，我们调用`revokeObjectURL`来清理数据 URL。

# 从变量值创建对象特性

我们可以通过在括号之间传递变量来创建一个对象。

例如，我们可以写:

```
obj[a] = 'foo';
```

然后我们将`a`的值设置为属性的标识符。

`a`可以是字符串或符号。

同样，我们可以在对象文字中使用相同的符号。

例如，我们可以写:

```
const obj = { [a]: b };
```

我们在对象文字的括号中传递变量`a`。

# JavaScript Hashmap 等价物

map 是一个相当于 hashmap 的 JavaScript。

为了使用它，我们创建了一个`Map`实例:

```
const map = new Map();
map.set('foo', 'bar');
map.set('baz', 1);
```

我们创建了一个`Map`实例并调用`set`方法来添加或更新条目。

然后我们可以使用`size`属性来获取地图的大小。

# 从字符串中提取一个数

为了从一个字符串中提取一个数字，我们可以使用`match`字母来查找匹配给定模式的所有子字符串。

例如，我们可以写:

```
const txt = "123 foo 456";
const nums = txt.match(/(\d+)/g);
```

这将匹配所有连续的数字。

`g`表示我们在整个字符串中搜索这个模式。

`\d`是一个数字。

那么`nums`就是`[“123”, “456”]`。

# 检查数组是空的还是存在的

我们可以通过使用`Array.isArray`方法和`length`属性来检查数组是空的还是存在的。

例如，我们可以写:

```
if (Array.isArray(array) && array.length) {
  // ...
}
```

我们检查`array`是否是一个有`Array.isArray`的数组。

然后我们使用`length`属性来获取数组的长度。

我们不必明确地检查长度，因为 0 是假的，所有其他数字都是真的。

所以`array.length`会检查长度是否非零。

# 深度合并对象，而不是进行浅层合并

我们可以使用 Lodash 的`merge`方法深度合并对象。

例如，我们可以写:

```
_.merge(obj1, obj2, obj3);
```

我们可以用它合并任意多的对象。

返回的对象不会有引用现有对象的属性。

# 用 Javascript 创建一个样式标签

通过使用`createElement`创建一个样式元素，我们可以用 JavaScriot 创建一个样式标签。

例如，我们可以写:

```
const css = 'h1 { background: red; }';
const head = document.head;
const style = document.createElement('style');
style.appendChild(document.createTextNode(css));
head.appendChild(style);
```

我们在一个字符串中创建了一些 CSS 样式。

然后我们用`document.head`得到头部元素。

接下来，我们创建了一个样式元素。

然后我们把 CSS 放到样式元素中。

最后，我们将样式元素添加到 head 元素中。

# 测试 JavaScript 代码的性能

为了测试 JavaScript 代码的性能，我们可以使用`console.time`和`console.timeEnd`方法。

例如，我们可以写:

```
console.time('test');
for(let i = 0; i < 1000000; i++){
  //...
};
console.timeEnd('test')
```

我们用一个字符串标识符调用`console.time`来启动性能测试计时器。

然后我们用相同的标识符调用`console.timeEnd`来结束计时器。

![](img/74f717a1e0b1ebbbb1992ab5a14223a4.png)

Photo by [Robert Metz](https://unsplash.com/@___rob__?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 结论

JavaScript `console`对象有让我们测量代码时间的方法。

我们可以用 Fetch API 下载文件。

`Map`是 JavaScript 的 hashmap。

Lodash `merge`让我们深度合并对象。

我们可以给对象添加动态属性。

## 简单英语的 JavaScript

喜欢这篇文章吗？如果有，通过 [**订阅我们的 YouTube 频道**](https://www.youtube.com/channel/UCtipWUghju290NWcn8jhyAw) **获取更多类似内容！**