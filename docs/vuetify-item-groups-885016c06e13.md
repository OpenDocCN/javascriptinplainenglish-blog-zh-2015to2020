# 虚拟化—项目组

> 原文：<https://javascript.plainenglish.io/vuetify-item-groups-885016c06e13?source=collection_archive---------8----------------------->

![](img/257d63faa255de6c6415329937269a04.png)

Photo by [Markus Winkler](https://unsplash.com/@markuswinkler?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

Vuetify 是一个流行的 Vue 应用程序 UI 框架。

在本文中，我们将了解如何使用 Vuetify 框架。

# 项目组

我们可以用`v-item-group`组件添加组项目。

例如，我们可以写:

```
<template>
  <v-container class="grey lighten-5">
    <v-row>
      <v-col>
        <v-item-group multiple>
          <v-container>
            <v-row>
              <v-col v-for="n in 3" :key="n" cols="12" md="4">
                <v-item v-slot:default="{ active, toggle }">
                  <v-card
                    :color="active ? 'primary' : ''"
                    class="d-flex align-center"
                    dark
                    height="200"
                    @click="toggle"
                  >
                    <v-scroll-y-transition>
                      <div v-if="active" class="display-3 flex-grow-1 text-center">Active</div>
                    </v-scroll-y-transition>
                  </v-card>
                </v-item>
              </v-col>
            </v-row>
          </v-container>
        </v-item-group>
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

我们用自己的内容填充默认的槽位`v-item`。

`active`是一个布尔值，表示该项目是否被按下。

`toggle`是一个功能，我们可以点击它来激活项目。

# 活动类

我们可以用`v-item-group`上的`active-class`道具设置活动类。

例如，我们可以写:

```
<template>
  <v-container class="grey lighten-5">
    <v-row>
      <v-col>
        <v-item-group active-class="primary">
          <v-container>
            <v-row>
              <v-col v-for="n in 3" :key="n" cols="12" md="4">
                <v-item v-slot:default="{ active, toggle }">
                  <v-card class="d-flex align-center" dark height="200" @click="toggle">
                    <v-scroll-y-transition>
                      <div v-if="active" class="display-3 flex-grow-1 text-center">Active</div>
                    </v-scroll-y-transition>
                  </v-card>
                </v-item>
              </v-col>
            </v-row>
          </v-container>
        </v-item-group>
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

当`active`为`true`时，我们将`active-class`设置为`primary`来设置类。

# 炸薯条

我们可以用`v-item`组件添加一个定制芯片组。

例如，我们可以写:

```
<template>
  <v-container class="grey lighten-5">
    <v-row>
      <v-col>
        <v-item-group multiple>
          <v-subheader>Tags</v-subheader>
          <v-item v-for="n in 8" :key="n" v-slot:default="{ active, toggle }">
            <v-chip active-class="purple--text" :input-value="active" @click="toggle">Tag {{ n }}</v-chip>
          </v-item>
        </v-item-group>
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

在一个`v-item-group`内添加一堆筹码。

# 列出项目组

我们可以将列表项组合成一个组。

例如，我们可以写:

```
<template>
  <v-container class="grey lighten-5">
    <v-row>
      <v-col>
        <v-list flat>
          <v-list-item-group v-model="model" color="indigo">
            <v-list-item v-for="(item, i) in items" :key="i">
              <v-list-item-icon>
                <v-icon v-text="item.icon"></v-icon>
              </v-list-item-icon><v-list-item-content>
                <v-list-item-title v-text="item.text"></v-list-item-title>
              </v-list-item-content>
            </v-list-item>
          </v-list-item-group>
        </v-list>
      </v-col>
    </v-row>
  </v-container>
</template>
<script>
export default {
  name: "HelloWorld",
  data: () => ({
    items: [
      {
        icon: "mdi-wifi",
        text: "Wifi",
      },
      {
        icon: "mdi-bluetooth",
        text: "Bluetooth",
      },
      {
        icon: "mdi-chart-donut",
        text: "Data Usage",
      },
    ],
  }),
};
</script>
```

我们使用`v-list`组件添加一个列表。

然后我们添加一个`v-list-item-group`来添加条目。

`v-list-item-icon`添加图标。

`v-list-item-content`为列表项添加内容。

# 结论

我们可以用 Vuetify 添加列表项和项组。

喜欢这篇文章吗？如果有，通过 [**订阅我们的 YouTube 频道**](https://www.youtube.com/channel/UCtipWUghju290NWcn8jhyAw?sub_confirmation=true) **获取更多类似内容！**