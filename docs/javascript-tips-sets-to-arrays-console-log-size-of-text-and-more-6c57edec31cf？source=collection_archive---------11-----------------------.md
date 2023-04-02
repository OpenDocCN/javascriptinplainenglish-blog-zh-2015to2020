# JavaScript 提示-设置为数组、文本大小等

> 原文：<https://javascript.plainenglish.io/javascript-tips-sets-to-arrays-console-log-size-of-text-and-more-6c57edec31cf?source=collection_archive---------11----------------------->

![](img/cfc78913c4d3fa8831aa98a942c68924.png)

Photo by [Annika Treial](https://unsplash.com/@mannika?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

像任何类型的应用程序一样，当我们编写 JavaScript 应用程序时，有一些困难的问题需要解决。

在本文中，我们将研究一些常见 JavaScript 问题的解决方案。

# 将集合转换为数组

有很多种将集合转换成数组的方法。

一种方法是使用`Array.from`方法。

例如，我们可以写:

```
const array = Array.from(set);
```

我们也可以使用 spread 运算符进行同样的操作:

```
const array = [...set];
```

# 从父组件的子组件调用方法

我们可以通过给子组件分配一个引用，从父组件中的子组件调用一个方法。

然后，我们可以使用`current`属性访问该孩子。

例如，我们可以写:

```
const { forwardRef, useRef, useImperativeHandle } = React;const Child = forwardRef((props, ref) => {
  useImperativeHandle(ref, () => ({
    getAlert() {
      console.log("alert from child");
    }
  }));
  return <h1>hello</h1>;
});const Parent = () => {
  const childRef = useRef();
  return (
    <div>
      <Child ref={childRef} />
      <button onClick={() => childRef.current.getAlert()}>Click</button>
    </div>
  );
};
```

我们使用`forwardRef`函数将引用传递给子组件。

`useImperativeHandle`让我们向父母展示孩子选择的方法。

现在我们给`Child`赋值，然后我们可以通过`current`属性从`Parent`调用`getAlert`方法。

# 如何修复错误:倾听 EADDRINUSE 错误

`EADDRINUSE`表示端口号已经被使用。

这意味着无论我们使用什么应用程序，我们都必须监听不同的端口。

或者我们可以结束正在使用我们想要使用的端口的程序。

例如，如果我们更改 HTTP 服务器的端口，我们可以写:

```
const http = require('http');const server = http.createServer((req, res) => {
  res.end('test');
});server.on('listening', () => {
  console.log('ok, server is running');
});server.listen(80);
```

最后一行正在监听端口 80。

我们可以把它换成别的东西。

要终止程序，我们可以写:

```
killall -9 node
```

# 在谷歌浏览器 JavaScript 控制台中打印调试消息

我们可以在带有`javascript:`前缀的浏览器地址栏中运行 JavaScript。

例如，我们可以键入:

```
javascript: console.log('foo');
```

登录控制台中的`'foo'`。

其他控制台方法包括`console.error`将事情记录为错误。

`console.info`记录信息性消息。

`console.warn`记录警告。

我们可以用一些标签格式化字符串。

例如，我们可以写:

```
console.log("this is %o, event is %o, host is %s", this, e, location.host);
```

`%o`表示插值对象，`%s`表示字符串。

火狐有`console.trace();`方法来记录堆栈跟踪。

# 更好的写作方式 v = (v === 0？1 : 0)

我们可以将 `v = (v === 0 ? 1 : 0)`改写为:

```
v = (v ? 0 : 1);
```

因为 0 是假的。

如果`v`是 0 或另一个假值，那么我们给它赋值 1。否则，我们分配 0。

# 异步回调函数的返回值

无法从异步回调函数中返回值。

这是因为我们在不确定的时间内得到了这个值。

相反，我们只使用函数中的值。

所以如果我们有:

```
asyncCode({ address }, (results, status) => {
  fn(results[0].geometry.location); 
});
```

然后我们只在函数内使用`results`和`status`。

# 计算文本宽度

我们可以通过将文本放入 div 来计算文本宽度。

然后我们可以使用`clientWidth`和`clientHeight`属性分别得到宽度和高度。

例如，我们可以写:

```
const fontSize = 12;
const text= document.getElementById("text");
text.style.fontSize = fontSize;
const height = `${text.clientHeight}px`;
const width = `${text.clientWidth}px`;
```

只需获取文本，将字体大小更改为我们想要的大小，并获取这些属性。

# 检查变量是数字还是字符串

要检查变量是数字还是字符串，我们可以使用`typeof`操作符。

例如，我们可以写:

```
typeof "hello"
```

并得到`'string'`

如果我们写下:

```
typeof 123
```

我们得到`'number'`。

![](img/a908ca1b8a9c022d2d30e5f794da8683.png)

Photo by [Melissa Regina](https://unsplash.com/@tadmint?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 结论

有几种方法可以将集合转换成数组。

我们可以通过使用 forward refs 从父组件调用 React 子组件的方法。

在 JavaScript 中有许多记录项目的方法。

没有办法从异步回调函数返回值。

## 简单英语的 JavaScript

你知道我们有四份出版物和一个 YouTube 频道吗？在 [**plainenglish.io**](https://plainenglish.io/) 和 [**找到它们订阅解码，我们的 YouTube 频道**](https://www.youtube.com/channel/UCtipWUghju290NWcn8jhyAw) **！**