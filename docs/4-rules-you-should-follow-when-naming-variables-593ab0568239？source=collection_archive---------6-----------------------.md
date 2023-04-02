# 命名变量时应遵循的 5 条规则

> 原文：<https://javascript.plainenglish.io/4-rules-you-should-follow-when-naming-variables-593ab0568239?source=collection_archive---------6----------------------->

## 使用这些技巧提高代码的可读性

![](img/7156d9a9a53e076b876a2784bc677d2c.png)

Photo by [Mihai Surdu](https://unsplash.com/@mihaisurdu?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) on [Unsplash](https://unsplash.com/s/photos/confusion?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)

在计算机程序设计中，**命名约定**是一组规则，用于选择字符序列，作为标识符，表示变量、类型、函数以及源代码和文档中的其他实体。

给事物命名很难！

本文试图将重点放在命名变量的一些规则和协议上，这将提高代码的可读性。

虽然这些建议可以应用于任何编程语言，但我们将在实践中使用 JavaScript 来说明它们。

## 1.跟着科学情报署走

一个名字必须是 **S** hort、 **I** ntuitive 和**D**descriptive*。*

```
/* Bad */
const a = 5 // "a" could mean anything
const isPaginatable = (postsCount > 10) // "Paginatable" sounds extremely unnatural
const shouldPaginatize = (postsCount > 10) // Made up verbs are so much fun!
```

建议:

```
/* Good */
const postsCount = 5
const hasPagination = (postsCount > 10)
const shouldDisplayPagination = (postsCount > 10) // alternatively
```

## 2.避免收缩

不要使用缩写。它们只会降低代码的可读性。找到一个简短的、描述性的名字可能很难，但缩写不是不这样做的借口。例如

```
/* Bad */
const onItmClk = () => {}

/* Good */
const onItemClick = () => {}
```

## 3.避免上下文重复

如果不降低名称的可读性，请总是从名称中删除上下文。

```
class MenuItem {
  /* Method name duplicates the context (which is "MenuItem") */
  handleMenuItemClick = (event) => { ... }

  /* Reads nicely as `MenuItem.handleClick()` */
  handleClick = (event) => { ... }
}
```

## 4.应反映预期结果

```
/* Bad */
const isEnabled = (itemsCount > 3)
return <Button disabled={!isEnabled} />

/* Good */
const isDisabled = (itemsCount <= 3)
return <Button disabled={isDisabled} />
```

## 5.考虑单数/复数

像前缀一样，变量名可以是单数或复数，这取决于它们是保存单个值还是多个值。

```
/* Bad */
const friends = 'Bob';
const friend = ['Bob', 'Tony', 'Tanya'];

/* Good */
const friend = 'Bob';
const friends = ['Bob', 'Tony', 'Tanya'];
```

这篇文章的灵感来自这个 [GitHub](https://github.com/kettanaito/naming-cheatsheet) 回购。快乐编码。

**感谢阅读！🍻**