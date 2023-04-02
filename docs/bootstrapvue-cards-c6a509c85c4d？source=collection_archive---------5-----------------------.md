# BootstrapVue —卡

> 原文：<https://javascript.plainenglish.io/bootstrapvue-cards-c6a509c85c4d?source=collection_archive---------5----------------------->

![](img/d22084c181006f3e46816d3a7a4988a3.png)

Photo by [Mike Benna](https://unsplash.com/@mbenna?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

为了做出好看的 Vue 应用，我们需要为我们的组件设计风格。

为了让我们的生活更轻松，我们可以使用内置样式的组件。

在本文中，我们将了解如何将卡片添加到 Vue 应用程序中。

# 卡片

在 Bootstrap 中，卡是一个灵活且可扩展的内容容器。

它可以黄金页眉、页脚和内容。

背景颜色也可以更改。

要添加一张卡片，我们可以使用`b-card`组件。

例如，我们可以写:

```
<template>
  <div id="app">
    <b-card
      title="Card Title"
      img-src="https://placekitten.com/g/600/200"
      img-alt="cat"
      img-top
      tag="article"
      style="max-width: 20rem;"
      class="mb-2"
    >
      <b-card-text>a cat</b-card-text><b-button href="#" variant="primary">Go</b-button>
    </b-card>
  </div>
</template><script>
export default {
  name: "App"
};
</script>
```

我们有带几个道具的`b-card`组件。

`img-src`是图像的 URL。

`img-alt`为图像的 alt 属性。

`style`有派头为牌。

`class`有类为卡。

`b-card-text`有内容为卡。

`title`有卡的标题。

`img-top`表示卡片图像位于卡片顶部。

我们还有一个带链接的按钮。

我们也可以用`image-bottom`在底部放置三个图像。

要增加一个读者，我们可以使用`header`道具。

同样，我们可以为脚注添加`footer`道具。

我们可以通过编写以下内容来添加页眉和页脚:

```
<template>
  <div id="app">
    <b-card
      header="header"
      header-tag="header"
      footer="footer"
      footer-tag="footer"
      title="title"
    >
      <b-card-text>Header and footers</b-card-text>
      <b-button href="#" variant="primary">Go</b-button>
    </b-card>
  </div>
</template><script>
export default {
  name: "App"
};
</script>
```

我们只是将这些道具添加到`b-card`组件中我们期望它们被添加的地方，以添加页眉和页脚。

# 水平布局

我们不必坚持垂直布局。

我们也可以创建一张水平布局的卡片，用`b-row`和`b-col`进行布局。

例如，我们可以写:

```
<template>
  <div id="app">
    <b-card no-body class="overflow-hidden">
      <b-row no-gutters>
        <b-col md="6">
          <b-card-img src="https://placekitten.com/200/50" alt="cat"></b-card-img>
        </b-col>
        <b-col md="6">
          <b-card-body title="Horizontal Card">
            <b-card-text>card</b-card-text>
          </b-card-body>
        </b-col>
      </b-row>
    </b-card>
  </div>
</template><script>
export default {
  name: "App"
};
</script>
```

我们有`b-col`把卡分成两半。

`md`值为 6 意味着我们有一个左列和一个右列。

`no-body`表示不允许 BootstrapVue 自动添加卡体。

相反，我们使用`b-card-body`来放置卡体。

`b-row`确保`b-col`部件保持在一排。

# 文本变体

我们可以用`bg-variant`道具更改文本字符串。

例如，我们可以写:

```
<template>
  <div id="app">
    <b-card bg-variant="dark" text-variant="white" title="title">
      <b-card-text>more content</b-card-text>
      <b-button href="#" variant="primary">Go</b-button>
    </b-card>
  </div>
</template><script>
export default {
  name: "App"
};
</script>
```

我们将`bg-variant`设置为`'dark'`，使背景为深灰色。

`title`仍为卡名。

`text-variant`是卡片文本的样式，我们将其更改为白色。

其他变体包括主要、次要、成功、信息、警告、危险、光明和黑暗。

# 有边的

我们可以为我们的卡片添加边框。

为此，我们使用了`border-variant`道具。

例如，我们可以写:

```
<template>
  <div id="app">
    <b-card border-variant="warning" header="header" header-border-variant="warning" align="center">
      <b-card-text>lorem ipsum.</b-card-text>
    </b-card>
  </div>
</template><script>
export default {
  name: "App"
};
</script>
```

我们有`header`属性来设置标题文本。

`header-border-variant`设置页眉的边框样式。

`border-variant`用于设置正文的边框样式。

# 页眉和页脚变体

页眉和页脚有自己的变体。

我们可以分别用`header-bg-variant`和`footer-bg-variant`来设置它们。

这些将设置背景颜色。

还有`header-border-variant`和`footer-border-variant`属性来设置页眉和页脚的边框样式。

# 导航集成

我们可以通过使用`b-nav`组件将 navbar 集成到卡中。

要在卡片上添加导航条，我们可以这样写:

```
<template>
  <div id="app">
  <b-card title="Ctitle" body-class="text-center" header-tag="nav">
    <template v-slot:header>
      <b-nav card-header tabs>
        <b-nav-item active>active</b-nav-item>
        <b-nav-item>inactive</b-nav-item>
        <b-nav-item disabled>disabled</b-nav-item>
      </b-nav>
    </template> <b-card-text>
      lorem ipsum.
    </b-card-text><b-button variant="primary">go</b-button>
  </b-card>
  </div>
</template><script>
export default {
  name: "App"
};
</script>
```

我们只是将`b-nav`组件放在`template`标签中来添加导航条。

按照`v-slot:header`指令的指示，将其放入标头槽。

我们有`active`属性来设置一个活动链接。

如果没有`active`道具组，那么它是不活动的。

如果有一个`disabled`道具，那么它是禁用的。

![](img/fdb6a42263ee697a0d0763099f78f862.png)

Photo by [AZGAN MjESHTRI](https://unsplash.com/@azganmjeshtri?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 结论

我们可以用卡片做很多事情。

我们可以设计它的样式，改变它的布局，给它添加一个导航条。