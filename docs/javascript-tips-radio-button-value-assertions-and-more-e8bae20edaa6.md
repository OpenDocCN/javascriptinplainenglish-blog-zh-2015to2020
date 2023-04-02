# JavaScript 提示—单选按钮值、断言等等

> 原文：<https://javascript.plainenglish.io/javascript-tips-radio-button-value-assertions-and-more-e8bae20edaa6?source=collection_archive---------8----------------------->

![](img/abf811af1bef7e1136c8817c568db2d8.png)

Photo by [Anthony Roberts](https://unsplash.com/@arcreates?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

像任何类型的应用程序一样，当我们编写 JavaScript 应用程序时，有一些困难的问题需要解决。

在本文中，我们将研究一些常见 JavaScript 问题的解决方案。

# 获取所选单选按钮的值

为了获得选中单选按钮的值，我们可以使用`checked`和`value`属性。

例如，如果我们有下面的 HTML:

```
<input type="radio" name="gender" value="female" checked="checked">Female</input>
<input type="radio" name="gender" value="male">Male</input>
```

我们可以通过`getElementsByName`得到单选按钮的名称:

```
const radios = document.getElementsByName('gender');for (const radio of radios) {
  if (radio.checked) {
    console.log(radio.value);
  }
}
```

我们循环遍历用 for-of 循环检索的单选按钮。

然后我们使用`checked`属性来查看它是否被选中。

然后我们用`value`属性得到`value`属性的值。

# 用 JavaScript 创建我们自己的断言函数

客户端 JavaScript 中没有`assert`函数。

在 Node 中，有一个`assert`模块。

要在没有库的情况下创建自己的`assert`函数，我们可以创建自己的函数:

```
const assert = (condition, message) => {
  if (!condition) {
    throw new Error(message || "Assertion failed");
  }
}
```

该功能仅检查`condition`是否正确。

如果不是，那么如果我们愿意，我们会用我们自己的消息抛出一个错误。

# 在 JavaScript 中向现有数组添加新值

我们可以调用所有的`push`方法来给一个现有的 JavaScript 数组添加一个新值。

例如，我们可以写:

```
const arr = [];
arr.push('foo');
arr.push('bar');
```

我们可以使用`unshift`将值添加到开头:

```
const arr = [];
arr.unshift('foo');
```

我们也可以直接通过索引来设置值，因为 JavaScript 数组是动态调整大小的:

```
const array = [];
array[index] = "foo";
```

# 如何用 JavaScript 检查 2 个数组是否相等

我们可以用 JavaScript 检查两个数组是否相等，方法是遍历所有条目，看它们是否都相等。

例如，我们可以写:

```
const arraysEqual = (a, b) => {
  if (a === b) { return true; }
  if (a === null || b === null) { return false; }
  if (a.length !== b.length) { return false; } for (let i = 0; i < a.length; i++) {
    if (a[i] !== b[i]) {
      return false;
    }
  }
  return true;
}
```

这适用于只有原始值的数组，因为我们不能用`===`或`!==`直接比较对象内容。

我们检查`a`是否引用了与`b`相同的对象。

如果是这样，那么我们返回`true`。

如果其中一个或多个是`null`，我们返回`false`。

如果它们的长度不相等，我们返回`false`。

如果我们通过了所有的检查，我们就比较里面的东西。

在循环体中，如果任何条目不相等，那么我们返回`false`。

如果它在循环中没有返回`false`，那么所有的条目都相等，所以我们返回`true`。

如果我们需要检查对象内容，那么我们需要递归地遍历对象的所有属性并检查它们的内容。

# 将字符串转换成 JavaScript 函数调用

我们可以将名称字符串传递给带有我们想要调用的函数的对象来调用该函数。

例如，我们可以写:

```
const fn = obj[functionName];
if (typeof fn === 'function') {
  fn(id);
}
```

我们将字符串`functionName`传递到`obj`后面的括号中，并将返回值赋给`fn`。

然后我们用`typeof`来检查它是不是一个函数。

如果这是一个函数，我们称之为。

# 基于对象属性删除数组元素

要删除基于对象属性的数组元素，我们可以使用`filter`方法。

例如，我们可以写:

```
arr = arr.filter(obj => obj.name !== 'james');
```

上面的代码返回一个数组，其中包含了所有属性不为`name`的条目。

因为它返回一个新数组，我们必须将它赋给`arr`来替换它的值。

![](img/18dc29fa3f9d399442017b5ca43d6ee6.png)

Photo by [Abe B. Ryokan](https://unsplash.com/@abe_b_ryokan?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 结论

我们可以使用`value`属性来获取复选框的`value`属性的值。

要创建一个断言函数，我们可以在条件不满足时抛出一个错误。

有一些方法可以向数组中添加项。

我们可以比较数组中的每一项，看它们是否相等。

## **用简单英语写的 JavaScript**

喜欢这篇文章吗？如果有，通过 [**订阅解码，我们的 YouTube 频道**](https://www.youtube.com/channel/UCtipWUghju290NWcn8jhyAw) **获取更多类似内容！**