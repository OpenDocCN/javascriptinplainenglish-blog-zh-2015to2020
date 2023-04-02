# 虚拟化—卡片样式

> 原文：<https://javascript.plainenglish.io/vuetify-card-styles-9edbc53144a6?source=collection_archive---------3----------------------->

![](img/9d3197cef200fc61532bbe50df670a99.png)

Photo by [Amanda Vick](https://unsplash.com/@amandavickcreative?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

Vuetify 是一个流行的 Vue 应用程序 UI 框架。

在本文中，我们将了解如何使用 Vuetify 框架。

# 水平卡片

我们可以制作水平卡片。

例如，我们可以写:

```
<template>
  <v-container>
    <v-row>
      <v-col col="12">
        <v-card max-width="400" class="mx-auto">
          <v-container>
            <v-row dense>
              <v-col v-for="(item, i) in items" :key="i" cols="12">
                <v-card :color="item.color" dark>
                  <div class="d-flex flex-no-wrap justify-space-between">
                    <div>
                      <v-card-title class="headline" v-text="item.title"></v-card-title>
                      <v-card-subtitle v-text="item.artist"></v-card-subtitle>
                    </div>
                    <v-avatar class="ma-3" size="125" tile>
                      <v-img :src="item.src"></v-img>
                    </v-avatar>
                  </div>
                </v-card>
              </v-col>
            </v-row>
          </v-container>
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
        color: "#1F7087",
        src: "http://placekitten.com/200/200",
        title: "Cat",
        artist: "James Smith",
      },
      {
        color: "#952175",
        src: "http://placekitten.com/200/200",
        title: "Cat",
        artist: "Ellie Jones",
      },
    ],
  }),
};
</script>
```

我们通过在循环中呈现`v-card`组件来创建卡片。

`v-card`在`v-col`里。

在卡片里面，我们有`v-card-title`和`v-card-subtitle`来显示标题和副标题。

右边是`v-avatar`。

# 自定义操作

我们可以在卡片上添加自定义操作。

例如，我们可以写:

```
<template>
  <v-container>
    <v-row>
      <v-col col="12">
        <v-card class="mx-auto" max-width="344">
          <v-img src="https://cdn.vuetifyjs.com/images/cards/sunshine.jpg" height="200px"></v-img>
          <v-card-title>Sunshine</v-card-title>
          <v-card-subtitle>Lorem ipsum</v-card-subtitle> <v-card-actions>
            <v-btn text>Share</v-btn>
            <v-btn color="purple" text>Explore</v-btn>
            <v-spacer></v-spacer>
            <v-btn icon @click="show = !show">
              <v-icon>{{ show ? 'mdi-chevron-up' : 'mdi-chevron-down' }}</v-icon>
            </v-btn>
          </v-card-actions> <v-expand-transition>
            <div v-show="show">
              <v-divider></v-divider>
              <v-card-text>Lorem ipsum.</v-card-text>
            </div>
          </v-expand-transition>
        </v-card>
      </v-col>
    </v-row>
  </v-container>
</template><script>
export default {
  name: "HelloWorld",
  data: () => ({
    show: false,
  }),
};
</script>
```

我们添加了`v-card-actions`组件来显示动作。

在它里面，我们有一些按钮让我们打开和关闭底部窗格。

底部窗格在`v-expand-transition`组件中，因此我们用转换来切换它。

# 推特卡

我们可以通过改变卡片的样式来添加 Twitter 卡片，使其看起来像 Twitter UI，并添加一个 Twitter 图标。

例如，我们可以写:

```
<template>
  <v-container>
    <v-row>
      <v-col col="12">
        <v-card class="mx-auto" color="#26c6da" dark max-width="400">
          <v-card-title>
            <v-icon large left>mdi-twitter</v-icon>
            <span class="title font-weight-light">Twitter</span>
          </v-card-title><v-card-text class="headline font-weight-bold">"hello world."</v-card-text> <v-card-actions>
            <v-list-item class="grow">
              <v-list-item-avatar color="grey darken-3">
                <v-img
                  class="elevation-6"
                  src="https://avataaars.io/?avatarStyle=Transparent&topType=ShortHairShortCurly&accessoriesType=Prescription02&hairColor=Black&facialHairType=Blank&clotheType=Hoodie&clotheColor=White&eyeType=Default&eyebrowType=DefaultNatural&mouthType=Default&skinColor=Light"
                ></v-img>
              </v-list-item-avatar> <v-list-item-content>
                <v-list-item-title>James Smith</v-list-item-title>
              </v-list-item-content> <v-row align="center" justify="end">
                <v-icon class="mr-1">mdi-heart</v-icon>
                <span class="subheading mr-2">100</span>
                <span class="mr-1">·</span>
                <v-icon class="mr-1">mdi-share-variant</v-icon>
                <span class="subheading">100</span>
              </v-row>
            </v-list-item>
          </v-card-actions>
        </v-card>
      </v-col>
    </v-row>
  </v-container>
</template><script>
export default {
  name: "HelloWorld",
  data: () => ({
    show: false,
  }),
};
</script>
```

添加一张 Twitter 风格的卡片。

我们有`mdi-twitter`图标。

而`v-card-text`有推文。

`v-card-actions`组件有用户的头像和名字。

还添加了赞和转发的图标。

![](img/e2fd6e885ce3b273f0b36aad4bf55594.png)

Photo by [tu tu](https://unsplash.com/@tutuwords?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 结论

我们可以用 Vuetify 添加一个 Twitter 风格的横卡。

喜欢这篇文章吗？如果有，通过 [**订阅我们的 YouTube 频道**](https://www.youtube.com/channel/UCtipWUghju290NWcn8jhyAw?sub_confirmation=true) **获取更多类似内容！**