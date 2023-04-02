# 用于添加加密、富文本查看器、JSON 查看器等的顶级 Vue 包

> 原文：<https://javascript.plainenglish.io/top-vue-packages-for-adding-encryption-rich-text-viewer-json-viewer-and-more-5b53f1bf9a0a?source=collection_archive---------7----------------------->

![](img/eb516b3b51e1fb93d42e367ec8105bb0.png)

Photo by [Martin Moreno](https://unsplash.com/@memoreno?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

Vue.js 是一个易于使用的 web 应用框架，我们可以用它来开发交互式前端应用。

在这篇文章中，我们将看看最好的包是如何添加加密、文本查看器、JSON 查看器和项目选择器的。

# vue-cryptojs

vue-cryptojs 允许我们通过 crypto-js 的包装器给我们的 vue 应用添加加密。

要安装它，我们运行:

```
npm i vue-cryptojs
```

然后我们可以通过写来使用它:

`main.js`

```
import Vue from "vue";
import App from "./App.vue";
import VueCryptojs from "vue-cryptojs";Vue.use(VueCryptojs);Vue.config.productionTip = false;new Vue({
  render: h => h(App)
}).$mount("#app");
```

`App.vue`

```
<template>
  <div></div>
</template><script>
export default {
  mounted() {
    const encryptedText = this.CryptoJS.AES.encrypt(
      "hello world",
      "secret"
    ).toString();
    console.log(encryptedText);
  }
};
</script>
```

我们注册了插件，这样我们就可以在我们的应用程序中调用`this.CryptoJS.AES.encrypt`。

此外，我们可以使用:

```
Vue.CryptoJS
this.$CryptoJS
```

来得到同样的物体。

# vue-样本

vue-swatches 允许我们用几行代码在我们的 vue 应用程序中添加一个颜色选择器。

我们可以通过运行以下命令来安装它:

```
npm i vue-swatches
```

现在我们可以在组件中使用它:

```
<template>
  <div>
    <v-swatches v-model="color"></v-swatches>
  </div>
</template><script>
import VSwatches from "vue-swatches";
import "vue-swatches/dist/vue-swatches.css";export default {
  components: { VSwatches },
  data() {
    return {
      color: "green"
    };
  }
};
</script>
```

`v-swatches`是拾色器的组件。

颜色代码字符串被绑定到带有`v-model`的组件。

# v-选择页面

v-selectpage 是一个包，它允许我们添加一个 UI 组件来让用户选择项目。

要安装它，我们运行:

```
npm i v-selectpage
```

然后我们可以通过写来使用它:

`main.js`

```
import Vue from "vue";
import App from "./App.vue";
import vSelectPage from "v-selectpage";
Vue.use(vSelectPage, {});
Vue.config.productionTip = false;new Vue({
  render: h => h(App)
}).$mount("#app");
```

`App.vue`

```
<template>
  <v-selectpage
    :data="list"
    key-field="id"
    show-field="name"
    placeholder="choose a fruit"
    language="en"
  ></v-selectpage>
</template>

<script>
export default {
  data() {
    return {
      list: [
        { id: 1, name: "apple", desc: "apple" },
        { id: 2, name: "orange", desc: "orange" }
      ]
    };
  }
};
</script>
```

我们使用带有`data`属性的`v-selectpage`组件来填充选择。

`key-field`是每个条目的唯一 ID。

`show-field`是要显示的字段的属性名，。

`placeholder`有占位符。

`language`是语言。

它还支持多重选择、分页、从右到左语言等等。

# vue-json 浏览器

vue-json-viewer 包为我们提供了一个可以在我们的 vue 应用中使用的 json 查看器。

为了使用它，我们运行:

```
npm i vue-json-viewer
```

然后我们通过书写来使用它:

`main.js`

```
import Vue from "vue";
import App from "./App.vue";
import JsonViewer from "vue-json-viewer";Vue.use(JsonViewer);
Vue.config.productionTip = false;new Vue({
  render: h => h(App)
}).$mount("#app");
```

`App.vue`

```
<template>
  <json-viewer :value="jsonData"></json-viewer>
</template>

<script>
export default {
  data() {
    return {
      jsonData: { foo: { bar: 2 } }
    };
  }
};
</script>
```

我们注册插件并使用`json-viewer`组件。

`value`道具有我们想要显示的 JSON。

我们可以用`copyable`道具让它可以复制。

`boxed`制作一个盒子。

`expand-depth`将折叠给定深度下的块。

它还有一个添加复制按钮的插槽。

# @toast-ui/vue-editor

我们可以使用 [@toast](http://twitter.com/toast) -ui/vue-editor 包给一个 Vue app 添加一个文本编辑器。

要安装它，我们运行:

```
npm i @toast-ui/vue-editor
```

然后我们可以通过写来使用它:

`main.js`

```
import Vue from "vue";
import App from "./App.vue";
import "codemirror/lib/codemirror.css";
import "@toast-ui/editor/dist/toastui-editor.css";Vue.config.productionTip = false;new Vue({
  render: h => h(App)
}).$mount("#app");
```

`App.vue`

```
<template>
  <editor
    :initialValue="editorText"
    :options="editorOptions"
    height="500px"
    initialEditType="wysiwyg"
    previewStyle="vertical"
  />
</template>
<script>
import "codemirror/lib/codemirror.css";
import "@toast-ui/editor/dist/toastui-editor.css";import { Editor } from "@toast-ui/vue-editor";export default {
  components: {
    editor: Editor
  },
  data() {
    return {
      editorText: "hello world.",
      editorOptions: {
        hideModeSwitch: true
      }
    };
  }
};
</script>
```

我们将`initialValue`设置为我们想要的文本。

同样，我们可以设置`options` bu，将一个对象传递给`options` prop。

`initialEditType`设置为`wysiwyg`启用所见即所得编辑。

![](img/b371db78a9f668bb1dc8a2bdf5737442.png)

Photo by [Mike Benna](https://unsplash.com/@mbenna?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 结论

@toast-ui/vue-editor 是一个文本编辑器，我们可以使用它来查看内容。

vue-cryptojs 允许我们将加密 js 添加到我们的 vue 应用程序中。

vue-swatches 允许我们添加颜色选择器。

vue-json-viewer 允许我们添加一个 json 查看器。

v-selectpage 允许我们添加一个项目选择器。