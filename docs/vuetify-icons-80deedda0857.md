# 虚拟化—图标

> 原文：<https://javascript.plainenglish.io/vuetify-icons-80deedda0857?source=collection_archive---------10----------------------->

![](img/7b596d3f2a123a2e55f2ebcc1637e9e6.png)

Photo by [Brett Jordan](https://unsplash.com/@brett_jordan?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

Vuetify 是一个流行的 Vue 应用程序 UI 框架。

在本文中，我们将了解如何使用 Vuetify 框架。

# 材料设计图标

Vuetify 有很多图标。

我们可以通过运行以下命令来安装材料设计图标:

```
npm install material-design-icons-iconfont -D
```

然后在`src/plugins/vuetify.js`中，我们添加:

```
import 'material-design-icons-iconfont/dist/material-design-icons.css'
```

然后我们可以用`v-icon`组件添加它们:

```
<template>
  <v-container class="grey lighten-5">
    <v-row>
      <v-col>
        <v-container fluid>
          <v-row justify="space-around" class="mb-2">
            <span class="group pa-2">
              <v-icon>home</v-icon>
              <v-icon>event</v-icon>
              <v-icon>info</v-icon>
            </span>
          </v-row>
        </v-container>
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

# 字体超赞图标

我们可以通过运行以下命令来安装字体很棒的图标:

```
npm install @fortawesome/fontawesome-free -D
```

然后在`src/plugins/vuetify.js`中，我们加上:

```
import '@fortawesome/fontawesome-free/css/all.css'
```

所以我们可以使用图标。

然后我们可以通过写来使用它:

```
<template>
  <v-container class="grey lighten-5">
    <v-row>
      <v-col>
        <v-row align="center" justify="space-around">
          <v-icon>fas fa-lock</v-icon>
          <v-icon>fas fa-search</v-icon>
          <v-icon>fas fa-list</v-icon>
          <v-icon>fas fa-edit</v-icon>
          <v-icon>fas fa-tachometer-alt</v-icon>
          <v-icon>fas fa-circle-notch fa-spin</v-icon>
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

# 按钮图标

我们可以给按钮添加图标。

例如，我们可以写:

```
<template>
  <v-container class="grey lighten-5">
    <v-row>
      <v-col>
        <div>
          <v-btn class="ma-2" color="primary" dark>
            Accept
            <v-icon dark right>mdi-checkbox-marked-circle</v-icon>
          </v-btn> <v-btn class="ma-2" color="red" dark>
            Decline
            <v-icon dark right>mdi-cancel</v-icon>
          </v-btn> <v-btn class="ma-2" dark>
            <v-icon dark left>mdi-minus_circle</v-icon>Cancel
          </v-btn>
        </div>
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

在图标文本旁边添加图标。

# 可点击的

我们可以用`v-icon`绑定到任何点击事件。

例如，我们可以写:

```
<template>
  <v-container class="grey lighten-5">
    <v-row>
      <v-col>
        <v-card>
          <v-toolbar color="pink" dark dense flat>
            <v-toolbar-title class="body-2">Upcoming Changes</v-toolbar-title>
          </v-toolbar>
          <v-card-text>Lorem ipsum.</v-card-text> <v-card-actions>
            <v-spacer></v-spacer>
            <v-icon large @click="next">mdi-chevron-right</v-icon>
          </v-card-actions>
        </v-card>
      </v-col>
    </v-row>
  </v-container>
</template>
<script>
export default {
  name: "HelloWorld",
  data: () => ({}),
  methods: {
    next() {
      alert("You clicked next");
    },
  },
};
</script>
```

我们将`@click`指令设置为`next`，这样当我们点击图标时就会调用`next`方法。

# 结论

我们可以添加材料设计和字体真棒图标到我们的应用程序。

此外，Vuetifyt 带有内置图标。

喜欢这篇文章吗？如果有，通过 [**订阅我们的 YouTube 频道**](https://www.youtube.com/channel/UCtipWUghju290NWcn8jhyAw?sub_confirmation=true) **获取更多类似内容！**