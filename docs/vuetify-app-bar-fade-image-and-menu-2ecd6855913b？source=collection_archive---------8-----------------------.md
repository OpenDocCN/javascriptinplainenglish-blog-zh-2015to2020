# Vuetify —应用程序栏淡入淡出图像和菜单

> 原文：<https://javascript.plainenglish.io/vuetify-app-bar-fade-image-and-menu-2ecd6855913b?source=collection_archive---------8----------------------->

![](img/fed0ad42e1e9f597319a97ae8d3ab7ad.png)

Photo by [Louis Reed](https://unsplash.com/@_louisreed?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

Vuetify 是一个流行的 Vue 应用程序 UI 框架。

在本文中，我们将了解如何使用 Vuetify 框架。

# 突出的应用程序栏，带有滚动收缩和图像，滚动时淡入淡出

我们可以将`fade-img-on-scroll`道具添加到`v-app-bar`中，使我们的应用程序栏的背景图像在滚动时消失。

例如，我们可以写:

```
<template>
  <v-container>
    <v-row class="text-center">
      <v-col col="12">
        <v-card class="overflow-hidden">
          <v-app-bar
            absolute
            color="green"
            dark
            shrink-on-scroll
            prominent
            src="https://picsum.photos/1920/1080?random"
            fade-img-on-scroll
            scroll-target="#scrolling"
          >
            <template v-slot:img="{ props }">
              <v-img
                v-bind="props"
                gradient="to top right, rgba(100,115,201,.7), rgba(25,32,72,.7)"
              ></v-img>
            </template> <v-app-bar-nav-icon></v-app-bar-nav-icon> <v-toolbar-title>Title</v-toolbar-title> <v-spacer></v-spacer> <v-btn icon>
              <v-icon>mdi-magnify</v-icon>
            </v-btn> <v-btn icon>
              <v-icon>mdi-heart</v-icon>
            </v-btn> <v-btn icon>
              <v-icon>mdi-dots-vertical</v-icon>
            </v-btn> <template v-slot:extension>
              <v-tabs align-with-title>
                <v-tab>Tab 1</v-tab>
                <v-tab>Tab 2</v-tab>
                <v-tab>Tab 3</v-tab>
              </v-tabs>
            </template>
          </v-app-bar>
          <v-sheet id="scrolling" class="overflow-y-auto" max-height="600">
            <v-container style="height: 1000px;"></v-container>
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

我们有一个`v-app-bar`和一个`fade-img-on-scroll`道具，当我们滚动时可以让图像消失。

# 带菜单的应用程序栏

我们可以添加一个带有菜单的应用程序栏，菜单上有`VMenu`组件。

例如，我们可以写:

```
<template>
  <v-container>
    <v-row class="text-center">
      <v-col col="12">
        <v-card class="overflow-hidden">
          <v-app-bar
            absolute
            color="#6A76AB"
            dark
            shrink-on-scroll
            prominent
            src="https://picsum.photos/1920/1080?random"
            fade-img-on-scroll
            scroll-target="#scrolling"
          >
            <template v-slot:img="{ props }">
              <v-img
                v-bind="props"
                gradient="to top right, rgba(100,115,201,.7), rgba(25,32,72,.7)"
              ></v-img>
            </template> <v-app-bar-nav-icon></v-app-bar-nav-icon> <v-toolbar-title>Title</v-toolbar-title> <v-spacer></v-spacer> <v-btn icon>
              <v-icon>mdi-magnify</v-icon>
            </v-btn> <v-btn icon>
              <v-icon>mdi-heart</v-icon>
            </v-btn> <v-menu bottom left>
              <template v-slot:activator="{ on, attrs }">
                <v-btn icon color="yellow" v-bind="attrs" v-on="on">
                  <v-icon>mdi-dots-vertical</v-icon>
                </v-btn>
              </template><v-list>
                <v-list-item v-for="i in 3" :key="i">
                  <v-list-item-title>item {{ i }}</v-list-item-title>
                </v-list-item>
              </v-list>
            </v-menu> <template v-slot:extension>
              <v-tabs align-with-title>
                <v-tab>1</v-tab>
                <v-tab>2</v-tab>
                <v-tab>3</v-tab>
              </v-tabs>
            </template>
          </v-app-bar>
          <v-sheet id="scrolling" class="overflow-y-auto" max-height="600">
            <v-container style="height: 1000px;"></v-container>
          </v-sheet>
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

我们有一个`v-app-bar`和一个`v-menu`不同的项目。

物品在`v-list`的`v-list-item`组件中。

菜单文本在`v-list-item-title`文本中。

![](img/57679c4fb318046fe6582ada704f7390.png)

Photo by [Alex Iby](https://unsplash.com/@alexiby?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 结论

我们可以改变应用程序，以淡化滚动图像，并添加一个菜单与 Vuetify。

喜欢这篇文章吗？如果有，通过 [**订阅解码，我们的 YouTube 频道**](https://www.youtube.com/channel/UCtipWUghju290NWcn8jhyAw?sub_confirmation=true) **获取更多类似内容！**