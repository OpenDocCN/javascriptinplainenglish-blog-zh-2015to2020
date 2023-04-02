# JavaScript 问题——对象属性、多个 Ajax 调用等等

> 原文：<https://javascript.plainenglish.io/javascript-problems-object-properties-multiple-ajax-calls-and-more-aac85a3cd7f4?source=collection_archive---------6----------------------->

![](img/fa57d5c546173e7eba54e0a9a3b7541f.png)

Photo by [Olav Ahrens Røtne](https://unsplash.com/@olav_ahrens?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

像任何类型的应用程序一样，当我们编写 JavaScript 应用程序时，有一些困难的问题需要解决。

在本文中，我们将研究一些常见 JavaScript 问题的解决方案。

# 检查对象属性是否存在

有几种方法可以检查对象属性是否存在。

我们可以使用`in`操作符，它检查对象本身及其原型。

例如，我们可以写:

```
const myProp = 'prop';
if (myProp in obj){
  //...
}
```

我们也可以使用`hasOwnProperty`:

```
const myProp = 'prop';
if (obj.hasOwnProperty(myProp)){
  //...
}
```

`hasOwnProperty`只检查对象本身内部的属性，不检查它的原型。

# 等到所有 JavaScript ajax 调用都完成后，再运行一些代码

jQuery 有一个`when`方法，可以进行 ajax 调用，并在完成所有调用后通过回调调用上面的`done`来运行代码。

例如，我们可以写:

```
$.when(ajax1(), ajax2(), ajax3())
  .done((a1, a2, a3) => {
    //...
  });
```

其中`ajax1`、`ajax2`和`ajax3`是返回`$.ajax`调用的函数:

```
const ajax1 = () => {
  return $.ajax({
    url: "url",
    dataType: "json",
    data,            
    ...
  });
}
```

`ajax2`和`ajax3`相同。

# 从日期获取月份名称

从`Date`实例中获取货币名称的最方便的方法是使用`toLocaleString`方法。

例如，我们可以写:

```
const date = new Date(2020, 10, 10);
const month = date.toLocaleString('default', { month: 'long' });
```

我们将`month`设置为`'long'`以返回完整的月份名称。

`'default'`是区域设置。

或者，有一个`Intl.DateTimeFormat`构造函数让我们做同样的事情:

```
const formatter = new Intl.DateTimeFormat('en', { month: 'long' });
const month = formatter.format(new Date(2020, 10, 10));
```

# 检查用户是否已经滚动到底部

我们可以使用`scrollTop`方法检查用户是否滚动到底部。

例如，我们可以写:

```
$(window).scroll(() => {
   if($(window).scrollTop() + $(window).height() === $(document).height()) {
     console.log('scrolled to bottom');
   }
});
```

我们通过对带有`height`的窗口使用`scrollTop`方法来检查 jQuery，以获得总的滚动高度。

我们对照文档的高度来检查用户是否滚动到了底部。

# 确定两个 JavaScript 对象是否相等

我们可以用 Lodash `isEqual`方法检查两个对象是否相等。

例如，我们可以写:

```
_.isEqual(object, other);
```

`object`和`other`是我们正在比较的对象。

# 在 JavaScript 中将字符串转换为日期

通过将字符串传递给`Date`构造函数，将字符串转换为`Date`。

例如，我们可以写:

```
const date = new Date('2020-01-01');
```

我们还可以将字母`Z`添加到字符串中，将时区设置为 UTC。

例如，我们可以写:

```
const date = new Date('2020-01-01T10:10:20Z')
```

# 枚举 JavaScript 对象的属性

有几种方法可以枚举 JavaScript 对象的属性。

我们可以使用 for-in 循环。例如，我们可以写:

```
for (const prop in obj) {
   //...
}
```

这将遍历`obj`的所有属性，包括它的原型。

我们还可以将 for-in 与`hasOwnProperty`结合起来，只遍历对象本身的属性。

例如，我们可以写:

```
for (const prop in obj) {
  if (obj.hasOwnProperty(prop)) {
    console.log(name);
  }
}
```

此外，我们可以写:

```
for (const prop of Object.keys(obj)) {
   //...
}
```

`Object.keys`返回一个我们用 for-of 循环遍历的键数组。

它只返回非继承的键。

# 替换字符串中某个字符的所有实例

我们可以替换字符串中的所有字符。

例如，我们可以写:

```
str.replace(/foo/g, "bar")
```

用字符串中的`'bar'`替换`'foo'`的所有实例。

同样，我们可以使用`RegExp`构造函数来创建正则表达式。

例如，我们可以写:

```
const pattern = "foo",
const re = new RegExp(pattern, "g");
```

![](img/a4dd7e87f31a375ea11105397065fc18.png)

Photo by [Robert Katzki](https://unsplash.com/@ro_ka?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 结论

使用`Date`构造函数将字符串转换成日期很容易。

我们可以用许多方式列举 JavaScript 属性。

还有一种方法可以检查页面的滚动位置。

为了从`Date`实例中获取月份名称，我们必须将`Date`对象格式化为一个字符串。

## 简单英语的 JavaScript

你知道我们有四份出版物和一个 YouTube 频道吗？在[**plain English . io**](https://plainenglish.io/)和 [**找到它们订阅我们的 YouTube 频道**](https://www.youtube.com/channel/UCtipWUghju290NWcn8jhyAw) **！**