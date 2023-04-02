# 虚拟化—卡片

> 原文：<https://javascript.plainenglish.io/vuetify-cards-fe5bbb7f7d5b?source=collection_archive---------4----------------------->

![](img/712c64622a1cf468ab78eff1abba407b.png)

Photo by [Joey Huang](https://unsplash.com/@onice?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

Vuetify 是一个流行的 Vue 应用程序 UI 框架。

在本文中，我们将了解如何使用 Vuetify 框架。

# 卡片

卡片是内容的容器。

我们可以用`v-card`组件添加一个

例如，我们可以写:

```
<template>
  <v-container>
    <v-row>
      <v-col col="12">
        <v-card class="mx-auto" max-width="344" outlined>
          <v-list-item three-line>
            <v-list-item-content>
              <div class="overline mb-4">Foo</div>
              <v-list-item-title class="headline mb-1">Headline</v-list-item-title>
              <v-list-item-subtitle>Lorem ipsum</v-list-item-subtitle>
            </v-list-item-content> <v-list-item-avatar tile size="80" color="grey"></v-list-item-avatar>
          </v-list-item> <v-card-actions>
            <v-btn text>Button</v-btn>
            <v-btn text>Button</v-btn>
          </v-card-actions>
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

我们为内容添加了带有`v-list-item`的`v-card`。

`three-line`使其显示 3 行。

`v-list-item-avatar`有神通。

`v-card-action`有卡片的操作按钮。

我们可以用它来包装内容。

例如，我们可以写:

```
<template>
  <v-container>
    <v-row>
      <v-col col="12">
        <v-card class="d-inline-block mx-auto">
          <v-container>
            <v-row justify="space-between">
              <v-col cols="auto">
                <v-img
                  height="200"
                  width="200"
                  src="https://placekitten.com/200/200"
                ></v-img>
              </v-col> <v-col cols="auto" class="text-center pl-0">
                <v-row class="flex-column ma-0 fill-height" justify="center">
                  <v-col class="px-0">
                    <v-btn icon>
                      <v-icon>mdi-heart</v-icon>
                    </v-btn>
                  </v-col> <v-col class="px-0">
                    <v-btn icon>
                      <v-icon>mdi-bookmark</v-icon>
                    </v-btn>
                  </v-col> <v-col class="px-0">
                    <v-btn icon>
                      <v-icon>mdi-share-variant</v-icon>
                    </v-btn>
                  </v-col>
                </v-row>
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
  data: () => ({}),
};
</script>
```

我们用`v-img`在左侧显示一幅图像。

`v-img`在`v-col`里面

我们打开了`v-col`组件，在右侧显示一些按钮。

# 信息卡

我们可以用`v-card-text`组件在卡片中添加各种文本。

例如，我们可以写:

```
<template>
  <v-container>
    <v-row>
      <v-col col="12">
        <v-card class="mx-auto" max-width="344">
          <v-card-text>
            <div>Lorem ipsum</div>
            <p class="display-1 text--primary">Lorem ipsum</p>
            <p>Lorem ipsum</p>
            <div class="text--primary">Lorem ipsum</div>
          </v-card-text>
          <v-card-actions>
            <v-btn text color="deep-purple accent-4">Learn More</v-btn>
          </v-card-actions>
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

我们有`v-card-text`组件来显示一些适合`v-card`的文本。

![](img/9d987ea24face89c483b32a9bc1ddc9e.png)

Photo by [Denise van der Heide](https://unsplash.com/@deniezie?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 结论

我们可以添加各种内容的卡片。

喜欢这篇文章吗？如果有，通过 [**订阅我们的 YouTube 频道**](https://www.youtube.com/channel/UCtipWUghju290NWcn8jhyAw?sub_confirmation=true) **获取更多类似内容！**