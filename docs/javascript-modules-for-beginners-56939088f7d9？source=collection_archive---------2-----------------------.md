# 如何在 JavaScript 中使用模块

> 原文：<https://javascript.plainenglish.io/javascript-modules-for-beginners-56939088f7d9?source=collection_archive---------2----------------------->

![](img/fc8c0f89303c8c9114d425164587d3c2.png)

Keep Calm and learn Javascript

这是我试图解释一个让很多学生困惑的话题:导入和导出模块。

首先，我们将回顾它如何与 CommonJS `require`和`module.exports`一起工作。然后，我们将看看 ES6 模块。

大多数学生从 NodeJS 中使用的 CommonJS 语法开始:

```
// For NPM packages
const moment = require('moment');
// For internal modules
const toLowerCase = require('./toLowerCase.js');
```

然后在`toLowerCase.js`你需要出口一些东西:

```
const convertToLowerCase = (sentence) => {...};module.exports = convertToLowerCase;
```

这里的关键是，无论有什么值`module.exports`都将是使用`require`时的值。在这种情况下，`module.exports`的值是一个函数。这意味着我们可以做以下事情:

```
const toLowerCase = require('./toLowerCase.js');toLowerCase('Hello World!');
```

然而，我们可能想要导出不止一个东西。假设我们有一个包含一些有用功能的模块。

```
const helpers = require('./helpers.js');
```

然后在我们的`helpers.js`:

```
const convertToLowerCase = (sentence) => {...};
const convertToCamelCase = (sentence) => {...};module.exports = {
  toCamelCase: convertToCamelCase,
  toLowerCase: convertToLowerCase,
};
```

在这种情况下，`module.exports`的值是一个对象。这个对象有两个属性:`toCamelCase`和`toLowerCase`，它们的作用是作为值。

这是我们使用它们的方式:

```
const helpers = require('./helpers.js');helpers.toCamelCase('Hello world');
```

> 记住:`module.exports`的值将是需要时变量的值。

让我们重写前面的`helper.js`文件。记住导出时重要的部分是`module.exports`的值。

```
module.exports = {};module.exports.toCamelCase = () => {...};
module.exports.toLowerCase = () => {...};
```

两者完全一样。在这两种情况下，`module.exports`是一个具有两种属性的对象:`toCamelCase`和`toLowerCase`。

唯一改变的是*当*我们创建对象的时候。

现在，我们来看看 ES6 模块。

我们可以说，对于 ES6 模块，您总是在导出和导入一个对象。一直都是。

但是，您可以选择仅从该对象中选择特定的内容。

```
import toLowerCase from './toLowerCase.js';
```

在这种情况下，我们在从`toLowerCase.js`导出的对象中选择名为`default`的属性中的值。

```
const toLowerCase = (sentence) => {...};export default toLowerCase;
```

前面的文件实际上导出了以下对象:

```
{
  default: (sentence) => {...}
}
```

使用时:

```
import toLowerCase from ‘./toLowerCase.js’;
```

`toLowerCase`在`default`中有值，这是一个函数。这样我们就可以做到:

```
import toLowerCase from ‘./toLowerCase.js’;toLowerCase('Hello World!');
```

> 关键在于，使用 ES6 模块时，您总是在导出一个对象。

让我们来看看创建该对象的不同方法:

```
export const toLowerCase = (sentence) => {...};
```

这实际上是在对象中创建以下内容:

```
{
  toLowerCase: (sentence) => {...}
}
```

我们可以对更多的变量这样做:

```
export const toCamelCase = () => {...};
export const toLowerCase = () => {...};
```

这将创建以下对象:

```
{
  toCamelCase: () => {...},
  toLowerCase: () => {...}
}
```

如果我们添加一个默认值呢？

```
export const toCamelCase = () => {...};
export const toLowerCase = () => {...};const NUM_HELPERS = 2;export default NUM_HELPERS;
```

这将创建以下对象:

```
{
  toCamelCase: () => {...},
  toLowerCase: () => {...},
  default: 2
}
```

这就是导出时的神奇之处，你在创建一个对象。

如何导入该对象？

当您想要导出所有对象时:

```
import * as helpers from './helpers.js';
```

`helpers`的值为:

```
{
  toCamelCase: () => {...},
  toLowerCase: () => {...},
  default: 2
}
```

假设您想要对象的`default`属性中的任何内容？

```
import n from './helpers.js';
```

`n`的值是 2。

你想要`toLowerCase`里的东西

```
import { toLowerCase } from './helpers.js';
```

`toLowerCase`的值是对象的`toLowerCase`属性中的函数。

注意:当导入`default`时，您选择变量的名称。导入特定属性的值时，名称必须匹配。

```
import { convertToLowerCase } from './helpers.js';// `convertToLowerCase` will be `undefined`
// the exported object has no property called `convertToLowerCase`
```

更高级的说法是，您可以在导入后更改名称。

傻乎乎的道:

```
import { toLowerCase } from './helpers.js';const convertToLowerCase = toLowerCase;
```

最佳实践:

```
import { toLowerCase as convertToLowerCase } from './helpers.js';
```

我希望这个总结对所有努力学习 Javascript 模块的初学者有所帮助。

当我开始的时候，我自己会很欣赏这个总结。

有关更多详细信息，请查看关于导入的 [MDN 文档](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/import)。

感谢阅读！

如果你喜欢这篇文章，可以考虑加入我在 GIMTEC 的时事通讯。

[](https://www.gimtec.io/) [## GIMTEC

### 训练营后教育。我希望在我完成训练营时就拥有的每周时事通讯和课程。

www.gimtec.io](https://www.gimtec.io/)