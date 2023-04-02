# 验证—滑块标签和验证

> 原文：<https://javascript.plainenglish.io/vuetify-slider-labels-and-validation-d655432e32d5?source=collection_archive---------10----------------------->

![](img/b866395f4ad19c9c490763f91823f2d6.png)

Photo by [Victor Barrios](https://unsplash.com/@thevictorbarrios?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

Vuetify 是一个流行的 Vue 应用程序 UI 框架。

在本文中，我们将了解如何使用 Vuetify 框架。

# 反向滑块标签

我们可以添加`inverse-label`道具来在它的末尾显示标签:

```
<template>
  <v-container>
    <v-row>
      <v-col col="12">
        <v-slider inverse-label label="Inverse label" value="30"></v-slider>
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

# 自定义范围滑块

`tick-labels`道具允许我们给滑块添加刻度标签。

例如，我们可以写:

```
<template>
  <v-container>
    <v-row>
      <v-col col="12">
        <v-range-slider
          :tick-labels="seasons"
          :value="[0, 1]"
          min="0"
          max="3"
          ticks="always"
          tick-size="4"
        >
          <template v-slot:thumb-label="props">
            <v-icon dark>{{ season(props.value) }}</v-icon>
          </template>
        </v-range-slider>
      </v-col>
    </v-row>
  </v-container>
</template>
<script>
export default {
  name: "HelloWorld",
  data: () => ({
    seasons: ["Winter", "Spring", "Summer", "Fall"],
    value: undefined,
    icons: ["mdi-snowflake", "mdi-leaf", "mdi-fire", "mdi-water"],
  }),
  methods: {
    season(val) {
      return this.icons[val];
    },
  },
};
</script>
```

我们有`tick-labels`道具让我们为范围滑块设置标签。

此外，我们有`ticks`和`ticks-size`道具来显示我们想要的刻度数。

`ticks`设置为`always`表示将显示刻度。

# 自定义颜色

我们可以分别用`track-color`和`thumb-color`改变轨迹和拇指的颜色。

例如，我们可以写:

```
<template>
  <v-container>
    <v-row>
      <v-col col="12">
        <v-slider v-model="ex1.val" :label="ex1.label" :track-color="ex1.color"></v-slider> <v-slider
          v-model="ex2.val"
          :label="ex2.label"
          :thumb-color="ex2.color"
          thumb-label="always"
        ></v-slider>
      </v-col>
    </v-row>
  </v-container>
</template>
<script>
export default {
  name: "HelloWorld",
  data: () => ({
    ex1: { label: "track-color", val: 75, color: "green" },
    ex2: { label: "thumb-color", val: 50, color: "red" },
  }),
};
</script>
```

轨道颜色是圆点右侧的颜色。

拇指颜色是圆点的颜色。

# 范围

我们可以用`v-range-slider`组件添加一个滑动范围。

范围滑块有 2 个点来设置最小值和最大值。

例如，我们可以写:

```
<template>
  <v-container>
    <v-row>
      <v-col col="12">
        <v-range-slider v-model="value"></v-range-slider>
      </v-col>
    </v-row>
  </v-container>
</template>
<script>
export default {
  name: "HelloWorld",
  data: () => ({
    value: [30, 60],
  }),
};
</script>
```

我们用数组`value`作为`v-model`的值。

第一个值是最小值，第二个值是最大值。

# 确认

我们可以用`rules`道具验证滑块值。

例如，我们可以写:

```
<template>
  <v-container>
    <v-row>
      <v-col col="12">
        <v-slider
          v-model="value"
          :rules="rules"
          label="How many?"
          step="10"
          thumb-label="always"
          ticks
        ></v-slider>
      </v-col>
    </v-row>
  </v-container>
</template>
<script>
export default {
  name: "HelloWorld",
  data: () => ({
    value: 30,
    rules: [(v) => v <= 50 || "Can't have more than 50"],
  }),
};
</script>
```

为滑块添加验证规则。

`rules`是一个包含验证规则的函数数组。

`v`是滑块的值。

如果该值无效，则返回一条错误消息。

# 结论

我们可以用 Vuetify 给滑块添加验证规则。

喜欢这篇文章吗？如果有，通过 [**订阅我们的 YouTube 频道**](https://www.youtube.com/channel/UCtipWUghju290NWcn8jhyAw?sub_confirmation=true) **获取更多类似内容！**