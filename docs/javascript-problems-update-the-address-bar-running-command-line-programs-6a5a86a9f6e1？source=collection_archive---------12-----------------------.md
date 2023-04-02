# JavaScript 问题—更新地址栏、运行命令行程序等等

> 原文：<https://javascript.plainenglish.io/javascript-problems-update-the-address-bar-running-command-line-programs-6a5a86a9f6e1?source=collection_archive---------12----------------------->

![](img/4ab99062149676babd10c2a2ff0e042b.png)

Photo by [NOAA](https://unsplash.com/@noaa?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

像任何类型的应用程序一样，当我们编写 JavaScript 应用程序时，有一些困难的问题需要解决。

在本文中，我们将研究一些常见 JavaScript 问题的解决方案。

# 用不带哈希的新 URL 更新地址栏，或者重新加载页面

我们可以通过使用`window.history.pushState`方法将地址栏中的 URL 改为一个新的 URL，而无需重新加载页面。

例如，我们可以写:

```
window.history.pushState("data", "title", "/new-url");
```

第一个参数是我们要传递给 URL 的数据。

第二个参数是标题，这是大多数浏览器所忽略的。

第三是新的网址本身。

或者，我们可以使用`document.location.href`来改变没有`pushState`的浏览器上的 URL。

例如，我们可以写:

```
document.location.href = "/new-url";
```

# 用 Node.js 运行命令行二进制文件

要用 Node.js 运行命令行程序，我们可以使用`child_process`模块的`exec`方法。

比如 m w 可以写；e:

```
const { exec } = require('child_process');
exec('cat *.js > jsFile', (err, stdout, stderr) => {
  if (err) {
    return;
  }
  console.log(`stdout: ${stdout}`);
  console.log(`stderr: ${stderr}`);
});
```

我们运行`cat`命令并在回调中记录输出。

我们可以使用`exec`的 promise 版本做同样的事情。

例如，我们可以写:

```
const util = require('util');
const exec = util.promisify(require('child_process').exec);const ls = async () => {
  const { stdout, stderr } = await exec('ls');
  console.log('stdout:', stdout);
  console.log('stderr:', stderr);
}
ls();
```

我们将`child_process`的`exec`方法转化为承诺。

然后我们用它来获取内容`ls`并将它们记录到控制台。

还有一个名为`execSync`的同步版本的`exec`函数。

例如，我们可以写:

```
const { execSync } = require('child_process');
const stdout = execSync('ls');
```

我们只是像其他代码一样一行一行地运行它。

# 用 jQuery 获取选定的元素标记名

我们可以使用`prop`属性获得元素的标签名。

例如，我们可以写:

```
$("a").prop("tagName");
```

获取一个`a`元素的标签名。

我们可以将它与其他选择器一起使用。

# JavaScript 绑定方法的目的

`bind`方法用于根据我们的需要改变函数中`this`的值。

它返回一个带有新值`thi`的函数。

例如，我们可以写:

```
const myButton = {
  content: 'OK',
  click() {
    console.log(this.content);
  }
};
```

`this.content`应该是`'OK'`，因为它是`this`对`myButton`的引用。

要改变`this.content`的值，我们可以通过编写调用`bind`:

```
const boundClick = myButton.click.bind({ content: 'Cancel' });
```

我们使用`bind`将`this`的值设置为:

```
{ content: 'Cancel' }
```

然后当我们通过写`boundClick()`来调用`boundClick`时，我们看到`'Cancel'`被记录了。

# 通过对象的属性来定义数组

我们可以通过将回调传入`sort`方法，根据条目的属性对数组中的条目进行排序。

例如，我们可以写:

```
people.sort((a, b) => {
  if(a.firstname < b.firstname) { return -1; }
  if(a.firstname > b.firstname) { return 1; }
  return 0;
})
```

我们用比较运算符比较字符串。

字符串按其字符串字符的代码点逐一比较排序。

如果返回的数字小于 0，则`a`的指数低于`b`。

如果返回的数字大于 0，则`b`的索引低于`a`。

否则，它保持不变。

为了进行区分区域的比较，我们使用`localeCompare`进行比较。

例如，我们可以写:

```
people.sort((a, b) => a.firstname.localeCompare(b.firstname))
```

# 延迟按键处理程序

我们可以通过使用`setTimeout`功能来延迟`keyup` 处理程序的运行时间。

我们在每次`keyup` 事件触发时重置超时，这样每次按键时都会发生延迟。

例如，我们可以写:

```
const delay = (fn, ms) => {
  let timer = 0;
  return (...args) => {
    clearTimeout(timer)
    timer = setTimeout(fn.bind(this, ...args), ms || 0)
  }
}
```

然后我们可以通过书写来使用它:

```
$('#input').keyup(delay(function (e) {
  console.log(this.value);
}, 500));
```

![](img/a1a327a632b3d22ba5db16ca99767eda.png)

Photo by [Fabrizio Frigeni](https://unsplash.com/@ffrige?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 结论

我们可以使用`pushState`方法更改网址，而无需刷新页面。

`bind`方法用于通过返回一个新的函数来改变`this`的值。

我们可以通过`exec`方法从节点程序运行命令行程序。

# 简单英语中的 JavaScript

你知道我们有四个出版物和一个 YouTube 频道吗？在 [**找到他们所有人。点击**](https://plainenglish.io/) 和 [**订阅解码，我们的 YouTube 频道**](https://www.youtube.com/channel/UCtipWUghju290NWcn8jhyAw) **！**