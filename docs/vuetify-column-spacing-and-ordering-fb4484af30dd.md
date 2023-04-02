# 虚拟化—列间距和排序

> 原文：<https://javascript.plainenglish.io/vuetify-column-spacing-and-ordering-fb4484af30dd?source=collection_archive---------5----------------------->

![](img/994ee07457f014d297af76ae541bbcfb.png)

Photo by [NASA](https://unsplash.com/@nasa?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

Vuetify 是一个流行的 Vue 应用程序 UI 框架。

在本文中，我们将了解如何使用 Vuetify 框架。

# 排在最后还是第一位

我们可以将`order`属性设置为`last`并将`first`设置为最后一列和第一列。

例如，我们可以写:

```
<template>
  <v-container class="grey lighten-5">
    <v-row no-gutters>
      <v-col order="last">
        <v-card class="pa-2" outlined tile>First, but last</v-card>
      </v-col>
      <v-col>
        <v-card class="pa-2" outlined tile>Second, but unordered</v-card>
      </v-col>
      <v-col order="first">
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

# 抵消

我们可以使用`offset`道具在列之间添加空格。

例如，我们可以写:

```
<template>
  <v-container class="grey lighten-5">
    <v-row class="mb-6" no-gutters>
      <v-col md="4">
        <v-card class="pa-2" outlined tile>.col-md-4</v-card>
      </v-col>
      <v-col md="2" offset-md="6">
        <v-card class="pa-2" outlined tile>.col-md-2 .offset-md-6</v-card>
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

当到达`md`断点时，我们使用`offset-md`属性来设置这个 div 和它之前的 div 之间的列数。

# 边际效用

我们可以给列添加填充和边距。

例如，我们可以写:

```
<template>
  <v-container class="grey lighten-5">
    <v-row>
      <v-col md="4">
        <v-card class="pa-2" outlined tile>.col-md-4</v-card>
      </v-col>
      <v-col md="4" class="ml-auto">
        <v-card class="pa-2" outlined tile>.col-md-4 .ml-auto</v-card>
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

`pa-2`类在类周围添加填充。

`ml-auto`增加`v-col`组件之间的边距。

# 嵌套网格

我们可以创建一个嵌套在父网格中的网格。

例如，我们可以写:

```
<template>
  <v-container class="grey lighten-5">
    <v-row>
      <v-col sm="9">
        <v-card class="pa-2" outlined tile>Level 1</v-card>
        <v-row no-gutters>
          <v-col cols="8" sm="6">
            <v-card class="pa-2" outlined style="background-color: lightgrey;" tile>Level 2</v-card>
          </v-col>
          <v-col cols="4" sm="6">
            <v-card class="pa-2" outlined style="background-color: lightgrey;" tile>Level 3</v-card>
          </v-col>
        </v-row>
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

我们在`v-col`中有`v-row`来嵌套它们。

# 间隔器

当我们填充可用空间或在两个组件之间留出空间时,`v-spacer`组件很有用。

例如，我们可以写:

```
<template>
  <v-container class="grey lighten-5">
    <v-row>
      <v-col>
        <v-card class="pa-2" outlined tile>.col</v-card>
      </v-col>
      <v-spacer></v-spacer>
      <v-col>
        <v-card class="pa-2" outlined tile>.col</v-card>
      </v-col>
      <v-spacer></v-spacer>
      <v-col>
        <v-card class="pa-2" outlined tile>.col</v-card>
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

增加`v-spacer`组件使`v-col`均匀间隔。

# 结论

我们可以订购柱子，并用各种道具按照我们喜欢的方式将它们隔开。

喜欢这篇文章吗？如果有，通过 [**订阅我们的 YouTube 频道**](https://www.youtube.com/channel/UCtipWUghju290NWcn8jhyAw?sub_confirmation=true) **获取更多类似内容！**