# 用于延迟加载图像、处理键盘快捷键等的顶级 Vue 包

> 原文：<https://javascript.plainenglish.io/top-vue-packages-for-lazy-loading-image-handling-keyboard-shortcut-and-more-4b36fc23ac58?source=collection_archive---------8----------------------->

![](img/6d6dbad01cfec7d2056929f23d2ea90e.png)

Photo by [Andy Holmes](https://unsplash.com/@andyjh07?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

Vue.js 是一个易于使用的 web 应用框架，我们可以用它来开发交互式前端应用。

在这篇文章中，我们将会看到最好的延迟加载图像、处理键盘快捷键、添加代码编辑器和添加数字输入的包。

# 懒惰图像

我们可以添加 v-lazy-image 包，为我们的应用程序添加图像的延迟加载功能。

要使用它，我们可以运行:

```
npm i v-lazy-image
```

然后我们可以通过写来使用它:

`main.js`

```
import Vue from "vue";
import App from "./App.vue";
import { VLazyImagePlugin } from "v-lazy-image";Vue.use(VLazyImagePlugin);
Vue.config.productionTip = false;new Vue({
  render: h => h(App)
}).$mount("#app");
```

`App.vue`

```
<template>
  <v-lazy-image src="http://placekitten.com/500/500"/>
</template>

<script>
export default {};
</script>
```

我们只是添加了`v-lazy-image`的成分。

此外，我们可以添加一个后备图像和图像加载时的模糊效果:

```
<template>
  <v-lazy-image
    src="http://placekitten.com/500/500"
    src-placeholder="http://placekitten.com/200/200"
  />
</template>

<script>
export default {};
</script>

 <style scoped>
.v-lazy-image {
  filter: blur(20px);
  transition: filter 0.7s;
}
.v-lazy-image-loaded {
  filter: blur(0);
}
</style>
```

我们设计了`v-lazy-image`和`v-lazy-image-loaded`的样式，以便在加载时获得模糊效果。

它还发出`intersect`和`load`事件。

`srcset`让我们添加多个不同尺寸的图像，根据不同尺寸加载它们。

# vue 快捷键

vue-shortkey 允许我们在 vue 应用程序中添加键盘快捷键处理。

我们可以通过运行以下命令来安装它:

```
npm i vue-shortkey
```

然后我们可以通过写来使用它:

`main.js`

```
import Vue from "vue";
import App from "./App.vue";
Vue.use(require("vue-shortkey"));Vue.config.productionTip = false;new Vue({
  render: h => h(App)
}).$mount("#app");
```

`App.vue`

```
<template>
  <div>
    <button v-shortkey="['ctrl', 'alt', 'o']" @shortkey="theAction">button</button>
  </div>
</template>

<script>
export default {
  methods: {
    theAction(event) {
      alert("hello");
    }
  }
};
</script>
```

我们注册了插件，然后将`v-shortkey`指令添加到一个按钮上。

数组中有我们想要的组合键。

`shortkey`按下组合键时发出事件。

然后运行`theAction`。

我们也可以在一个处理器中处理多个组合键。

为此，我们写道:

```
<template>
  <div>
    <button v-shortkey="{up: ['arrowup'], down: ['arrowdown']}" @shortkey="theAction">button</button>
  </div>
</template>

<script>
export default {
  methods: {
    theAction(event) {
      switch (event.srcKey) {
        case "up":
          alert("up");
          break;
        case "down":
          alert("down");
          break;
        default:
          alert("default");
          break;
      }
    }
  }
};
</script>
```

我们向指令传递一个对象。

然后我们检查`theAction`方法来检查按下的键。

# vue-prism 编辑器

vue-prism-editor 允许我们添加一个简单的代码编辑器，带有语法高亮和行号。

要安装它，我们可以运行:

```
npm i vue-prism-editor prismjs
```

然后我们可以写:

`main.js`

```
import Vue from "vue";
import App from "./App.vue";
import "prismjs";
import "prismjs/themes/prism.css";
import VuePrismEditor from "vue-prism-editor";
import "vue-prism-editor/dist/VuePrismEditor.css"; // import the styles
Vue.component("prism-editor", VuePrismEditor);Vue.config.productionTip = false;new Vue({
  render: h => h(App)
}).$mount("#app");
```

`App.vue`

```
<template>
  <div>
    <prism-editor v-model="code" language="js"></prism-editor>
    <pre>{{code}}</pre>
  </div>
</template>

<script>
import PrismEditor from "vue-prism-editor";
export default {
  components: {
    PrismEditor
  },
  data() {
    return {
      code: ""
    };
  }
};
</script>
```

将`prism-editor`添加到我们的应用程序。

我们将语言设置为`js`以突出 JavaScript。

`v-model`将输入的代码绑定到`code`状态。

它支持许多功能，如撤销或重做，复制和粘贴，保留缩进等。

它发出 change、keydown、keyup 和编辑器单击事件。

我们可以用`lineNumbers`道具添加行号。

我们也可以禁用编辑器或使其只读。

# rack beat-vue-数字

rackbeat-vue-numeric 允许我们在应用程序中添加数字输入。

要安装它，我们运行:

```
npm i rackbeat-vue-numeric
```

然后我们可以通过写来使用它:

`main.js`

```
import Vue from "vue";
import App from "./App.vue";
import VueNumeric from "vue-numeric";Vue.use(VueNumeric);Vue.config.productionTip = false;new Vue({
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
      price: ""
    };
  }
};
</script>
```

我们将货币符号设置为`currency` 属性。

`separator`是千位分隔符。

当输入值是有效数字时，`v-model`将输入值绑定到`price`状态。

我们可以用`min`和`max`限制有效范围，并用占位符的值设置`placeholder`属性。

![](img/34cf6a67b051ecaed87d8b05a7baf008.png)

Photo by [Sébastien LAVALAYE](https://unsplash.com/@lavalaye?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 结论

v-lazy-image 允许我们添加惰性加载图像。

vue-shortkey 允许我们添加键盘快捷键处理。

rackbeat-vue-numeric 允许我们在应用程序中添加数字输入。

vue-prism-editor 是一个易于添加的代码编辑器，我们可以在我们的 vue 应用程序中使用。

## 简单英语的 JavaScript

喜欢这篇文章吗？如果有，通过 [**订阅我们的 YouTube 频道**](https://www.youtube.com/channel/UCtipWUghju290NWcn8jhyAw) **获取更多类似内容！**