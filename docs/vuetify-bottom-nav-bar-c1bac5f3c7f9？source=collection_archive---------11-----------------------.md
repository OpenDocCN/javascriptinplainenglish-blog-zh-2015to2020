# 虚拟化—底部导航栏

> 原文：<https://javascript.plainenglish.io/vuetify-bottom-nav-bar-c1bac5f3c7f9?source=collection_archive---------11----------------------->

![](img/c46a4dcbfcb5e00571087074bbda15da.png)

Photo by [Salvador Altamirano](https://unsplash.com/@salva_alt?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

Vuetify 是一个流行的 Vue 应用程序 UI 框架。

在本文中，我们将了解如何使用 Vuetify 框架。

# 水平导航条

我们可以添加`horizontal`道具，让底部的导航文本显示在图标旁边，而不是图标下面:

```
<template>
  <v-container>
    <v-row class="text-center">
      <v-col col="12">
        <v-bottom-navigation :value="activeBtn" color="purple lighten-1" horizontal>
          <v-btn>
            <span>History</span>
            <v-icon>mdi-history</v-icon>
          </v-btn> <v-btn>
            <span>Favorites</span>
            <v-icon>mdi-heart</v-icon>
          </v-btn> <v-btn>
            <span>Map</span>
            <v-icon>mdi-map-marker</v-icon>
          </v-btn>
        </v-bottom-navigation>
      </v-col>
    </v-row>
  </v-container>
</template><script>
export default {
  name: "HelloWorld",
  data: () => ({
    activeBtn: undefined,
  }),
};
</script>
```

# 变化

`shift`道具让我们隐藏按钮文本，直到它被激活。

栏中的`v-btn`文本应该用一个`span`标签包装。

例如，我们可以写:

```
<template>
  <v-container>
    <v-row class="text-center">
      <v-col col="12">
        <v-bottom-navigation :value="activeBtn" color="purple lighten-1" shift>
          <v-btn>
            <span>History</span>
            <v-icon>mdi-history</v-icon>
          </v-btn> <v-btn>
            <span>Favorites</span>
            <v-icon>mdi-heart</v-icon>
          </v-btn> <v-btn>
            <span>Map</span>
            <v-icon>mdi-map-marker</v-icon>
          </v-btn>
        </v-bottom-navigation>
      </v-col>
    </v-row>
  </v-container>
</template><script>
export default {
  name: "HelloWorld",
  data: () => ({
    activeBtn: undefined,
  }),
};
</script>
```

我们只需添加一个`shift`道具，使文本仅在图标被点击时显示。

# 触发器

`v-bottom-navigation`的显示状态可以通过`input-value`按钮切换。

例如，我们可以写:

```
<template>
  <v-container>
    <v-row class="text-center">
      <v-col col="12">
        <div class="overflow-hidden">
          <div class="text-center mb-2">
            <v-btn text color="deep-purple" @click="showNav = !showNav">Toggle Nav</v-btn>
          </div><v-bottom-navigation :value="activeBtn" color="purple lighten-1" :input-value="showNav">
            <v-btn>
              <span>History</span>
              <v-icon>mdi-history</v-icon>
            </v-btn> <v-btn>
              <span>Favorites</span>
              <v-icon>mdi-heart</v-icon>
            </v-btn> <v-btn>
              <span>Map</span>
              <v-icon>mdi-map-marker</v-icon>
            </v-btn>
          </v-bottom-navigation>
        </div>
      </v-col>
    </v-row>
  </v-container>
</template><script>
export default {
  name: "HelloWorld",
  data: () => ({
    activeBtn: undefined,
    showNav: false,
  }),
};
</script>
```

我们添加了一个`v-btn`来切换导航。

当`showNav`为`true`时，显示出来。

否则，它是隐藏的。

我们设置`input-value`来显示导航。

# 在卷轴上隐藏

我们只能在目标元素滚动时显示`v-bottom-navigation`。

为此，我们将设置了`scroll-target`道具的`hide-on-scroll`道具添加到我们正在滚动的项目的选择器中。

例如，我们可以写:

```
<template>
  <v-container>
    <v-row class="text-center">
      <v-col col="12">
        <v-card class="overflow-hidden mx-auto" height="200" max-width="500">
          <v-bottom-navigation scroll-target="#scroll-area" hide-on-scroll absolute horizontal>
            <v-btn text color="deep-purple accent-4">
              <span>History</span>
              <v-icon>mdi-history</v-icon>
            </v-btn> <v-btn text color="deep-purple accent-4">
              <span>Favorites</span>
              <v-icon>mdi-heart</v-icon>
            </v-btn> <v-btn text color="deep-purple accent-4">
              <span>Map</span>
              <v-icon>mdi-map-marker</v-icon>
            </v-btn>
          </v-bottom-navigation> <v-sheet id="scroll-area" class="overflow-y-auto" max-height="600">
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
    activeBtn: undefined,
    showNav: false,
  }),
};
</script>
```

`scroll-target`被添加到`v-bottom-navigation`组件中。

`hide-on-scroll`使其隐藏在卷轴上。

![](img/31af09b3c671d6adbb35ecfc62ec7586.png)

Photo by [Dan Gold](https://unsplash.com/@danielcgold?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 结论

我们可以用 Vuetify 让底部的导航条按照我们想要的方式运行。

喜欢这篇文章吗？如果有，通过 [**订阅获取更多类似内容解码，我们的 YouTube 频道**](https://www.youtube.com/channel/UCtipWUghju290NWcn8jhyAw?sub_confirmation=true) **！**