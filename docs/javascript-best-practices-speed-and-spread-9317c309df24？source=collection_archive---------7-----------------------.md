# JavaScript 最佳实践—速度和传播

> 原文：<https://javascript.plainenglish.io/javascript-best-practices-speed-and-spread-9317c309df24?source=collection_archive---------7----------------------->

![](img/792474922c587cc764c8d23a4a187f67.png)

Photo by [Nicola Nuttall](https://unsplash.com/@nicnut?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

像任何类型的应用程序一样，JavaScript 应用程序也必须写得很好。

否则，我们以后会遇到各种各样的问题。

在本文中，我们将研究一些在编写 JavaScript 代码时应该遵循的最佳实践。

# 扩展运算符和数组

扩展操作符可以做很多事情。

我们可以用它们来合并数组。

例如，我们可以写:

```
const a = [1, 2, 3];
const b = [4, 5, 6];
const c = [...a, ...b];
```

`c`拥有`a`和`b`的所有条目。

我们也可以将一个数组扩展到一个函数中作为它的参数。

例如，我们可以写:

```
const sum = (x, y, z) => x + y + z;
const nums = [1, 2, 3];
const total = sum(...nums);
```

我们使用 spread 操作符将`nums`条目展开作为参数。

# 推迟加载脚本

我们可以推迟加载带有`defer`属性的脚本。

这样，它们将按照列出的顺序异步加载。

例如，我们可以写:

```
<script defer src='script.js'></script>
```

我们也可以将脚本标签放在文件的末尾，这样它们就可以最后加载。

# 通过使用缩小来节省字节

在我们将我们的应用程序部署到生产环境之前，我们应该缩小它们，这样我们就可以减小包的大小。

如果我们使用框架，这应该由那些框架的 CLI 程序自动完成。

如果我们没有，我们可以使用 package 或 Webpack 为我们做修改。

# 避免不必要的循环

我们应该避免嵌套循环，以保持代码的线性。

此外，我们不希望我们的循环通过许多对象，因为它会很慢。

# 优先选择 Map 而不是 for 循环

如果我们将数组条目从一个东西映射到另一个东西，我们可以使用数组实例的`map`方法。

这样，原始数组不会改变。

范围是隔离的，因为映射是通过回调完成的。

它也比循环短。

我们可以用其他操作，比如循环和其他数组方法来组合。

例如，我们可以写:

```
let strings = ['1', '2', '3', '4', '5'];
const total = strings
  .map(str => +str)
  .filter(num => num % 2 === 1)
  .reduce((sum, number) => sum + number);
```

用`map`将`strings`条目转换成数字。

然后我们调用`filter`从映射的数组中返回奇数。

最后，我们调用`reduce`来获得奇数的总数。

有了循环，这个时间就长多了。

我们要写:

```
let strings = ['1', '2', '3', '4', '5'];
let nums = [];
let oddNums = [];
let total = 0;for (const s of strings) {
  nums.push(+s);
}for (const n of nums) {
  if (n % 2 === 1) {
    oddNums.push(n);
  }
}for (const o of oddNums) {
  total += o;
}
```

我们有 3 个循环来分别进行映射、过滤和添加。

使用数组实例方法要短得多。

# 使用适当的事件处理程序

我们可以通过使用事件委托来使用事件处理程序，以检查哪个 DOM 元素触发了事件。

同样，我们应该在使用完`removeEventListener`后移除它们。

要进行事件委托，我们可以编写:

```
document.addEventListener('click', (e) => {
  if (e.srcElement.tagName.toLowerCase() === 'div') {
    //...
  }
})
```

检查 div 的点击量。

要调用`removeEventListener`，我们可以写:

```
target.removeEventListener('click', onClick);
```

其中`onClick`是一个事件监听器函数。

# 移除未使用的 JavaScript

如果我们有未使用的代码，那么我们应该删除它们。

这样，就不会有人对它们感到困惑了。

# 避免使用太多内存

每当我们的代码使用更多的内存时，垃圾收集器将在稍后清理未使用的资源时运行。

如果垃圾收集经常运行，那么我们的应用程序运行缓慢。

所以我们首先应该避免预留内存。

# 浏览器中的缓存

我们可以和服务人员一起保管东西。

我们可以用缓存 API 来缓存东西。

要添加缓存，我们可以写:

```
const CACHE_VERSION = 1;
const CURRENT_CACHES = {
  font: `font-cache-v${CACHE_VERSION}`;
};self.addEventListener('activate', (event) => {
  let expectedCacheNamesSet = new Set(Object.values(CURRENT_CACHES));
  event.waitUntil(
    caches.keys().then((cacheNames) => {
      return Promise.all(
        cacheNames.map((cacheName) => {
          if (!expectedCacheNamesSet.has(cacheName)) {
            return caches.delete(cacheName);
          }
        })
      );
    })
  );
});
```

我们通过遍历键并检查没有在`CURRENT_CACHES`中命名的条目来从缓存中删除数据。

![](img/a919e0708acd5260bca3b793fa950aa5.png)

Photo by [Geran de Klerk](https://unsplash.com/@gerandeklerk?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 结论

我们可以使用 service works 和 spread operator 等现代功能来帮助我们改进应用程序。

此外，我们应该记住一些要寻找的东西的性能。

## 简单英语的 JavaScript

喜欢这篇文章吗？如果有，通过 [**订阅获取更多类似内容解码，我们的 YouTube 频道**](https://www.youtube.com/channel/UCtipWUghju290NWcn8jhyAw) **！**