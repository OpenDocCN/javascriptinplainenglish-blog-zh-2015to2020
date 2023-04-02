# 检查变量是否是 JavaScript 对象

> 原文：<https://javascript.plainenglish.io/javascript-check-if-a-variable-is-an-object-and-nothing-else-not-an-array-a-set-etc-a3987ea08fd7?source=collection_archive---------4----------------------->

## 除此之外什么也没有(不是数组、集合等。)

![](img/bbbab6fc99628470add15052bb51d73c.png)

我目前正在做一个项目，在这个项目中，我们将某些值存储为字符串或对象。根据值的类型，我们将以不同的方式处理它。尽管这里我们只希望处理一个字符串或一个对象，但我想编写一个健壮的通用函数，它将显式地确定一个值是否是一个普通的 JavaScript 对象。我想处理基于 JavaScript 对象的其他数据结构(如数组、集合、null 等)将返回 false 的边缘情况，因为它们不仅仅是一个普通的对象。

经过一番挖掘，我找到了多个可能的答案，并做了一些 TDD 来选择正确的答案，因为我想确保我能想到的所有情况都包括在内。我将编写并测试一个`isObject`函数。

# 项目设置

我们这里不需要疯狂花哨的东西。一个文件夹包含两个文件就足够了:

*   `isObject.js`
*   `isObject.test.js`

```
mkdir isObject && cd isObject
touch isObject.js && touch isObject.test.js
```

我们还需要一个测试库(以及编写现代 JS 的 Babel 支持)。
我将使用 [Jest](https://jestjs.io/) ，因为它易于设置和使用。

```
yarn init
yarn add jest -D
yarn add --dev babel-jest @babel/core @babel/preset-env
```

最后一个`babel.config.js`文件。

```
module.exports = {
  presets: [
    [
      '@babel/preset-env',
      {
        targets: {
          node: 'current',
        },
      },
    ],
  ],
};
```

你可以在 [Jest 官方文档](https://jestjs.io/docs/en/getting-started)中找到所有的 Jest 设置。

# TDD 基础

现在让我们写一个虚拟函数

现在测试。我们希望我们的函数能够在不执行任何预先检查的情况下测试任何值。

我们正在测试`string`和`object`类型，这是我们肯定会遇到最多的情况，但也是为了确保我们的函数能够处理边缘情况。

运行测试套件:

```
jest isObject.test.js
```

它应该会失败。

# 解决方法

现在让我们实现我们的`isObject`函数。

## 类型 of

我想到的第一个解决方案是用`typeof`

运行测试。

```
jest isObject.test.js
```

它失败了。

```
FAIL  ./isObject.test.js
  isObject
    ✓ String (3ms)
    ✓ Object (1ms)
    ✕ Array (6ms)
    ✕ Set
    ✕ Date (1ms)
    ✓ Undefined
    ✕ Null
```

`typeof`的问题在于`typeof [] === 'object'`其实就是`true`。为什么？因为在 Javascript 中数组实际上是对象。布景、日期等也是如此。

需要注意的有趣事情:

*   `typeof undefined`不是`object`。是`undefined`。
*   `typeof null`是一个`object`虽然。

## 实例 of

结果是:

```
FAIL  ./isObject.test.js
  isObject
    ✓ String (3ms)
    ✓ Object (1ms)
    ✕ Array (2ms)
    ✕ Set
    ✕ Date (1ms)
    ✓ Undefined
    ✓ Null
```

这稍微好一点(`null`和`undefied`被以同样的方式处理)，但是对于我们的一些情况仍然失败。为什么？因为`instanceof`检查指定的原型(这里是`Object`)是否出现在原型链中的任何地方。

如上所述，`array`实际上是 JavaScript 中的`object`,所以:

```
[] instanceof Array  // true
[] instanceof Object // true
```

有趣的是:

*   `null`是`object`类型，但不是`object`的实例

## 构造器

它失败了。又来了。

```
FAIL  ./isObject.test.js
  isObject
    ✓ String (3ms)
    ✓ Object (1ms)
    ✓ Array
    ✓ Set
    ✓ Date
    ✕ Undefined (1ms)
    ✕ Null
```

快到了！这一次是因为`undefined`和`null`没有`constructor`属性，但是它适用于所有其他类型。我们可以这样想:

这样做的工作，但它迫使我们检查特定的值，这是我想避免的事情。我不想通过排除所有其他可能性来猜测一个变量的`type`。

经过一番研究，我找到了我需要的东西。

# Object.prototype.toString.call()

```
PASS  ./isObject.test.js
  isObject
    ✓ String (3ms)
    ✓ Object
    ✓ Array (1ms)
    ✓ Set
    ✓ Date
    ✓ Undefined
    ✓ Null
```

正是我需要的。

`toString()`方法返回一个表示对象的字符串。例如这里它为一个`array`返回什么:

```
Object.prototype.toString.call([]) // [object Array]
```

这样我确定我处理的是一个`object`而不是别的！

## **用简单英语写的 JavaScript 笔记**

我们已经推出了三种新的出版物！请关注我们的新出版物:[**AI in Plain English**](https://medium.com/ai-in-plain-english)[**UX in Plain English**](https://medium.com/ux-in-plain-english)[**Python in Plain English**](https://medium.com/python-in-plain-english)**—谢谢，继续学习！**

**我们也一直有兴趣帮助推广高质量的内容。如果您有一篇文章想要提交给我们的任何出版物，请发送电子邮件至[**submissions @ plain English . io**](mailto:submissions@plainenglish.io)**，使用您的 Medium 用户名，我们会将您添加为作者。另外，请让我们知道您想加入哪个/哪些出版物。****