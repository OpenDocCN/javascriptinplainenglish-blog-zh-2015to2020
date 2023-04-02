# JavaScript ES6 模块举例说明

> 原文：<https://javascript.plainenglish.io/javascript-es6-modules-explained-with-examples-3a85e693d56e?source=collection_archive---------9----------------------->

## 通过实例了解 JavaScript 中的 ES6 模块

![](img/3a2cf2c021ae0dfaa9228cd18a28b738.png)

Photo by [Arnold Francisca](https://unsplash.com/@clark_fransa?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 介绍

如今，我们无法想象没有 JavaScript 的 web。有些网站几乎完全是用这种神奇的语言建立的。为了使 JavaScript 更加模块化、简洁和可维护，ES6 引入了一种在 JavaScript 文件间轻松共享代码的方法。这涉及到使用模块来导出一个文件的某些部分，以便在一个或多个其他文件中使用，并在您需要的地方导入您需要的部分。

在本文中，我们将通过一些实际例子来学习 JavaScript 中的 ES6 模块。让我们开始吧。

# 什么是 ES6 模块？

模块让我们将我们的代码库分成多个文件以获得更多的可维护性，这让我们避免将所有的代码放在一个大文件中。

为了利用这个功能，您需要在 HTML 文档中创建一个类型为`module`的脚本。

这里有一个例子:

```
**<script type="module" src="filename.js"></script>**
```

使用这种`module`类型的脚本现在可以使用`import`和`export`特性。

# 使用导出来共享代码块

比方说，你在一个名为`index.js`的文件中有一段代码，你想在几个不同的文件中使用这段代码。你可以通过**导出**那个代码块，然后**导入**到其他文件来实现。

看看下面的例子，我们导出了函数`multiply`:

```
**export** const multiply = (x, y) => {
  return x * y;
}
```

您也可以通过下面的示例实现同样的目的:

```
const multiply = (x, y) => {
  return x * y;
}

**export { multiply }**;
```

当您导出变量或函数时，您可以将其导入到另一个文件中并使用它，而不必重写代码。通过对每个要导出的内容重复第一个示例，或者将它们全部放在第二个示例的 export 语句中，可以导出多个内容，如下所示:

```
**export { multiply, add }**;
```

# 使用导入重用 JavaScript 代码

假设您想要将乘法函数从文件`index.js`导入到另一个名为`app.js`的文件中，以便使用它。您可以通过在文件`app.js`中编写以下代码来实现这一点:

```
**import { multiply } from './index.js';**
```

现在您可以使用并调用文件`app.js`中的函数`multiply`，而无需为其编写代码。

假设您有一个文件，并希望将其所有内容导入到当前文件中。这可以用`**import * as**`语法来完成。

下面是一个例子，其中文件`index.js`的所有内容被导入到同一目录下的一个文件中:

```
**import * as myMathModule from "./index.js";**
```

上面的语句将创建一个名为`myMathModule`的对象。这只是一个变量名，你可以给它起任何名字。所以现在您可以像处理任何其他对象属性一样访问文件内部的函数。

下面是如何通过访问`myMathModule`来使用文件中的`multiply`函数:

```
**myMathModule.add(2,3);**
```

# 导出默认值

仅当从文件中导出一个值或一个函数时，才可以使用导出默认值。

这里有一个例子:

```
// named function
**export default** function multiply(x, y) {
  return x * y;
}

// anonymous function
**export default** function(x, y) {
  return x * y;
}
```

# 导入默认导出

只需使用 import 语法就可以导入默认导出。

下面是一个从文件`index.js`导入默认导出的例子:

```
**import multiply from "./index.js";**
```

正如你在上面看到的，`multiply`没有被花括号`{}`包围。`multiply` 这里只是文件`index.js`的默认导出的一个变量名。导入默认导出时，您可以在此处使用任何名称。

# 结论

如您所见，JavaScript 中的 ES6 模块允许您在不同的文件中导出和导入 JavaScript 代码。这有助于将代码分解成更小粒度的文件，从而节省时间并使代码更整洁。

感谢您阅读本文，希望您觉得有用。如果有，通过 [**订阅我们的 YouTube 频道**](https://www.youtube.com/channel/UCtipWUghju290NWcn8jhyAw?sub_confirmation=true) **获取更多类似内容！**

# 更多阅读

[](https://medium.com/javascript-in-plain-english/4-useful-html5-features-you-probably-dont-know-a4be822378d0) [## 你可能不知道的 4 个有用的 HTML5 特性

### 非常有用的 HTML 特性和例子

medium.com](https://medium.com/javascript-in-plain-english/4-useful-html5-features-you-probably-dont-know-a4be822378d0)