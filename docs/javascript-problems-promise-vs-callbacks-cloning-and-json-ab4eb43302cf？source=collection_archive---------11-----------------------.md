# JavaScript 问题——承诺与回调、克隆和 JSON

> 原文：<https://javascript.plainenglish.io/javascript-problems-promise-vs-callbacks-cloning-and-json-ab4eb43302cf?source=collection_archive---------11----------------------->

![](img/6c737b977b05b8544679faae8aeb4f77.png)

Photo by [G-R Mottez](https://unsplash.com/@grmot?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

像任何类型的应用程序一样，当我们编写 JavaScript 应用程序时，有一些困难的问题需要解决。

在本文中，我们将研究一些常见 JavaScript 问题的解决方案。

# 格式化 JavaScript 浮点

要格式化一个 JavaScript 浮点数，我们可以使用`toFixed`方法。

例如，我们可以写:

```
consr x = 5.574859569;
console.log(x.toFixed(2));
```

我们调用`x`上的`toFixed`来返回一个将它四舍五入到两位小数的字符串。

# 递增日期

我们可以用`getDate`的方法加上 1 来埋葬一个日期。

例如，我们可以写:

```
const tomorrow = new Date();
tomorrow.setDate(tomorrow.getDate() + 1);
```

要用 moment.js 做同样的事情，我们可以写:

```
const today = moment();
const tomorrow = moment(today).add(1, 'days');
```

两者都获得今天的日期，并在其上加上 1 天。

# 如何替换字符串中的所有点

要替换字符串中的所有点，我们可以使用带有正则表达式的`replace`方法。

例如，我们可以写:

```
str = str.replace(/\./g, ' ')
```

我们使用正则表达式得到所有的点。

然后我们用空格代替它们。

# 承诺和回访

承诺不是回调，尽管它们可以使用回调。

这是异步操作的未来结果。

我们可以通过回调得到结果来使用它们:

```
api()
.then((result) => {
  return api2();
})
.then((result2) => {
  return api3();
})
.then((result3) => {
  // ...
});
```

但是我们也可以使用 async 和 await:

```
async () => {
  const result = await api();
  const result2 = await api2();
  const result3 = await api3();
}
```

我们不一定要用承诺来回拨。

如果它们互不依赖，也可以同时运行:

```
Promise
  .all([api(), api2(), api3()])
  .then((result) => {
    // ...
  })
  .catch((error) => {
    // handle error
  });
```

我们用`Promise.all`同时运行它们。

此外，我们可以写:

```
async () => {
  try {
    const result = await Promise.all([api(), api2(), api3()]);
  }
  catch(e){
    // handle error
  }
}
```

# 并行调用异步/等待函数

我们可以使用`Promise.all`并行运行承诺。

正如我们在上面看到的，我们有带异步和等待的`Promise.all`。

我们可以破坏每个承诺的结果:

```
async () => {
  try {
    const [r1, r2, r3] = await Promise.all([api(), api2(), api3()]);
  }
  catch(e){
    // handle error
  }
}
```

# 自动执行功能的目的是什么？

我们可以使用自执行函数来防止变量从外部进入。

例如，如果我们有:

```
(() => { 
  let foo = 3; 
  console.log(foo); 
})();
```

那么`foo`只在函数内可用。

# 迭代 JavaScript 对象

有几种方法可以迭代 JavaScript 对象。

一种方法是使用 for-in 循环:

```
for (const key in obj) {
  console.log(key, obj[key]);
}
```

我们可以使用`hasOwnProperty`来避免记录继承的属性:

```
for (const key in obj) {
  if (obj.hasOwnProperty(key)) {
     console.log(key, obj[key]);
  }
}
```

我们也可以使用带有 for-of 循环的`Object.entries`方法:

```
for (const [key, value] of Object.entries(obj)) {
  console.log(key, value);
}
```

# 如何克隆对象数组？

克隆对象数组最简单的方法是使用`JSON.stringify`和`JSON.parse`。

例如，我们可以写:

```
const cloned = JSON.parse(JSON.stringify(arr))
```

如果所有条目都只有可序列化的内容，这将是可行的。

这意味着没有函数，无穷大等。

同样，我们可以使用`map`和`Object.assign`:

```
const cloned = arr.map(a => Object.assign({}, a));
```

我们调用`map`并用`Object.assign`返回每个条目的克隆。

我们可以用扩展语法来简化它:

```
const cloned = arr.map(a => ({...a}));
```

我们也可以直接传播:

```
const cloned = [...arr];
```

我们做一个`arr`的浅层克隆。

# 打印 JavaScript 对象的内容

要打印 JavaScript 对象的内容，我们可以使用`JSON.stringify`方法:

```
console.log(JSON.stringify(obj, undefined, 2));
```

我们通过传递要打印的对象作为第一个参数来使用`JSON.stringify`。

第二个参数是条目的可选映射函数。

我们不需要，所以传入`undefined`。

第三个参数是用于缩进的空格数，我们传入 2 是为了方便阅读。

![](img/4720b7965565d06229d5b048d7a3beca.png)

Photo by [Aswathy N](https://unsplash.com/@abnair?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 结论

我们可以用`JSON.stringify`来克隆和打印物体。

`toFixed`让我们格式化浮点数。

承诺不是回电。

## **简单英语的 JavaScript**

喜欢这篇文章吗？如果有，通过 [**订阅获取更多类似内容解码，我们的 YouTube 频道**](https://www.youtube.com/channel/UCtipWUghju290NWcn8jhyAw) **！**