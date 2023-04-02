# Vuetify —时间选取器自定义

> 原文：<https://javascript.plainenglish.io/vuetify-time-picker-customization-55dda3eed7f2?source=collection_archive---------10----------------------->

![](img/94a71bebdbc4425182fe4df4a36057ba.png)

Photo by [Jen Theodore](https://unsplash.com/@jentheodore?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

Vuetify 是一个流行的 Vue 应用程序 UI 框架。

在本文中，我们将了解如何使用 Vuetify 框架。

# 时间选择器宽度

我们可以用`width`道具设置时间选择器的宽度:

```
<template>
  <v-row justify="space-around">
    <v-time-picker v-model="time" type="month" width="290" class="ml-4"></v-time-picker>
  </v-row>
</template>
<script>
export default {
  name: "HelloWorld",
  data: () => ({
    time: "11:15",
  }),
};
</script>
```

宽度以像素为单位。

此外，我们可以添加`full-width`道具，使时间选择器全幅:

```
<template>
  <v-row justify="space-around">
    <v-time-picker v-model="time" :landscape="$vuetify.breakpoint.mdAndUp" full-width type="month"></v-time-picker>
  </v-row>
</template>
<script>
export default {
  name: "HelloWorld",
  data: () => ({
    time: "11:15",
  }),
};
</script>
```

我们可以在我们想要的断点处将其设置为横向模式，这样标题将显示为侧边栏。

# 时间选取器的高度

时间选择器下面可以有阴影。

我们可以添加`flat`道具来移除阴影:

```
<template>
  <v-row justify="space-around">
    <v-time-picker v-model="time" flat></v-time-picker>
  </v-row>
</template>
<script>
export default {
  name: "HelloWorld",
  data: () => ({
    time: "11:15",
  }),
};
</script>
```

或者我们可以添加`elevation`道具来添加我们的阴影:

```
<template>
  <v-row justify="space-around">
    <v-time-picker v-model="time" elevation="15"></v-time-picker>
  </v-row>
</template>
<script>
export default {
  name: "HelloWorld",
  data: () => ({
    time: "11:15",
  }),
};
</script>
```

该值介于 0 到 24 之间。

# 标题中的 AM/PM 开关

我们可以添加`ampm-in-title`属性来将 AM 和 PM 文本添加到标题中。

例如，我们可以写:

```
<template>
  <v-row justify="space-around">
    <v-time-picker v-model="time" ampm-in-title></v-time-picker>
  </v-row>
</template>
<script>
export default {
  name: "HelloWorld",
  data: () => ({
    time: "11:15",
  }),
};
</script>
```

这样，我们可以选择上午或下午的时间。

# 没有标题

我们可以用`no-title`道具移除标题栏:

```
<template>
  <v-row justify="space-around">
    <v-time-picker v-model="time" no-title></v-time-picker>
  </v-row>
</template>
<script>
export default {
  name: "HelloWorld",
  data: () => ({
    time: "11:15",
  }),
};
</script>
```

# 带秒的时间选择器

时间选择器可以让我们选择秒数。

为此，我们添加了`use-seconds`道具:

```
<template>
  <v-row justify="space-around">
    <v-time-picker v-model="picker" use-seconds></v-time-picker>
  </v-row>
</template>
<script>
export default {
  name: "HelloWorld",
  data: () => ({
    time: "11:15",
  }),
};
</script>
```

现在我们可以选秒了。

# 可滚动时间选择器

我们可以使用`scrollable`属性滚动时间值:

```
<template>
  <v-row justify="space-around">
    <v-time-picker v-model="picker" scrollable></v-time-picker>
  </v-row>
</template>
<script>
export default {
  name: "HelloWorld",
  data: () => ({
    time: "11:15",
  }),
};
</script>
```

现在我们用滚轮滚动来选择时间值。

# 范围

可以用`min`和`max`道具限制时间值范围:

```
<template>
  <v-row justify="space-around" align="center">
    <v-col style="width: 290px; flex: 0 1 auto;">
      <h2>Start:</h2>
      <v-time-picker v-model="start" :max="end"></v-time-picker>
    </v-col>
    <v-col style="width: 290px; flex: 0 1 auto;">
      <h2>End:</h2>
      <v-time-picker v-model="end" :min="start"></v-time-picker>
    </v-col>
  </v-row>
</template>
<script>
export default {
  name: "HelloWorld",
  data: () => ({
    start: null,
    end: null,
  }),
};
</script>
```

我们有`max`道具来设置我们可以选择的最大时间。

并且`min`道具设置最小时间限制。

# 结论

我们可以用很多方式定制时间选择器。

喜欢这篇文章吗？如果有，通过 [**订阅我们的 YouTube 频道**](https://www.youtube.com/channel/UCtipWUghju290NWcn8jhyAw?sub_confirmation=true) **获取更多类似内容！**