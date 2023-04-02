# JavaScript 最佳实践—数组和异常

> 原文：<https://javascript.plainenglish.io/javascript-best-practices-arrays-and-exceptions-87cb2fd104d5?source=collection_archive---------5----------------------->

![](img/b3bf0e2ad9f966d18f0f121a65367102.png)

Photo by [Luca Bravo](https://unsplash.com/@lucabravo?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

像任何类型的应用程序一样，JavaScript 应用程序也必须写得很好。

否则，我们以后会遇到各种各样的问题。

在本文中，我们将研究一些在编写 JavaScript 代码时应该遵循的最佳实践。

# 避免对数组使用 for-in 循环

我们应该避免对数组使用 for-in 循环。

例如，不写:

```
let sum = 0;  
for (let i in arrayNumbers) {  
  sum += arrayNumbers[i];  
}
```

我们可以使用 for 循环或 for-of 循环:

```
let sum = 0;  
for (let a of arrayNumbers) {  
  sum += a;  
}
```

或者:

```
let sum = 0;
for (let i = 0, len = arrayNumbers.length; i < len; i++) {  
  sum += arrayNumbers[i];  
}
```

for-in 循环用于遍历常规对象，而不是数组。

# 向 setTimeout()和 setInterval()传递函数，而不是字符串

我们应该总是将功能传递给`setTimeout`和`setInterval`。

例如，不要写:

```
setInterval('doSomething()', 1000);  
setTimeout('doSomething()', 5000);
```

我们写道:

```
setInterval(doSomething, 1000);  
setTimeout(doSomething, 5000);
```

# 使用 switch/case 语句代替一系列 if/else 语句

`switch`和`case`比一系列的`if`和`else`要短。

比如，我们可以写；

```
switch (val) {
  case 1:
    //...
    break;
  case 2:
    //...
    break;
  case 3:
    //...
    break;
  case 4:
    //...
    break;
  default:
    //...
}
```

这比以下组织得更好:

```
if (val === 1) {
  //...
}
else if (val === 2) {
  //...
}
else if (val === 3) {
  //...
}
else if (val === 4) {
  //...
}
else {
  //...
}
```

如果我们有超过 10 个案例，我们应该避免一系列的`if`和`else`。

# 使用带有数值范围的 switch/case 语句

我们可以将`switch`和`case`用于数值范围。

为此，我们可以写:

```
switch (true) {
  case isNaN(age):
    category = "not an age";
    break;
  case (age >= 50):
    category = "old";
    break;
  case (age <= 20):
    category = "kid";
    break;
  default:
    category = "young";
    break;
};
```

我们传入的不是变量`switch`，而是`true`。

另外，`case`有表达式而不是值。

# 创建一个原型是给定对象的对象

我们可以用`Object.create`方法创建一个对象，其原型是 gin 对象。

例如，我们可以写:

```
const obj = Object.create(proto);
```

其中`proto`是`obj`的原型对象。

# 一个 HTML 转义函数

我们可以用一些正则表达式来转义 HTML。

例如，我们可以写:

```
function escapeHTML(text) {
  const replacements = {
    "<": "&lt;",
    ">": "&gt;",
    "&": "&amp;",
    "\"": "&quot"
  };
  return text.replace(/[<>&"]/g, (character) => {
    return replacements[character];
  });
}
```

我们用正则表达式搜索括号、与号和引号，然后从`replacements`对象中获取替换字符。

# 避免在循环中使用 try-catch-finally

每次运行`catch`子句时，try-catch-finally 都会在运行时在当前范围内创建一个新变量。

所以与其写:

```
let arr = ['foo', 'bar'], i;
for (i = 0, len = arr.length; i < len; i++) {
  try {
    // ...
  } catch (e) {
    // ...
  }
}
```

我们写道:

```
let arr = ['foo', 'bar'],  i;
try {
  for (i = 0, len = arr.length; i < len; i++) {
    // ...
  }
} catch (e) {
  // ...
}
```

现在第二个例子中的`catch`子句只能运行一次。

所以那里的变量只能创建一次。

![](img/0fc699595ee54a1b76e1ea87937b7032.png)

Photo by [Pietro De Grandi](https://unsplash.com/@peter_mc_greats?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 结论

我们不应该在数组中使用 for-in 循环。

同样，我们可以用原型和`Object.create`创建对象。

关于数组，还有很多事情需要注意。

## 简单英语的 JavaScript

喜欢这篇文章吗？如果是这样，通过 [**订阅解码获得更多类似内容，我们的 YouTube 频道**](https://www.youtube.com/channel/UCtipWUghju290NWcn8jhyAw) **！**