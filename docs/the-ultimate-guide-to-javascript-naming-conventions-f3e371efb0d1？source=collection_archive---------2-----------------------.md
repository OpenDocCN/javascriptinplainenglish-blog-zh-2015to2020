# JavaScript 命名约定的终极指南

> 原文：<https://javascript.plainenglish.io/the-ultimate-guide-to-javascript-naming-conventions-f3e371efb0d1?source=collection_archive---------2----------------------->

## 在命名方面不要太有创意，否则你以后会搞混的

![](img/7815e3b9efd986b9b919e26c69902ace.png)

Photo by [Shahadat Rahman](https://unsplash.com/@hishahadat?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

当有许多成员在一个项目中工作时，为了可伸缩性，必须有一个特定的标准来遵循。

今天，我们将讨论最基本的标准，命名惯例:我们应该做什么，不应该做什么。

让我们开始吧。

# 使用特定的名称

```
// Don’t
function fetchData() {}// Do
function fetchUsers() {}
```

我们知道`fetchData()`会调用一个 API 并从服务器获取数据，但是什么数据呢？不如起个更具体的名字吧？越具体，你就越不会花时间去思考这些名字是关于什么的，而不做任何评论。

# 避免一个字母的名字

除了在循环语句中常见的名字如`i`代表`index`，你应该避免使用单字母的名字，因为它会让你以后感到困惑。

```
// Don’t
let n = 0;// Do
let number = 0;
```

# 如有必要，请使用较长的名称

很多时候，我们的目标是简短的名字。然而，在某些情况下，为了更清晰和更具描述性，一个长名字应该不成问题。

```
// Don’t
function findBooks() {}// Do
function findBooksByAuthor() {}
```

# 变量

仅仅为变量定义写注释更可能是多余的。变量名应该是描述性的，这样当你看到它的时候，你就会知道它的确切用途。

```
// Don’t
let d = ‘01/01/2021’;
let number = 28;// Do
let date = ‘01/01/2021’;
let age = 28;
```

有许多命名风格，但是，为了保持一致性，您应该使用 camelCase。

```
// Don’t
let book_title = ‘JavaScript’;
let BOOKTITLE = ‘JavaScript’;
let booktitle = ‘JavaScript’;// Do
let bookTitle = ‘JavaScript’;
```

## 常数

JavaScript 中的常量不能更改。为了区别于其他变量，常量中的所有字母都应该大写。

```
// Don’t
const pi = 3.14;// Do
const PI = 3.14;
```

## 布尔运算

布尔值就像是一个是/否问题:

*   读了吗？
*   你做到了吗？
*   那些相等吗？

因此，你命名一个布尔变量的方式应该类似于这些问题。

```
// Don’t
let decoration = true;
let red = true;
let visible = false;// Do
let hasDecoration = true;
let isRed = true;
let isVisible = false;
```

# 功能

当你想执行一个动作时，你调用一个函数。因此，函数名应该以动词开头。

```
// Don’t
function data() {}// Do
function fetchData() {}
```

你应该喜欢一致的动词，而不是对同一个概念有创造性。

```
// Don’t
function getUsers() {}
function fetchBooks() {}
function retriveAuthors() {}// Do
function fetchUsers() {}
function fetchBooks() {}
function fetchAuthors() {}
```

# 班级

像其他编程语言一样，命名类名应该遵循 PascalCase，类名应该是名词。

```
// Don’t
class mobileDevice {}// Do
class MobileDevice {}
```

# 文件

对于 JavaScript 文件，我们有两种常见的命名方式:kebab-case 和 PascalCase。

烤肉串-案例:

```
-components/
--product/
---product-details.js
---product-list.js
```

帕斯卡塞:

```
-components/
--product/
---ProductDetails.js
---ProductList.js
```

你可以使用其中任何一个。对我来说，我选择烤肉串。

# 组件(反应)

这是特异反应。我们应该使用 PascalCase 作为组件名。

```
// Don’t
class profilePicture extends React.Component {
  render() {
    return <h1>Profile Picture</h1>;
  }
}// Do
class ProfilePicture extends React.Component {
  render() {
    return <h1>Profile Picture</h1>;
  }
}
```

# 结论

关于 JavaScript 中的命名约定，你只需要知道这些。你可以把这个有用的列表收藏起来，如果需要的话，以后可以参考。

希望你喜欢这篇文章。

# 进一步阅读

*   [编写可伸缩 JavaScript 代码的 9 个技巧](https://medium.com/javascript-in-plain-english/9-tips-for-writing-scalable-javascript-code-e6bcfc791882)