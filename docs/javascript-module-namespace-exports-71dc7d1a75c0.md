# JavaScript 模块名称空间导出

> 原文：<https://javascript.plainenglish.io/javascript-module-namespace-exports-71dc7d1a75c0?source=collection_archive---------4----------------------->

## JavaScript ES2020 的新特性

![](img/fc8c0f89303c8c9114d425164587d3c2.png)

Grayscale photo of a computer with code, white-notebook, and a cup of coffee

从 ES6 开始，我们可以使用 import 和 export 关键字分别导入或导出函数、对象或原语。有许多方法可以使用导入和导出关键字，例如，默认导出或命名导出，但很快我们将有另一种方法:“模块名称空间导出”

模块名称空间导出在 ECMAScript Stage4 中，这意味着添加已经准备好包含在正式的 [ECMAScript 标准](https://tc39.es/process-document/)中。

## 默认导出

我们使用默认导出只导出一个值。在导入过程中，我们可以省略花括号，并且可以使用我们想要的名称:

```
//------ utils.js ------
export default 10;
//------ main.js ------
import aValue from 'utils.mjs';//------ utils.js ------
export default class MyClass {}
//------ main.js ------
import oneClass from 'utils.mjs';
```

## 指定出口

我们可以为每个文件指定多个导出。在导入过程中，必须使用相同的名称。命名导出主要是指导出和导入任何具有定义名称的对象、函数或原始值:

```
//------ utils.js ------
export function sum(x) {
    return x + x;
}
export function square(x) {
    return x * x;
}
//------ main.js ------
import {sum, square} from 'utils.mjs';
```

## 混合默认/命名导出

每个文件可以有多个命名导出，但同一个文件中只有一个默认导出:

```
//------ utils.js ------
export default class MyClass {}
export function sum(x) {
    return x + x;
}//------ main.js ------
import mYClass, { sum} from 'utils.mjs';
```

我们还可以使用*选择器从脚本中导入所有导出，如下所示:

```
//------ utils.js ------
export default class MyClass {}
export function sum(x) {
    return x + x;
}//------ main.js ------
import * from './utils.mjs';.
```

正如我们在前面的案例中所看到的，导出声明和导入声明之间存在语法上的对称性。这种对称行为在编程时为我们提供了类似的体验。
然而，在实际的规范中，这种对称性有一个缺口，它会导致令人困惑的行为。

例如，对于以下导入，我们没有预期的对称导出行为:

```
//------ main.js ------
**import * from './utils.mjs';.**
```

模拟这种情况的方法是:

```
import * as utils from './utils.mjs';
export {utils};
```

很快，使用对称语法建议“模块名称空间导出”，我们将能够以类似于导入语句的方式重写上面的代码:

```
//------ utils.js ------
**export * as utils from './utils.mjs'**
```

## 结论

名称空间导出为开发人员引入了相同的导入/导出关键字使用风格，获得了对称的行为，并有助于避免错误。

非常感谢您阅读这篇关于 JavaScript ES2020 中这个有用特性的小文章。

我希望它对你有用！

更多在[**es 2020 中的 JavaScript 新特性(ES11)**](https://medium.com/javascript-in-plain-english/new-javascript-features-in-es2020-c2d76acf9c5a)

【JavaScript 用简单的英语写的一句话:我们总是对帮助推广高质量的内容感兴趣。如果你有一篇文章想用简单的英语提交给 JavaScript，用你的中级用户名发邮件到[submissions@javascriptinplainenglish.com](mailto:submissions@javascriptinplainenglish.com)给我们，我们会把你添加为作者。