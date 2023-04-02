# 用于添加滑块、文本滑块、日期选择器和交叉点检测的顶级 Vue 包

> 原文：<https://javascript.plainenglish.io/top-vue-packages-for-adding-a-slider-text-slider-datepicker-and-intersection-detection-8f3c9a0a9bb3?source=collection_archive---------16----------------------->

![](img/98d139df59336ff844f4d828e78837a3.png)

Photo by [Priscilla Du Preez](https://unsplash.com/@priscilladupreez?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

Vue.js 是一个易于使用的 web 应用框架，我们可以用它来开发交互式前端应用。

在这篇文章中，我们将看看添加滑块、日期选择器、时间轴和交叉点检测的最佳包。

# vue-textra

vue-textra 允许我们在我们的 vue 应用程序中添加一个文本滑块。

要安装它，我们运行:

```
npm i vue-textra
```

然后我们可以通过写来使用它:

`main.js`

```
import Vue from "vue";
import App from "./App.vue";
import Textra from "vue-textra";Vue.use(Textra);
Vue.config.productionTip = false;new Vue({
  render: h => h(App)
}).$mount("#app");
```

`App.vue`

```
<template>
  <div id="app">
    <textra :data='words' :timer="4" filter="flash" />
  </div>
</template><script>export default {
  data(){
    return {
      words: ['foo', 'bar', 'baz']
    }  
  }
};
</script>
```

我们注册插件。

然后，我们添加了`textra`组件，它逐个显示数组中的每个字符串。

`timer`是以秒为单位显示每个字符串的时间。

`filter`是文本间过渡时的效果。

我们可以用`infinite`道具让组件无限循环。

其他滤镜效果有`simple`、`bottom-top`、`top-bottom`、`right-left`、`left-right`、`press`、`scale`、`flash`、`flip`。

# 虚拟视点

VueWaypoint 允许我们根据基于视口的元素位置运行函数。

要安装它，我们运行:

```
npm i vue-waypoint
```

然后我们可以写:

`main.js`

```
import Vue from "vue";
import App from "./App.vue";
import VueWaypoint from "vue-waypoint";Vue.use(VueWaypoint);
Vue.config.productionTip = false;new Vue({
  render: h => h(App)
}).$mount("#app");
```

`App.vue`

```
<template>
  <div id="app">
    <div
      v-for="n in 50"
      :key="n"
      v-waypoint="{ active: true, callback: onWaypoint, options: intersectionOptions }"
    >{{n}}</div>
  </div>
</template><script>
export default {
  data() {
    return {
      intersectionOptions: {
        root: null,
        rootMargin: "0px 0px 0px 0px",
        threshold: [0, 1]
      }
    };
  },
  methods: {
    onWaypoint({ going, direction }) {
      if (going === this.$waypointMap.GOING_IN) {
        console.log("waypoint going in!");
      } if (direction === this.$waypointMap.DIRECTION_TOP) {
        console.log("waypoint going top!");
      }
    }
  }
};
</script>
```

我们在`main.js`中注册插件。

然后我们使用`v-waypoint`指令来观察元素进入和退出视口。

`this.$waypointMap.GOING_IN`表示它正在进入视口。

而`DIRECTION_TOP`表示它正在向顶端移动。

它还可以检测向右、向下和向左的方向。

# 真空管道

vue-pipeline 允许我们添加一个管道来显示到我们的 vue 应用程序中。

我们可以通过运行以下命令来安装它:

```
npm install vue-pipeline
```

然后我们可以写:

`main.js`

```
<template>
  <div id="app">
    <vue-pipeline :data="data"></vue-pipeline>
  </div>
</template><script>
export default {
  data() {
    return {
      data: [
        {
          name: "Start",
          hint: "Start",
          status: "start",
          next: [{ index: 1, weight: 2 }]
        },
        {
          name: "eat",
          hint: "eat",
          status: "success",
          next: [{ index: 2, weight: 0 }, { index: 4, weight: 2 }]
        },
        {
          name: "drink",
          hint: "drink",
          status: "failure",
          next: [{ index: 3, weight: 0 }]
        },
        {
          name: "sleep",
          hint: "sleep",
          status: "paused",
          next: [{ index: 4, weight: 0 }]
        },
        { name: "end ", hint: "2m23s", status: "end", next: [] }
      ]
    };
  }
};
</script>
```

我们注册组件，然后将`vue-pipeline`组件添加到我们的组件中。

数据是具有各种属性的数组。

`name`是物品的名称。悬停时显示`hint`。

`status`显示为图标。`next`表示下一步。

其他道具包括`xstep`改变前一个节点的水平位置。

`ystep`改变前一个节点的垂直位置。

`lineStyle`是线条样式的字符串。可以是`default`、`bessel`或`line`。

# vue js-日期选择器

我们可以添加使用 vuejs-datepicker 添加一个日期选择器到我们的应用程序。

要安装它，我们可以运行:

```
npm i vuejs-datepicker
```

然后，我们可以通过添加以下内容来使用它:

```
<template>
  <div id="app">
    <datepicker v-model="date"></datepicker>
    <p>{{date}}</p>
  </div>
</template><script>
import Datepicker from "vuejs-datepicker";export default {
  data() {
    return {
      date: undefined
    };
  },
  components: {
    Datepicker
  }
};
</script>
```

我们注册了`Datepicker`组件，然后将它添加到模板中。

我们用`date`状态设置`v-model`来绑定它。

现在，当我们选择一个日期时，就会显示该日期。

它支持更改日期选择器的许多部分，包括图标、按钮、将其设置为必需或禁用、更改为首先显示星期一等等。

![](img/be5ae0cc57a9a82e743653cac42c6a17.png)

Photo by [Annie Spratt](https://unsplash.com/@anniespratt?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 结论

vuejs-datepicker 允许我们添加一个日期选择器。

vue-pipeline 允许我们添加时间线显示。

vue-textra 允许我们添加一个文本滑块。

VueWaypoint 为我们的应用程序添加了路口检测功能。

## 简单英语的 JavaScript

你知道我们有四份出版物和一个 YouTube 频道吗？在 [**plainenglish.io**](https://plainenglish.io/) 和 [**找到它们订阅解码，我们的 YouTube 频道**](https://www.youtube.com/channel/UCtipWUghju290NWcn8jhyAw) **！**