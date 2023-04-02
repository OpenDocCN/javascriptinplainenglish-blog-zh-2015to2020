# 日期和月份选择器

> 原文：<https://javascript.plainenglish.io/vuetify-date-and-month-picker-867244b00c37?source=collection_archive---------15----------------------->

![](img/c87f0c5ff8775c9362a6a3a17536ade2.png)

Photo by [Isabella and Louisa Fischer](https://unsplash.com/@twinsfisch?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

Vuetify 是一个流行的 Vue 应用程序 UI 框架。

在本文中，我们将了解如何使用 Vuetify 框架。

# 日期选取器的当前日期指示器

我们可以用`show-current`按钮改变当前日期指示器。

例如，我们可以写:

```
<template>
  <v-row justify="center">
    <v-date-picker v-model="date" :show-current="false"></v-date-picker>
  </v-row>
</template><script>
export default {
  name: "HelloWorld",
  data: () => ({
    date: new Date().toISOString().substr(0, 10),
  }),
};
</script>
```

我们用它来使当前日期周围的圆圈变亮。

它也可以设置为日期字符串:

```
<template>
  <v-row justify="center">
    <v-date-picker v-model="date" show-current="2020-07-13"></v-date-picker>
  </v-row>
</template><script>
export default {
  name: "HelloWorld",
  data: () => ({
    date: new Date().toISOString().substr(0, 10),
  }),
};
</script>
```

然后我们会看到给定的日期周围有一个圆圈。

# 日期选择器—年、月和日期按钮的 DOM 事件

日期选择器为年、月和日期按钮发出各种事件。

我们可以在日期选择器上收听`@click`、`@dblclick`、`@mouseenter`等事件，以及更多事件。

例如，我们可以写:

```
<template>
  <v-row>
    <v-col class="my-2 px-1" cols="12" sm="6">
      <v-date-picker
        v-model="date"
        @contextmenu:year="contextMenu"
        @dblclick:date="dblClick"
        @mouseenter:month="mouseEnter"
        @mouseleave:month="mouseLeave"
      ></v-date-picker>
    </v-col> <v-col class="my-2 px-1" cols="12" sm="6">
      <div class="title mb-2">{{ mouseMonth }}</div>
    </v-col>
  </v-row>
</template><script>
export default {
  name: "HelloWorld",
  data: () => ({
    date: new Date().toISOString().substr(0, 10),
    mouseMonth: null,
  }), methods: {
    contextMenu(year, event) {
      event.preventDefault();
      alert(`You have activated context menu for year ${year}`);
    },
    dblClick(date) {
      alert(`You have just double clicked the following date: ${date}`);
    },
    mouseEnter(month) {
      this.mouseMonth = month;
    },
    mouseLeave() {
      this.mouseMonth = null;
    },
  },
};
</script>
```

我们监听月份选择器上的`mouseenter`和`mouseleave`事件，并通过处理程序获取其值。

`month`修饰符表示我们监听月份选择器的动作。

`contextmenu`有`year`修饰符听年份。

# 月采摘者

我们可以通过将`v-date-picker`的`type`属性设置为`month`来创建一个月份选择器。

例如，我们可以写:

```
<template>
  <v-row>
    <v-col class="my-2 px-1" cols="12" sm="6">
      <v-date-picker v-model="date" type="month"></v-date-picker>
    </v-col>
  </v-row>
</template><script>
export default {
  name: "HelloWorld",
  data: () => ({
    date: new Date().toISOString().substr(0, 10),
  }),
};
</script>
```

`landscape`道具使日期选择器以横向模式显示。

例如，我们可以写:

```
<template>
  <v-row justify="space-around">
    <v-date-picker v-model="date" type="month" landscape></v-date-picker>
  </v-row>
</template>
<script>
export default {
  name: "HelloWorld",
  data: () => ({
    date: new Date().toISOString().substr(0, 10),
  }),
};
</script>
```

# 月份选择器颜色

我们可以用`color`和`header-color`道具改变月份选择器的颜色。

例如，我们可以写:

```
<template>
  <v-row justify="space-around">
    <v-date-picker v-model="date" type="month" color="green lighten-1"></v-date-picker>
  </v-row>
</template><script>
export default {
  name: "HelloWorld",
  data: () => ({
    date: new Date().toISOString().substr(0, 10),
  }),
};
</script>
```

`header-color`设置日期选择器标题的颜色。

# 结论

我们可以改变日期选择器的各种风格。

`v-date-picker`组件也可以是月份选择器。

喜欢这篇文章吗？如果有，通过 [**订阅我们的 YouTube 频道**](https://www.youtube.com/channel/UCtipWUghju290NWcn8jhyAw?sub_confirmation=true) **获取更多类似内容！**