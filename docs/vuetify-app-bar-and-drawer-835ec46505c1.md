# 虚拟化—应用程序栏和抽屉

> 原文：<https://javascript.plainenglish.io/vuetify-app-bar-and-drawer-835ec46505c1?source=collection_archive---------9----------------------->

![](img/fdab7099e9164486fbeb21f94ca17016.png)

Photo by [Maksym Kaharlytskyi](https://unsplash.com/@qwitka?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

Vuetify 是一个流行的 Vue 应用程序 UI 框架。

在本文中，我们将了解如何使用 Vuetify 框架。

# 切换导航抽屉

我们可以通过使用`v-app-bar-nav-icon`组件来切换导航抽屉。

例如，我们可以写:

```
<template>
  <v-container>
    <v-row class="text-center">
      <v-col col="12">
        <v-card class="mx-auto overflow-hidden" height="400">
          <v-app-bar color="deep-purple" dark>
            <v-app-bar-nav-icon @click="drawer = true"></v-app-bar-nav-icon> <v-toolbar-title>Title</v-toolbar-title>
          </v-app-bar> <v-navigation-drawer v-model="drawer" absolute temporary>
            <v-list nav dense>
              <v-list-item-group v-model="group" active-class="deep-purple--text text--accent-4">
                <v-list-item>
                  <v-list-item-icon>
                    <v-icon>mdi-home</v-icon>
                  </v-list-item-icon>
                  <v-list-item-title>Home</v-list-item-title>
                </v-list-item><v-list-item>
                  <v-list-item-icon>
                    <v-icon>mdi-account</v-icon>
                  </v-list-item-icon>
                  <v-list-item-title>Account</v-list-item-title>
                </v-list-item>
              </v-list-item-group>
            </v-list>
          </v-navigation-drawer>
        </v-card>
      </v-col>
    </v-row>
  </v-container>
</template><script>
export default {
  name: "HelloWorld",
  data: () => ({
    drawer: false
  }),
};
</script>
```

我们用`v-navigation-drawer`在左侧显示一个菜单。

何时显示取决于`drawer`州。

如果是`true`，那么就会显示出来。

当我们点击`v-app-ba-nav-icon`时，我们使它成为`true`。

# 滚动阈值

我们可以在`v-app-bar`上设置滚动阈值。

例如，我们可以写:

```
<template>
  <v-container>
    <v-row class="text-center">
      <v-col col="12">
        <v-card class="overflow-hidden">
          <v-app-bar
            absolute
            color="#43a047"
            dark
            shrink-on-scroll
            prominent
            src="https://picsum.photos/1920/1080?random"
            fade-img-on-scroll
            scroll-target="#scrolling"
            scroll-threshold="500"
          >
            <template v-slot:img="{ props }">
              <v-img v-bind="props" gradient="to top right, rgba(55,236,186,.7), lightblue"></v-img>
            </template> <v-app-bar-nav-icon></v-app-bar-nav-icon> <v-toolbar-title>Title</v-toolbar-title> <v-spacer></v-spacer> <v-btn icon>
              <v-icon>mdi-magnify</v-icon>
            </v-btn> <v-btn icon>
              <v-icon>mdi-heart</v-icon>
            </v-btn> <v-btn icon>
              <v-icon>mdi-dots-vertical</v-icon>
            </v-btn>
          </v-app-bar>
          <v-sheet id="scrolling" class="overflow-y-auto" max-height="600">
            <v-container style="height: 1500px;"></v-container>
          </v-sheet>
        </v-card>
      </v-col>
    </v-row>
  </v-container>
</template><script>
export default {
  name: "HelloWorld",
  data: () => ({
    drawer: false,
  }),
};
</script>
```

我们添加了`scroll-threshold`道具来使工具栏从一个带有背景图像的高工具栏变成一个没有图像的纯色背景工具栏。

`fade-img-on-scroll`道具让我们在滚动时让应用程序栏上的图像消失。

此外，我们必须将`scroll-target`设置为我们正在观察滚动的 div 的选择器。

![](img/6d2639ba81083e1994e5cd9270b4f15e.png)

Photo by [chuttersnap](https://unsplash.com/@chuttersnap?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 结论

我们可以在应用程序栏中添加一个导航抽屉。

此外，我们可以让应用程序栏在滚动时以不同的方式显示。

喜欢这篇文章吗？如果有，通过 [**订阅解码，我们的 YouTube 频道**](https://www.youtube.com/channel/UCtipWUghju290NWcn8jhyAw?sub_confirmation=true) **获取更多类似内容！**