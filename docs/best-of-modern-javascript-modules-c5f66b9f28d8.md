# 现代 JavaScript 的精华——模块

> 原文：<https://javascript.plainenglish.io/best-of-modern-javascript-modules-c5f66b9f28d8?source=collection_archive---------19----------------------->

![](img/b1b0a0d62858ba5d5499d466e8e45a54.png)

Photo by [Michael Dziedzic](https://unsplash.com/@lazycreekimages?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

自 2015 年以来，JavaScript 有了很大的改进。

现在使用它比以往任何时候都愉快。

在本文中，我们将了解如何使用 JavaScript 模块。

# 导出函数表达式

导出函数表达式，我们可以在我们的`export`语句周围加上括号。

例如，我们可以写:

```
export default (function () {});
export default (class {});
```

类是 JavaScript 中的函数，所以同样的规则适用。

# 直接导出默认值

我们可以直接默认导出值。

例如，我们可以写:

```
export default 'foo';
```

要导出字符串`'foo'`。

同样，我们可以写道:

```
const foo = function() {};export { foo as default };
```

我们创建了一个函数`foo`并使用`as default`关键字导出，以进行默认导出。

我们需要这个语法，这样我们就可以将变量声明转换成默认的导出。

# 进出口必须处于最高水平

进出口必须处于最高水平。

例如，我们可以写:

```
import 'foo';
```

但是我们不能写:

```
if (true) {
  import 'foo';
}
```

或者

```
{
  import 'foo';
}
```

他们都会提高语法分析器。

# 进口被吊起

进口产品不是吊装的，所以我们可以在定义之前使用它们。

例如，我们可以写:

```
console.log(add(1, 2));import { add } from "./math";
```

并且`add`的返回值将被记录。

# 进出口

导入是只读的。

这使得模块系统允许循环依赖。

此外，我们可以将代码分成多个模块，只要我们不改变它们的价值，它仍然可以工作。

# 循环依赖

循环依赖是 2 个模块相互导入成员的地方。

它们应该避免，因为它使两个模块紧密耦合。

然而，我们可能无法完全消除它们，所以我们不得不忍受它们。

我们可以通过如下方式在 ES6 中添加循环依赖

例如，我们可以写:

`math.js`

```
import { foo } from "./index";export const bar = () => {
  foo();
};
```

`index.js`

```
import { bar } from "./math";export const foo = () => {
  bar();
};
```

我们从`math.js`中的`index.js`导入`foo`，并使用导入的功能。

同样，我们从`math.js`导入`bar`，并将其称为。

# 其他导入样式

除了命名和默认导出之外。

我们可以使用`import`只加载模块，不导入任何东西。

例如，我们可以写:

```
import 'lib';
```

同样，要重命名导入，我们可以使用`as`关键字。

例如，我们可以写:

```
import { name as foo, bar } from "baz";
```

`as`关键字用于重命名已命名的导出`name`。

我们也可以使用它来重命名默认导出。

例如，我们可以写:

```
import { default as foo } from "baz";
```

我们也可以通过书写来使用`as`关键词:

```
import * as baz from "baz";
```

导入整个模块并命名为`baz`。

默认导入可以与命名导入混合使用。

例如，我们可以写:

```
import foo, { bar, qux } from "baz";
```

![](img/41e00759404f98c8d300d97162a37c35.png)

Photo by [Justin Veenema](https://unsplash.com/@justinveenema?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 结论

我们可以用各种方式导出和导入模块成员，

循环依赖也适用于 ES6 的模块系统。

喜欢这篇文章吗？如果是这样，通过 [**订阅解码获得更多类似内容，我们的 YouTube 频道**](https://www.youtube.com/channel/UCtipWUghju290NWcn8jhyAw) **！**