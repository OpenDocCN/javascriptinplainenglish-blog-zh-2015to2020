# 用于添加货币输入、菜单、图标和可调整大小的元素的顶级 Vue 包

> 原文：<https://javascript.plainenglish.io/top-vue-packages-for-adding-currency-input-menu-icons-and-resizable-elements-a2c18f45cf1d?source=collection_archive---------10----------------------->

![](img/adc55065f1ebf1fa8e16dc9a8275d984.png)

Photo by [Jason Leung](https://unsplash.com/@ninjason?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

Vue.js 是一个易于使用的 web 应用框架，我们可以用它来开发交互式前端应用。

在这篇文章中，我们将看看如何添加像菜单，数字货币输入，图标和可调整大小的元素最好的包。

# vue-条纹菜单

vue-stripe-menu 允许我们在应用程序中添加一个类似条纹的菜单。

要安装它，我们运行:

```
npm i vue-stripe-menu
```

然后我们可以通过写来使用它:

`main.js`

```
import Vue from "vue";
import App from "./App.vue";
import VueStripeMenu from "vue-stripe-menu";
import "vue-stripe-menu/dist/vue-stripe-menu.css";
Vue.use(VueStripeMenu);Vue.config.productionTip = false;new Vue({
  render: h => h(App)
}).$mount("#app");
```

`App.vue`

```
<template>
  <vsm-menu
    :menu="menu"
    :base-width="380"
    :base-height="400"
    :screen-offset="10"
    element="header"
    @open-dropdown="onOpenDropdown"
    @close-dropdown="onCloseDropdown"
  >
    <template #default="{ item }">
      <div class="wrap-content">
        <div class="wrap-content__block">Header: {{ item.title }}</div>
        <div class="wrap-content__item">{{ item }}</div>
      </div>
    </template>
    <template #before-nav>
      <li class="vsm-section logo-section">
        <img src="http://placekitten.com/100/100" alt="logo">
      </li>
    </template>
    <template #title="data">{{ data.item.title }}</template>
    <template #after-nav>
      <li class="vsm-section vsm-mob-hide">
        <button>My Button</button>
      </li>
      <vsm-mob>Mobile Content</vsm-mob>
    </template>
  </vsm-menu>
</template>

<script>
export default {
  data() {
    return {
      menu: [
        {
          title: "App",
          dropdown: "app",
          element: "span",
          attributes: {
            class: ["my-class1", { "my-class2": true }],
            "data-big": "yes"
          },
          listeners: {
            mouseover: evt => {
              console.log("news hover", evt);
            }
          },
          new_section: false
        },
        {
          title: "External Link",
          attributes: {
            href: "https://example.com",
            target: "_blank"
          }
        }
      ]
    };
  },
  methods: {
    onOpenDropdown() {
      console.log("onOpenDropdown");
    },
    onCloseDropdown() {
      console.log("onCloseDropdown");
    }
  }
};
</script>
```

我们得到一个`App`链接，显示悬停时的内容。

`External Link`链接显示一个我们可以点击的链接。

我们还有一个按钮。

`item`拥有物品。

我们也可以得到标志。

该菜单具有响应性，因此当屏幕较窄时，它会显示不同的内容。

我们可以按照自己喜欢的方式来设计。

有插槽来填充各种项目。

`default`槽有标题项。

`title`有标题。

`after-nav`有按钮。

# vue-数字-货币

vue-numeric-currency 允许我们在 vue 应用程序中添加货币输入。

要使用它，我们首先通过编写以下内容来安装它:

```
npm i vue-numeric-currency
```

然后我们可以写:

`main.js`

```
import Vue from "vue";
import App from "./App.vue";
import VueNumeric from "vue-numeric-currency";Vue.use(VueNumeric);Vue.config.productionTip = false;new Vue({
  render: h => h(App)
}).$mount("#app");
```

`App.vue`

```
<template>
  <div>
    <vue-numeric currency="$" separator="," v-model="price"></vue-numeric>
    <p>{{price}}</p>
  </div>
</template>

<script>
export default {
  data() {
    return {
      price: 0
    };
  }
};
</script>
```

我们将`vue-numeric`组件添加到`App`组件中。

`currency`拥有货币。

`separator`是千位分隔符。

`v-model`绑定到`price`状态。

只有我们输入一个数字，绑定才会完成。

我们可以用`min`和`max`道具限制范围。

`precision`让我们改变小数位数。

`placeholder`让我们更改占位符。

当我们关注输入时，我们也可以使用`autoselect`道具来自动选择输入的文本。

# 可调整大小

vue-resizable 允许我们创建一个可拖动和可调整大小的组件。

要安装它，我们可以运行:

```
npm i vue-resizable
```

然后，我们可以编写一个可拖动的组件:

```
<template>
  <vue-resizable>
    <div class="resizable-content"></div>
  </vue-resizable>
</template>

<script>
import VueResizable from "vue-resizable";export default {
  components: { VueResizable }
};
</script>

<style scoped>
.resizable-content {
  height: 100%;
  width: 100%;
  background-color: green;
}
</style>
```

我们使用了`vue-resizable`组件来包装任何可拖动和可调整大小的东西。

必须将`height`和`width`设置为百分比或`vw` 或`vh` 才能让美国更改其大小。

它发出各种事件。当大小改变或被拖动时，它会发出事件。

它在挂载时还会发出一个事件。

# vue-unicon

vue-unicons 是一组有用的图标，我们可以在我们的应用程序中使用。

要安装它，我们运行:

```
npm i vue-unicons
```

然后我们可以通过写来使用它:

`main.js`

```
import Vue from "vue";
import App from "./App.vue";
import Unicon from "vue-unicons";
import { uniLayerGroupMonochrome, uniCarWash } from "vue-unicons/src/icons";Unicon.add([uniLayerGroupMonochrome, uniCarWash]);
Vue.use(Unicon);Vue.config.productionTip = false;new Vue({
  render: h => h(App)
}).$mount("#app");
```

`App.vue`

```
<template>
  <unicon name="car-wash" fill="green"></unicon>
</template>

<script>
export default {};
</script>
```

我们在`main.js`注册了图标，并在`App.vue`中使用。

![](img/ce7b88c66ff4a5ffd5b84aac85fa3725.png)

Photo by [Ian Robinson](https://unsplash.com/@ianrobinson?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 结论

vue-stripe-menu 允许我们在应用程序中添加一个类似条纹的菜单。

vue-numeric-currency 允许我们添加带有各种选项的数字货币输入。

要添加一个可调整大小的元素，我们可以使用 vue-resizable 包。

vue-unicons 为我们提供了一组可以在我们的 vue 应用程序中使用的图标。

## 简单英语的 JavaScript

喜欢这篇文章吗？如果有，通过 [**订阅我们的 YouTube 频道**](https://www.youtube.com/channel/UCtipWUghju290NWcn8jhyAw) **获取更多类似内容！**