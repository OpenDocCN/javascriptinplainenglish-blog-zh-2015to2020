# DOM 操作—样式表和脚本

> 原文：<https://javascript.plainenglish.io/dom-manipulation-stylesheets-and-scripts-af47f0705aa9?source=collection_archive---------8----------------------->

![](img/2032014187d24bb347eeb36df97f3546.png)

Photo by [Ave Calvar](https://unsplash.com/@shotbyrain?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

JavaScript 是世界上最流行的编程语言之一。为了有效地使用它，我们必须了解它的基本知识。

在本文中，我们将研究如何操作样式表中的数据和下载脚本。

# CSSStyleRule 概述

一个`CSSStyleRule`对象代表一个样式表中的每个 CSS 规则，

它是附加到选择器的 CSS 属性和值的接口。

来访问它们，如果我们有:

```
<style id='styles'>
  body {
    background-color: orange;
    margin: 20px;
  }

  p {
    font-weight: 20px;
    color: blue;
  }
</style>
```

然后我们可以写:

```
const styles = document.querySelector('#styles');
console.dir(styles.sheet.cssRules);
```

从`style`元素中获取 CSS 规则。

# CSSStyleRule 属性和方法

一个`CSSStyleRule`有它自己的属性和方法。

它具有以下属性:

*   `cssText`—CSS 规则的文本
*   `parentRule` —给定样式块的父规则
*   `parentStylesSheet` —父样式表对象
*   `selectorText` —规则的选择器
*   `style` —规则的样式
*   `type` —规则的类型，为 CSS 规则，值为 1

例如，我们可以写:

```
const styles = document.querySelector('#styles');
console.dir(styles.sheet.cssRules[0].cssText);
```

然后我们得到 body 元素的样式，因为这是第一个条目。

# 获取样式表中的 CSS 规则列表

`cssRules`有样式表的 CSS 规则列表。

例如，如果我们有与上一个例子相同的样式元素，我们写:

```
const styles = document.querySelector('#styles');
console.dir(styles.sheet.cssRules);
```

为了得到所有的`cssRules`。

# 插入或删除 CSS 规则

`insertRule`让我们在样式表中插入一个 CSS 规则。

`deleteRule`让我们删除它们。

例如，如果我们有一个样式元素:

```
<style id='styles'>
  body {
    background-color: orange;
    margin: 20px;
  }

  p {
    font-weight: 20px;
    color: blue;
  }
</style>
```

我们可以写:

```
const styles = document.querySelector('#styles');
styles.sheet.insertRule('p { color:green }', 1);
```

然后我们为 p 标签添加了一个新的 CSS 规则，它被现有的规则覆盖。

我们可以通过写下以下内容来删除旧规则:

```
styles.sheet.deleteRule(1);
```

现在我们的新规则是页面上 p 元素的规则。

我们所做的就是为要删除的规则传入索引。

# 编辑 CSSStyleRule 的值

我们可以给`style`属性赋值来改变样式值。

例如，如果我们有:

```
<style id='styles'>
  body {
    background-color: orange;
    margin: 20px;
  }p {
    font-weight: 20px;
    color: blue;
  }</style>
<p>
  foo
</p>
```

我们可以写:

```
const styles = document.querySelector('#styles');
styles.sheet.cssRules[1].style.color = 'green';
```

将 p 元素的颜色设置为绿色。

# 创建新的内联 CSS 样式表

我们可以通过使用`innerHTML`属性向样式元素添加新的样式。

例如，如果我们有:

```
const style = document.createElement('style');
style.innerHTML = 'body { color: orange }';document.querySelector('head').appendChild(style);
```

现在，主体的所有内容都将是橙色的，因为我们有了一个添加了该样式的样式元素。

# 以编程方式添加外部样式表

以编程方式添加外部样式表可以像添加样式标签一样完成。

为此，我们只需使用`createElement`:

```
const link = document.createElement('link');
link.setAttribute('rel', 'stylesheet');
link.setAttribute('type', 'text/css');
link.setAttribute('id', 'link');
link.setAttribute('href', 'https://stackpath.bootstrapcdn.com/bootstrap/4.4.1/css/bootstrap.min.css');
```

我们用 Bootstrap 样式表添加了一个新的 link 元素。

# 禁用或启用样式表

属性是一个我们可以在样式表上设置的属性，用来打开和关闭样式表。

如果我们有一个链接元素:

```
<link rel="stylesheet" href="https://stackpath.bootstrapcdn.com/bootstrap/4.4.1/css/bootstrap.min.css">
```

然后，我们可以通过书写来打开或关闭:

```
document.querySelector('link').disabled = true;
```

现在我们禁用了它。要启用它，我们可以写:

```
document.querySelector('link').disabled = false;
```

# 插入和运行 JavaScript

我们可以在 HTML 文档中插入 JavaScript。

JavaScript 代码总是作为外部文件或内联代码存在于脚本标签中。

脚本元素没有任何必需的属性。

还应该避免自结束脚本标签。

试图包含一个操纵页面上脚本元素的外部脚本文件会导致页面上的脚本元素被忽略。

例如，我们可以写:

```
<script>
console.log('hello');
</script>
```

包含脚本标记。

![](img/bf98be3d23ae4ed6a4610de9d57e7856.png)

Photo by [James Barnett](https://unsplash.com/@jimo?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# JavaScript 同步加载

加载脚本的默认选项是同步加载。

这意味着他们会阻止其他脚本的渲染和下载，如果它们存在的话。

如果一个脚本对文档来说是永恒的，并且阻止了其他所有东西的加载，那么这个脚本必须在解析之前先下载，这意味着它会更慢。

# 结论

可以用 JavaScript 修改样式规则。

默认情况下，脚本是同步加载的。

## 简单英语的 JavaScript

喜欢这篇文章吗？如果有，通过 [**订阅我们的 YouTube 频道**](https://www.youtube.com/channel/UCtipWUghju290NWcn8jhyAw) **获取更多类似内容！**