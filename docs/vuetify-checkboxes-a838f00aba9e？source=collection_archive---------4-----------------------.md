# 虚拟化—复选框

> 原文：<https://javascript.plainenglish.io/vuetify-checkboxes-a838f00aba9e?source=collection_archive---------4----------------------->

![](img/f0e75654f4d17ac2da1ab96440b26ecb.png)

Photo by [David Beale](https://unsplash.com/@davidbeale?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

Vuetify 是一个流行的 Vue 应用程序 UI 框架。

在本文中，我们将了解如何使用 Vuetify 框架。

# 复选框

我们可以添加带有`v-checkbox`组件的复选框。

例如，我们可以写:

```
<template>
  <v-container>
    <v-row>
      <v-col col="12">
        <v-checkbox v-model="checked" :label="`Checkbox: ${checked.toString()}`"></v-checkbox>
      </v-col>
    </v-row>
  </v-container>
</template>
<script>
export default {
  name: "HelloWorld",
  data: () => ({
    checked: false,
  }),
};
</script>
```

我们添加了`v-model`指令，将它绑定到一个状态值。

然后我们在`label`中显示检查过的值。

此外，我们可以将多个复选框绑定到同一个值。

这样，状态将是一个数组:

```
<template>
  <v-container>
    <v-row>
      <v-col col="12">
        <p>{{ selected }}</p>
        <v-checkbox v-model="selected" label="james" value="james"></v-checkbox>
        <v-checkbox v-model="selected" label="mary" value="mary"></v-checkbox>
      </v-col>
    </v-row>
  </v-container>
</template>
<script>
export default {
  name: "HelloWorld",
  data: () => ({
    selected: [],
  }),
};
</script>
```

我们添加了`value`属性，这样当复选框被选中时，值将在`selected`数组中。

# 复选框状态

复选框也可以是不确定的。

我们可以添加`indeterminate`道具使其不确定:

```
<template>
  <v-container>
    <v-row>
      <v-col col="12">
        <v-checkbox value indeterminate></v-checkbox>
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

它们也可以被禁用:

```
<template>
  <v-container>
    <v-row>
      <v-col col="12">
        <v-checkbox input-value="true" value disabled></v-checkbox>
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

# 复选框颜色

我们可以用`color`道具改变复选框的颜色:

```
<template>
  <v-container>
    <v-row>
      <v-col col="12">
        <v-checkbox v-model="checked" label="red" color="red" value="red" hide-details></v-checkbox>
      </v-col>
    </v-row>
  </v-container>
</template>
<script>
export default {
  name: "HelloWorld",
  data: () => ({
    checked: false,
  }),
};
</script>
```

我们有一个红色的复选框，因为我们将`color`设置为`red`。

# 文本栏内的复选框

我们可以添加与文本字段内嵌的复选框。

为此，我们写道:

```
<template>
  <v-container>
    <v-row>
      <v-col col="12">
        <v-row align="center">
          <v-checkbox v-model="checked" hide-details class="shrink mr-2 mt-0"></v-checkbox>
          <v-text-field label="Include files"></v-text-field>
        </v-row>
      </v-col>
    </v-row>
  </v-container>
</template>
<script>
export default {
  name: "HelloWorld",
  data: () => ({
    checked: false,
  }),
};
</script>
```

我们将复选框和文本字段放在同一个`v-row`中，使它们并排显示。

# 结论

我们可以用 Vuetify 添加各种样式和行为的复选框。

喜欢这篇文章吗？如果有，通过 [**订阅我们的 YouTube 频道**](https://www.youtube.com/channel/UCtipWUghju290NWcn8jhyAw?sub_confirmation=true) **获取更多类似内容！**