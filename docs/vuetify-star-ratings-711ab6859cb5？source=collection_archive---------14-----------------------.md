# Vuetify —星级评定

> 原文：<https://javascript.plainenglish.io/vuetify-star-ratings-711ab6859cb5?source=collection_archive---------14----------------------->

![](img/a0afc34587c34d1538de51dff5631710.png)

Photo by [Kristopher Roller](https://unsplash.com/@krisroller?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

Vuetify 是一个流行的 Vue 应用程序 UI 框架。

在本文中，我们将了解如何使用 Vuetify 框架。

# 等级

`v-rating`组件允许我们在应用程序中添加星级输入。

要使用它，我们可以写:

```
<template>
  <div>
    <v-rating v-model="rating" background-color="purple lighten-3" color="purple" small></v-rating>
  </div>
</template>
<script>
export default {
  name: "HelloWorld",
  data: () => ({
    rating: 4,
  }),
};
</script>
```

`v-model`有额定值。

`background-color`有填色。

`color`有轮廓颜色。

`small`让星星变得格外小。

我们也可以将尺寸设置为`large`、`x-large`和`size`道具。

`size`道具可以让我们将星星设置为任意大小的像素值。

# 自定义长度

我们可以改变显示的星星数量。

为此，我们使用`length`道具:

```
<template>
  <div>
    <v-rating v-model="rating" background-color="purple lighten-3" color="purple" :length="10"></v-rating>
  </div>
</template>
<script>
export default {
  name: "HelloWorld",
  data: () => ({
    rating: 4,
  }),
};
</script>
```

我们将`length`设置为 10，所以我们看到显示了 10 颗星星。

# 递增

我们可以添加完整的图标，半图标，或空图标。

例如，我们可以写:

```
<template>
  <div>
    <v-rating v-model="rating" background-color="purple lighten-3" color="purple" half-increments></v-rating>
  </div>
</template>
<script>
export default {
  name: "HelloWorld",
  data: () => ({
    rating: 4.5,
  }),
};
</script>
```

`half-increments`道具让我们添加半颗星。

所以当我们设置`rating`为 4.5 时，我们会看到 4.5 颗星星。

# 时间

我们可以填充星星插槽来定制我们想要的星星。

例如，我们可以写:

```
<template>
  <div class="text-center">
    <v-rating v-model="rating">
      <template v-slot:item="props">
        <v-icon
          :color="props.isFilled ? genColor(props.index) : 'grey lighten-1'"
          large
          @click="props.click"
        >{{ props.isFilled ? 'mdi-star-circle' : 'mdi-circle-outline' }}</v-icon>
      </template>
    </v-rating>
  </div>
</template>
<script>
export default {
  name: "HelloWorld",
  data: () => ({
    colors: ["green", "purple", "orange", "indigo", "red"],
    rating: 4.5,
  }),
  methods: {
    genColor(i) {
      return this.colors[i];
    },
  },
};
</script>
```

我们有来自老虎机道具的`isFilled`属性来决定是否应该填充星星。

`click`方法可以作为点击处理程序传递到`@click`指令中。

这样，当我们单击它时，我们将看到用星号显示的值。

通过从`genColor`方法返回颜色名称，我们可以得到不同星星的不同颜色。

# 卡片评级

我们可以在一张卡上放一个`v-rating`组件。

例如，我们可以写:

```
<template>
  <v-card class="mx-auto elevation-20" color="purple" dark style="max-width: 400px;">
    <v-row justify="space-between">
      <v-col cols="8">
        <v-card-title>
          <div>
            <div class="headline">Song</div>
          </div>
        </v-card-title>
      </v-col>
      <v-col cols="4">
        <v-img
          class="shrink ma-2"
          contain
          height="125px"
          src="https://randomuser.me/api/portraits/women/68.jpg"
          style="flex-basis: 125px"
        ></v-img>
      </v-col>
    </v-row>
    <v-divider dark></v-divider>
    <v-card-actions class="pa-4">
      Rate this album
      <v-spacer></v-spacer>
      <span class="grey--text text--lighten-2 caption mr-2">({{ rating }})</span>
      <v-rating
        v-model="rating"
        background-color="white"
        color="yellow accent-4"
        dense
        half-increments
        hover
        size="18"
      ></v-rating>
    </v-card-actions>
  </v-card>
</template>
<script>
export default {
  name: "HelloWorld",
  data: () => ({
    rating: 4.5,
  }),
  methods: {
    genColor(i) {
      return this.colors[i];
    },
  },
};
</script>
```

我们将`v-rating`组件放在`v-card-actions`中，使其显示在卡片的底部。

# 结论

我们可以添加`v-rating`组件，让我们添加一个星级输入到我们的应用程序。

喜欢这篇文章吗？如果有，通过 [**订阅我们的 YouTube 频道**](https://www.youtube.com/channel/UCtipWUghju290NWcn8jhyAw?sub_confirmation=true) **获取更多类似内容！**