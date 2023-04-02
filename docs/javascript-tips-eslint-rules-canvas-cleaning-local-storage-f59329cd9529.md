# JavaScript 技巧——ESLint 规则、画布、清理本地存储等等

> 原文：<https://javascript.plainenglish.io/javascript-tips-eslint-rules-canvas-cleaning-local-storage-f59329cd9529?source=collection_archive---------11----------------------->

![](img/8fcf4474610ee46bb1cbac0c62f38e62.png)

Photo by [The Honest Company](https://unsplash.com/@honest?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

像任何类型的应用程序一样，当我们编写 JavaScript 应用程序时，有一些困难的问题需要解决。

在本文中，我们将研究一些常见 JavaScript 问题的解决方案。

# 将 JavaScript 数组转换成逗号分隔的列表

我们可以使用数组实例的`join`方法用逗号将字符串连接在一起。

例如，我们可以写:

```
const arr = ["foo", "bar", "baz"];
console.log(arr.join(", "));
```

# JavaScript 对象中的构造函数

JavaScript 对象可以从构造函数中创建。

去创造它。我们写道:

```
function Box(color) {
  this.color = color;
}Box.prototype.getColor = function(){
  return this.color;
};
```

我们用`color`实例变量创建了`Box`构造函数。

此外，我们将`getColor`方法与`prototype`属性连接起来，以返回`color`。

然后我们可以使用`new`操作符来创建一个`Box`实例:

```
const box = new Box("brown");
console.log(box.getColor());
```

等价地，我们可以使用类语法:

```
class Box {
  constructor(color) {
    this.color = color;
  }

  getColor(){
    return this.color;
  }
}
```

我们把所有东西都放在`Box`类中，而不是使用原型。

然而，它们是一样的。

# 禁用特定文件的 ESLint 规则

我们可以禁用带有注释的特定文件的 ESLint 规则。

例如，我们可以写:

```
/* eslint no-use-before-define: 0 */
```

关闭一个`no-use-before-define`规则。

或者我们可以写:

```
/* eslint-disable no-use-before-define */
```

做同样的事情。

我们可以写:

```
/* eslint no-use-before-define: 2 */
```

打开它。

我们还可以通过将文件路径放在`.eslintignore`中来告诉 ESLint 忽略特定的文件:

```
build/*.js
node_modules/**/*.js
```

# 添加 JavaScript / jQuery DOM 更改监听器

为了监听 DOM 的变化，我们可以监听`DOMSubtreeModified`事件。

例如，我们可以写:

```
$("#div").bind("DOMSubtreeModified", () => {
  console.log("tree changed");
});
```

我们使用 jQuery 的`bind`方法来监听事件。

# 提前退出函数

要提前结束一个函数，我们可以使用`return`关键字。

它可以放在任何地方以提前结束一个函数。

例如，我们可以写:

```
const myfunction = () => {
  if (shouldStop) {
    return;
  }
}
```

# 当标签或窗口关闭时，从本地存储中移除项目

当标签或窗口关闭时，要从本地存储中删除项目，我们可以监听`beforeunload`事件。

在监听器函数中，我们可以调用`removeItem`来移除该项。

例如，我们可以写:

```
window.onbeforeunload = () => {
  localStorage.removeItem(key);
  return '';
};
```

侦听器应该返回一个字符串。

# 在 React 组件中选定的<select>选项上选择“选定”</select>

我们可以设置`select`元素的`value`属性来设置选中的选项。

例如，我们可以写:

```
<select value={chosenItem}>
  <option value="A">apple</option>
  <option value="B">banana</option>
  <option value="O">orange</option>
</select>
```

我们用类组件中的`setState`设置`chosenItem`状态。

或者我们可以使用`useState`方法返回的状态改变函数来设置它。

# 调整 HTML5 画布大小以适合窗口

要调整 HTML5 画布的大小以适应窗口，我们可以用 JavaScript 更改画布的宽度和高度。

例如，我们可以写:

```
const resizeCanvas = () => {
  const canvas = document.getElementById('canvas'),
  const ctx = canvas.getContext('2d');
  ctx.canvas.width  = window.innerWidth;
  ctx.canvas.height = window.innerHeight;
}window.addEventListener('resize', resizeCanvas, false);
```

`innerWidth`将宽度设置为窗口宽度，而`innerHeight`将高度设置为窗口高度。

我们观察`resize`事件来设定维度。

# 向 JavaScript 日期对象添加小时

我们可以用`getTime`方法向 JavaScript date 对象添加小时来获取时间戳。

然后我们将毫秒加到时间戳上，以增加我们想要的小时数。

例如，我们可以写:

```
date.setTime(date.getTime() + (hours * 60 * 60 * 1000));
```

我们使用`setTime`来设置新日期的时间戳。

# 检查数组是否为空或不存在

要检查数组是否为空或不存在，我们可以使用`Array.isArray`方法并检查`length`属性。

例如，我们可以写:

```
if (!Array.isArray(array) || !array.length) {
  // ...
}
```

我们检查`array`是否是一个带有`Array.isArray`的数组。

然后我们用`!array.length`检查长度是否为 0。

如果两者都是`true`，那么我们知道它不是一个数组

如果只有`!array.length`是`true`，那么我们知道`array`是一个空数组。

![](img/fe39f9264b95f4231fdcdf0a52fa06b4.png)

Photo by [Johannes Plenio](https://unsplash.com/@jplenio?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 结论

我们可以用注释或`.eslintignore`禁用 ESLint 规则。

当 DOM 结构改变时，发出`DOMSubtreeModified`。

JavaScript 对象可以从构造函数或类中创建。

## 简单英语的 JavaScript

喜欢这篇文章吗？如果有，通过 [**订阅我们的 YouTube 频道**](https://www.youtube.com/channel/UCtipWUghju290NWcn8jhyAw) **获取更多类似内容！**