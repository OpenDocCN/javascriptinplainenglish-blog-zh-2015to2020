# 虚拟化—按钮组

> 原文：<https://javascript.plainenglish.io/vuetify-button-groups-48d107f052af?source=collection_archive---------9----------------------->

![](img/bab3e04b99115e8437274ee6a8a29e70.png)

Photo by [Duy Pham](https://unsplash.com/@miinyuii?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

Vuetify 是一个流行的 Vue 应用程序 UI 框架。

在本文中，我们将了解如何使用 Vuetify 框架。

# 按钮组

`v-btn-toggle`组件是与`v-btn`组件一起工作的`v-item-group`的包装器。

我们可以用它来添加一组可以切换的按钮。

例如，我们可以写:

```
<template>
  <v-container class="grey lighten-5">
    <v-row>
      <v-col>
        <v-card flat class="py-12">
          <v-card-text>
            <v-row align="center" justify="center">
              <v-btn-toggle v-model="toggle" rounded>
                <v-btn>
                  <v-icon>mdi-format-align-left</v-icon>
                </v-btn>
                <v-btn>
                  <v-icon>mdi-format-align-center</v-icon>
                </v-btn>
                <v-btn>
                  <v-icon>mdi-format-align-right</v-icon>
                </v-btn>
                <v-btn>
                  <v-icon>mdi-format-align-justify</v-icon>
                </v-btn>
              </v-btn-toggle>
            </v-row>
          </v-card-text>
        </v-card>
      </v-col>
    </v-row>
  </v-container>
</template>
<script>
export default {
  name: "HelloWorld",
  data: () => ({
    toggle: undefined,
  }),
};
</script>
```

用`v-btn-toggle`组件添加按钮组。

我们只是把`v-btn`组件放在组里面。

`rounded`道具使按钮组变圆。

# 强制按钮组

`mandatory`道具使`v-btn-toggle`始终有值。

例如，我们可以写:

```
<template>
  <v-container class="grey lighten-5">
    <v-row>
      <v-col>
        <v-card flat class="py-12">
          <v-card-text>
            <v-row align="center" justify="center"> <v-btn-toggle v-model="toggle" multiple>
                <v-btn>
                  <v-icon>mdi-format-align-left</v-icon>
                </v-btn>
                <v-btn>
                  <v-icon>mdi-format-align-center</v-icon>
                </v-btn>
                <v-btn>
                  <v-icon>mdi-format-align-right</v-icon>
                </v-btn>
                <v-btn>
                  <v-icon>mdi-format-align-justify</v-icon>
                </v-btn>
              </v-btn-toggle> <v-col cols="12" class="text-center">{{ toggle }}</v-col>
            </v-row>
          </v-card-text>
        </v-card>
      </v-col>
    </v-row>
  </v-container>
</template>
<script>
export default {
  name: "HelloWorld",
  data: () => ({
    toggle: []
  }),
};
</script>
```

`multiple`道具让我们进行多重选择。

当我们点击一个按钮时，它的索引将被添加到状态中。

# 工具栏中的按钮

我们可以给`v-toolbar`添加按钮。

例如，我们可以写:

```
<template>
  <v-container class="grey lighten-5">
    <v-row>
      <v-col>
        <v-toolbar dense>
          <v-overflow-btn :items="dropdownFont" label="Select font" hide-details class="pa-0"></v-overflow-btn>
          <template v-if="$vuetify.breakpoint.mdAndUp">
            <v-divider vertical></v-divider>
            <v-overflow-btn
              :items="dropdownEdit"
              editable
              label="Select size"
              hide-details
              class="pa-0"
              overflow
            ></v-overflow-btn>
            <v-divider vertical></v-divider>
            <v-spacer></v-spacer>
            <v-btn-toggle v-model="toggleMultiple" color="primary" dense group multiple>
              <v-btn :value="1" text>
                <v-icon>mdi-format-bold</v-icon>
              </v-btn>
              <v-btn :value="2" text>
                <v-icon>mdi-format-italic</v-icon>
              </v-btn>
              <v-btn :value="3" text>
                <v-icon>mdi-format-underline</v-icon>
              </v-btn>
            </v-btn-toggle>
            <div class="mx-4"></div>
            <v-btn-toggle v-model="toggleExclusive" color="primary" dense group>
              <v-btn :value="1" text>
                <v-icon>mdi-format-align-left</v-icon>
              </v-btn>
              <v-btn :value="2" text>
                <v-icon>mdi-format-align-center</v-icon>
              </v-btn>
              <v-btn :value="3" text>
                <v-icon>mdi-format-align-right</v-icon>
              </v-btn>
            </v-btn-toggle>
          </template>
        </v-toolbar>
      </v-col>
    </v-row>
  </v-container>
</template>
<script>
export default {
  name: "HelloWorld",
  data: () => ({
    dropdownFont: [
      { text: "Arial" },
      { text: "Calibri" },
      { text: "Courier" },
      { text: "Verdana" },
    ],
    dropdownEdit: [
      { text: "100%" },
      { text: "75%" },
      { text: "50%" },
      { text: "25%" },
      { text: "0%" },
    ],
    toggleExclusive: 2,
    toggleMultiple: [1, 2, 3],
  }),
};
</script>
```

我们有带`v-overflow-btn`组件的`v-toolbar`来添加下拉菜单。

我们添加`v-divider`来将分隔线添加到我们的工具栏中。

`v-btn-toggle`让我们添加按钮开关。

此外，我们添加了`dense`道具，使工具栏变得更小。

# 结论

我们可以用 Vuetify 给工具栏添加按钮组等等。

喜欢这篇文章吗？如果有，通过 [**订阅我们的 YouTube 频道**](https://www.youtube.com/channel/UCtipWUghju290NWcn8jhyAw?sub_confirmation=true) **获取更多类似内容！**