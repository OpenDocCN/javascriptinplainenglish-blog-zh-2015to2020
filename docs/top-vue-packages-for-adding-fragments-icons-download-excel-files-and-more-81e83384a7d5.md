# 添加片段、图标、下载 Excel 文件等的顶级软件包

> 原文：<https://javascript.plainenglish.io/top-vue-packages-for-adding-fragments-icons-download-excel-files-and-more-81e83384a7d5?source=collection_archive---------8----------------------->

![](img/7c2d7c2f8caf896a6f1b6a66579bd6c6.png)

Photo by [Victoria Strukovskaya](https://unsplash.com/@struvictoryart?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

Vue.js 是一个易于使用的 web 应用程序框架，我们可以用它来开发交互式前端应用程序。

在本文中，我们将了解添加片段、图标、将 JSON 数据下载为 Excel 文件以及检测空闲和调整大小的最佳软件包。

# vue-碎片

vue-fragment 允许我们在 vue 应用程序中渲染片段。

片段是不呈现任何包装元素的组件。

要使用它，我们运行:

```
npm i vue-fragment
```

安装它。

然后我们可以通过书写来使用它:

`main.js`

```
import Vue from "vue";
import App from "./App.vue";
import { Plugin } from "vue-fragment";
Vue.use(Plugin);
Vue.config.productionTip = false;new Vue({
  render: h => h(App)
}).$mount("#app");
```

`App.vue`

```
<template>
  <div id="app">
    <fragment>
      <p>hello</p>
    </fragment>
  </div>
</template><script>
export default {};
</script>
```

# Vue 材料设计图标组件

Vue 材料设计图标组件是一个包含材料设计图标的包，我们可以使用。

要使用它，我们运行:

```
npm i vue-material-design-icons
```

安装它。

然后我们通过书写来使用它:

```
<template>
  <div id="app">
    <menu-icon/>
  </div>
</template><script>
import MenuIcon from "vue-material-design-icons/Menu.vue";export default {
  components: {
    MenuIcon
  }
};
</script>
```

我们导入图标组件，以便可以在模板中使用它。

此外，我们还可以更改大小:

```
<template>
  <div id="app">
    <menu-icon :size="48"/>
  </div>
</template><script>
import MenuIcon from "vue-material-design-icons/Menu.vue";export default {
  components: {
    MenuIcon
  }
};
</script>
```

# vue-json-excel

VUE 2 的 JSON 到 Excel 是一个包，它允许我们将 JSON 数据转换成 Excel 文件并下载。

要使用它，我们首先通过运行以下命令安装它:

```
npm i vue-json-excel
```

然后我们可以通过书写来使用它:

```
<template>
  <div id="app">
    <download-excel name="filename.xls" :fields="jsonFields" :data="jsonData">Download Data</download-excel>
  </div>
</template><script>
export default {
  data() {
    return {
      jsonFields: {
        "first name": "name",
        phone: {
          field: "phone.landline",
          callback: value => {
            return `landline Phone - ${value}`;
          }
        }
      },
      jsonData: [
        {
          name: "james",
          phone: {
            landline: "(541) 754-3010"
          }
        },
        {
          name: "alex",
          phone: {
            landline: "(463) 582-2244"
          }
        }
      ]
    };
  }
};
</script>
```

我们使用`download-excel`组件添加一个元素，将一些 JSON 下载到我们的计算机上。

`name`是文件名、

`fields`是我们作为标题的领域。

它是一个以键为标题的对象。该值是我们的`jsonData`数组条目中的一个属性。

它也可以是一个回调，按照我们喜欢的方式格式化数据。

如果它是嵌套的，那么我们需要用属性的路径来指定`field`属性。

`jsonData`是我们希望在 Excel 文件中包含的数据。

字段可以是任何结构。

# 怠速-vue

idle-vue 是一个 vue 插件，它可以检测用户已经有一段时间没有和我们的应用程序交互了。

要使用它，我们可以通过运行以下命令来安装它:

```
npm i idle-vue
```

然后我们可以通过书写来使用它:

`main.js`

```
import Vue from "vue";
import App from "./App.vue";
import IdleVue from "idle-vue";const eventsHub = new Vue();Vue.use(IdleVue, {
  eventEmitter: eventsHub,
  idleTime: 2000
});
Vue.config.productionTip = false;new Vue({
  render: h => h(App)
}).$mount("#app");
```

我们通过将`eventEmitter`属性设置为新的`Vue`实例的对象传入来注册插件。

这让我们可以发送应用程序范围内的事件。

`idleTime`设置空闲超时时间，单位为毫秒。

`idle`事件将在超时时发出。

`App.vue`

```
<template>
  <div id="app">{{messageStr}}</div>
</template><script>
export default {
  data() {
    return {
      messageStr: ""
    };
  },
  onIdle() {
    this.messageStr = "idle";
  },
  onActive() {
    this.messageStr = "hello";
  }
};
</script>
```

# vue-size-传感器

vue-resize-sensor 是一个检测元素大小调整的 vue 插件。

我们通过运行以下程序来安装它:

```
npm i vue-resize-sensor
```

然后我们可以通过书写来使用它:

```
<template>
  <div id="app">
    <resize-sensor @resize="resize"></resize-sensor>
  </div>
</template><script>
import ResizeSensor from "vue-resize-sensor";export default {
  components: {
    ResizeSensor
  },
  methods: {
    resize({ width, height }) {
      console.log(width, height);
    }
  }
};
</script>
```

我们使用`resize-sensor`组件。

每当组件调整大小时，就调用`resize`。

![](img/03b65f87c377060ceecd3deed693d886.png)

Photo by [Johann Siemens](https://unsplash.com/@johannsiemens?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 结论

vue-fragment 是一个组件，它允许我们添加不呈现任何内容的包装组件。

Vue 材料设计图标组件为我们提供了可在 Vue 应用程序中使用的图标。

vue-json-excel 允许我们下载 json 数据作为 excel 电子表格。

空闲-vue 让我们检测 vue 应用程序何时空闲。

vue-resize-sensor 让我们检测元素的大小调整。

# 简单英语的 JavaScript

喜欢这篇文章吗？如果是这样，通过 [**订阅解码获得更多类似内容，我们的 YouTube 频道**](https://www.youtube.com/channel/UCtipWUghju290NWcn8jhyAw) **！**