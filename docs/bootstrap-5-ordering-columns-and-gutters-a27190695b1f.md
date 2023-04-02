# Bootstrap 5 —订购列和装订线

> 原文：<https://javascript.plainenglish.io/bootstrap-5-ordering-columns-and-gutters-a27190695b1f?source=collection_archive---------0----------------------->

![](img/9113376e6ee4b5b9220484ff6b34345c.png)

Photo by [Martin Moreno](https://unsplash.com/@memoreno?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

**写这篇文章时，Bootstrap 5 还处于 alpha 版本，可能会有变化。**

Bootstrap 是任何 JavaScript 应用程序的流行 UI 库。

在本文中，我们将了解如何使用 Bootstrap 5 重新排序列和添加间距。

# 重新排序列

我们可以用`.order-*`类对列重新排序。

例如，我们可以写:

```
<div class="container">
  <div class="row">
    <div class="col">
      First in DOM
    </div>
    <div class="col order-2">
      Second in DOM
    </div>
    <div class="col order-1">
      Third in DOM
    </div>
  </div>
</div>
```

第二和第三个分区的视觉顺序被颠倒了，因为我们将`order-2`应用于第二个分区，将`order-1`应用于第三个分区。

还有`.order-first`和`.order-last`类来改变元素的顺序。

`.order-first`将项目作为第一个元素对齐。

`.ordet-last`将项目作为最后一个元素对齐。

例如，我们可以写:

```
<div class="container">
  <div class="row">
    <div class="col order-last">
      First in DOM
    </div>
    <div class="col">
      Second in DOM
    </div>
    <div class="col order-first">
      Third in DOM
    </div>
  </div>
</div>
```

来使用这些类。

# 偏移列

我们可以添加偏移量类来根据偏移量的大小移动列。

例如，我们可以写:

```
<div class="container">
  <div class="row">
    <div class="col-md-4">.col-md-4</div>
    <div class="col-md-4 offset-md-4">.col-md-4 .offset-md-4</div>
  </div>
  <div class="row">
    <div class="col-md-3 offset-md-3">.col-md-3 .offset-md-3</div>
    <div class="col-md-3 offset-md-3">.col-md-3 .offset-md-3</div>
  </div>
  <div class="row">
    <div class="col-md-6 offset-md-3">.col-md-6 .offset-md-3</div>
  </div>
</div>
```

用`offset`类来移动列。

需要断点和大小来移动。

`offset-md-4`表示如果视窗到达`md`断点或更高位置，该列将向右移动 4 列。

`offset-md-3`表示如果视口到达`md`断点或更高位置，该列将向右移动 3 列。

# 边际效用

Bootstrap 5 附带保证金实用程序。

`.mr-auto`类强制兄弟列彼此远离。

# 独立列类

`.col-*`类可以在`.row`之外使用，给元素一个特定的宽度。

如果我们在`.row`之外使用`.col-*`类，填充将被省略。

例如，我们可以写:

```
<div class="col-6 bg-light p-3 border">
  width of 50%
</div>
<div class="col-sm-8 bg-light p-3 border">
  width of 67% above sm breakpoint
</div>
```

对于第一个 div，我们可以用视口宽度的 50%来调整列的大小。

第二个是 67%的大小，当它是 sm 或以上。

# 沟壑

檐槽让我们在栏之间添加填充。

我们可以用它来分隔和对齐内容。

例如，我们可以写:

```
<div class="container px-4">
  <div class="row gx-5">
    <div class="col">
     <div class="p-3 border">column padding</div>
    </div>
    <div class="col">
      <div class="p-3 border">column padding</div>
    </div>
  </div>
</div>
```

用`gx-5`类在行内的两个 div 之间添加一些填充。

同样，我们可以添加`.overflow-hidden`类来做同样的事情。

例如，我们可以写:

```
<div class="container overflow-hidden">
  <div class="row gx-5">
    <div class="col">
     <div class="p-3 border">column padding</div>
    </div>
    <div class="col">
      <div class="p-3 border">column padding</div>
    </div>
  </div>
</div>
```

# 垂直排水沟

我们可以用`.gy-*`类添加垂直槽。

例如，我们可以写:

```
<div class="container overflow-hidden">
  <div class="row gy-5">
    <div class="col-6">
      <div class="p-3 border">column padding</div>
    </div>
    <div class="col-6">
      <div class="p-3 border">column padding</div>
    </div>
    <div class="col-6">
      <div class="p-3 border">column padding</div>
    </div>
    <div class="col-6">
      <div class="p-3 border">column padding</div>
    </div>
  </div>
</div>
```

我们有`gy-5`类来在 div 之间垂直添加一些填充。

# 水平和垂直排水沟

我们可以结合`.gx-*`和`.gy-*`类来添加垂直和水平的檐槽。

例如，我们可以写:

```
<div class="container">
  <div class="row g-2">
    <div class="col-6">
      <div class="p-3 border">column padding</div>
    </div>
    <div class="col-6">
      <div class="p-3 border">column padding</div>
    </div>
    <div class="col-6">
      <div class="p-3 border">column padding</div>
    </div>
    <div class="col-6">
      <div class="p-3 border">column padding</div>
    </div>
  </div>
</div>
```

我们将它与`g-2`类结合在一起。这将给我们横向和纵向的空间。

![](img/b6ce27b90dc37658a6b55a5c19740d9f.png)

Photo by [Tony Lam Hoang](https://unsplash.com/@tonylamhoang?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 结论

我们可以添加檐槽来增加栏与栏之间的空间。

列也可以重新排序。

## 坦白地说

喜欢这篇文章吗？如果有，通过 [**订阅我们的 YouTube 频道**](https://www.youtube.com/channel/UCtipWUghju290NWcn8jhyAw) **获取更多类似内容！**