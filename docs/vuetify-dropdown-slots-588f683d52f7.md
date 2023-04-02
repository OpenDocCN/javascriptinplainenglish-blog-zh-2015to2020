# 虚拟化—下拉插槽

> 原文：<https://javascript.plainenglish.io/vuetify-dropdown-slots-588f683d52f7?source=collection_archive---------3----------------------->

![](img/1ff30f25949723d2c4071d401935b5e0.png)

Photo by [DEAR](https://unsplash.com/@riverse?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

Vuetify 是一个流行的 Vue 应用程序 UI 框架。

在本文中，我们将了解如何使用 Vuetify 框架。

# 预先计划和附加项目槽

我们可以用各种物品填充插槽。

例如，我们可以通过填充`prepend-item`槽，在现有选项之上添加一个项目:

```
<template>
  <v-container>
    <v-row>
      <v-col col="12">
        <v-select v-model="selectedFruits" :items="fruits" label="Favorite Fruits" multiple>
          <template v-slot:prepend-item>
            <v-list-item ripple @click="toggle">
              <v-list-item-action>
                <v-icon :color="selectedFruits.length > 0 ? 'indigo darken-4' : ''">{{ icon }}</v-icon>
              </v-list-item-action>
              <v-list-item-content>
                <v-list-item-title>Select All</v-list-item-title>
              </v-list-item-content>
            </v-list-item>
            <v-divider class="mt-2"></v-divider>
          </template>
          <template v-slot:append-item>
            <v-divider class="mb-2"></v-divider>
            <v-list-item disabled>
              <v-list-item-avatar color="grey lighten-3">
                <v-icon>mdi-food-apple</v-icon>
              </v-list-item-avatar><v-list-item-content v-if="likesAllFruit">
                <v-list-item-title>all selected</v-list-item-title>
              </v-list-item-content><v-list-item-content v-else-if="likesSomeFruit">
                <v-list-item-title>{{ selectedFruits.length }} selected</v-list-item-title>
              </v-list-item-content><v-list-item-content v-else>
                <v-list-item-title>no fruit selected</v-list-item-title>
              </v-list-item-content>
            </v-list-item>
          </template>
        </v-select>
      </v-col>
    </v-row>
  </v-container>
</template>
<script>
export default {
  name: "HelloWorld",
  data: () => ({
    fruits: ["apple", "orange", "pear"],
    selectedFruits: [],
  }),
  computed: {
    likesAllFruit() {
      return this.selectedFruits.length === this.fruits.length;
    },
    likesSomeFruit() {
      return this.selectedFruits.length > 0 && !this.likesAllFruit;
    },
    icon() {
      if (this.likesAllFruit) return "mdi-close-box";
      if (this.likesSomeFruit) return "mdi-minus-box";
      return "mdi-checkbox-blank-outline";
    },
  },
  methods: {
    toggle() {
      if (this.likesAllFruit) {
        this.selectedFruits = [];
      } else {
        this.selectedFruits = [...this.fruits];
      }
    },
  },
};
</script>
```

我们有带有`prepend-item`插槽的`v-select`组件，该插槽带有全选复选框。

我们有`v-list-item`，当点击它来切换水果的选择时，它会调用`toggle`。

该开关显示在顶部，因为它在`prepend-item`槽中。

此外，我们有一个`append-item`插槽来填充一些文本。

文本显示在下拉列表的底部。

# 更改选择外观

我们可以更改选择外观。

例如，我们可以写:

```
<template>
  <v-container>
    <v-row>
      <v-col col="12">
        <v-select v-model="value" :items="items" label="Select Item" multiple>
          <template v-slot:selection="{ item, index }">
            <v-chip v-if="index === 0">
              <span>{{ item }}</span>
            </v-chip>
            <span v-if="index === 1" class="grey--text caption">(+{{ value.length - 1 }} others)</span>
          </template>
        </v-select>
      </v-col>
    </v-row>
  </v-container>
</template>
<script>
export default {
  name: "HelloWorld",
  data: () => ({
    items: ["apple", "orange", "pear"],
    value: [],
  }),
};
</script>
```

我们填充了`selection`插槽，以向该插槽添加项目的数量。

这将显示在选项的右侧。

![](img/eda47477ab62f5834b456ef199950d8c.png)

Photo by [Aysha Begum](https://unsplash.com/@aysha_be?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 结论

通过填充槽，下拉菜单可以显示各种内容。

喜欢这篇文章吗？如果有，通过 [**订阅我们的 YouTube 频道**](https://www.youtube.com/channel/UCtipWUghju290NWcn8jhyAw?sub_confirmation=true) **获取更多类似内容！**