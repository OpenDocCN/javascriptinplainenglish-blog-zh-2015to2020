# JavaScript 技巧——移除选项、创建随机令牌等等

> 原文：<https://javascript.plainenglish.io/javascript-tips-removing-options-create-random-tokens-and-more-b0db8c5adf2a?source=collection_archive---------12----------------------->

![](img/1593bb59c0c800e4e6e100e8c78b31b0.png)

Photo by [Eric Prouzet](https://unsplash.com/@eprouzet?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

像任何类型的应用程序一样，当我们编写 JavaScript 应用程序时，有一些困难的问题需要解决。

在本文中，我们将研究一些常见 JavaScript 问题的解决方案。

# Node.js 中的安全随机令牌

我们可以用 Node.js 轻松地生成一个安全的随机令牌。

例如，我们可以写:

```
require('crypto').randomBytes(48, (err, buffer) => {
  const token = buffer.toString('hex');
});
```

我们导入`crypto`库并调用它的`randomBytes`方法。

它用一个我们可以转换成十六进制字符串的`buffer`来调用回调。

# 表演。连接对象数组中的值

如果我们首先调用`map`来获取值，那么我们可以对对象数组中的值调用`join`，

例如，如果我们有:

```
const students = [
  { name: "james", age: 12 },
  { name: "alex", age: 14 },
  { name: "mary", age: 11 }
]
```

然后我们可以通过编写来使用`map`和`join`:

```
students.map(s => s.name).join(",");
```

我们获取数组中每个对象的`name`属性，并调用`join`用逗号将它们连接起来。

# 仅在指定字符的第一个实例上拆分字符串，并获取其后的子字符串

我们可以在和`split`一起使用的正则表达式中使用捕捉括号。

例如，我们可以写:

```
"foo_bar_baz".split(/_(.+)/)[1]
```

这个模式将只捕捉第一个下划线。

`_.+`表示下划线及其后的所有内容。

然后我们得到从它返回的`'bar_baz'`。

我们也可以使用`substring`和`indexOf`来做同样的事情。

例如，我们可以写:

```
const str = "foo_bar_baz";
const newStr = str.substring(str.indexOf('_') + 1);
```

`indexOf`得到第一个`_`实例的索引，然后我们加 1 得到字符串的其余部分。

# 解决一个又一个承诺

为了解决一个接一个的承诺，我们可以写一个`async`函数。

例如，我们可以写:

```
async function writeFiles(texts) {
  for(const text of texts) {
    await writeFile(text);
  }
};
```

我们可以通过 for-of 循环使用`async`和`await`来写文件。

`writeFile`是一个返回承诺的函数。

此外，我们可以承诺在 Noe 应用程序中现有的异步调用。

我们可以用蓝鸟做到这一点:

```
const Promise = require("bluebird");
const fs = Promise.promisifyAll(require("fs"));const readAll = Promise.resolve(files).map(fs.readFile,{ concurrency: 1 });readAll.then((allFileContents) => {
    // do stuff to read files.
});
```

我们将`fs.readFile`方法转换成承诺，这样我们就可以调用`then`来锁住它们。

# 获取不带查询字符串的 URL

要获得没有查询字符串部分的 URL，我们可以使用`split`方法。

例如，我们可以写:

```
window.location.href.split('?')[0]
```

我们带着问号调用`split`，得到第一部分。

此外，我们可以写:

```
const url = `${location.origin}${location.pathname}${location.hash}`;
```

获取查询字符串之前的所有部分并将它们连接在一起。

获取 URL 的域部分。

`location.pathname`获取 IRL 的路径。

而`location.hash`得到散列部分。

# 用 Moment.js 从日期中删除时间

要用 moment.js 从日期中删除时间，我们可以调用`format`来格式化日期字符串。

例如，我们可以写:

```
moment().format('LL');
```

我们也可以使用:

```
moment().startOf('day')
```

为了得到这一天。

# 从选择框中删除项目

我们可以使用 jQuery 从选择框中移除一个项目。

例如，如果我们有给定的 select 元素:

```
<select name="fruit" id="fruit">
  <option value="apple">Apple</option>
  <option value="orange">Orange</option>
  <option value="grape">Grape</option>
</select>
```

然后我们可以通过写来删除它:

```
$("#fruit option[value='grape']").remove();
```

我们可以使用选择器删除值为`grape`的选项。

# 使用 jQuery 访问 iFrame 父页面

我们可以向`$`函数传递第二个参数来指定我们想要访问父窗口。

例如，我们可以写:

```
$('#parentEl', window.parent.document).html();
```

我们传入`window.parent.document`值来从父窗口而不是当前窗口中选择 ID 为`parentEl`的元素。

![](img/8696a8497441c06a88fbe067ee63e1f0.png)

Photo by [Mike Benna](https://unsplash.com/@mbenna?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 结论

我们可以使用`crypto`模块创建一个随机令牌。

要对数组中对象的属性调用 join，我们可以先调用`map`来获取数组中的值。

同样，我们可以通过使用`location`对象的各种属性来获得没有查询字符串的 URL。

我们可以通过选择器删除选项元素。