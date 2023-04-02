# 用于添加 JSON 查看器、图像裁剪器、日期选择器和通知的顶级 Vue 包

> 原文：<https://javascript.plainenglish.io/top-vue-packages-for-adding-a-json-viewer-image-cropper-date-picker-and-6e470eac418c?source=collection_archive---------13----------------------->

![](img/59bf9c399be2b283be560dc33c0a1222.png)

Photo by [Zdeněk Macháček](https://unsplash.com/@zmachacek?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

Vue.js 是一个易于使用的 web 应用框架，我们可以用它来开发交互式前端应用。

在本文中，我们将看看添加 JSON 查看器、裁剪器、日期选择器和通知的最佳包。

# vue-JSON-树视图

vue-json-tree-view 是一个不错的 json 数据查看器。

为了使用它，我们运行:

```
npm i vue-json-tree-view
```

来安装它。

然后我们可以写:

```
<template>
  <div>
    <tree-view :data="jsonSource" :options="{ maxDepth: 3 }"></tree-view>
  </div>
</template>

<script>
export default {
  data() {
    return {
      jsonSource: {
        foo: {
          bar: 1
        },
        baz: [1, 2, 3]
      }
    };
  }
};
</script>
```

在屏幕上显示 JSON。

我们使用了`tree-view`组件在屏幕上显示`jsonSource`对象。

`maxDepth`是我们显示扩展的最大深度。

组件发出一个我们可以监听的`change-data` emit。

也可以应用自定义样式。

# VueCtkDateTimePicker

VueCtkDateTimePicker 是一个简单易用的 Vue 应用程序的日期和时间选择器。

为了使用它，我们运行:

```
npm i vue-ctk-date-time-picker
```

来安装它。

然后我们可以写:

`main.js`

```
import Vue from "vue";
import App from "./App.vue";
import VueCtkDateTimePicker from "vue-ctk-date-time-picker";
import "vue-ctk-date-time-picker/dist/vue-ctk-date-time-picker.css";Vue.component("VueCtkDateTimePicker", VueCtkDateTimePicker);
Vue.config.productionTip = false;new Vue({
  render: h => h(App)
}).$mount("#app");
```

`App.vue`

```
<template>
  <div>
    <VueCtkDateTimePicker v-model="value"/>
    <p>{{value}}</p>
  </div>
</template>

<script>
export default {
  data() {
    return {
      value: undefined
    };
  }
};
</script>
```

去使用它。

我们导入了`VueCtkDateTimePicker`组件并在我们的代码中使用了它。

此外，我们导入了组件的 CSS。

`v-model`将选择的日期和时间绑定到`value`状态。

它还有一个黑暗模式。

选取器的格式也可以改变。

其他事情也可以改变，如一周的第一天，输入大小，快捷键，禁用日期和时间，等等。

# 武埃-克罗帕

vue-croppa 允许我们在我们的 vue 应用程序中添加一个图像裁剪器。

我们可以通过运行以下命令来安装它:

```
npm i vue-croppa
```

然后我们可以通过写来使用它:

`main.js`

```
import Vue from "vue";
import App from "./App.vue";
import Croppa from "vue-croppa";
import "vue-croppa/dist/vue-croppa.css";Vue.use(Croppa);
Vue.config.productionTip = false;new Vue({
  render: h => h(App)
}).$mount("#app");
```

`App.vue`

```
<template>
  <div>
    <croppa v-model="myCroppa"></croppa>
  </div>
</template>

<script>
export default {
  data() {
    return {
      myCroppa: {}
    };
  }
};
</script>
```

我们导入 CSS 和组件，这样我们就可以在代码中使用它。

现在，我们将获得一个图片上传占位符，我们可以在其中选择要操作的图片。

我们还可以做许多其他的事情。

例如，我们可以更改缩放设置、占位符、初始图像等等。

当选择一个文件时，或者当文件有问题(如文件太大)时，它也会发出事件。

裁剪器也可以通过编程来使用。

# vue-snotify

vue-snotify 是 vue 应用程序的通知库。

要使用它，我们可以运行:

```
npm i vue-snotify
```

然后我们可以通过写来使用它:

`main.js`

```
import Vue from "vue";
import App from "./App.vue";
import Snotify from "vue-snotify";
Vue.use(Snotify);
Vue.config.productionTip = false;new Vue({
  render: h => h(App)
}).$mount("#app");
```

`App.vue`

```
<template>
  <div>
    <vue-snotify></vue-snotify>
  </div>
</template>

<script>
export default {
  mounted() {
    this.$snotify.success("success");
  }
};
</script>
```

我们有`vue-snotify`组件来显示通知。

然后我们用`this.$nitify.success`显示。

我们也有可以改变的选择。

例如，我们可以改变超时，显示进度条，等等。

所以我们可以写:

```
<template>
  <div>
    <vue-snotify></vue-snotify>
  </div>
</template>

<script>
export default {
  mounted() {
    this.$snotify.success("success", {
      timeout: 2000,
      showProgressBar: false,
      closeOnClick: false,
      pauseOnHover: true
    });
  }
};
</script>
```

我们也可以编写 HTML:

```
<template>
  <div>
    <vue-snotify></vue-snotify>
  </div>
</template>

<script>
export default {
  mounted() {
    this.$snotify.html(`<div><b>bold</b></div>`, {
      timeout: 5000,
      showProgressBar: true,
      closeOnClick: false,
      pauseOnHover: true
    });
  }
};
</script>
```

![](img/e68eac9c71e8b2509e2fb7f87184a044.png)

Photo by [Thomas Bonometti](https://unsplash.com/@bonopeppers?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 结论

vue-json-tree-view 是 vue 应用程序的一个有用的 json 浏览器。

VueCtkDateTimePicker 让我们添加一个日期时间选择器。

vue-croppa 让我们添加一个图像裁剪器。

vue-snotify 允许我们在 vue 应用程序中添加通知。

# 简单英语的 JavaScript

喜欢这篇文章吗？如果是这样，通过 [**订阅解码获得更多类似内容，我们的 YouTube 频道**](https://www.youtube.com/channel/UCtipWUghju290NWcn8jhyAw) **！**