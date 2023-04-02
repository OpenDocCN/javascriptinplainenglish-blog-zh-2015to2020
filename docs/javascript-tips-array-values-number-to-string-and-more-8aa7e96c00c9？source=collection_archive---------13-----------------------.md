# JavaScript 提示-数组值、字符串数字等

> 原文：<https://javascript.plainenglish.io/javascript-tips-array-values-number-to-string-and-more-8aa7e96c00c9?source=collection_archive---------13----------------------->

![](img/c900f2d7580a442fc6c52457304b53e7.png)

Photo by [Clem Onojeghuo](https://unsplash.com/@clemono2?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

像任何类型的应用程序一样，当我们编写 JavaScript 应用程序时，有一些困难的问题需要解决。

在本文中，我们将研究一些常见 JavaScript 问题的解决方案。

# 检查某个数组索引处是否存在值

我们可以检查某个数组索引处是否存在一个值。

为此，我们可以写道:

```
if (typeof array[index] !== 'undefined') {
  //...
}
```

或:

```
if (typeof array[index] !== 'undefined' && array[index] !== null) {
  //...
}
```

这样，我们通过检查它不是`undefined`或`null`来检查数组条目是否存在。

# 如何迭代 JSON 结构？

我们可以用 for-for 循环遍历一个数组。

例如，我们可以写:

```
const cars = [{ name: 'Suzuki' }, { name: 'BMW' }];

for (const car of cars) {
  console.log(car.name);
}
```

我们还可以写:

```
const cars = [{ name: 'Suzuki' }, { name: 'BMW' }];

for (const { name } of cars) {
  console.log(name);
}
```

我们通过直接获取`name`属性来重组`car`对象。

# 将元素放在数组的开头

要在数组的开头添加元素，可以使用`unshift`方法。

例如，我们可以写:

```
array.unshift('foo');
```

现在我们有`'foo'`作为`array`的第一个条目。

# 将数字转换成字符串的最佳方法

有很多种将数字转换成字符串的方法。

我们可以写道:

*   `String(n)`
*   `n.toString()`
*   `“”+n`
*   `n+””`

根据像这样的测试[http://jsben.ch/#/ghQYR](http://jsben.ch/#/ghQYR)，`toString`是火狐中最快的。

它可以在 0.1 秒内完成 100 万次。

然而在 Chrome 中`num + ''`比 http://jsben.ch/#/ghQYR 更快

# 将数组拆分为块

我们可以通过使用 for 循环将一个数组分成几个块:

```
const chunk = 10;
let tempArray = [];
for (let i = 0; i < array.length; i += chunk) {
  tempArray = array.slice(i, i + chunk);  
}
```

我们循环数组，然后通过用`chunk`递增`i`得到块。

然后在循环体中，我们调用`slice`来获取数组的块。

切片从索引`i`到`i + chunk — 1`。

# 将数组转换为对象

我们可以使用`Object.assign`将一个对象转换成一个数组。

例如，我们可以写:

```
Object.assign({}, ['a', 'b', 'c']);
```

我们将数组条目放入空对象中。

我们也可以使用扩散运算符:

```
{ ...['a', 'b', 'c'] }
```

# npx 和 npm 之间的差异

npx 允许我们直接从 NPM 存储库中运行一个包，而无需安装它。

它对我们不常使用的包很有用。

npx 总是运行包的最新版本。

npm 用于管理包，包括安装。

我们可以使用 npm 安装带有`npm install`的软件包。

或者我们可以用它来运行带有`npm run`的脚本。

# 多箭头功能是什么意思？

多个箭头函数意味着它是嵌套的。

例如，如果我们有:

```
handleChange = field => e => {
  e.preventDefault();
  // ...
}
```

然后我们有一个带参数`field`的函数，然后它返回一个带参数`e`的函数。

这也被称为一个咖喱功能。

我们返回一个函数，它一次应用一个参数，而不是一次应用所有参数。

这对于共享应用了一些参数的函数来说很方便。

如果我们写:

```
const add = x => y => x + y
```

然后我们可以写下一个论点:

```
const add2 = add(2)
```

那么我们可以这样写:

```
add2(3)
```

并得到 5 或:

```
add2(6)
```

比如得到 8。

# 基于属性过滤对象数组

我们可以使用`filter`方法根据属性过滤对象数组。

例如，我们可以写:

```
const homesIWant = homes.filter((el) => {
  return el.price <= 2000 &&
    el.sqft >= 500 &&
    el.numBeds >= 2 &&
    el.numBaths >= 2.5;
});
```

我们返回一个数组，它包含所有的`homes`条目，其中`price`小于或等于 2000、`sqft`大于或等于 500、`numBeds`大于或等于 2、`numBaths`大于 2.5。

![](img/334ddff5dd8619e638930d13d3220362.png)

Photo by [Artur Wayne](https://unsplash.com/@lctune?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 结论

我们可以用 for-of 循环迭代数组。

我们可以通过按块长度递增来获取数组的块。

让我们根据我们想要的任何条件来过滤数组。

npx 和 npm 是不同的。

## 简单英语的 JavaScript

喜欢这篇文章吗？如果有，通过 [**订阅解码，我们的 YouTube 频道**](https://www.youtube.com/channel/UCtipWUghju290NWcn8jhyAw) **获取更多类似内容！**