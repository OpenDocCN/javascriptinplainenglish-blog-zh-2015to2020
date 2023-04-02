# JavaScript 提示——等待所有承诺、高度、日期等等

> 原文：<https://javascript.plainenglish.io/javascript-tips-wait-for-all-promises-heights-dates-and-more-ad53d4028004?source=collection_archive---------17----------------------->

![](img/f5b1b955c96580cf5ef84aa30d24e4f5.png)

Photo by [Ignacio Brosa](https://unsplash.com/@ignaciobrosa?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

像任何类型的应用程序一样，当我们编写 JavaScript 应用程序时，有一些困难的问题需要解决。

在本文中，我们将研究一些常见 JavaScript 问题的解决方案。

# 跳过中的元素。地图()

我们可以跳过`map`中的一个元素，先用`filter`过滤掉我们不想要的条目。

例如，我们可以写:

```
const sources = images.filter((img) => {
  return img.src.split('.').pop() !== "json"
})
.map(img => img.src);
```

现在我们只绘制不以`'json'`结尾的项目。

# 用 JavaScript 在字符串中插入变量

我们可以使用模板字符串在字符串中插入变量。

例如，我们可以写:

```
const a = 5;
const b = 1;
console.log(`sum is ${a + b}.`);
```

我们将`a + b`的结果插入到最后一行的字符串中。

# JavaScript 中比较字符串的最佳方式？

要比较 JavaScript 中的两个字符串，我们可以使用`localeCompare`方法。

例如，我们可以写:

```
strA.localeCompare(strB)
```

这使我们能够以一种区域敏感的方式比较字符串。

# 等到所有的承诺完成，即使有些被拒绝

为了等待承诺完成，我们可以使用`Promise.allSettled`方法。

即使有些可能被拒绝，它也会完成。

例如，我们可以写:

```
Promise.allSettled([promise1, promise2])
.then(([result1, result2]) => {
  // ...
});
```

我们传递了一系列承诺。

然后我们调用`then`并在一个数组中得到所有的结果。

每个结果对象都有`status`和`reason`或`value`属性。

`status`是诺言的状态。如果实现了，`status`就会变成`'fulfilled'`。不然就`'rejected'`了。

`value`为解析值。

`reason`是拒绝的错误。

# HTMLCollection 元素的 For 循环

要循环遍历一个`HTMLCollection`对象，我们可以使用 for-of 循环来完成。

例如，我们可以写:

```
const els = document.getElementsByClassName("foo");
for (const item of els) {
  console.log(item.id);
}
```

我们得到所有具有类名`foo`的项目。

然后我们使用 for-of 循环遍历它们。

要将其转换为数组，我们可以使用`Array.from`或 spread 运算符:

```
const arr = Array.from(document.getElementsByClassName("foo"));
```

或者:

```
const arr = [...document.getElementsByClassName("foo")];
```

现在我们可以对它使用数组方法。

# 获取两个日期字符串之间的天数

我们可以通过解析日期得到两天之间的天数，然后得到它们的差值。

假设我们的日期是 MM/DD/YYYY 格式，我们可以写:

```
const parseDate = (str) => {
  const [month, day, year] = str.split('/');
  return new Date(year, month-1, day);
}

const dateDiff = (first, second) => {
  return Math.round((second - first)/(1000 * 60 * 60 * 24));
}
```

我们拆分`str`日期字符串。

然后我们将拆分后的条目作为参数传递给`Date`构造函数。

`year`是年份，`month`是月份，但是我们必须减去 1，因为 JavaScript 构造函数假设 0 是一月，1 是二月，等等。

`day`是一个月中的某一天。

在`dateDiff`中，我们用`first`减去`second`得到差值。

它们都是`Date`实例。

然后我们除以一天中的毫秒数。

然后我们可以写:

```
const diff = dateDiff(parseDate('2/1/2000'), parseDate('3/2/2000'));
```

去使用它。

# 在 JavaScript 对象文本中使用变量作为键

我们可以在 JavaScript 对象文本中使用变量作为键。

例如，我们可以写:

```
const prop = "foo",
const obj = { [prop]: 10 };
```

我们只需将`prop`传入括号中，然后键就是`foo`。

# 获取元素的渲染高度

我们可以使用`clientHeight`、`offsetHeight`和`scrollHeight`来获取元素的渲染高度。

例如，我们可以写:

```
const height = document.querySelector('div').clientHeight;
```

或者:

```
const height = document.querySelector('div').offsetHeight;
```

或者:

```
const height = document.querySelector('div').scrollHeight;
```

`clientHeight`包括高度和垂直填充。

`offsetHeight`包括高度、垂直填充和垂直边框。

`scrollHeight`包括所包含文档的高度、垂直填充和垂直边框。

![](img/1c4c2c826d71250f7139afaaaf2287f7.png)

Photo by [George Potter](https://unsplash.com/@georgepotter?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 结论

我们可以使用`filter`和`map`来映射数组的选定条目。

有几种方法可以得到元素的高度。

我们可以通过创建日期对象，然后减去两个日期，再除以一天中的毫秒数，得到两个日期之间的天数。

让我们等待所有的承诺完成，不管它们的状态如何。

## 简单英语的 JavaScript

你知道我们有四份出版物和一个 YouTube 频道吗？在[**plain English . io**](https://plainenglish.io/)和 [**找到它们订阅解码，我们的 YouTube 频道**](https://www.youtube.com/channel/UCtipWUghju290NWcn8jhyAw) **！**