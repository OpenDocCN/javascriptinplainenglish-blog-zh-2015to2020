# 用于添加日历、本地存储和覆盖的顶级 Vue 包

> 原文：<https://javascript.plainenglish.io/top-vue-packages-for-adding-calendars-local-storage-overlays-and-resizing-caee66817cc?source=collection_archive---------6----------------------->

![](img/01d4fb63b6542cc8f09b8d4a20a2c77b.png)

Photo by [Fuu J](https://unsplash.com/@fuuj?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

Vue.js 是一个易于使用的 web 应用框架，我们可以用它来开发交互式前端应用。

在本文中，我们将看看添加日历、覆盖和处理本地存储的最佳包。

# vue-完整日历

vue-fullcalendar 为我们提供了一个简单的日历组件来显示事件。

要使用它，首先我们通过运行以下命令来安装它:

```
npm i vue-fullcalendar
```

然后我们可以通过写来使用它:

`main.js`

```
import Vue from "vue";
import App from "./App.vue";
import fullCalendar from "vue-fullcalendar";Vue.component("full-calendar", fullCalendar);
Vue.config.productionTip = false;new Vue({
  render: h => h(App)
}).$mount("#app");
```

`App.vue`

```
<template>
  <div id="app">
    <full-calendar :events="fcEvents" lang="en"></full-calendar>
  </div>
</template><script>
const fcEvents = [
  {
    title: "eat",
    start: "2020-05-25",
    end: "2020-05-27"
  }
];
export default {
  data() {
    return {
      fcEvents
    };
  }
};
</script>
```

我们注册组件。然后我们在我们的`App`组件中使用它。

`full-calendar`带一个`events`道具，带一组物体。

这些对象具有`title`、`start`和`end`属性。

`title`是事件标题。

`start`是开始日期字符串。`end`是结束日期字符串。

当月份被更改和事件被单击时，它会发出事件。

# 虚拟日历

v-calendar 是另一个日历组件。

要安装它，我们运行:

```
npm i v-calendar
```

现在我们可以通过书写来使用它:

`main.js`

```
import Vue from "vue";
import App from "./App.vue";
import Calendar from "v-calendar/lib/components/calendar.umd";
import DatePicker from "v-calendar/lib/components/date-picker.umd";Vue.component("calendar", Calendar);
Vue.component("date-picker", DatePicker);
Vue.config.productionTip = false;new Vue({
  render: h => h(App)
}).$mount("#app");
```

`App.vue`

```
<template>
  <div id="app">
    <date-picker :mode="mode" v-model="selectedDate"/>
  </div>
</template><script>
export default {
  data() {
    return {
      mode: "single",
      selectedDate: null
    };
  }
};
</script>
```

添加日期选取器。

此外，我们可以写:

```
<template>
  <div id="app">
    <calendar></calendar>
  </div>
</template><script>
export default {};
</script>
```

添加日历。

# VueLocalStorage

VueLocalStorage 是一个 Vue 插件，让我们在 Vue 应用中处理本地存储。

要安装它，我们运行:

```
npm i vue-localstorage
```

然后我们可以通过写来使用它:

`main.js`

```
import Vue from "vue";
import App from "./App.vue";
import VueLocalStorage from "vue-localstorage";Vue.use(VueLocalStorage);
Vue.config.productionTip = false;new Vue({
  render: h => h(App)
}).$mount("#app");
```

`App.vue`

```
<template>
  <div id="app">
  </div>
</template><script>
export default {
  mounted() {
    this.$localStorage.set('foo', 'bar')
  }
};
</script>
```

我们注册插件。

然后我们可以用`this.$localStorage`属性操作本地存储。

我们可以调用`get`来获得一个值:

```
this.$localStorage.get('foo')
```

要删除一个条目，我们调用`remove`:

```
this.$localStorage.remove('foo')
```

# vue-加载-覆盖

vue-loading-overlay 为我们提供了一个在加载时显示的覆盖图。

要安装它，我们运行:

```
npm i vue-loading-overlay
```

然后我们可以通过写来使用它:

```
<template>
  <div>
    <loading active can-cancel :on-cancel="onCancel" is-full-page></loading>
  </div>
</template>

<script>
import Loading from "vue-loading-overlay";
import "vue-loading-overlay/dist/vue-loading.css";export default {
  components: {
    Loading
  },
  methods: {
    onCancel() {}
  }
};
</script>
```

我们使用`loading`组件添加一个覆盖图来显示数据加载的时间。

`active`是一个设置为`true`的道具，如果我们想要显示覆盖图的话。

`can-cancel`使覆盖图可取消。

然后当我们取消时，调用`onCancel`方法，因为我们将它设置为`on-cancel`属性的值。

我们也可以把它作为一个插件。

例如，我们可以写:

`main.js`

```
import Vue from "vue";
import App from "./App.vue";
import Loading from "vue-loading-overlay";
import "vue-loading-overlay/dist/vue-loading.css";Vue.use(Loading);
Vue.config.productionTip = false;new Vue({
  render: h => h(App)
}).$mount("#app");
```

`App.vue`

```
<template>
  <div ref="container"></div>
</template>

<script>
export default {
  mounted() {
    this.$loading.show({
      container: this.$refs.container,
      canCancel: true,
      onCancel: this.onCancel
    });
  },
  methods: {
    onCancel() {}
  }
};
</script>
```

我们访问`this.$loading.show`方法来显示覆盖图。

属性已经被设置为我们想要覆盖图所在的容器的引用，这样它就会被渲染。

`onCancel`有取消时调用的方法。

![](img/3c2bd749703d0c3254c48286acd8be46.png)

Photo by [shannon VanDenHeuvel](https://unsplash.com/@shannonnicolevandy?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 结论

vue-fullcalendar 是一个日历和日期选择器组件。

vue-loading-overlay 是一个在我们加载数据时使用的覆盖组件。

VueLocalStorage 是一个在 Vue 应用中处理本地存储的插件。

## 简单英语的 JavaScript

喜欢这篇文章吗？如果有，通过 [**订阅获取更多类似内容解码，我们的 YouTube 频道**](https://www.youtube.com/channel/UCtipWUghju290NWcn8jhyAw) **！**