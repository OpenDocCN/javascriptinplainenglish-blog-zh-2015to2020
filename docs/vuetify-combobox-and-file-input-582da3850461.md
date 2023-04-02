# Vuetify —组合框和文件输入

> 原文：<https://javascript.plainenglish.io/vuetify-combobox-and-file-input-582da3850461?source=collection_archive---------8----------------------->

![](img/f3e17af269ea5419735256f2256318b2.png)

Photo by [Viktor Talashuk](https://unsplash.com/@viktortalashuk?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

Vuetify 是一个流行的 Vue 应用程序 UI 框架。

在本文中，我们将了解如何使用 Vuetify 框架。

# 带芯片的组合框无数据

我们可以添加一个没有数据的组合框。

为此，我们可以填充`no-data`槽，以便在搜索或创建项目时向用户提供上下文。

例如，我们可以写:

```
<template>
  <v-container>
    <v-row>
      <v-col col="12">
        <v-combobox
          v-model="model"
          :items="items"
          :search-input.sync="search"
          hide-selected
          hint="Maximum of 5 tags"
          label="Add some tags"
          multiple
          persistent-hint
          small-chips
        >
          <template v-slot:no-data>
            <v-list-item>
              <v-list-item-content>
                <v-list-item-title>No results matching {{ search }}</v-list-item-title>
              </v-list-item-content>
            </v-list-item>
          </template>
        </v-combobox>
      </v-col>
    </v-row>
  </v-container>
</template>
<script>
export default {
  name: "HelloWorld",
  data: () => ({
    items: ["Programming", "Vue", "Vuetify"],
    model: ["Vuetify"],
    search: null,
  }), watch: {
    model(val) {
      if (val.length > 5) {
        this.$nextTick(() => this.model.pop());
      }
    },
  },
};
</script>
```

我们添加了`no-data`并用文本填充它，以便在没有匹配时显示。

# 文件输入

我们可以用`v-file-input`组件添加一个文件输入。

例如，我们可以写:

```
<template>
  <v-container>
    <v-row>
      <v-col col="12">
        <v-file-input multiple label="File input"></v-file-input>
      </v-col>
    </v-row>
  </v-container>
</template>
<script>
export default {
  name: "HelloWorld",
};
</script>
```

添加文件输入。

`multiple`道具允许多文件选择。

# 文件输入接受格式

我们可以用`accept`属性设置文件输入接受的文件格式:

```
<template>
  <v-container>
    <v-row>
      <v-col col="12">
        <v-file-input accept="image/*" label="File input"></v-file-input>
      </v-col>
    </v-row>
  </v-container>
</template>
<script>
export default {
  name: "HelloWorld",
};
</script>
```

# 用芯片输入文件

我们可以用芯片添加文件输入。

所以我们可以写:

```
<template>
  <v-container>
    <v-row>
      <v-col col="12">
        <v-file-input chips multiple label="File input with chips"></v-file-input>
        <v-file-input small-chips multiple label="File input with small chips"></v-file-input>
      </v-col>
    </v-row>
  </v-container>
</template>
<script>
export default {
  name: "HelloWorld",
};
</script>
```

我们有带`v-file-input`的`chips`和`small-chips`道具。

# 尺寸显示

我们可以用`show-size`属性添加一个文件大小显示。

例如，我们可以写:

```
<template>
  <v-container>
    <v-row>
      <v-col col="12">
        <v-file-input show-size multiple label="File input"></v-file-input>
      </v-col>
    </v-row>
  </v-container>
</template>
<script>
export default {
  name: "HelloWorld",
};
</script>
```

我们有`show-size`属性，所以将显示大小。

# 计数器

我们可以添加一个带有`show-size`属性的`counter`属性来显示文件的总数和大小:

```
<template>
  <v-container>
    <v-row>
      <v-col col="12">
        <v-file-input show-size counter multiple label="File input"></v-file-input>
      </v-col>
    </v-row>
  </v-container>
</template>
<script>
export default {
  name: "HelloWorld",
};
</script>
```

![](img/f47b52cfb08997e94a84159345c15482.png)

Photo by [Omid Kashmari](https://unsplash.com/@omidkashmari?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 结论

我们可以添加包含各种内容的组合框。

此外，我们可以添加带有大小和计数器的文件输入。

喜欢这篇文章吗？如果有，通过 [**订阅我们的 YouTube 频道**](https://www.youtube.com/channel/UCtipWUghju290NWcn8jhyAw?sub_confirmation=true) **获取更多类似内容！**