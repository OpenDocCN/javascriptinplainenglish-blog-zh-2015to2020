# 现代 JavaScript 精华—填充、模块和数字

> 原文：<https://javascript.plainenglish.io/best-of-modern-javascript-fills-modules-and-numbers-2c3b7ac187af?source=collection_archive---------3----------------------->

![](img/fb280817615c8482cbceeae45ae1464d.png)

Photo by [Max van den Oetelaar](https://unsplash.com/@maxvdo?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

自 2015 年以来，JavaScript 有了巨大的进步。

现在用起来比以前舒服多了。

在本文中，我们将研究 JavaScript 的核心特性。

# 数组.原型.填充

从 ES6 开始，数组有了`fill`实例方法，让我们用想要的值填充数组。

例如，我们可以写:

```
const arr = new Array(3).fill(undefined);
```

那么`arr`就是`[undefined, undefined, undefined]`。

在 ES6 之前，没有好的对等物。

然而，我们可以使用`apply`作为黑客来做同样的事情:

```
var arr = Array.apply(null, new Array(3));
```

然后我们得到同样的结果。

然而，除了`undefined`之外，没有其他方法可以填充它。

`fill`将孔视为元素。

# ES6 模块

模块是现代 JavaScript 的另一个重要特性。

是和 ES6 一起推出的。

在 ES6 之前，没有原生模块。

有各种各样的模块系统，比如 CommonJS。

在 CommonJS 中，我们可以在一个模块中导出多个项目。

例如，我们可以写:

`math.js`

```
function add(x, y) {
  return x + y;
}function square(x) {
  return x * x;
}function diag(x, y) {
  return sqrt(square(x) + square(y));
}module.exports = {
  add: add,
  square: square,
  diag: diag,
};
```

我们将函数放在`module.exports`对象中以导出它们。

然后，我们可以通过编写以下内容来导入它们:

```
var add = require('math').add;
var diag = require('math').diag;
```

我们导入`math`模块并从模块中获取属性。

此外，我们可以要求整个模块:

```
var meth = require('math')
```

使用 ES6，我们可以使用`export`和`import`关键字分别导出和导入模块项目。

例如，我们可以写:

`math.js`

```
export function add(x, y) {
  return x + y;
}export function square(x) {
  return x * x;
}export function diag(x, y) {
  return sqrt(square(x) + square(y));
}
```

我们只是在函数前面放了`export`关键字来导出它们。

要导入它们，我们可以写:

```
import { add, diag } from 'math';
```

我们用关键字`import`和花括号从`math.js`导入`add`和`diag`函数。

带有花括号的导入称为命名导入。

# 单一出口

对于 CommonJS，我们通过给`module.exports`赋值来进行单次导出:

`foo.js`

```
module.exports = function() {
  //··· 
};
```

然后，我们可以通过书写来要求它:

```
var foo = require('foo');
```

有了 ES6，我们可以使用`export default`关键字进行单次导出。

例如，我们可以写:

`foo.js`

```
export default function() {
  //··· 
}
```

然后我们可以通过编写以下内容来导入它:

```
import foo from 'foo';
```

# 数字和数学

ES5 引入了十六进制数字。

例如，我们可以写:

```
0x1F
```

写出十进制 31 的十六进制形式。

ES6 引入了二进制数。

例如，我们可以写:

```
0b11
```

写出二进制数 3。

我们也可以写八进制数:

```
0o11
```

用八进制形式写出十进制数 9。

![](img/903c75266b5753994e8e288c7ee1e738.png)

Photo by [Louis Hansel @shotsoflouis](https://unsplash.com/@louishansel?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 结论

ES6 或更高版本具有原生模块系统，这是以前版本所没有的。

它还引入了有用的数组方法和新的数字类型。

## 简单英语的 JavaScript

你知道我们有三份出版物和一个 YouTube 频道吗？在 [**寻找一切的链接 plainenglish.io**](https://plainenglish.io/) ！