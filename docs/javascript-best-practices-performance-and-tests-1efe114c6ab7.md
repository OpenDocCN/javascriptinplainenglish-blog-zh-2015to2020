# JavaScript 最佳实践—性能和测试

> 原文：<https://javascript.plainenglish.io/javascript-best-practices-performance-and-tests-1efe114c6ab7?source=collection_archive---------7----------------------->

![](img/c3bf781f137bf061b25e4d93b48e9894.png)

Photo by [Tiago Gerken](https://unsplash.com/@tiagogerken?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

像任何类型的应用程序一样，JavaScript 应用程序也必须写得很好。

否则，我们以后会遇到各种问题。

在本文中，我们将研究在编写 JavaScript 代码时应该遵循的一些最佳实践。

# RAIL 模型

RAIL 模型是由谷歌创建的，让我们从用户的角度来考虑性能。

RAIL 表示响应、动画、空闲和负载。

响应是页面第一次响应用户输入的时间。

动画是当页面移动时。

空闲是页面空闲的时间。

加载是页面加载所需的时间。

我们应该让我们的应用程序尽快响应互动。

# 查找内存泄漏

我们应该用各种工具检查内存泄漏。

一个很好的工具是 Chrome 开发工具，它有性能来显示我们应用程序中内存的使用情况。

我们也可以用 Chrome 任务管理器检查一个标签的内存使用情况。

时间线记录，直观显示随时间推移的内存使用情况。

堆快照帮助我们识别分离的 DOM 树。

分配时间线记录让我们能够发现何时在我们的 JS 堆中分配了新的内存。

如果内存使用量随着时间的推移而增加，直到所有可用内存都被使用时，我们就知道内存泄漏了。

# 使用网络工作人员执行密集型任务

如果我们的应用程序有密集型任务，我们可以使用网络工作人员在后台运行它们。

它通过向工作人员发送输入来工作，然后当工作人员完成时，结果被发送回主应用程序。

他们在不同的线程上工作，所以他们不会阻塞主线程。

# 当心计算复杂性

我们应该意识到我们使用的算法的计算复杂性。

应该避免嵌套循环和其他缓慢的代码。

# 简化

如果我们能以更简单的方式写一些东西，那么我们就应该这样做。

# 避免递归调用

如果有一种迭代的方式来代替递归，那么我们应该使用迭代。

它更容易阅读，性能也很好。

# 正则表达式

我们可以使用正则表达式来搜索字符串中的模式。

所以我们可以用它们来提取信息。

例如，我们可以写:

```
str.replace(/\s+/g, '');
```

修剪空白。

我们还可以通过以下方式检查电子邮件地址格式:

```
function validateEmail(email) {
  const re = /\S+@\S+\.\S+/;
  return re.test(email);
}
```

# 使用搜索数组

我们可以通过`find`功能搜索数组。

例如，我们可以写:

```
let array = [1, 2, 3, 4, 5];
let found = array.find((element) => {
  return element > 2;
});
```

查找数组中的元素。

我们也可以通过检查数组中的某个东西来避免`switch`语句，而不是将每个数组条目用作`case`。

# 使用谷歌灯塔

灯塔是一个很好的工具来检查我们的浏览器应用程序的性能。

它检查性能、可访问性、最佳实践和搜索引擎优化。

灯塔运行在灯塔选项卡的 Chrome 开发工具中，它也可以作为节点包提供，因此我们可以自动运行检查。

# 谷歌页面速度

谷歌页面速度让我们了解网站的性能优化，并找到我们可以改进的地方。

它用于识别与 Google 的 web 性能最佳实践的一致性问题。

# JavaScript 导航计时 API

JavaScript 导航计时 API 让我们可以测量网站的性能。

我们可以通过书写来使用它:

```
const perfData = window.performance.timing; 
const pageLoadTime = perfData.loadEventEnd - perfData.navigationStart;
```

我们只需减去`loadEventEnd`和`navigationStarty`时间戳就可以得到加载时间。

我们还可以用以下公式计算请求响应时间:

```
const connectTime = perfData.responseEnd - perfData.requestStart;
```

和页面呈现时间:

```
const renderTime = perfData.domComplete - perfData.domLoading;
```

这描述的是 1 级 API，现在已经弃用了，但是 2 级即将到来。

我们可以使用这个直到 2 级可用。

# 测试我们的代码

我们应该用单元测试、集成测试和用户界面测试来测试我们的代码。

单元测试测试代码的一小部分，比如函数。

集成测试测试更大的代码部分，比如多个函数或类。

UI 测试从用户的角度进行测试，但是以自动化的方式进行。

点击和视觉检查完成。

![](img/9cfd7f0148b68fbf79ab043d959277be.png)

Photo by [Harley-Davidson](https://unsplash.com/@harleydavidson?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 结论

我们应该确保我们的应用程序性能良好。

此外，我们应该以各种方式测试我们的代码。

## 简单英语的 JavaScript

喜欢这篇文章吗？如果有，通过 [**订阅获取更多类似内容解码，我们的 YouTube 频道**](https://www.youtube.com/channel/UCtipWUghju290NWcn8jhyAw) **！**