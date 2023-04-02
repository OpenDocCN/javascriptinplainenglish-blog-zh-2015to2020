# 有用的 JavaScript 技巧—查询字符串舍入和 URL

> 原文：<https://javascript.plainenglish.io/useful-javascript-tips-query-strings-rounding-and-urls-c53b4153cf40?source=collection_archive---------12----------------------->

![](img/359a74e01db451f47beffd32c8e881f4.png)

Photo by [Alfred Lutz](https://unsplash.com/@lutzelhof?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

像任何类型的应用程序一样，JavaScript 应用程序也必须写得很好。

否则，我们以后会遇到各种各样的问题。

在本文中，我们将了解一些应该遵循的技巧，以便更快更好地编写 JavaScript 代码。

# 获取查询字符串值

JavaScript 的标准库有`URLSearchParams`构造函数，可以用来提取查询参数并让我们搜索它们。

例如，我们可以写:

```
const urlParams = new URLSearchParams(window.location.search);
const val  = urlParams.get('key');
```

`window.location.search`返回 URL 的查询字符串部分。

`get`方法通过查询参数的键获取值。

# 将数字四舍五入到最多两位小数

我们可以使用`Math.round`让我们四舍五入到小数点后两位。

例如，我们可以写:

```
Math.round(num * 100) / 100
```

我们将`num`乘以 100，四舍五入，然后除以 100，得出一个最多有两位小数的数字。

如果有少于第一百位的十进制数字，那么我们在乘以 100 之前加上`Number.EPSILON`。

所以我们写道:

```
Math.round((num + Number.EPSILON) * 100) / 100
```

此外，我们可以使用`parseFloat`和`toFixed`将一个数字格式化为一个包含两位小数的字符串。

我们可以写:

```
parseFloat("575.393").toFixed(2);
```

# 将字符串转换为布尔值

我们可以通过编写以下代码将字符串`'true'`和`'false'`转换为它们对应的布尔值:

```
val === 'true'
```

假设`val`可以设置为`'true'`或`'false'`，我们只需与`'true'`进行比较即可。

# 将对象存储在本地存储中

要在本地存储中存储一个对象，我们必须先把它转换成一个字符串。

例如，我们可以写:

```
const obj = { 'foo': 1, 'bar': 2, 'baz': 3 };
localStorage.setItem('data', JSON.stringify(obj));
```

在调用`setItem`之前，我们调用了`JSON.stringify`来将带有`key`数据的 stringified 对象存储在本地存储中。

`JSON.stringify`不会保留`undefined`、函数、`Infinity`和正则表达式，所以我们必须小心。

# 合并两个或更多对象

我们可以使用扩展操作符或`Object.assign`将两个或更多对象的属性合并在一起。

例如，我们可以写:

```
const merged = {...obj1, ...obj2, ...obj3};
```

或者:

```
const merged = Object.assign({}, obj1, obj2, obj3, etc);
```

两段代码都将`obj1`、`obj2`和`obj3`的属性填充到一个空对象中。

# 检测元素外部的单击

我们可以调用`stopPropagation`来阻止我们从外部检查点击的元素的点击事件。

所以我们可以写:

```
window.addEventListener('click', () => {
  // do something when clicked outside
});document.querySelector('#menucontainer').click((event) => {
  event.stopPropagation();
});
```

`window`的`click`事件处理程序让我们在用户点击外部时运行代码。

# 刷新页面

用 JavaScript 刷新页面有很多方法，

我们可以做到:

*   `location.reload();`
*   `history.go(0)`
*   `location.href = location.href`
*   `location.href = location.pathname`
*   `location.replace(location.pathname)`
*   `location.reload(false)`

`location.pathname`是相对 URL。

`history.go`也通过重新加载同一页面来刷新。

# 漂亮的打印 JSON

我们可以用`JSON.stringify`方法美化 JSON。

例如，我们可以写:

```
const str = JSON.stringify(obj, null, 2);
```

2 表示我们缩进 2 个空格。

# 将命令行参数传递给 Node.js 程序

节点程序可以访问`process.argv`属性来获取程序的所有命令行参数。

例如，我们可以写:

```
for (const a of process.argv){
  console.log(a)
}
```

第一个参数总是`'node'`。第二个总是我们运行的文件的路径。

其余的是其他命令行参数。

# 检查十进制数

我们可以通过使用`isNaN`函数和`isFinite`函数来检查十进制数。

为了进行检查，我们编写:

```
!isNaN(parseFloat(n)) && isFinite(n);
```

`parseFloat`将字符串解析成浮点数。

`isNaN`检查返回值是否为`NaN`。

`isFinite`检查数字或数字串是否代表有限的数字。

# 修改 URL 而不重新加载页面

无需用 JavaScript 重新加载页面就可以修改 URL。

`window.history.pushState`让我们向浏览历史添加一个新的 URL。

我们可以写:

```
window.history.pushState('foo', 'title', '/foo.html');
```

第一个参数是状态对象。

我们可以用`popstate`来获取参数的值。

`'title'`是标题，在大多数浏览器中被忽略。

`'/foo.html'`是要写入浏览历史的 URL。

![](img/a2558decff6d6caf641e133b281faba8.png)

Photo by [Katerina Kerdi](https://unsplash.com/@katekerdi?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 结论

`URLSearchParams`构造函数让我们将查询字符串解析成一个我们可以查找的有用对象。

我们可以使用`Math.round`将一个数字四舍五入到小数点后两位。

此外，我们可以更改 URL 并将其添加到浏览历史中，而无需重新加载页面。

## 简单英语中的 JavaScript

你知道我们有四个出版物和一个 YouTube 频道吗？在 [**找到他们所有人。点击**](https://plainenglish.io/) 和 [**订阅解码，我们的 YouTube 频道**](https://www.youtube.com/channel/UCtipWUghju290NWcn8jhyAw) **！**