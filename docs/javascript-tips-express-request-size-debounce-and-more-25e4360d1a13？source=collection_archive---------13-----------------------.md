# JavaScript 技巧——快速请求大小、去抖等等

> 原文：<https://javascript.plainenglish.io/javascript-tips-express-request-size-debounce-and-more-25e4360d1a13?source=collection_archive---------13----------------------->

![](img/199dd30baa2ff0b03b7496c914ba38bc.png)

Photo by [Parsing Eye](https://unsplash.com/@parsingeye?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

像任何类型的应用程序一样，当我们编写 JavaScript 应用程序时，有一些困难的问题需要解决。

在本文中，我们将研究一些常见 JavaScript 问题的解决方案。

# 错误:快速应用程序中的请求实体太大

如果我们发出的请求超过了允许的大小限制，我们会在 Express 应用程序中得到请求实体太大的错误。

要调整极限，我们可以写:

```
app.use(express.json({ limit: '50mb' }));
app.use(express.urlencoded({ limit: '50mb' }));
```

我们有`limit`属性，让我们设置请求的最大大小。

`json`设置 JSON 请求的最大大小。

`urlencoded`为 URL 编码的请求设置相同的内容。

# React 分析错误:相邻的 JSX 元素必须用封闭标记括起来

在 React 组件的顶层不能有多个元素。

为了确保不是这种情况，我们可以将它们包装在片段或根元素周围。

例如，我们可以写:

```
return (
  <div>
    <Comp1 />
    <Comp2 />
  </div>
)
```

将`Comp1`和`Comp2`包裹在一个 div 周围。

我们可以使用如下的片段:

```
return (
  <>
    <Comp1 />
    <Comp2 />
  </>
)
```

或者:

```
return (
  <React.Fragment>
    <Comp1 />
    <Comp2 />
  </React.Fragment>
)
```

他们都是一样的。

片段是不呈现元素的包装组件。

# 检查选择器是否与 jQuery 中的内容匹配

我们可以检查 jQuery 对象的`length`属性，看看是否有任何东西具有给定的选择器。

例如，我们可以写:

```
if ($(selector).length ) {
  //...
}
```

我们检查是否有给定的`selector`元素存在。

如果什么都没有，则`length`为 0。否则，则`length`大于 0。

# 当用户完成输入而不是按键时运行 JavaScript 函数

我们可以通过使用来自下划线或 Lodash 的`debounce`方法来延迟按键处理程序。

例如，我们可以写:

```
$('#input-box').keyup(_.debounce(callback, 500));
```

我们用回调和以毫秒为单位的延迟来调用`debounce`函数。

# 遍历数组并移除项，而不中断 for 循环

要循环遍历一个数组并删除项目，我们可以使用`while`循环反向循环遍历一个项目。

例如，我们可以写:

```
let i = arr.length;
while (i--) {
  // ...
  if (...) { 
    arr.splice(i, 1);
  } 
}
```

我们使用`while`循环，索引从`length`下降到 0。

0 是 falsy，所以当它是 0 的时候它会破。

在主体中，我们调用`splice`来删除具有给定索引的项目。

向后循环确保重新索引不会影响迭代的下一项。

重新索引只影响从当前点到数组末尾的项目。

另一种以更短的方式做同样事情的方法是使用`filter`方法:

```
arr = arr.filter((el) => {
  return el.seconds > 0;
});
```

然后我们返回一个属性大于 0 的数组。

然后，我们将其指定为`arr`的新值。

# 倾听 JavaScript 中的可变变化

我们可以使用代理对象来监听变量的变化。

例如，我们可以写:

```
const targetObj = {};
const targetProxy = new Proxy(targetObj, {
  set(target, key, value) {
    console.log(key, value);
    target[key] = value;
    return true;
  }
});
```

我们创建了一个新的`Proxy`实例，它有`set`方法来倾听分配给`targetObj`的键和值。

然后，当我们为代理分配属性时:

```
targetProxy.foo = "bar";
```

将调用`set`方法。

# JavaScript 中的 trim()方法在 IE 中不起作用

字符串实例的`trim`方法在 IE 中不可用。

因此，我们必须自己添加。

例如，我们可以写:

```
if (typeof String.prototype.trim !== 'function') {
  String.prototype.trim = function() {
    return this.replace(/^\s+|\s+$/g, ''); 
  }
}
```

我们检查`trim`实例是否是一个函数，如果不是，我们将其设置为我们的函数。

![](img/39b3db6ab9b8e927d0ae7f6eb37960fc.png)

Photo by [TS Sergey](https://unsplash.com/@ttsergey?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 结论

我们可以用一个小的聚合器将`trim`方法添加到 IE 中。

快递中的最大请求数量可以更改。

我们可以使用反应片段作为包装元素，不呈现任何东西。

如果我们向后循环数组，那么重新索引不会影响循环，因为它只应用于从给定索引到末尾的项。

## 简单英语中的 JavaScript

你知道我们有四个出版物和一个 YouTube 频道吗？在 [**找到他们。io**](https://plainenglish.io/) 和 [**订阅解码，我们的 YouTube 频道**](https://www.youtube.com/channel/UCtipWUghju290NWcn8jhyAw) **！**