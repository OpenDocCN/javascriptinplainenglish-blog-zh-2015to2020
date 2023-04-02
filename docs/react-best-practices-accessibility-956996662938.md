# 反应最佳实践—可访问性

> 原文：<https://javascript.plainenglish.io/react-best-practices-accessibility-956996662938?source=collection_archive---------8----------------------->

![](img/b5e00dab2b9b51872c9dc8f164ce5143.png)

Photo by [Marcin Skalij](https://unsplash.com/@m_skalij?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

像任何类型的应用程序一样，React 应用程序也必须写得很好。

否则，我们以后会遇到各种各样的问题。

在本文中，我们将研究一些最大化可访问性的最佳实践。

# 咏叹调道具

我们应该使用有效的 aria 属性。

例如，我们写道:

```
<div id="name_label">Enter your name</div>
<input aria-labelledby="name_label">
```

我们用来标记输入的元素的 id 必须与`aria-labelledBy`属性的值相匹配。

# Aria 原型

我们应该添加有效的 aria 状态和属性值。

例如，我们写道:

```
<span aria-hidden="true">foo</span>
```

# 咏叹调角色

我们应该有合适的咏叹调角色。

例如，我们写道:

```
<div role="button"></div>
```

由于`'button'`是一个有效的 aria 角色，我们可以有这样的代码。

# Aria 不支持的元素

我们不应该在不支持 aria 属性的元素中使用 aria 属性。

它们包括`meta`、`html`、`style`和`script`元素。

例如，以下是有效的:

```
<meta charset="UTF-8" />
```

但下面不是:

```
<meta charset="UTF-8" aria-hidden="false" />
```

# 标题应该有内容

如果我们使用 h1、h2 等标题标签，那么它们应该有内容。

这使得它们可以被屏幕读取。

它也不应该被`aria-hidden`道具隐藏。

例如，我们应该写:

```
<h1>Heading</h1>
```

# href 中没有哈希

如果我们有一个`href`属性，那么如果我们想让它们可访问，我们不应该有一个 hash 作为它的值。

如果我们想放一个 hash 作为`href`的值，那么我们应该用一个按钮来代替。

例如，我们写道:

```
<a href="https://example.com" />
```

而不是:

```
<a href="#" />
```

# iframes 应该有一个标题

Iframes 应该有一个`title`属性，这样屏幕阅读器就可以区分它们。

例如，我们应该写:

```
<iframe title="unique title" />
```

# 对 img 元素的 alt 属性值使用描述性文本

在我们的 alt 描述中，我们不需要像“照片”、“图像”或“图片”这样多余的词。

这是因为屏幕阅读器已经知道我们有一个 img 标签。

因此，与其写:

```
<img src="foo" alt="photo of the beach" />
```

我们应该写:

```
<img src="foo" alt="the beach" />
```

# 没有 accessKey 属性

我们不应该在元素上添加`accessKey`道具。

我们可以使用 prop 为用户分配键盘快捷键，

但是，由于它们在不同屏幕阅读器之间的行为不一致，我们应该避免使用它们。

因此，我们应该写:

```
<div />
```

而不是:

```
<div accessKey="h" />
```

# 没有分散注意力的因素

视觉上分散注意力的元素会导致视觉障碍用户的可访问性，所以我们不应该拥有它们。

这些包括像选取框或闪烁。

如果我们有，我们应该删除它们。

# 没有多余的角色

角色属性的值不应该包含标记名中已经存在的单词。

这是因为它们是多余的。

例如，我们不应该写:

```
<button role="button" />
```

相反，我们写道:

```
<button role="presentation" />
```

# 角色应该配有咏叹调道具

如果我们有角色属性，那么我们应该有值所需的所有属性。

例如，我们写道:

```
<span role="checkbox" aria-checked="false" aria-labelledby="foo" tabindex="0"></span>
```

而不是:

```
<span role="checkbox"></span>
```

# 咏叹调道具

并非所有 aria 属性都可以与给定的角色值一起使用。

因此，我们应该只拥有可以与角色一起使用的属性。

例如，我们写道:

```
<span role="checkbox" aria-checked="false" aria-labelledby="foo" tabindex="0"></span>
```

`aria-checked`、`aria-labvelledby`、`tabindex`可以和`role='chekcbox'`一起用，所以要加上。

# 范围

`scope`属性应该只和`th`元素一起使用。

例如，我们写道:

```
<th scope="col" />
```

而不是:

```
<div scope />
```

![](img/fd71ddfe76392afdf225c784b8679e30.png)

Photo by [Petar Tonchev](https://unsplash.com/@ptonchev?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 结论

我们应该在元素中拥有正确的属性组合。

否则，我们会混淆屏幕阅读器。

## 简单英语的 JavaScript

你知道我们有四种出版物吗？通过 [**plainenglish.io**](https://plainenglish.io/) 找到他们——通过关注我们的出版物和 [**订阅我们的 YouTube 频道**](https://www.youtube.com/channel/UCtipWUghju290NWcn8jhyAw) **来表达爱意吧！**