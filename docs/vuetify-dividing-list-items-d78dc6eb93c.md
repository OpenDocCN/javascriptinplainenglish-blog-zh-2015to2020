# Vuetify —划分列表项

> 原文：<https://javascript.plainenglish.io/vuetify-dividing-list-items-d78dc6eb93c?source=collection_archive---------11----------------------->

![](img/4a42aabc7425178a504e4255f3b94c82.png)

Photo by [Kelly Sikkema](https://unsplash.com/@kellysikkema?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

Vuetify 是一个流行的 Vue 应用程序 UI 框架。

在本文中，我们将了解如何使用 Vuetify 框架。

# 分隔线和副标题

我们可以在副标题上添加分隔线。

例如，我们可以写:

```
<template>
  <v-container>
    <v-row>
      <v-col col="12">
        <v-card>
          <v-toolbar color="orange lighten-1" dark>
            <v-app-bar-nav-icon></v-app-bar-nav-icon>
            <v-toolbar-title>Message Board</v-toolbar-title>
            <v-spacer></v-spacer>
            <v-btn icon>
              <v-icon>mdi-magnify</v-icon>
            </v-btn>
          </v-toolbar><v-list two-line>
            <template v-for="(item, index) in items">
              <v-divider v-if="item.divider" :key="index" inset></v-divider>
              <v-list-item v-else :key="item.title" ripple>
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
      { divider: true },
      {
        avatar: "https://picsum.photos/250/300?image=660",
        title: "Meeting",
        subtitle: "subtitle",
      },
      { divider: true },
      {
        avatar: "https://picsum.photos/250/300?image=146",
        title: "So long",
        subtitle: "subtitle",
      },
      { divider: true },
      {
        avatar: "https://picsum.photos/250/300?image=1008",
        title: "Breakfast",
        subtitle: "subtitle",
      },
    ],
  }),
};
</script>
```

我们在`v-divider`和`v-if`指令中动态添加了`v-divider`。

我们用它来划分`v-list-item`。

`v-list-item`有我们要分的物品。

# 纵向视图中的分隔线

此外，我们可以在纵向视图中添加分隔线。

例如，我们可以写:

```
<template>
  <v-container>
    <v-row>
      <v-col col="12">
        <v-card>
          <v-card-title class="cyan darken-1">
            <span class="headline white--text">Sarah Smith</span> <v-spacer></v-spacer> <v-btn dark icon>
              <v-icon>mdi-chevron-left</v-icon>
            </v-btn> <v-btn dark icon>
              <v-icon>mdi-pencil</v-icon>
            </v-btn> <v-btn dark icon>
              <v-icon>mdi-dots-vertical</v-icon>
            </v-btn>
          </v-card-title> <v-list>
            <v-list-item>
              <v-list-item-action>
                <v-icon>mdi-phone</v-icon>
              </v-list-item-action> <v-list-item-content>
                <v-list-item-title>123-456-7890</v-list-item-title>
              </v-list-item-content>
              <v-list-item-action>
                <v-icon>mdi-message-text</v-icon>
              </v-list-item-action>
            </v-list-item> <v-divider inset></v-divider> <v-list-item>
              <v-list-item-action>
                <v-icon>mdi-email</v-icon>
              </v-list-item-action> <v-list-item-content>
                <v-list-item-title>sarah@example.com</v-list-item-title>
              </v-list-item-content>
            </v-list-item> <v-divider inset></v-divider> <v-list-item>
              <v-list-item-action>
                <v-icon>mdi-map-marker</v-icon>
              </v-list-item-action> <v-list-item-content>
                <v-list-item-title>New York</v-list-item-title>
              </v-list-item-content>
            </v-list-item>
          </v-list> <v-img src="http://placekitten.com/600/200" height="200px"></v-img>
        </v-card>
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

我们在有`inset`道具的`v-list-item`之间有`v-divider`。

这将使分隔线显示在文本下方的右侧。

图标下面没有分隔线。

![](img/4a81e20f8bd3507197f5afb41ae31a0e.png)

Photo by [James Lee](https://unsplash.com/@picsbyjameslee?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 结论

我们可以添加分隔条来分隔列表项。

喜欢这篇文章吗？如果有，通过 [**订阅获取更多类似内容解码，我们的 YouTube 频道**](https://www.youtube.com/channel/UCtipWUghju290NWcn8jhyAw?sub_confirmation=true) **！**