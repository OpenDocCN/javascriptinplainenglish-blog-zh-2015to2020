# BootstrapVue —折叠和下拉

> 原文：<https://javascript.plainenglish.io/bootstrapvue-accordions-and-dropdowns-f0939a2cb07d?source=collection_archive---------4----------------------->

![](img/d5e8ed1a668d9a10f5428806290067fb.png)

Photo by [Kouji Tsuru](https://unsplash.com/@pafuxu?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

为了制作好看的 Vue 应用，我们需要设计组件的样式。

为了使我们的生活更容易，我们可以使用内置样式的组件。

在本文中，我们将看看如何用 BootstrapVue 添加一个 accordion 和一个 dropdown。

# 手风琴

我们可以用`b-collapse`组件创建一个手风琴。

要创建一个，我们必须将它嵌套在一个`b-card`组件中。

例如，我们可以写:

```
<template>
  <div id="app">
    <b-card no-body class="mb-1">
      <b-card-header header-tag="header" class="p-1" role="tab">
        <b-button block href="#" v-b-toggle.accordion-1 variant="info">accordion 1</b-button>
      </b-card-header>
      <b-collapse id="accordion-1" visible accordion="my-accordion" role="tabpanel">
        <b-card-body>
          <b-card-text>accordion 1 text</b-card-text>
        </b-card-body>
      </b-collapse>
    </b-card> <b-card no-body class="mb-1">
      <b-card-header header-tag="header" class="p-1" role="tab">
        <b-button block href="#" v-b-toggle.accordion-2 variant="info">accordion 2</b-button>
      </b-card-header>
      <b-collapse id="accordion-2" accordion="my-accordion" role="tabpanel">
        <b-card-body>
          <b-card-text>accordion 2 text</b-card-text>
        </b-card-body>
      </b-collapse>
    </b-card> <b-card no-body class="mb-1">
      <b-card-header header-tag="header" class="p-1" role="tab">
        <b-button block href="#" v-b-toggle.accordion-3 variant="info">accordion 3</b-button>
      </b-card-header>
      <b-collapse id="accordion-3" accordion="my-accordion" role="tabpanel">
        <b-card-body>
          <b-card-text>accordion 3 text</b-card-text>
        </b-card-body>
      </b-collapse>
    </b-card>
  </div>
</template><script>
export default {
  name: "App"
};
</script>
```

我们有 3 张卡片，它们组合在一起就成了我们的手风琴。

我们在外面有`b-card`组件。

然后我们在内部安装了`b-collapse`组件来创建手风琴。

这样，我们总是显示标题。

但是只有当我们点击标题时，才会显示正文，这也隐藏了其他的卡片。

# 下拉菜单

下拉菜单是可切换的上下文覆盖，用于显示链接和动作列表。

为了创建一个组件，我们创建了带有一些`b-dropdown-item`组件的`b-dropdown`组件。

例如，我们可以通过编写以下内容来创建一个简单的下拉列表:

```
<template>
  <div id="app">
    <b-dropdown id="dropdown-1" text="menu" class="m-md-2">
      <b-dropdown-item>apple</b-dropdown-item>
      <b-dropdown-divider></b-dropdown-divider>
      <b-dropdown-item active>active</b-dropdown-item>
      <b-dropdown-item disabled>disabled</b-dropdown-item>
    </b-dropdown>
  </div>
</template><script>
export default {
  name: "App"
};
</script>
```

我们有带有`text`属性的`b-dropdown`组件，它被设置为下拉按钮的文本。

因此，我们将下拉按钮文本设置为`'menu'`，因为这是我们为`text`设置的值。

在里面，我们有带各种道具的`b-dropdown-item`。

内容在标签之间。

`active`激活下拉项目。

`disabled`禁用下拉项目。

`b-dropdown-divider`是我们下拉菜单上的分隔线。

# 下拉按钮内容

我们可以用一个`template`元素来定制 drop 的内容，以填充`button-content`槽。

例如，我们可以写:

```
<template>
  <div id="app">
    <b-dropdown>
      <template v-slot:button-content>
        <b>menu</b>
      </template>
      <b-dropdown-item>apple</b-dropdown-item>
    </b-dropdown>
  </div>
</template><script>
export default {
  name: "App"
};
</script>
```

我们有填充`button-content`的`template`元素，如`v-slot:button-content`所示。

在它里面，我们有`<b>menu</b>`来显示粗体文本。

在它下面，我们有通常的下拉项目。

# 菜单对齐

菜单排列可以根据我们的喜好而改变。

默认设置是左对齐菜单。

如果我们想右对齐菜单，我们可以将`right`道具添加到`b-dropdown`组件中。

例如，我们可以写:

```
<template>
  <div id="app" style="padding: 100px">
    <b-dropdown right text="right">
      <b-dropdown-item>apple</b-dropdown-item>
    </b-dropdown>
  </div>
</template><script>
export default {
  name: "App"
};
</script>
```

如果屏幕左边缘有足够的空间，将菜单右对齐。

否则，它会回到左对齐菜单。

# 下降

我们也可以让菜单停留在按钮上方而不是下方。

我们只需添加`dropup`道具。

例如，我们写道:

```
<template>
  <div id="app" style="padding-top: 100px">
    <b-dropdown dropup text="right">
      <b-dropdown-item>apple</b-dropdown-item>
    </b-dropdown>
  </div>
</template><script>
export default {
  name: "App"
};
</script>
```

为了做到这一点。

# 向右或向左移动

我们可以添加类似的道具，让菜单显示在右边或左边，而不是按钮的上方或下方。

`dropright`使其显示在按钮的右侧。

`dropleft`使其显示在按钮的左侧。

![](img/cd771aaa26ed9dbacbbc225605153b49.png)

Photo by [Waldemar Brandt](https://unsplash.com/@waldemarbrandt67w?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 结论

我们可以用卡片和折叠组件创建手风琴。

另外，BootstrapVue 有我们可以使用的菜单组件。

## 简单英语的 JavaScript

你知道我们有四种出版物吗？通过 [**plainenglish.io**](https://plainenglish.io/) 找到他们——通过关注我们的出版物和 [**订阅我们的 YouTube 频道**](https://www.youtube.com/channel/UCtipWUghju290NWcn8jhyAw) **来表达你的爱吧！**