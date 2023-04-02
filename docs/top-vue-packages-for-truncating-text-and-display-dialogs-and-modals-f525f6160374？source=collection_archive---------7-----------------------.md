# 截断文本、显示对话框和模态的顶级 Vue 包

> 原文：<https://javascript.plainenglish.io/top-vue-packages-for-truncating-text-and-display-dialogs-and-modals-f525f6160374?source=collection_archive---------7----------------------->

![](img/6be1c87eded7419717fdcf4640932dab.png)

Photo by [Clem Onojeghuo](https://unsplash.com/@clemono2?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

Vue.js 是一个易于使用的 web 应用框架，我们可以用它来开发交互式前端应用。

在本文中，我们将看看添加对话框的最佳包，从树中选择项目，并显示截断的文本。

# 薄莫代尔

vue-thin-modal 是一个简单的模态组件，我们可以在我们的 vue 应用程序中使用。

为了使用它，我们运行:

```
npm i vue-thin-modal
```

来安装它。

然后我们写道:

`main.js`

```
import Vue from "vue";
import App from "./App.vue";
import VueThinModal from "vue-thin-modal";
import "vue-thin-modal/dist/vue-thin-modal.css";Vue.use(VueThinModal);
Vue.config.productionTip = false;new Vue({
  render: h => h(App)
}).$mount("#app");
```

`App.vue`

```
<template>
  <div>
    <button type="button" [@click](http://twitter.com/click)="open">Open Modal</button>
    <modal name="hello">
      <div class="basic-modal">
        <h1 class="title">hello</h1>
        <button class="button" type="button" @click="close">Close Modal</button>
      </div>
    </modal>
  </div>
</template>

<script>
export default {
  methods: {
    open() {
      this.$modal.push("hello");
    }, close() {
      this.$modal.pop();
    }
  }
};
</script>
```

我们导入组件和它的 CSS。

然后我们显示带有`modal`组件的模态。

内容在标签之间。

我们调用`this.$modal.push`来显示它。参数是模态的`name`道具的值。

我们调用`this.$modal.pop`来关闭它。

# 真空夹钳

vue-clamp 是一个简单的截断文本的指令。

为了使用它，我们运行:

```
npm i vue-clamp
```

来安装它。

然后我们写道:

```
<template>
  <v-clamp autoresize :max-lines="1">{{ text }}</v-clamp>
</template><script>
import VClamp from "vue-clamp";export default {
  components: {
    VClamp
  },
  data() {
    return {
      text: `Lorem ipsum dolor sit amet, consectetur adipiscing elit. Duis nibh neque, dignissim sit amet libero at, aliquet tempus urna. Aenean luctus accumsan feugiat. Nunc dui purus, imperdiet eu suscipit et, convallis vehicula nulla. Quisque nec nisi elementum, tincidunt est eu, sollicitudin enim. Sed fermentum massa urna, at malesuada leo egestas vitae. Aenean vulputate sem libero, nec sagittis arcu varius et. Donec et felis augue. Nunc euismod gravida nibh, id volutpat mauris placerat nec. Integer sit amet purus non ligula ullamcorper tempor in at massa. Donec pellentesque convallis pharetra.`
    };
  }
};
</script>
```

截断我们的文本。

我们使用带有`autoresize`道具的`v-clamp`组件使其自动调整大小。

`max-line`设置显示的最大行数。

# vue-tree-halower

vue-tree-halower 是一个输入组件，我们可以从树结构中选择值。

要使用它，我们可以运行:

```
npm i vue-tree-halower
```

来安装它。

然后我们可以写:

`main.js`

```
import Vue from "vue";
import App from "./App.vue";
import { VTree, VSelectTree } from "vue-tree-halower";
import "vue-tree-halower/dist/halower-tree.min.css";
Vue.use(VTree);
Vue.use(VSelectTree);
Vue.config.productionTip = false;new Vue({
  render: h => h(App)
}).$mount("#app");
```

`App.vue`

```
<template>
  <div>
    <v-select-tree :data="treeData" v-model="selected"/>
  </div>
</template><script>
export default {
  data() {
    return {
      treeData: [
        {
          title: "fruit",
          expanded: true,
          children: [
            {
              title: "apple",
              expanded: true,
              children: [
                {
                  title: "mcintosh"
                },
                {
                  title: "golden delicious"
                }
              ]
            },
            {
              title: "drink",
              children: [
                {
                  title: "<span style='color: red'>beer</span>"
                },
                {
                  title: "<span style='color: green'>wine</span>"
                }
              ]
            }
          ]
        }
      ],
      selected: []
    };
  }
};
</script>
```

我们使用`v-select-item`组件来添加组件。

然后我们将`data`属性设置为`tree`数据来填充选择。

`title`显示为节点的名称。

`children`是节点的子节点。

`expanded`表示如果是`true`则树节点展开。

`v-model`绑定到选择的选项。

它包括许多其他选择或定制。

# Vuejs 对话框插件

Vuejs 对话框插件是一个插件，为我们提供了一个基于承诺的对话框。

为了使用它，我们运行:

```
npm i vuejs-dialog
```

然后我们通过书写来使用它:

`main.js`

```
import Vue from "vue";
import App from "./App.vue";
import VuejsDialog from "vuejs-dialog";
import "vuejs-dialog/dist/vuejs-dialog.min.css";
Vue.use(VuejsDialog);
Vue.config.productionTip = false;new Vue({
  render: h => h(App)
}).$mount("#app");
```

`App.vue`

```
<template>
  <div></div>
</template><script>
export default {
  async mounted() {
    const dialog = this.$dialog.alert("alert");
    console.log("closed");
  }
};
</script>
```

我们导入插件和样式。

然后我们可以调用`this.$dialog.alert`来显示对话框。

![](img/7423a418e499cb9b8406f3b7d0f15b1b.png)

Photo by [Clem Onojeghuo](https://unsplash.com/@clemono2?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 结论

vue-thin-modal 允许我们向我们的 vue 应用程序添加一个简单的模型。

vue-clamp 是一个让我们显示截断文本的组件。

vue-tree-halower 是一个选择输入，让我们从树中选择项目。

Vuejs 对话插件是一个基于承诺的对话插件。

## 简单英语的 JavaScript

喜欢这篇文章吗？如果是这样，通过 [**订阅解码获得更多类似内容，我们的 YouTube 频道**](https://www.youtube.com/channel/UCtipWUghju290NWcn8jhyAw) **！**