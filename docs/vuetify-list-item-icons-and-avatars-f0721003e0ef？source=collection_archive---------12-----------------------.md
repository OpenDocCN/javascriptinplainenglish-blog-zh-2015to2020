# Vuetify —列出项目图标和头像

> 原文：<https://javascript.plainenglish.io/vuetify-list-item-icons-and-avatars-f0721003e0ef?source=collection_archive---------12----------------------->

![](img/b3aa7e11173849de332b0cf0922802dc.png)

Photo by [Sarah Brown](https://unsplash.com/@sweetpagesco?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

Vuetify 是一个流行的 Vue 应用程序 UI 框架。

在本文中，我们将了解如何使用 Vuetify 框架。

# 列出带有标题和动作的头像的项目

我们可以用`avatar`道具添加列表项。

例如，我们可以写:

```
<template>
  <v-row>
    <v-col cols="12">
      <v-card max-width="500" class="mx-auto">
        <v-list>
          <v-list-item v-for="item in items" :key="item.title">
            <v-list-item-icon>
              <v-icon v-if="item.icon" color="pink">mdi-star</v-icon>
            </v-list-item-icon><v-list-item-content>
              <v-list-item-title v-text="item.title"></v-list-item-title>
            </v-list-item-content><v-list-item-avatar>
              <v-img :src="item.avatar"></v-img>
            </v-list-item-avatar>
          </v-list-item>
        </v-list>
      </v-card>
    </v-col>
  </v-row>
</template>
<script>
export default {
  name: "HelloWorld",
  data: () => ({
    items: [
      {
        icon: true,
        title: "Jason Smith",
        avatar: "https://cdn.vuetifyjs.com/images/lists/1.jpg",
      },
      {
        title: "Travis Jones",
        avatar: "https://cdn.vuetifyjs.com/images/lists/2.jpg",
      },
      {
        title: "Ali Simpson",
        avatar: "https://cdn.vuetifyjs.com/images/lists/3.jpg",
      },
    ],
  }),
};
</script>
```

我们有标题为的`v-list-item-title`组件。

还有`v-list-item-avatar`里面有头像图像的`v-img`组件。

`v-list-item-icon`让我们在左侧添加一个图标。

# 双线图标和动作

我们可以添加横跨两行的图标。

例如，我们可以写:

```
<template>
  <v-row>
    <v-col cols="12">
      <v-card max-width="600" class="mx-auto">
        <v-list two-line subheader>
          <v-subheader inset>Folders</v-subheader><v-list-item v-for="item in items" :key="item.title">
            <v-list-item-avatar>
              <v-icon :class="[item.iconClass]" v-text="item.icon"></v-icon>
            </v-list-item-avatar><v-list-item-content>
              <v-list-item-title v-text="item.title"></v-list-item-title>
              <v-list-item-subtitle v-text="item.subtitle"></v-list-item-subtitle>
            </v-list-item-content><v-list-item-action>
              <v-btn icon>
                <v-icon color="grey lighten-1">mdi-information</v-icon>
              </v-btn>
            </v-list-item-action>
          </v-list-item>
        </v-list>
      </v-card>
    </v-col>
  </v-row>
</template>
<script>
export default {
  name: "HelloWorld",
  data: () => ({
    items: [
      {
        icon: "folder",
        iconClass: "grey lighten-1 white--text",
        title: "Photos",
        subtitle: "Jan 9, 2020",
      },
      {
        icon: "folder",
        iconClass: "grey lighten-1 white--text",
        title: "Recipes",
        subtitle: "Jan 17, 2020",
      },
      {
        icon: "folder",
        iconClass: "grey lighten-1 white--text",
        title: "Work",
        subtitle: "Jan 28, 2020",
      },
    ],
  }),
};
</script>
```

我们有带`v-icon`组件的`v-list-item-avatar`组件。

我们用入口上的`iconClass`设置`class`道具。

图标自动横跨两行。

# 有 3 条线的头像

我们可以添加超过 3 行的头像。

例如，我们可以写:

```
<template>
  <v-row>
    <v-col cols="12">
      <v-card max-width="450" class="mx-auto">
        <v-list three-line>
          <template v-for="(item, index) in items">
            <v-subheader v-if="item.header" :key="item.header" v-text="item.header"></v-subheader><v-divider v-else-if="item.divider" :key="index" :inset="item.inset"></v-divider><v-list-item v-else :key="item.title">
              <v-list-item-avatar>
                <v-img :src="item.avatar"></v-img>
              </v-list-item-avatar><v-list-item-content>
                <v-list-item-title v-html="item.title"></v-list-item-title>
                <v-list-item-subtitle v-html="item.subtitle"></v-list-item-subtitle>
              </v-list-item-content>
            </v-list-item>
          </template>
        </v-list>
      </v-card>
    </v-col>
  </v-row>
</template>
<script>
export default {
  name: "HelloWorld",
  data: () => ({
    items: [
      { header: "Today" },
      {
        avatar: "https://cdn.vuetifyjs.com/images/lists/1.jpg",
        title: "Brunch this weekend?",
        subtitle:
          "<span class='text--primary'>Lorem ipsum dolor sit amet, consectetur adipiscing elit. Phasellus efficitur tellus sit amet purus ullamcorper, ac vulputate lectus imperdiet.</span>",
      },
      {
        avatar: "https://cdn.vuetifyjs.com/images/lists/2.jpg",
        title: 'Summer BBQ <span class="grey--text text--lighten-1">4</span>',
        subtitle:
          "<span class='text--primary'>Lorem ipsum dolor sit amet, consectetur adipiscing elit. Phasellus efficitur tellus sit amet purus ullamcorper, ac vulputate lectus imperdiet.</span>",
      },
    ],
  }),
};
</script>
```

我们有`three-line`道具来让列表项显示 3 行。

# 结论

我们可以用各种图标添加列表项，用 Vuetify 添加其他内容。

喜欢这篇文章吗？如果有，通过 [**订阅我们的 YouTube 频道**](https://www.youtube.com/channel/UCtipWUghju290NWcn8jhyAw?sub_confirmation=true) **获取更多类似内容！**