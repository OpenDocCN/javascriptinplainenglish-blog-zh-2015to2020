# 验证—单选按钮和开关

> 原文：<https://javascript.plainenglish.io/vuetify-radio-buttons-and-switches-6cfed5162c93?source=collection_archive---------6----------------------->

![](img/7d533c231845ffa56585c35b0a8239b3.png)

Photo by [Eric Nopanen](https://unsplash.com/@rexcuando?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

Vuetify 是一个流行的 Vue 应用程序 UI 框架。

在本文中，我们将了解如何使用 Vuetify 框架。

# 单选按钮

我们可以用`v-radio`组件添加单选按钮。

例如，我们可以写:

```
<template>
  <v-container>
    <v-row>
      <v-col col="12">
        <p>{{ radios }}</p>
        <v-radio-group v-model="radios" :mandatory="false">
          <v-radio label="Radio 1" value="radio-1"></v-radio>
          <v-radio label="Radio 2" value="radio-2"></v-radio>
        </v-radio-group>
      </v-col>
    </v-row>
  </v-container>
</template>
<script>
export default {
  name: "HelloWorld",
  data: () => ({
    radios: '',
  }),
};
</script>
```

将`mandatory`道具设置为`false`使其可选。

# 收音机按钮方向

单选按钮可以在一行或一列中。

例如，我们可以写:

```
<template>
  <v-container>
    <v-row>
      <v-col col="12">
        <p>{{ radios }}</p>
        <v-radio-group v-model="radios" column>
          <v-radio label="Radio 1" value="radio-1"></v-radio>
          <v-radio label="Radio 2" value="radio-2"></v-radio>
        </v-radio-group>
      </v-col>
    </v-row>
  </v-container>
</template>
<script>
export default {
  name: "HelloWorld",
  data: () => ({
    radios: "",
  }),
};
</script>
```

在列中显示单选按钮。

我们可以用一个`row`道具让它们排成一行显示:

```
<template>
  <v-container>
    <v-row>
      <v-col col="12">
        <p>{{ radios }}</p>
        <v-radio-group v-model="radios" row>
          <v-radio label="Radio 1" value="radio-1"></v-radio>
          <v-radio label="Radio 2" value="radio-2"></v-radio>
        </v-radio-group>
      </v-col>
    </v-row>
  </v-container>
</template>
<script>
export default {
  name: "HelloWorld",
  data: () => ({
    radios: "",
  }),
};
</script>
```

# 收音机按钮颜色

单选按钮的颜色可以用`color`道具改变:

```
<template>
  <v-container>
    <v-row>
      <v-col col="12">
        <p>{{ radios }}</p>
        <v-radio-group v-model="radios">
          <v-radio label="Radio 1" value="radio-1" color="red"></v-radio>
          <v-radio label="Radio 2" value="radio-2" color="red"></v-radio>
        </v-radio-group>
      </v-col>
    </v-row>
  </v-container>
</template>
<script>
export default {
  name: "HelloWorld",
  data: () => ({
    radios: "",
  }),
};
</script>
```

我们用每个按钮上的`color`道具来改变单选按钮的颜色。

# 开关

我们可以添加带有`v-switch`组件的开关。

例如，我们可以写:

```
<template>
  <v-container>
    <v-row>
      <v-col col="12">
        <v-container fluid>
          <v-switch v-model="sw" :label="`Switch: ${sw.toString()}`"></v-switch>
        </v-container>
      </v-col>
    </v-row>
  </v-container>
</template>
<script>
export default {
  name: "HelloWorld",
  data: () => ({
    sw: false,
  }),
};
</script>
```

我们将`v-switch`和`v-model`绑定到一个布尔状态。

# 开关阵列

我们可以将多个开关绑定到同一个变量。

例如，我们写道:

```
<template>
  <v-container>
    <v-row>
      <v-col col="12">
        <v-container fluid>
          <p>{{ people }}</p>
          <v-switch v-model="people" label="james" value="james"></v-switch>
          <v-switch v-model="people" label="mary" value="mary"></v-switch>
        </v-container>
      </v-col>
    </v-row>
  </v-container>
</template>
<script>
export default {
  name: "HelloWorld",
  data: () => ({
    people: [],
  }),
};
</script>
```

我们有`value`道具，如果我们选中它，它将被添加到`people`数组中。

# 结论

Vuetify 为我们提供了单选按钮和开关。

喜欢这篇文章吗？如果有，通过 [**订阅我们的 YouTube 频道**](https://www.youtube.com/channel/UCtipWUghju290NWcn8jhyAw?sub_confirmation=true) **获取更多类似内容！**