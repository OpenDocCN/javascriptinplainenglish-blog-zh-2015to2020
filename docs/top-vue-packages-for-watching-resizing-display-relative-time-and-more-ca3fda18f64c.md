# 顶级 Vue 包，用于观看调整大小，显示相对时间，等等

> 原文：<https://javascript.plainenglish.io/top-vue-packages-for-watching-resizing-display-relative-time-and-more-ca3fda18f64c?source=collection_archive---------9----------------------->

![](img/456d65dc18ea72a3705f78bffc50dfa3.png)

Photo by [Martin Moreno](https://unsplash.com/@memoreno?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

Vue.js 是一个易于使用的 web 应用框架，我们可以用它来开发交互式前端应用。

在这篇文章中，我们将看看如何最好的包来观察元素大小调整，显示相对时间，添加分割窗格，并将数据复制到剪贴板。

# 调整大小

Vue.resize 是一个让我们检测 HTML 调整大小事件的包。

要安装它，我们运行:

```
npm i vue-resize-directive
```

然后我们可以通过写来使用它:

```
<template>
  <div id="app">
    <div
      v-resize="onResize"
    >Lorem ipsum dolor sit amet, consectetur adipiscing elit. Nunc nec elit ornare, sollicitudin lacus vel, volutpat ipsum. Aliquam vel erat sodales, faucibus dolor vel, ultricies augue. Morbi a posuere eros. Nulla bibendum tristique massa vel volutpat. Aenean in odio erat. Interdum et malesuada fames ac ante ipsum primis in faucibus. Etiam eros dolor, viverra ac arcu ac, venenatis commodo lectus.</div>
  </div>
</template><script>
import resize from "vue-resize-directive";export default {
  directives: {
    resize
  },
  methods: {
    onResize(e) {
      console.log(e);
    }
  }
};
</script>
```

我们注册该指令并使用`v-resize`指令。

我们可以压制或谴责被调用的侦听器。

# vue 数字

vue-numeric 是一个输入字段组件，用于显示格式化的货币值。

为了使用它，我们运行:

```
npm i vue-numeric
```

然后我们可以通过写作来使用它

```
<template>
  <div>
    <vue-numeric currency="$" separator="," v-model="price"></vue-numeric>
    <p>{{price}}</p>
  </div>
</template>

<script>
import VueNumeric from "vue-numeric";export default {
  name: "App",
  components: {
    VueNumeric
  },
  data() {
    return {
      price: ""
    };
  }
};
</script>
```

我们使用`vue-numeric`组件。

`currency`是显示在左侧的货币符号。

`separator`是千位分隔符。

`v-model`将输入值绑定到`price`状态。

我们还可以改变精度，启用或禁用负值，等等。

# Vue 分割窗格

Vue 分割窗格是一个让我们显示分割窗格的组件。

要安装它，我们运行:

```
npm i vue-splitpane
```

然后我们可以通过写来使用它:

`main.js`

```
import Vue from "vue";
import App from "./App.vue";
import splitPane from "vue-splitpane";
Vue.component("split-pane", splitPane);
Vue.config.productionTip = false;new Vue({
  render: h => h(App)
}).$mount("#app");
```

`App.vue`

```
<template>
  <div>
    <split-pane v-on:resize="resize" :min-percent="20" :default-percent="30" split="vertical">
      <template slot="paneL">left</template>
      <template slot="paneR">right</template>
    </split-pane>
  </div>
</template>

<script>
export default {
  methods: {
    resize() {}
  }
};
</script>
```

我们通过用我们自己的内容填充捆绑的槽来使用`split-pane`组件。

`panelL`是左面板。

`panelR`是右边的面板。

窗格可以嵌套。

# v-剪贴板

v-clipboard 是一个插件，可以让我们将我们想要的内容从我们的 Vue 应用程序复制到用户的剪贴板上。

要安装它，我们运行:

```
npm i v-clipboard
```

然后我们可以通过写来使用它:

`main.js`

```
import Vue from "vue";
import App from "./App.vue";
import Clipboard from "v-clipboard";Vue.use(Clipboard);
Vue.config.productionTip = false;new Vue({
  render: h => h(App)
}).$mount("#app");
```

`App.vue`

```
<template>
  <div id="app">
    <button v-clipboard="value">Copy to clipboard</button>
  </div>
</template><script>
export default {
  name: "App",
  data() {
    return {
      value: "foo"
    };
  }
};
</script>
```

我们传递给`v-clipboard`指令的任何内容都将被复制。

它还发出`success`和`error`事件。

我们可以听他们写:

```
<template>
  <div id="app">
    <button
      v-clipboard="value"
      v-clipboard:success="clipboardSuccessHandler"
      v-clipboard:error="clipboardErrorHandler"
    >Copy to clipboard</button>
  </div>
</template><script>
export default {
  name: "App",
  data() {
    return {
      value: "foo"
    };
  }, methods: {
    clipboardSuccessHandler({ value, event }) {
      console.log("success", value);
    }, clipboardErrorHandler({ value, event }) {
      console.log("error", value);
    }
  }
};
</script>
```

我们也可以使用`this.$clipboard`属性来复制:

```
<template>
  <div id="app">
    <button @click="copy">Copy to clipboard</button>
  </div>
</template><script>
export default {
  name: "App",
  data() {
    return {
      value: "foo"
    };
  }, methods: {
    copy() {
      this.$clipboard(this.value);
    }
  }
};
</script>
```

# vue-时间之前

vue-timeago 是一个让我们显示今天的相对时间的组件。

要安装它，我们运行:

```
npm i vue-timeago
```

然后我们可以用它来写:

`main.js`

```
import Vue from "vue";
import App from "./App.vue";
import VueTimeago from "vue-timeago";Vue.use(VueTimeago, {
  name: "Timeago",
  locale: "en"
});Vue.config.productionTip = false;new Vue({
  render: h => h(App)
}).$mount("#app");
```

`App.vue`

```
<template>
  <div id="app">
    <timeago :datetime="time"></timeago>
  </div>
</template><script>
export default {
  name: "App",
  data() {
    return {
      time: new Date(2020, 1, 1)
    };
  }
};
</script>
```

我们注册插件，并在注册时设置语言环境。

然后我们可以使用`timeago`组件。

传入`datetime`属性来计算和显示从今天开始的相对时间。

![](img/bfee648313aae03c25839e17d5d6955b.png)

Photo by [Behzad Ghaffarian](https://unsplash.com/@behz?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 结论

Vue.resize 让我们可以观察元素调整事件。

vue-numeric 是用于输入货币的数字输入。

Vue Split Pane 让我们可以轻松地创建分割窗格。

v-clipboard 是一个让用户将数据复制到剪贴板的插件。

vue-timeago 让我们显示今天的相对时间。

# 简单英语的 JavaScript

喜欢这篇文章吗？如果是这样，通过 [**订阅解码获得更多类似内容，我们的 YouTube 频道**](https://www.youtube.com/channel/UCtipWUghju290NWcn8jhyAw) **！**