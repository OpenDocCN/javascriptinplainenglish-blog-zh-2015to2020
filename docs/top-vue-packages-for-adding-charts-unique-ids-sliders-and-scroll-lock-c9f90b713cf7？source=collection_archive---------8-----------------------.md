# 用于添加图表、唯一 id、滑块和滚动锁定的顶级 Vue 包

> 原文：<https://javascript.plainenglish.io/top-vue-packages-for-adding-charts-unique-ids-sliders-and-scroll-lock-c9f90b713cf7?source=collection_archive---------8----------------------->

![](img/3b8eb3ff978ada6bceb38aa1cd4a7b3b.png)

Photo by [Martin Moreno](https://unsplash.com/@memoreno?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

Vue.js 是一个易于使用的 web 应用框架，我们可以用它来开发交互式前端应用。

在本文中，我们将看看添加图表、惟一 id、滑块和滚动锁的最佳包。

# vue 唯一 id

vue-unique-id 允许我们向我们的 vue 组件添加一个唯一的 id。

为了使用它，我们运行:

```
npm i vue-unique-id
```

来安装它。

然后我们写道:

`main.js`

```
import Vue from "vue";
import App from "./App.vue";
import UniqueId from "vue-unique-id";
Vue.use(UniqueId);Vue.config.productionTip = false;new Vue({
  render: h => h(App)
}).$mount("#app");
```

`App.vue`

```
<template>
  <div id="app"></div>
</template>

<script>
export default {
  created() {
    console.log(this.uid);
  }
};
</script>
```

我们注册了插件，并使用`this.uid`属性来获取惟一的 ID。

同样，我们可以用`$id`方法获得 ID。

例如，我们可以写:

```
<template>
  <div id="app"></div>
</template>

<script>
export default {
  created() {
    console.log(this.$id("foo"));
  }
};
</script>
```

获得一个添加了`'foo'`后缀的 ID。

# 不可避免的

VueVisible 是一个指令，让我们有条件地显示一些东西。

为了使用它，我们运行:

```
npm i vue-visible
```

来安装它。

然后我们通过书写来使用它:

```
<template>
  <div id="app">
    <div v-visible="isVisible">I'm visible</div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      isVisible: true
    };
  }
};
</script>
```

我们只是像使用`v-show`指令一样使用`v-visible`指令来有条件地显示一些东西。

# v 形涡卷锁

v-scroll-lock 允许我们在 Vue 组件中添加一个滚动锁来防止滚动。

为了使用它，我们运行:

```
npm i v-scroll-lock
```

然后我们可以通过写来使用它:

`main.js`

```
import Vue from "vue";
import App from "./App.vue";
import VScrollLock from "v-scroll-lock";Vue.use(VScrollLock);Vue.config.productionTip = false;new Vue({
  render: h => h(App)
}).$mount("#app");
```

`Modal.vue`

```
<template>
  <div>
    <div class="modal" v-if="open">
      <button [@click](http://twitter.com/click)="closeModal">X</button>
      <div class="modal-content" v-scroll-lock="open">
        <p v-for="n in 100" :key="n">{{n}}</p>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: "Modal",
  data() {
    return {
      open: true
    };
  },
  methods: {
    openModal() {
      this.open = true;
    },
    closeModal() {
      this.open = false;
    }
  }
};
</script>
```

`App.vue`

```
<template>
  <div>
    <modal></modal>
  </div>
</template>

<script>
import Modal from "./components/Modal";
export default {
  components: { Modal }
};
</script>
```

我们将带有`v-scroll-lock`指令的`Modal`组件应用于内容。

现在我们将无法滚动，即使我们有很多内容溢出了屏幕。

# 滑翔

Vue Glide 是 Vue 应用程序的滑块组件。

为了使用它，我们运行:

```
npm i vue-glide-js
```

来安装它。

然后我们可以通过写来使用它:

`main.js`

```
import Vue from "vue";
import App from "./App.vue";
import VueGlide from "vue-glide-js";
import "vue-glide-js/dist/vue-glide.css";Vue.use(VueGlide);
Vue.config.productionTip = false;new Vue({
  render: h => h(App)
}).$mount("#app");
```

`App.vue`

```
<template>
  <div id="app">
    <vue-glide>
      <vue-glide-slide v-for="i in 10" :key="i">Slide {{ i }}</vue-glide-slide>
    </vue-glide>
  </div>
</template>

<script>
import { Glide, GlideSlide } from "vue-glide-js";export default {
  components: {
    [Glide.name]: Glide,
    [GlideSlide.name]: GlideSlide
  }
};
</script>
```

我们使用`vue-slide`和`vue-glide-slide`组件在屏幕上显示幻灯片。

# vue-charist

vue-chartist 是基于 Chartist.js 的图表库。

为了使用它，我们运行:

```
npm i vue-chartist chartist
```

来安装它。

我们对 CSS 使用`chartist`。

然后我们写道:

`main.js`

```
import Vue from "vue";
import App from "./App.vue";
Vue.use(require("vue-chartist"));Vue.config.productionTip = false;new Vue({
  render: h => h(App)
}).$mount("#app");
```

`App.vue`

```
<template>
  <div id="app">
    <chartist ratio="ct-major-second" type="Line" :data="chartData" :options="chartOptions"></chartist>
  </div>
</template>

<script>
import "chartist/dist/chartist.css";export default {
  data() {
    return {
      chartData: {
        labels: ["A", "B", "C"],
        series: [[1, 3, 2], [4, 6, 5]]
      },
      chartOptions: {
        lineSmooth: false
      }
    };
  }
};
</script>
```

去使用它。

我们使用`chartist`组件来创建图表。

`type`是我们想要创建的那种图形。

`data`有标签和系列数据。

`labels`代表 x 轴，`series`代表 y 轴。

`chartOptions`有选项。我们将`lineSmooth`设置为`false`以禁用线条平滑。

![](img/6386f33077da18d867aa64623d3e53ed.png)

Photo by [Mark Basarab](https://unsplash.com/@ignitedit?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 结论

vue-unique-id 为我们的 vue 组件创建一个唯一的 id。

VueVisible 是一个类似于`v-show`的指令。

v-scroll-lock 禁用元素中的滚动。

Vue Glide 让我们创建一个滑块。

vue-chartist 让我们不费吹灰之力就能创建图表。

## 简单英语的 JavaScript

喜欢这篇文章吗？如果有，通过 [**订阅获取更多类似内容解码，我们的 YouTube 频道**](https://www.youtube.com/channel/UCtipWUghju290NWcn8jhyAw) **！**