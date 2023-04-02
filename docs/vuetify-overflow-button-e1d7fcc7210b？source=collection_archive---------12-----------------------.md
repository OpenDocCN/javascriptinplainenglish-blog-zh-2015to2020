# Vuetify —溢出按钮

> 原文：<https://javascript.plainenglish.io/vuetify-overflow-button-e1d7fcc7210b?source=collection_archive---------12----------------------->

![](img/31e9b87013a9c38aaa71cbe5fd0899b8.png)

Photo by [Thom Holmes](https://unsplash.com/@thomholmes?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

Vuetify 是一个流行的 Vue 应用程序 UI 框架。

在本文中，我们将了解如何使用 Vuetify 框架。

# 溢出按钮提示

我们可以添加`hint`属性来显示溢出按钮下面的提示。

例如，我们可以写:

```
<template>
  <v-container>
    <v-row>
      <v-col col="12">
        <v-overflow-btn
          class="my-2"
          :items="dropdownItems"
          label="Overflow Btn"
          hint="Select fruit"
          item-value="text"
        ></v-overflow-btn>
      </v-col>
    </v-row>
  </v-container>
</template>
<script>
export default {
  name: "HelloWorld",
  data: () => ({
    dropdownItems: [{ text: "apple" }, { text: "orange" }, { text: "grape" }],
  }),
};
</script>
```

我们有带文本显示的`hint`道具。

# 装载杆

`loading`道具让我们显示溢出按钮底部的装载栏:

```
<template>
  <v-container>
    <v-row>
      <v-col col="12">
        <v-overflow-btn
          class="my-2"
          :items="dropdownItems"
          label="Overflow Btn"
          hint="Select fruit"
          item-value="text"
          loading
        ></v-overflow-btn>
      </v-col>
    </v-row>
  </v-container>
</template>
<script>
export default {
  name: "HelloWorld",
  data: () => ({
    dropdownItems: [{ text: "apple" }, { text: "orange" }, { text: "grape" }],
  }),
};
</script>
```

# 菜单道具

我们可以设置`menu-props`按钮来改变菜单位置:

```
<template>
  <v-container>
    <v-row>
      <v-col col="12">
        <v-overflow-btn
          class="my-2"
          :items="dropdownItems"
          label="Overflow Btn"
          hint="Select fruit"
          item-value="text"
          menu-props="top"
        ></v-overflow-btn>
      </v-col>
    </v-row>
  </v-container>
</template>
<script>
export default {
  name: "HelloWorld",
  data: () => ({
    dropdownItems: [{ text: "apple" }, { text: "orange" }, { text: "grape" }],
  }),
};
</script>
```

我们将它设置为`top`，这样它就可以在顶部打开。

# 只读

我们可以用`readonly`道具让`v-overflow-btn`只读。

它将变为非活动状态，但不会改变任何样式。

例如，我们可以写:

```
<template>
  <v-container>
    <v-row>
      <v-col col="12">
        <v-overflow-btn
          class="my-2"
          :items="dropdownItems"
          label="Overflow Btn"
          hint="Select fruit"
          item-value="text"
          readonly
        ></v-overflow-btn>
      </v-col>
    </v-row>
  </v-container>
</template>
<script>
export default {
  name: "HelloWorld",
  data: () => ({
    dropdownItems: [{ text: "apple" }, { text: "orange" }, { text: "grape" }],
  }),
};
</script>
```

现在我们不能从下拉列表中选择项目。

# 分段按钮

我们可以将`v-overflow-btn`分段。

这意味着它在内容和图标之间有一个额外的分隔线。

例如，我们可以写:

```
<template>
  <v-container>
    <v-row>
      <v-col col="12">
        <v-overflow-btn
          class="my-2"
          :items="dropdownItems"
          label="Overflow Btn"
          hint="Select fruit"
          item-value="text"
          segmented
        ></v-overflow-btn>
      </v-col>
    </v-row>
  </v-container>
</template>
<script>
export default {
  name: "HelloWorld",
  data: () => ({
    dropdownItems: [
      { text: "apple", callback: () => console.log("apple") },
      { text: "orange", callback: () => console.log("orange") },
      { text: "grape", callback: () => console.log("grape") },
    ],
  }),
};
</script>
```

我们为每个条目设置了`callback`属性，这是必需的。

当我们点击条目时，它将被调用。

![](img/65ef1992cf6b998e3f58149d627758cb.png)

Photo by [Glen Noble](https://unsplash.com/@glennoble?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 结论

我们可以添加一个溢出按钮，将下拉菜单添加到我们的 Vuetify 应用程序中。

喜欢这篇文章吗？如果有，通过 [**订阅我们的 YouTube 频道**](https://www.youtube.com/channel/UCtipWUghju290NWcn8jhyAw?sub_confirmation=true) **获取更多类似内容！**