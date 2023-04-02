# BootstrapVue —下拉菜单定制和嵌入

> 原文：<https://javascript.plainenglish.io/bootstrapvue-dropdown-customization-and-embedding-51bc7f699415?source=collection_archive---------1----------------------->

![](img/4efc605b56a6e9627dd21c429a5a2023.png)

Photo by [Mihály Köles](https://unsplash.com/@mihaly_koles?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

为了制作好看的 Vue 应用，我们需要设计组件的样式。

为了使我们的生活更容易，我们可以使用内置样式的组件。

在本文中，我们将了解如何定制 BootstrapVue 下拉菜单和嵌入媒体。

# 禁用翻转

下拉菜单的对齐方式可能会根据其视口位置而变化。

为了防止这种情况发生，我们可以在我们的`b-dropdown`中添加`no-filip`道具。

# 菜单偏移

我们可以用`offset`道具在下拉列表中添加一个偏移量。

例如，我们可以写:

```
<template>
  <div id="app">
    <b-dropdown id="dropdown-offset" offset="25" text="Offset Dropdown" class="m-2">
      <b-dropdown-item href="#">Action</b-dropdown-item>
    </b-dropdown>
  </div>
</template><script>
export default {
  name: "App"
};
</script>
```

然后我们将下拉菜单向右移动 25 个像素。

# 拆分按钮

我们可以创建一个菜单按钮，它有一个按钮，除了打开下拉菜单之外，还可以做其他事情。

为此，我们添加了`split`道具:

```
<template>
  <div id="app">
    <b-dropdown split id="dropdown" text="Dropdown" class="m-2">
      <b-dropdown-item href="#">Action</b-dropdown-item>
    </b-dropdown>
  </div>
</template><script>
export default {
  name: "App"
};
</script>
```

现在我们只有一个箭头在右边打开下拉菜单。

# 拆分按钮链接

拆分按钮中不打开下拉列表的部分可以打开一个 URL。

我们可以添加`split-href`道具来打开一个网址。

例如，我们可以写:

```
<template>
  <div id="app">
    <b-dropdown split id="dropdown" split-href="#foo" text="Dropdown" class="m-2">
      <b-dropdown-item href="#">Action</b-dropdown-item>
    </b-dropdown>
  </div>
</template><script>
export default {
  name: "App"
};
</script>
```

我们添加了`split-href=”#foo”`,这样当我们点击左边的部分时就会打开`#foo`路径。

# 拆分按钮类型

拆分按钮可以具有按钮的所有类型。

为了指定类型，我们使用`split-button-type`道具。

该值可以是`'button'`、`'submit'`或`'reset'`。

# 胶料

我们可以用`size`道具改变下拉按钮的大小。

例如，我们可以写:

```
<template>
  <div id="app">
    <b-dropdown id="dropdown" size="lg" text="Dropdown" class="m-2">
      <b-dropdown-item href="#">Action</b-dropdown-item>
    </b-dropdown>
  </div>
</template><script>
export default {
  name: "App"
};
</script>
```

然后我们得到一个额外的大按钮。

`'sm'`将使按钮小于默认尺寸。

# 下拉颜色变体

我们可以设置`variant`属性来改变下拉按钮的颜色变量。

例如，如果我们有:

```
<template>
  <div id="app">
    <b-dropdown id="dropdown" variant="primary" text="Dropdown" class="m-2">
      <b-dropdown-item href="#">Action</b-dropdown-item>
    </b-dropdown>
  </div>
</template><script>
export default {
  name: "App"
};
</script>
```

然后我们把它变成蓝色。

所有轮廓和填充变量都可用。

# 拆分按钮颜色变体

如果我们有一个分割按钮，我们可以为左右部分设置不同的变量。

为了设置左边部分，我们使用`split-variant`道具。

为了设置正确的部分，我们使用`variant`道具。

例如，我们可以写:

```
<template>
  <div id="app">
    <b-dropdown
      id="dropdown"
      split
      split-variant="outline-primary"
      variant="primary"
      text="Dropdown"
      class="m-2"
    >
      <b-dropdown-item href="#">Action</b-dropdown-item>
    </b-dropdown>
  </div>
</template><script>
export default {
  name: "App"
};
</script>
```

现在左边部分有一个蓝色的轮廓和文字。

右边部分有蓝色背景和白色箭头。

# 块级下拉菜单

下拉菜单可以是块级别的。

这意味着它填充了整个页面宽度。

为了使 dropdown 成为一个 block 元素，我们添加了`block`属性

例如，我们可以写:

```
<template>
  <div id="app">
    <b-dropdown id="dropdown" block text="Dropdown" class="m-2">
      <b-dropdown-item href="#">Action</b-dropdown-item>
    </b-dropdown>
  </div>
</template><script>
export default {
  name: "App"
};
</script>
```

现在按钮填充了视口，因为我们给`b-dropdown`添加了`block`道具。

`block`可以和拆分按钮道具组合做一个块级的拆分按钮。

# 响应性地嵌入项目

BootstrapVue 带有一个`b-embed`组件来响应性地嵌入东西。

要使用它，我们可以写:

```
<template>
  <div id="app">
    <b-embed
      type="iframe"
      aspect="16by9"
      src="https://www.youtube.com/embed/wtH-hdOF1uA?rel=0"
      allowfullscreen
    ></b-embed>
  </div>
</template><script>
export default {
  name: "App"
};
</script>
```

我们只需将`src`道具设置为 YouTube 视频的嵌入 URL 来嵌入一个 YouTube 视频。

`allowfullscreen`表示我们允许视频全屏播放。

`aspect`是长宽比，我们设置为 16 乘 9。

`type`是我们要嵌入的对象类型。嵌入在 iframe 中的 YouTube 视频，所以我们将其设置为`'iframe'`。

除了 iframes，我们还可以嵌入`video`、`embed`和`object`元素。

我们还可以在`b-embed`组件中添加`source`元素来嵌入我们想要的东西。

![](img/2b9d64fc78a09034d492f6ad52aad00c.png)

Photo by [Kouji Tsuru](https://unsplash.com/@pafuxu?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 结论

我们可以用各种选项定制我们的下拉菜单。

`b-embed`可用于嵌入物品。

## 坦白地说

你知道我们有四种出版物吗？通过[**plain English . io**](https://plainenglish.io/)—关注我们的出版物和 [**订阅我们的 YouTube 频道**](https://www.youtube.com/channel/UCtipWUghju290NWcn8jhyAw) **来表达你的爱吧！**