# 虚拟化—幻灯片组

> 原文：<https://javascript.plainenglish.io/vuetify-slide-group-32a6c29d9546?source=collection_archive---------3----------------------->

![](img/c7b0776c4f50639f0fe4a62c3e2037a5.png)

Photo by [National Cancer Institute](https://unsplash.com/@nci?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

Vuetify 是一个流行的 Vue 应用程序 UI 框架。

在本文中，我们将了解如何使用 Vuetify 框架。

# 幻灯片项目活动类

我们可以使用`active-class`道具更改幻灯片项目的活动类别:

```
<template>
  <v-container class="grey lighten-5">
    <v-row>
      <v-col>
        <v-sheet class="mx-auto" elevation="8" max-width="800">
          <v-slide-group
            v-model="model"
            class="pa-4"
            prev-icon="mdi-minus"
            next-icon="mdi-plus"
            show-arrows
            active-class="success"
          >
            <v-slide-item v-for="n in 15" :key="n" v-slot:default="{ active, toggle }">
              <v-card
                :color="active ? 'primary' : 'grey lighten-1'"
                class="ma-4"
                height="200"
                width="100"
                [@click](http://twitter.com/click)="toggle"
              >
                <v-row class="fill-height" align="center" justify="center">
                  <v-scale-transition>
                    <v-icon
                      v-if="active"
                      color="white"
                      size="48"
                      v-text="'mdi-close-circle-outline'"
                    ></v-icon>
                  </v-scale-transition>
                </v-row>
              </v-card>
            </v-slide-item>
          </v-slide-group>
        </v-sheet>
      </v-col>
    </v-row>
  </v-container>
</template>
<script>
export default {
  name: "HelloWorld",
  data: () => ({
    model: undefined,
  }),
};
</script>
```

`show-arrows`支柱使导航箭头显示在两个 sies 上。

此外，我们有`active-class`道具，以不同的风格选择项目。

# 命令的

我们可以使用`mandatory`道具在组中选择至少一个项目:

```
<template>
  <v-container class="grey lighten-5">
    <v-row>
      <v-col>
        <v-sheet class="mx-auto" elevation="8" max-width="800">
          <v-slide-group
            v-model="model"
            class="pa-4"
            prev-icon="mdi-minus"
            next-icon="mdi-plus"
            show-arrows
            mandatory
          >
            <v-slide-item v-for="n in 15" :key="n" v-slot:default="{ active, toggle }">
              <v-card
                :color="active ? 'primary' : 'grey lighten-1'"
                class="ma-4"
                height="200"
                width="100"
                @click="toggle"
              >
                <v-row class="fill-height" align="center" justify="center">
                  <v-scale-transition>
                    <v-icon
                      v-if="active"
                      color="white"
                      size="48"
                      v-text="'mdi-close-circle-outline'"
                    ></v-icon>
                  </v-scale-transition>
                </v-row>
              </v-card>
            </v-slide-item>
          </v-slide-group>
        </v-sheet>
      </v-col>
    </v-row>
  </v-container>
</template>
<script>
export default {
  name: "HelloWorld",
  data: () => ({
    model: undefined,
  }),
};
</script>
```

现在第一个项目将被默认选中。

# 伪旋转木马

我们可以在所选幻灯片下方显示内容。

例如，我们可以写:

```
<template>
  <v-container class="grey lighten-5">
    <v-row>
      <v-col>
        <v-sheet class="mx-auto" elevation="8" max-width="800">
          <v-slide-group v-model="model" class="pa-4" show-arrows>
            <v-slide-item v-for="n in 15" :key="n" v-slot:default="{ active, toggle }">
              <v-card
                :color="active ? 'primary' : 'grey lighten-1'"
                class="ma-4"
                height="200"
                width="100"
                @click="toggle"
              >
                <v-row class="fill-height" align="center" justify="center">
                  <v-scale-transition>
                    <v-icon
                      v-if="active"
                      color="white"
                      size="48"
                      v-text="'mdi-close-circle-outline'"
                    ></v-icon>
                  </v-scale-transition>
                </v-row>
              </v-card>
            </v-slide-item>
          </v-slide-group> <v-expand-transition>
            <v-sheet v-if="model != null" color="grey lighten-4" height="200" tile>
              <v-row class="fill-height" align="center" justify="center">
                <h3 class="title">{{ model }}</h3>
              </v-row>
            </v-sheet>
          </v-expand-transition>
        </v-sheet>
      </v-col>
    </v-row>
  </v-container>
</template>
<script>
export default {
  name: "HelloWorld",
  data: () => ({
    model: undefined,
  }),
};
</script>
```

我们在`v-slide-group`下面添加了`v-expand-transition`组件，向用户显示我们想要的内容。

当我们点击这个项目时，我们会看到一个过渡效果。

`model`有我们点击的项目的索引。

# 结论

我们可以用`v-slide-group`组件添加幻灯片，并让我们在点击它时选择项目。

喜欢这篇文章吗？如果有，通过 [**订阅我们的 YouTube 频道**](https://www.youtube.com/channel/UCtipWUghju290NWcn8jhyAw?sub_confirmation=true) **获取更多类似内容！**