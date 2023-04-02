# 区分——分隔线

> 原文：<https://javascript.plainenglish.io/vuetify-dividers-2f465a915d17?source=collection_archive---------9----------------------->

![](img/e13e8277b790a67b09a6f6a9a255e303.png)

Photo by [Mike Petrucci](https://unsplash.com/@mikepetrucci?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

Vuetify 是一个流行的 Vue 应用程序 UI 框架。

在本文中，我们将了解如何使用 Vuetify 框架。

# 圆规

我们可以使用分隔线来划分内容。

例如，我们可以写:

```
<template>
  <v-container>
    <v-row>
      <v-col col="12">
        <v-card>
          <v-list two-line>
            <template v-for="(item, index) in items.slice(0, 6)">
              <v-divider v-if="item.divider" :key="index" :inset="item.inset"></v-divider>
              <v-list-item v-else :key="index">
                <v-list-item-avatar>
                  <img :src="item.avatar" />
                </v-list-item-avatar>
                <v-list-item-content>
                  <v-list-item-title v-html="item.title"></v-list-item-title>
                  <v-list-item-subtitle v-html="item.subtitle"></v-list-item-subtitle>
                </v-list-item-content>
              </v-list-item>
            </template>
          </v-list>
        </v-card>
      </v-col>
    </v-row>
  </v-container>
</template><script>
export default {
  name: "HelloWorld",
  data: () => ({
    items: [
      {
        avatar: "https://cdn.vuetifyjs.com/images/lists/1.jpg",
        title: "title",
        subtitle: "subtitle",
      },
      { divider: true, inset: true },
      {
        avatar: "https://cdn.vuetifyjs.com/images/lists/2.jpg",
        title: "title",
        subtitle: "subtitle",
      },
      { divider: true, inset: true },
      {
        avatar: "https://cdn.vuetifyjs.com/images/lists/3.jpg",
        title: "title",
        subtitle: "subtitle",
      },
    ],
  }),
};
</script>
```

我们添加了`v-divider`组件来用分隔线划分行。

与`v-list-item`齐平。

# 垂直分隔线

我们可以用`vertical`道具添加垂直分隔线。

例如，我们可以写:

```
<template>
  <v-container>
    <v-row>
      <v-col col="12">
        <v-toolbar color="purple" dark>
          <v-toolbar-title>Title</v-toolbar-title>
          <v-divider class="mx-4" vertical></v-divider>
          <span class="subheading">My Home</span>
          <v-spacer></v-spacer>
          <v-toolbar-items class="hidden-sm-and-down">
            <v-btn text>News</v-btn>
            <v-divider vertical></v-divider>
            <v-btn text>Blog</v-btn>
            <v-divider vertical></v-divider>
            <v-btn text>Music</v-btn>
            <v-divider vertical></v-divider>
          </v-toolbar-items>
          <v-app-bar-nav-icon></v-app-bar-nav-icon>
        </v-toolbar>
      </v-col>
    </v-row>
  </v-container>
</template><script>
export default {
  name: "HelloWorld",
  data: () => ({}),
};
</script>
```

我们有带`vertical`支柱的`v-divider`来添加垂直分隔器。

还和工具栏齐平。

# 垂直插入分隔线

我们可以使用`inset`道具来增加分隔线的边距:

```
<template>
  <v-container>
    <v-row>
      <v-col col="12">
        <v-toolbar color="purple" dark>
          <v-toolbar-title>Title</v-toolbar-title>
          <v-divider class="mx-4" vertical inset></v-divider>
          <span class="subheading">My Home</span>
          <v-spacer></v-spacer>
          <v-toolbar-items class="hidden-sm-and-down">
            <v-btn text>News</v-btn>
            <v-divider vertical inset></v-divider>
            <v-btn text>Blog</v-btn>
            <v-divider vertical inset></v-divider>
            <v-btn text>Music</v-btn>
            <v-divider vertical inset></v-divider>
          </v-toolbar-items>
          <v-app-bar-nav-icon></v-app-bar-nav-icon>
        </v-toolbar>
      </v-col>
    </v-row>
  </v-container>
</template><script>
export default {
  name: "HelloWorld",
  data: () => ({}),
};
</script>
```

![](img/e24ad460cf512d422c6e6da53cadbc1d.png)

Photo by [Gerardo Marrufo](https://unsplash.com/@sirmarrufo?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 结论

我们可以为菜单项和行添加分隔条。

喜欢这篇文章吗？如果有，通过 [**订阅获取更多类似内容解码，我们的 YouTube 频道**](https://www.youtube.com/channel/UCtipWUghju290NWcn8jhyAw?sub_confirmation=true) **！**