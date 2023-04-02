# 验证—嵌入工具栏

> 原文：<https://javascript.plainenglish.io/vuetify-embedding-toolbar-16164eed9341?source=collection_archive---------11----------------------->

![](img/a2d49214ca2117d5ecb82f260780f9ca.png)

Photo by [NeONBRAND](https://unsplash.com/@neonbrand?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

Vuetify 是一个流行的 Vue 应用程序 UI 框架。

在本文中，我们将了解如何使用 Vuetify 框架。

# 扩展工具栏

工具栏可以用`extension`槽扩展:

```
<template>
  <v-container>
    <v-row class="text-center">
      <v-col col="12">
        <v-card color="grey lighten-4" flat height="200px" tile>
          <v-toolbar extended>
            <v-app-bar-nav-icon></v-app-bar-nav-icon> <v-toolbar-title>Title</v-toolbar-title> <v-spacer></v-spacer> <v-btn icon>
              <v-icon>mdi-magnify</v-icon>
            </v-btn> <v-btn icon>
              <v-icon>mdi-heart</v-icon>
            </v-btn> <v-btn icon>
              <v-icon>mdi-dots-vertical</v-icon>
            </v-btn>
          </v-toolbar>
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

# 延伸高度

伸出高度可以用`extension-height`支柱改变:

```
<template>
  <v-container>
    <v-row class="text-center">
      <v-col col="12">
        <v-card color="grey lighten-4" flat height="200px" tile>
          <v-toolbar extended extension-height="150">
            <v-app-bar-nav-icon></v-app-bar-nav-icon> <v-toolbar-title>Title</v-toolbar-title> <v-spacer></v-spacer> <v-btn icon>
              <v-icon>mdi-magnify</v-icon>
            </v-btn> <v-btn icon>
              <v-icon>mdi-heart</v-icon>
            </v-btn> <v-btn icon>
              <v-icon>mdi-dots-vertical</v-icon>
            </v-btn>
          </v-toolbar>
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

# 倒塌

工具栏可以折叠以节省空间。

例如，我们可以写:

```
<template>
  <v-container>
    <v-row class="text-center">
      <v-col col="12">
        <v-card color="grey lighten-4" flat height="200px" tile>
          <v-toolbar collapse>
            <v-btn icon>
              <v-icon>mdi-magnify</v-icon>
            </v-btn> <v-btn icon>
              <v-icon>mdi-dots-vertical</v-icon>
            </v-btn>
          </v-toolbar>
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

用`collapse`道具添加一个折叠的工具栏。

# 灵活的工具栏和卡片工具栏

我们可以制作一个灵活的工具栏，添加到卡片上。

例如，我们可以写:

```
<template>
  <v-container>
    <v-row class="text-center">
      <v-col col="12">
        <v-card flat>
          <v-toolbar color="primary" dark extended flat>
            <v-app-bar-nav-icon></v-app-bar-nav-icon>
          </v-toolbar> <v-card class="mx-auto" max-width="700" style="margin-top: -64px;">
            <v-toolbar flat>
              <v-toolbar-title class="grey--text">Title</v-toolbar-title> <v-spacer></v-spacer> <v-btn icon>
                <v-icon>mdi-magnify</v-icon>
              </v-btn> <v-btn icon>
                <v-icon>mdi-apps</v-icon>
              </v-btn> <v-btn icon>
                <v-icon>mdi-dots-vertical</v-icon>
              </v-btn>
            </v-toolbar> <v-divider></v-divider> <v-card-text style="height: 200px;"></v-card-text>
          </v-card>
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

我们将`v-toolbar`放在一个`v-card`中，以嵌入卡片上的工具栏。

我们在它里面还有另一个`v-card`和另一个`v-toolbar`来显示工具栏内容。

![](img/d0632f5173ba3c63b70f9b3f15b7fa0b.png)

Photo by [SHOP SLO®](https://unsplash.com/@shop_slo?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 结论

我们可以用 Vuetify 给各种容器添加工具栏。

喜欢这篇文章吗？如果有，通过 [**订阅我们的 YouTube 频道**](https://www.youtube.com/channel/UCtipWUghju290NWcn8jhyAw?sub_confirmation=true) **获取更多类似内容！**