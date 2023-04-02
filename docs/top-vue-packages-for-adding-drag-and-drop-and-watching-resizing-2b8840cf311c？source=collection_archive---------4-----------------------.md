# 用于添加拖放和观看调整大小的顶级 Vue 包

> 原文：<https://javascript.plainenglish.io/top-vue-packages-for-adding-drag-and-drop-and-watching-resizing-2b8840cf311c?source=collection_archive---------4----------------------->

![](img/827ab44338c9db1e2a129a87eb52cb01.png)

Photo by [Kelvin Yup](https://unsplash.com/@kelvinyup?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

Vue.js 是一个易于使用的 web 应用框架，我们可以用它来开发交互式前端应用。

在这篇文章中，我们将看看最好的添加拖放和观察元素调整到我们的 Vue 应用程序的包。

# VueDraggableResizable

VueDraggableResizable 是一个易于使用的软件包，让我们可以进行可拖动和可调整大小的评论。

要使用它，我们通过运行以下命令来安装它:

```
npm i vue-draggable-resizable
```

然后我们可以通过写来使用它:

```
import Vue from "vue";
import App from "./App.vue";
import VueDraggableResizable from "vue-draggable-resizable";import "vue-draggable-resizable/dist/VueDraggableResizable.css";Vue.component("vue-draggable-resizable", VueDraggableResizable);
Vue.config.productionTip = false;new Vue({
  render: h => h(App)
}).$mount("#app");
```

注册组件并添加样式。

然后在我们的组件中，我们编写:

```
<template>
  <div id="app">
    <div id="draggable">
      <vue-draggable-resizable
        :w="100"
        :h="100"
        [@dragging](http://twitter.com/dragging)="onDrag"
        [@resizing](http://twitter.com/resizing)="onResize"
        :parent="true"
      >
        <p>drag me ({{x}},{{y}})</p>
      </vue-draggable-resizable>
    </div>
  </div>
</template><script>
export default {
  name: "App",
  data() {
    return {
      width: 0,
      height: 0,
      x: 0,
      y: 0
    };
  },
  methods: {
    onResize(x, y, width, height) {
      this.x = x;
      this.y = y;
      this.width = width;
      this.height = height;
    },
    onDrag(x, y) {
      this.x = x;
      this.y = y;
    }
  }
};
</script><style>
#draggable {
  height: 500px;
  width: 500px;
  position: relative;
}
</style>
```

我们有一个 500 像素乘 500 像素的 div，由于有了`vue-draggable-resizable`组件，我们可以拖动它并调整其大小。

`x`和`y`坐标发生变化，可以通过`onDrag`处理程序监听，该处理程序在参数中具有最新的坐标。

同样，我们可以对 resize 进行同样的操作:

```
<template>
  <div id="app">
    <div id="draggable">
      <vue-draggable-resizable
        :w="100"
        :h="100"
        [@dragging](http://twitter.com/dragging)="onDrag"
        [@resizing](http://twitter.com/resizing)="onResize"
        :parent="true"
      >
        <p>drag me ({{x}},{{y}})</p>
        <p>size ({{width}}x{{height}})</p>
      </vue-draggable-resizable>
    </div>
  </div>
</template><script>
export default {
  name: "App",
  data() {
    return {
      width: 0,
      height: 0,
      x: 0,
      y: 0
    };
  },
  methods: {
    onResize(x, y, width, height) {
      this.x = x;
      this.y = y;
      this.width = width;
      this.height = height;
    },
    onDrag(x, y) {
      this.x = x;
      this.y = y;
    }
  }
};
</script><style>
#draggable {
  height: 500px;
  width: 500px;
  position: relative;
}
</style>
```

坐标和尺寸的`onResize`方法。

# 小道具

它附带了其他类似`class-name`的道具来设置类名。

`class-name-draggable` 启用`draggable`添加样式。

`class-name-resizable`让我们在调整元素大小时设置它的样式。

让我们在它调整大小时添加样式。

`class-name-handle`设计手柄。

`disable-user-select`让我们禁用用户选择。

我们可以使用许多其他道具来设计和处理事件。

初始`x`和`y`坐标也可以用同名道具设置。

# vue-调整大小

我们可以用 vue-resize 包来观察元素的大小调整。

要使用它，我们通过运行以下命令来安装它:

```
npm i vue-resize
```

然后，我们通过添加以下内容来注册组件:

```
import Vue from "vue";
import App from "./App.vue";
import "vue-resize/dist/vue-resize.css";
import VueResize from "vue-resize";Vue.use(VueResize);Vue.config.productionTip = false;new Vue({
  render: h => h(App)
}).$mount("#app");
```

它带有样式和`resize-observer`组件。

然后在我们的组件中，我们添加:

```
<template>
  <div class="demo">
    <p>Lorem ipsum dolor sit amet, consectetur adipiscing elit. Sed eu facilisis lorem. In arcu nisl, vulputate id diam eget, ultricies aliquet neque. Phasellus sapien lacus, consectetur ut nisi quis, mattis tincidunt ante. Aliquam ultrices nisl ornare augue laoreet, a vehicula libero consequat. Sed aliquam aliquet turpis, ut sodales elit sodales sit amet. Donec ullamcorper velit neque, in maximus odio tempus eu. Praesent ullamcorper, nibh sodales maximus feugiat, erat tellus condimentum sem, ac convallis tellus diam suscipit tellus. Proin egestas tellus neque. Vestibulum porttitor tempus tellus sit amet volutpat. Sed urna nibh, molestie non pulvinar in, accumsan nec nisl.</p>
    <resize-observer @notify="handleResize"/>
  </div>
</template>

<script>
export default {
  methods: {
    handleResize({ width, height }) {
      console.log("resized", width, height);
    }
  }
};
</script> 

<style scoped>
.demo {
  position: relative;
}
</style>
```

我们将`notify`处理程序设置为`handleResize`来观察大小的变化。

`width`和`height`在处理器中供我们使用。

![](img/bc529c33143bbf70ed50388a3de66c00.png)

Photo by [Chris Lejarazu](https://unsplash.com/@chrislejarazu?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 结论

我们可以使用 vue-draggable-resizable 包来创建可拖动和可调整大小的元素。

vue-resize 包让我们可以观察正在调整大小的元素。

## 坦白地说

你知道我们有四种出版物吗？通过 [**plainenglish.io**](https://plainenglish.io/) 找到他们——通过关注我们的出版物和 [**订阅我们的 YouTube 频道**](https://www.youtube.com/channel/UCtipWUghju290NWcn8jhyAw) **来表达爱意吧！**