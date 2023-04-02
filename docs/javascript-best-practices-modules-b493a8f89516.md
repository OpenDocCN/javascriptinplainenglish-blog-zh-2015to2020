# JavaScript 最佳实践—模块

> 原文：<https://javascript.plainenglish.io/javascript-best-practices-modules-b493a8f89516?source=collection_archive---------3----------------------->

![](img/0baf89f1fbea2d3f1927143fa8aa7269.png)

Photo by [Holger Link](https://unsplash.com/@photoholgic?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

JavaScript 是一种非常宽容的语言。编写可以运行但有错误的代码很容易。

在本文中，我们将研究从 JavaScript 模块导入和导出的一些最佳实践。

# 仅从一个位置的路径导入

如果我们从一个模块中导入多个成员，那么所有成员都应该在一个`import`语句中导入。

例如，不用编写以下代码来导入成员:

`module.js`

```
export const foo = 1;
export const bar = 2;
```

`index.js`

```
import { foo } from "./module";
import { bar } from "./module";
console.log(foo, bar);
```

在上面的代码中，我们用 2 行代码从同一个模块导入 2 个不同的成员。这样不好，因为我们在一个地方有很多重复的代码。

相反，我们应该把它们都放在同一个模块中，以节省空间并删除重复的代码。

例如，我们可以改为编写以下内容:

`module.js`

```
export const foo = 1;
export const bar = 2;
```

`index.js`

```
import { foo, bar } from "./module";
console.log(foo, bar);
```

在上面的代码中，我们在一行中导入了`index.js`中`module.js`的成员。正如我们所看到的，自从我们将两行合并成一行后，我们节省了大量的打字和重复工作。

# 不要导出可变绑定

我们不应该导出可变实体，因为我们不想在导出它们的模块中意外地改变它们。例如，如果我们有以下代码:

`module.js`

```
export let foo = 1;
foo = 3;
export const bar = 2;
```

`index.js`

```
import { foo, bar } from "./module";
console.log(foo, bar);
```

在上面的代码中，我们从`module.js`中导出了`foo`成员，然后在稍后更改了`foo`的值。

然后，当我们在`index.js`中导入`foo`时，我们看到为`foo`记录了值 3。

因此，每当我们通过重新分配来更改值时，新值将被导出。

因此，我们不应该导出已经用`let`定义的成员，因为如果我们从一个库而不是我们通常编辑的代码中导入，这些变化可能会被隐藏。

如果`foo`是用`const`定义的，那么在我们导出它之后，我们将不能把它重新赋值给一个新值。

# 在具有单个导出的模块中，首选默认导出，而不是命名导出

如果我们在一个模块中只有一个成员要导出，那么我们应该把它作为默认导出来导出。

这样做的好处是，在导入时，我们可以不使用`as`关键字来命名它。

例如，不要写以下内容:

`module.js`

```
export const bar = 2;
```

`index.js`

```
import { bar as foo } from "./module";
console.log(foo);
```

我们可以改为编写以下内容:

`module.js`

```
const bar = 2;
export default bar;
```

`index.js`

```
import foo from "./module";
console.log(foo);
```

在第二个例子中，我们将`bar`导出为默认导出。然后在`index.js`中，我们可以用名字`foo`或者任何名字代替名字`bar`导入，而不用使用`as`关键字。

如果我们有很多导入，这可以方便地避免与导入的名称冲突。

![](img/79feb0f8c7eb02de13dbc99510cca323.png)

Photo by [Daisy Chen](https://unsplash.com/@cqy12854?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 将所有 I `mport`置于非进口陈述之上

进口应该在非进口声明之上，因为这让每个人都清楚出口在哪里。

例如，不用编写下面的代码:

`bar.js`

```
export const bar = 1;
```

`module.js`

```
export const foo = 2;
```

`index.js`

```
import { foo } from "./module";
console.log(foo);import { bar } from "./bar";
console.log(bar);
```

在上面的代码中，我们从`index.js`的两个不同模块中导入了项目，这两个导入语句相距甚远。

我们用一个`console.log`调用将`foo`导入与`bar`导入分开。

分散的 Import 语句很难找到。

相反，我们应该这样写:

```
import { foo } from "./module";
import { bar } from "./bar";console.log(foo, bar);
```

将所有的`import`语句组合在我们代码文件的顶部。

# 结论

我们应该将导入语句组合在一起，这样他们就可以很容易地从代码的顶部找到导入。

如果我们只有一个成员要导出，那么我们可以默认导出它。

此外，我们应该导出可变绑定，因为它们在导出后可以更改。

# **简明英语团队的笔记**

你知道我们有四份出版物和一个 YouTube 频道吗？你可以在我们的主页[**plain English . io**](https://plainenglish.io/)找到所有这些内容——关注我们的出版物并 [**订阅我们的 YouTube 频道**](https://www.youtube.com/channel/UCtipWUghju290NWcn8jhyAw) **来表达你的爱吧！**