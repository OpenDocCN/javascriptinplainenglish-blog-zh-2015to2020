# 用于二维码、渲染降价等的顶级 Vue 包

> 原文：<https://javascript.plainenglish.io/top-vue-packages-for-qr-codes-rendering-markdown-and-more-64e05d9b39df?source=collection_archive---------9----------------------->

![](img/155f4ec9f001bd3183d0b09344e9f619.png)

Photo by [Ryan Loughlin](https://unsplash.com/@rylomedia?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

Vue.js 是一个易于使用的 web 应用框架，我们可以用它来开发交互式前端应用。

在这篇文章中，我们将看看添加一个加载微调器，转盘，渲染 Markdown 到 HTML，显示一个二维码，并显示一个侧边栏菜单的最佳包。

# 猫头鹰旋转木马

vue-owl-carousel 允许我们在我们的 vue 应用程序中添加一个图片轮播。

为了使用它，我们运行:

```
npm i vue-owl-carousel
```

来安装它。

然后我们写道:

```
<template>
  <div>
    <carousel>
      <img v-for="n in 10" :key="n" :src="`https://placeimg.com/200/200/any?${n}`">
    </carousel>
  </div>
</template>

<script>
import carousel from "vue-owl-carousel";export default {
  components: { carousel }
};
</script>
```

向我们的应用程序添加旋转木马。

我们使用`carousel`组件来添加转盘。

然后我们在标签之间添加图像来填充图像。

当屏幕无法同时显示屏幕上的所有图像时，导航会自动显示。

这需要一些道具。

`autoplay`让我们启用自动播放。

`nav`让我们启用或禁用导航。

它还发出一些事件。

`changed`和`updated`事件被发射出来。

# vue-侧栏-菜单

vue-sidebar-menu 是一个库，可以让我们轻松地在我们的 vue 应用程序中添加侧边栏。

要安装它，我们运行:

```
npm i vue-sidebar-menu
```

然后我们写道:

```
<template>
  <div>
    <sidebar-menu :menu="menu"/>
  </div>
</template>

<script>
import { SidebarMenu } from "vue-sidebar-menu";
import "vue-sidebar-menu/dist/vue-sidebar-menu.css";export default {
  components: {
    SidebarMenu
  },
  data() {
    return {
      menu: [
        {
          header: true,
          title: "menu",
          hiddenOnCollapse: true
        },
        {
          href: "/",
          title: "user",
          icon: "fa fa-user"
        },
        {
          href: "/charts",
          title: "charts",
          icon: "fa fa-chart-area",
          child: [
            {
              href: "/charts/sublink",
              title: "child"
            }
          ]
        }
      ]
    };
  }
};
</script>
```

来补充一下。

`icon`是图标的类别。

`href`是要做的路径。

`child`是子链接。

`title`是显示在菜单项上的名称。

`hiddenOnCollapse`让我们选择折叠时是否隐藏菜单。

# Vue 对决

Vue 摊牌是一个降价展示组件。

我们可以用它将 Markdown 转换成 HTML。

要安装它，我们运行:

```
npm install vue-showdown
```

然后我们可以通过写来使用它:

`main.js`

```
import Vue from "vue";
import App from "./App.vue";
import VueShowdown from "vue-showdown";Vue.use(VueShowdown, {
  flavor: "github",
  options: {
    emoji: false
  }
});
Vue.config.productionTip = false;new Vue({
  render: h => h(App)
}).$mount("#app");
```

`App.vue`

```
<template>
  <div>
    <VueShowdown markdown="## markdown" flavor="github" :options="{ emoji: true }"/>
  </div>
</template>

<script>
export default {
  data() {
    return {};
  }
};
</script>
```

我们通过注册一些选项来使用`VueShowdown`组件。

`flavor`是我们渲染的减价的味道。

`emoji`让我们选择是否渲染表情符号。

我们将降价信息传递给`markdown`道具。

# v-二维码

v-qrcode 允许我们在 Vue 应用程序中呈现 QR 码。

为了使用它，我们运行:

```
npm i v-qrcode
```

来安装它。

然后我们写道:

```
<template>
  <div>
    <qrcode :cls="qrCls" :value="qrText"></qrcode>
  </div>
</template><script>
import Qrcode from "v-qrcode";export default {
  data() {
    return {
      qrCls: "qrcode",
      qrText: "hello"
    };
  }, components: {
    Qrcode
  }
};
</script>
```

去使用它。

我们传入`cls`属性来设置包装器的类名。

`value`是生成二维码的字符串。

我们还可以更改大小、级别、背景、前景、添加以及呈现代码的元素。

# 真空元件加载

vue-element-loading 插件允许我们在 vue 应用程序中添加一个加载微调器。

为了使用它，我们运行:

```
npm i vue-element-loading
```

来安装它。

然后我们写:
`main.js`

```
import Vue from "vue";
import App from "./App.vue";
import VueElementLoading from "vue-element-loading";Vue.component("VueElementLoading", VueElementLoading);
Vue.config.productionTip = false;new Vue({
  render: h => h(App)
}).$mount("#app");
```

`App.vue`

```
<template>
  <div>
    <vue-element-loading active spinner="bar-fade-scale"/>
  </div>
</template><script>
export default {};
</script>
```

去使用它。

我们有装载旋转器的`vue-element`组件。

`active`使它显示出来。

让我们设置想要显示的微调器的类型。

![](img/acae52fff8dbc0bb11376650895ba1ce.png)

Photo by [Todd Quackenbush](https://unsplash.com/@toddquackenbush?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 结论

vue-owl-carousel 是一个易于使用的轮播库。

vue-sidebar-menu 让我们在应用程序中显示侧边栏菜单。

Vue 摊牌渲染成 HTML 的 Markdown。

v-qrcode 显示从字符串创建的 QR 码。

vue-element-loading 是一个加载微调器。

## 简单英语的 JavaScript

喜欢这篇文章吗？如果有，通过 [**订阅获取更多类似内容解码，我们的 YouTube 频道**](https://www.youtube.com/channel/UCtipWUghju290NWcn8jhyAw) **！**