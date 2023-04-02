# 虚拟化—弹性布局

> 原文：<https://javascript.plainenglish.io/vuetify-flex-layouts-cf9f6e1375ef?source=collection_archive---------6----------------------->

![](img/488814d5cbd52b0ae6d87c1449f7213d.png)

Photo by [Jonathan Borba](https://unsplash.com/@jonathanborba?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

Vuetify 是一个流行的 Vue 应用程序 UI 框架。

在本文中，我们将了解如何使用 Vuetify 框架。

# 独特的布局

我们可以添加各种行和列的布局。

一列的列数可以根据断点设置:

```
<template>
  <v-container class="grey lighten-5">
    <v-row>
      <v-col cols="12" md="8">
        <v-card class="pa-2" outlined tile>.col-12 .col-md-8</v-card>
      </v-col>
      <v-col cols="6" md="4">
        <v-card class="pa-2" outlined tile>.col-6 .col-md-4</v-card>
      </v-col>
    </v-row>
  </v-container>
</template>
<script>
export default {
  name: "HelloWorld",
  data: () => ({}),
};
</script>
```

我们有一个包含不同大小的列的行。

第一列默认为 12 列，如果是`md`及以上，则为 8 列。

第二个默认为 6 列，如果是`md`及以上，则为 4 列。

这些列将被重新排列，以便它们总是适合屏幕。

# 竖向定线

我们可以用`align`和`align-self`道具改变 flex 项目及其父项目的垂直对齐。

例如，我们可以写:

```
<template>
  <div>
    <v-container class="grey lighten-5 mb-6">
      <v-row align="start" no-gutters>
        <v-col v-for="n in 3" :key="n" cols="2">
          <v-card class="pa-2" outlined tile>column</v-card>
        </v-col>
      </v-row>
    </v-container>
  </div>
</template>
<script>
export default {
  name: "HelloWorld",
  data: () => ({}),
};
</script>
```

我们用`v-col`组件和`align`支柱将柱子放在左边。

`align`的其他值包括将项目放在中间的`center`和将列放在最后的`end`。

# 水平线向

伸缩项与`justify`属性的水平对齐:

```
<template>
  <div>
    <v-container class="grey lighten-5 mb-6">
      <v-row justify="start" no-gutters>
        <v-col v-for="n in 3" :key="n" cols="2">
          <v-card class="pa-2" outlined tile>column</v-card>
        </v-col>
      </v-row>
    </v-container>
  </div>
</template>
<script>
export default {
  name: "HelloWorld",
  data: () => ({}),
};
</script>
```

# 没有水槽

`no-gutters`支柱拆除立柱间的水槽:

```
<template>
  <v-container class="grey lighten-5">
    <v-row no-gutters>
      <v-col cols="12" sm="5" md="6">
        <v-card class="pa-2" outlined tile>col</v-card>
      </v-col>
      <v-col cols="6" md="6">
        <v-card class="pa-2" outlined tile>col</v-card>
      </v-col>
    </v-row>
  </v-container>
</template>
<script>
export default {
  name: "HelloWorld",
  data: () => ({}),
};
</script>
```

# 列换行

如果一行中有超过 12 列，那么多余的列将换行。

# 订单类别

我们可以用`v-col`上的`order`道具改变网格项目的顺序:

```
<template>
  <v-container class="grey lighten-5">
    <v-row no-gutters>
      <v-col>
        <v-card class="pa-2" outlined tile>First, but unordered</v-card>
      </v-col>
      <v-col order="3">
        <v-card class="pa-2" outlined tile>Second, but last</v-card>
      </v-col>
      <v-col order="1">
        <v-card class="pa-2" outlined tile>Third, but first</v-card>
      </v-col>
    </v-row>
  </v-container>
</template>
<script>
export default {
  name: "HelloWorld",
  data: () => ({}),
};
</script>
```

# 结论

我们可以用各种 flexbox 类和道具创建布局。

喜欢这篇文章吗？如果有，通过 [**订阅我们的 YouTube 频道**](https://www.youtube.com/channel/UCtipWUghju290NWcn8jhyAw?sub_confirmation=true) **获取更多类似内容！**