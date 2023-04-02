# 如何写出更好的 JavaScript 模块

> 原文：<https://javascript.plainenglish.io/how-to-write-better-javascript-modules-5c9c269086c9?source=collection_archive---------5----------------------->

![](img/c4fcf791443565d9d836f37f82d882d8.png)

Photo by [Artem Beliaikin](https://unsplash.com/@belart84?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

像任何类型的应用程序一样，JavaScript 应用程序也必须写得很好。

否则，我们以后会遇到各种各样的问题。

在本文中，我们将看看在编写 JavaScript 模块时应该遵循的一些最佳实践。

# 首选指定的导出

命名导出必须以导出成员的名称导入。

这不同于默认导出，默认导出可以以任何名称导入。

因此，命名导出比默认导出更容易混淆。

例如，不要写:

```
export default class Person {
  constructor(name) {
    this.name = name;
  } greet() {
    return `Hello, ${this.name}!`;
  }
}
```

我们写道:

```
export class Person {
  constructor(name) {
    this.name = name;
  } greet() {
    return `Hello, ${this.name}!`;
  }
}
```

第一个示例可以用任何名称导入。

第二个必须作为`Person`导入。

如果我们正在使用的文本编辑器具有自动完成功能，那么它也可以处理命名导出。

# 导入期间不工作

我们不应该对导出的代码做任何事情。

如果我们用一个有效的表达式导出一些东西，很容易得到意想不到的结果。

例如，以下导出是不好的:

`config.js`

```
export const config = {
  data: JSON.parse(str)
};
```

在导出完成后，工作仍然会被完成，所以我们得到最新的值。

当我们进口它的时候，`JSON.parse`就叫做。

这意味着导入速度会变慢。

如果我们有:

```
import { config } from 'config';
```

然后`JSON.parse`就会运行。

为了让`JSON.parse`以一种懒惰的方式运行，我们可以写:

`config.js`

```
let parsedData = null;export const config  = {
  get data() {
    if (parsedData === null) {
      parsedData = JSON.parse(str);
    }
    return parsedData;
  }
};
```

这样，我们将解析后的字符串缓存在`parsedData`中。

`JSON.parse`仅在`parsedData`为`null`时运行。

# 高内聚模块

内聚性描述了一个模块中的组件属于一起的程度。

我们应该确保一个模块属于同一个模块。

这意味着我们应该在一个模块中有相关的实体。

例如，我们可以用做算术运算的函数制作一个`math`模块。

我们可以写:

`math.js`

```
export const add = (a, b) => a + b;
export const subtract = (a, b) => a - b;
export const multiply = (a, b) => a * b;
export const divide = (a, b) => a / b;
```

所有的函数做相似的事情，所以它们属于一个模块。

如果我们有低内聚的模块，那么就很难理解这个模块有什么。

一个模块里不相关的东西就是没有意义。

# 避免长的相对路径

如果它们嵌套很深，就很难找到一个模块。

我们应该避免深度嵌套以避免长的相对路径。

所以与其写:

```
import { addDates } from '../../date/add';
import { subtractDates }   from '../../date/subtract';
```

我们写道:

```
import { addDates } from './date/add';
import { subtractDates } from './date/subtract';
```

如果我们将它们放在节点包中，我们可以使用绝对路径:

```
import { addDates } from 'utils/date/add';
import { subtractDates } from 'utils/date/subtract';
```

我们把所有东西都放在一个`utils`包中，这样我们就可以在一个绝对路径中引用它。

![](img/5bdab1cfd5b826bf3cb23551f4bf055e.png)

Photo by [Grant Durr](https://unsplash.com/@blizzard88?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 结论

我们应该制作内聚的模块，以便更容易理解它们。

此外，命名导出比默认导出更好。

我们应该导出在导出过程中有效的代码，因为它们会在我们导入时运行。

## **简单英语的 JavaScript**

喜欢这篇文章吗？如果是这样，通过 [**订阅解码获得更多类似内容，我们的 YouTube 频道**](https://www.youtube.com/channel/UCtipWUghju290NWcn8jhyAw) **！**