# BootstrapVue—自定义覆盖和分页

> 原文：<https://javascript.plainenglish.io/bootstrapvue-customizing-overlays-and-pagination-f26a5ecdde22?source=collection_archive---------4----------------------->

![](img/11d93ecf308d5133c6b88b597f326377.png)

Photo by [Larry Li](https://unsplash.com/@larryli?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

为了制作好看的 Vue 应用，我们需要设计组件的样式。

为了使我们的生活更容易，我们可以使用内置样式的组件。

在这篇文章中，我们将看看如何自定义我们的应用程序覆盖。

我们还将了解如何向页面添加分页按钮。

# 宽度

我们可以将`b-overlay`更改为内嵌块组件，然后我们可以更改宽度。

例如，我们可以写:

```
<b-overlay class="d-inline-block">
  ...
</b-overlay>
```

现在我们可以改变宽度。

# 非包装模式

我们可以添加`no-wrap`属性来禁用包装的渲染并忽略默认槽。

例如，我们可以写:

```
<template>
  <div id="app">
    <div>baz</div>
    <b-overlay no-wrap show>
      <b-card title="Card">
        <b-card-text>foo</b-card-text>
      </b-card>
    </b-overlay>
    <div>qux</div>
  </div>
</template>
<script>
export default {
  name: "App"
};
</script>
```

这样，所有东西都在覆盖图后面，而不仅仅是在`b-overlay`组件里面。

# 页码

我们可以用`b-pagination`组件将分页按钮添加到我们的应用程序中。

例如，我们可以写:

```
<template>
  <div id="app">
    <b-pagination v-model="page" :total-rows="rows" :per-page="perPage"></b-pagination> <p>Current Page: {{ page }}</p>
  </div>
</template>
<script>
export default {
  name: "App",
  data() {
    return {
      rows: 100,
      perPage: 10,
      page: 1
    };
  }
};
</script>
```

我们有需要一些道具的`b-pagination`组件。

`v-model`将当前页码绑定到`page`状态。

`total-rows`有我们数据的总行数。

`per-page`是我们希望每页显示多少行。

现在我们得到了一系列可以点击的分页链接。

p 元素中的`page`值也将被更新。

# 按钮内容

按钮内容可以自定义。

例如，我们可以写:

```
<template>
  <div id="app">
    <b-pagination
      v-model="page"
      :total-rows="rows"
      :per-page="perPage"
      first-text="First"
      prev-text="Prev"
      next-text="Next"
      last-text="Last"
    ></b-pagination> <p>Current Page: {{ page }}</p>
  </div>
</template>
<script>
export default {
  name: "App",
  data() {
    return {
      rows: 100,
      perPage: 10,
      page: 1
    };
  }
};
</script>
```

我们有`first-text`道具来设置按钮的内容，以转到第一页。

`prev-text`道具用于设置按钮的内容，以转到上一页。

`next-text`道具用于将按钮的内容设置为 fo 到下一页。

而`last-text`道具是用来设置按钮的内容转到最后一页。

如果我们想为按钮设置 HTML 内容，我们可以为它们填充插槽。

例如，我们可以写:

```
<template>
  <div id="app">
    <b-pagination v-model="page" :total-rows="rows" :per-page="perPage">
      <template v-slot:first-text>
        <span class="text-success">First</span>
      </template>
      <template v-slot:prev-text>
        <span class="text-success">Prev</span>
      </template>
      <template v-slot:next-text>
        <span class="text-success">Next</span>
      </template>
      <template v-slot:last-text>
        <span class="text-success">Last</span>
      </template>
      <template v-slot:ellipsis-text>
        <b-spinner small type="grow"></b-spinner>
        <b-spinner small type="grow"></b-spinner>
      </template>
      <template v-slot:page="{ page, active }">
        <b v-if="active">{{ page }}</b>
        <span v-else>{{ page }}</span>
      </template>
    </b-pagination> <p>Current Page: {{ page }}</p>
  </div>
</template>
<script>
export default {
  name: "App",
  data() {
    return {
      rows: 100,
      perPage: 10,
      page: 1
    };
  }
};
</script>
```

我们填充了`first-text`槽来设置按钮的内容，以便转到第一页。

我们填充了`next-text`槽来设置按钮的内容，以便转到下一页。

此外，我们填充了`prev-text`槽来设置按钮的内容，以便转到上一页。

我们填充了`last-text`槽来设置按钮的内容，以便转到最后一页。

用省略号的内容填充`ellipsist-text`。

我们用闪烁的圆点替换了默认文本。

`page`槽用于填充页码的内容。

我们可以从中获得`active`和`page`状态。

`active`表示页面是否活动。

`page`是页码。

![](img/c9c29aa8b690f8da445519c46246c460.png)

Photo by [Benjamin Voros](https://unsplash.com/@vorosbenisop?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 结论

我们可以改变覆盖的宽度，只要我们使它成为一个内嵌块组件。

分页允许我们创建一个分页栏来浏览页面。

我们可以用自己的内容填充分页项。

## 简单英语的 JavaScript

喜欢这篇文章吗？如果有，通过 [**订阅我们的 YouTube 频道**](https://www.youtube.com/channel/UCtipWUghju290NWcn8jhyAw) **获取更多类似内容！**