# 用于添加代码编辑器、通知和监视元素大小调整的顶级 Vue 包

> 原文：<https://javascript.plainenglish.io/top-vue-packages-for-adding-code-editors-notifications-and-watch-element-resizing-f0bf1c2d3ee2?source=collection_archive---------11----------------------->

![](img/0a624ef403445c0e5423c13d5a5ef895.png)

Photo by [Martin Moreno](https://unsplash.com/@memoreno?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

Vue.js 是一个易于使用的 web 应用框架，我们可以用它来开发交互式前端应用。

在本文中，我们将看看添加代码编辑器、通知和观察元素大小调整的最佳包。

# vue 代码镜像

vue-codemirror 为我们提供了一个显示代码的框。

要使用它，我们通过运行以下命令来安装它:

```
npm i vue-codemirror
```

然后我们可以写:

`main.js`

```
import Vue from "vue";
import App from "./App.vue";
import VueCodemirror from "vue-codemirror";
import "codemirror/lib/codemirror.css";Vue.use(VueCodemirror);
Vue.config.productionTip = false;new Vue({
  render: h => h(App)
}).$mount("#app");
```

`App.vue`

```
<template>
  <div>
    <codemirror v-model="code" :options="cmOptions"></codemirror>
  </div>
</template>

<script>
import "codemirror/mode/javascript/javascript.js";
import "codemirror/theme/base16-dark.css";export default {
  data() {
    return {
      code: "const a = 1",
      cmOptions: {
        tabSize: 4,
        mode: "text/javascript",
        theme: "base16-dark",
        lineNumbers: true,
        line: true
      }
    };
  },
  methods: {
    onCmReady(cm) {
      console.log("the editor is readied!", cm);
    },
    onCmFocus(cm) {
      console.log("the editor is focus!", cm);
    },
    onCmCodeChange(newCode) {
      console.log("this is new code", newCode);
      this.code = newCode;
    }
  }
};
</script>
```

我们有`codemirror`的成分。

代码与`v-model`绑定到`code`。

我们还可以添加一些选项，如标签尺寸的`tabSize`。

对于我们正在显示的语言。

`theme`为显示主题。

`lineNumber`表示我们是否显示行号。

它还带有一个合并模式，让我们可以并排比较代码。

为了使用它，我们可以写:

```
<template>
  <div>
    <codemirror merge :options="cmOption" @scroll="onCmScroll"></codemirror>
  </div>
</template>

<script>
import "codemirror/addon/merge/merge.js";
import "codemirror/addon/merge/merge.css";
import DiffMatchPatch from "diff-match-patch";
window.diff_match_patch = DiffMatchPatch;
window.DIFF_DELETE = -1;
window.DIFF_INSERT = 1;
window.DIFF_EQUAL = 0;
export default {
  data() {
    return {
      cmOption: {
        value: "<p>james</p>",
        origLeft: null,
        orig: "<p>james smith</p>",
        connect: "align",
        mode: "text/html",
        lineNumbers: true,
        collapseIdentical: false,
        highlightDifferences: true
      }
    };
  },
  methods: {
    onCmScroll() {
      console.log("onCmScroll");
    }
  }
};
</script>
```

`value`有了新代码。

`orig`有原码。

`collapseIdentical`让我们默认折叠相同的代码。

`highlightDifference`让我们突出显示原始代码和当前代码之间的任何代码差异。

# vue 通知

vue-notification 是一个让我们显示通知的库。

要安装它，我们运行:

```
npm i vue-notification
```

然后我们可以通过写来使用它:

`main.js`

```
import Vue from "vue";
import App from "./App.vue";
import Notifications from "vue-notification";Vue.use(Notifications);
Vue.config.productionTip = false;new Vue({
  render: h => h(App)
}).$mount("#app");
```

`App.vue`

```
<template>
  <div>
    <notifications group="foo"/>
  </div>
</template>

<script>
export default {
  mounted() {
    this.$notify({
      group: "foo",
      title: "title",
      text: "Hello"
    });
  }
};
</script>
```

我们添加了带有`group`支柱的`notification`组件。

然后，当我们调用`this.$notify`来显示通知时，我们使用相同的`group`值。

`title`是标题，`text`是内容。

我们可以用生成的类来设计它的样式。

我们还可以通过填充它附带的插槽来创建自定义通知:

```
<template>
  <div>
    <notifications group="foo" position="top left">
      <template slot="body" slot-scope="props">
        <div>
          <a class="title">{{props.item.title}}</a>
          <a class="close" @click="props.close">close</a>
          <div v-html="props.item.text"></div>
        </div>
      </template>
    </notifications>
  </div>
</template>

<script>
export default {
  mounted() {
    this.$notify({
      group: "foo",
      title: "title",
      text: "Hello"
    });
  }
};
</script>
```

我们用自己的内容填充了`body`槽。

它带有一个`props.close`来关闭通知。

宽度和速度可以改变。

# vue-调整大小

vue-resize 让我们可以观察正在调整大小的元素。

要安装它，我们运行:

```
npm i vue-resize
```

然后，我们可以通过以下方式使用它:

`main.js`

```
import Vue from "vue";
import App from "./App.vue";
import { ResizeObserver } from "vue-resize";Vue.component("resize-observer", ResizeObserver);
Vue.config.productionTip = false;new Vue({
  render: h => h(App)
}).$mount("#app");
```

`App.vue`

```
<template>
  <div>
    <resize-observer @notify="handleResize"/>
  </div>
</template>

<script>
export default {
  methods: {
    handleResize({ width, height }) {
      console.log(width, height);
    }
  }
};
</script> 

 <style scoped>
div {
  position: relative;
  width: 100vw;
}
</style>
```

我们监听用`resize-observer`组件调整大小的元素。

![](img/84662e4137f955b41d5235e095256c1d.png)

Photo by [AZGAN MjESHTRI](https://unsplash.com/@azganmjeshtri?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 结论

vue-codemirror 允许我们添加一个支持区分的代码编辑器。

vue-notification 允许我们添加任何种类的通知。

vue-resize 让我们可以观察正在调整大小的元素。

## 简单英语的 JavaScript

喜欢这篇文章吗？如果是这样，通过 [**订阅我们的 YouTube 频道**](https://www.youtube.com/channel/UCtipWUghju290NWcn8jhyAw) **获取更多类似内容！**