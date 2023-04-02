# 虚拟化—文本字段

> 原文：<https://javascript.plainenglish.io/vuetify-text-fields-aa0349c5da78?source=collection_archive---------11----------------------->

![](img/fe1d88968449b49dfb139c59856d6564.png)

Photo by [Benjamin Davies](https://unsplash.com/@bendavisual?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

Vuetify 是一个流行的 Vue 应用程序 UI 框架。

在本文中，我们将了解如何使用 Vuetify 框架。

# 文本字段

我们可以添加文本字段来接受用户输入。

例如，我们可以写:

```
<template>
  <v-container>
    <v-row>
      <v-col col="12">
        <v-text-field label="Regular" single-line></v-text-field>
      </v-col>
      <v-col col="12">
        <v-text-field label="Solo" single-line solo></v-text-field>
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

我们有`single-line`属性使文本字段显示为单行。

`solo`道具使输入显示为方框。

还有一个`outlined`道具来显示带有轮廓的输入。

# 异形文本输入

如果应用了`outlined`道具，则`shaped`文本字段会变圆。

例如，我们可以写:

```
<template>
  <v-container>
    <v-row>
      <v-col col="12">
        <v-text-field v-model="first" label="First Name" outlined shaped></v-text-field>
      </v-col>
    </v-row>
  </v-container>
</template>
<script>
export default {
  name: "HelloWorld",
  data: () => ({
    first: "",
  }),
};
</script>
```

`outlined`和`shaped`道具一起将使文本字段的两个顶角变圆。

# 禁用和只读文本字段

文本字段也可以是`disabled`和`readonly`。

例如，我们可以写:

```
<template>
  <v-container>
    <v-row>
      <v-col col="12">
        <v-text-field v-model="first" label="First Name" disabled></v-text-field>
      </v-col>
    </v-row>
  </v-container>
</template>
<script>
export default {
  name: "HelloWorld",
  data: () => ({
    first: "",
  }),
};
</script>
```

或者:

```
<template>
  <v-container>
    <v-row>
      <v-col col="12">
        <v-text-field v-model="first" label="First Name" readonly></v-text-field>
      </v-col>
    </v-row>
  </v-container>
</template>
<script>
export default {
  name: "HelloWorld",
  data: () => ({
    first: "",
  }),
};
</script>
```

这两个属性都使输入无效。

但是`readonly`不会改变风格。

# 稠密的

`dense`道具使文本输入短于默认大小。

所以我们可以写:

```
<template>
  <v-container>
    <v-row>
      <v-col col="12">
        <v-text-field dense label="Regular"></v-text-field>
      </v-col>
    </v-row>
  </v-container>
</template>
<script>
export default {
  name: "HelloWorld",
  data: () => ({
    first: "",
  }),
};
</script>
```

# 核标准情报中心

我们可以用`prepend-icon`、`append-icon`和`append-outer-icon`道具添加图标。

例如，我们可以写:

```
<template>
  <v-container>
    <v-row>
      <v-col col="12">
        <v-text-field label="Prepend" prepend-icon="mdi-map-marker"></v-text-field>
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

或者:

```
<template>
  <v-container>
    <v-row>
      <v-col col="12">
        <v-text-field label="Append" append-icon="mdi-map-marker"></v-text-field>
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

或者:

```
<template>
  <v-container>
    <v-row>
      <v-col col="12">
        <v-text-field label="Append Outer" append-outer-icon="mdi-map-marker"></v-text-field>
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

我们设置道具以便添加图标。

`prepend-icon`和`append-icon`在线内添加图标。

`append-outer-icon`在线外添加图标。

# 结论

我们可以添加带有图标的文本字段，并使它们只读。

喜欢这篇文章吗？如果有，通过 [**订阅我们的 YouTube 频道**](https://www.youtube.com/channel/UCtipWUghju290NWcn8jhyAw?sub_confirmation=true) **获取更多类似内容！**