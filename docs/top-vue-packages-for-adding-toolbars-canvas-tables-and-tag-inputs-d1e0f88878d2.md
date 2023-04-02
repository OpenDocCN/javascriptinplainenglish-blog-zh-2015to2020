# 用于添加工具栏、画布、表格和标签输入的顶级 Vue 包

> 原文：<https://javascript.plainenglish.io/top-vue-packages-for-adding-toolbars-canvas-tables-and-tag-inputs-d1e0f88878d2?source=collection_archive---------11----------------------->

![](img/40415c0c7fb0801d5569305d879ce268.png)

Photo by [Natalie Thornley](https://unsplash.com/@natthornley?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

Vue.js 是一个易于使用的 web 应用框架，我们可以用它来开发交互式前端应用。

在这篇文章中，我们将看看添加工具栏、画布、表格和标签输入的最佳包。

# SimplebarVue

SimplebarVue 允许我们在 Vue 应用程序中添加工具栏。

要安装它，我们运行:

```
npm i simplebar-vue
```

然后我们可以通过写来使用它:

`main.js`

```
<template>
  <simplebar data-simplebar-auto-hide="false">toolbar</simplebar>
</template><script>
import simplebar from "simplebar-vue";
import "simplebar/dist/simplebar.min.css";export default {
  components: {
    simplebar
  }
};
</script>
```

我们只需导入`simplebar`组件并使用它。

# 武埃孔瓦

vue-konva 是一个库，它使得使用 HTML 画布比使用内置库容易得多。

要使用它，我们首先通过运行以下命令来安装它:

```
npm i vue-konva konva
```

Konva 是 vue-konva 的必需依赖项。

然后我们可以通过写来使用它:

`main.js`

```
import Vue from "vue";
import App from "./App.vue";
import VueKonva from "vue-konva";Vue.use(VueKonva);
Vue.config.productionTip = false;new Vue({
  render: h => h(App)
}).$mount("#app");
```

我们注册了`VueKonva`插件。

然后我们可以使用内置组件购买编写:

```
<template>
  <div>
    <v-stage :config="configKonva">
      <v-layer>
        <v-circle :config="configCircle"></v-circle>
      </v-layer>
    </v-stage>
  </div>
</template><script>
export default {
  data() {
    return {
      configKonva: {
        width: 500,
        height: 500
      },
      configCircle: {
        x: 150,
        y: 150,
        radius: 70,
        fill: "red",
        stroke: "green",
        strokeWidth: 2
      }
    };
  }
};
</script>
```

我们通过使用`v-stage`和`v-layer`组件来设置画布。

在它里面，我们有`v-circle`组件来创建一个圆。

`configKonva`设置画布的大小。

`configCircle`设置圆的选项。

我们将圆圈填充为红色，边框填充为绿色。

`x`是中心的 x 坐标。

`y`是中心的 y 坐标。

# vue-表格-组件

vue-table-component 是一个表格组件，具有排序和过滤功能。

要安装它，我们运行:

```
npm i vue-table-component moment
```

Moment 是 vue-table-component 的必需依赖项。

然后我们可以通过写来使用它:

`main.js`

```
import Vue from "vue";
import App from "./App.vue";
import TableComponent from "vue-table-component";Vue.use(TableComponent);
Vue.config.productionTip = false;new Vue({
  render: h => h(App)
}).$mount("#app");
```

`App.vue`

```
<template>
  <div>
    <table-component :data="data" sort-by="firstName" sort-order="asc">
      <table-column show="firstName" label="First name"></table-column>
      <table-column show="lastName" label="Last name"></table-column>
      <table-column label :sortable="false" :filterable="false">
        <template slot-scope="row">
          <a :href="`#${row.firstName}`">Edit</a>
        </template>
      </table-column>
    </table-component>
  </div>
</template><script>
export default {
  data() {
    return {
      data: [
        {
          firstName: "john",
          lastName: "smith"
        },
        {
          firstName: "mary",
          lastName: "green"
        },
        {
          firstName: "alex",
          lastName: "wong"
        },
        {
          firstName: "jane",
          lastName: "doe"
        }
      ]
    };
  }
};
</script>
```

现在我们有了一个内置了排序和过滤功能的表。

`sort-by`让我们设置排序依据字段。

`sort-order`是排序顺序。

`table-column`设置表格列。

`show`具有我们想要显示的条目的属性。

`sortable`让我们设置列是否可排序。

`filterable`让我们设置列是否可过滤。

还有很多其他的选择。

# vue-标签-输入

vue-tags-input 允许我们向我们的 vue 应用程序添加标签输入。

要安装它，我们运行:

```
npm i @vojtechlanka/vue-tags-input
```

为了使用它，我们写下:

`main.js`

```
<template>
  <div>
    <vue-tags-input
      v-model="tag"
      :tags="tags"
      :is-draggable="true"
      @tags-changed="newTags => tags = newTags"
      @tag-order-changed="newTags => tags = newTags"
    />
    <p>{{tags}}</p>
  </div>
</template>
<script>
import VueTagsInput from "@vojtechlanka/vue-tags-input";export default {
  components: {
    VueTagsInput
  },
  data() {
    return {
      tag: "",
      tags: []
    };
  }
};
</script>
```

`tags-changed`通过添加或其他方式改变标签时发出。

`tag-order-changed`通过拖动改变顺序时发出。

`v-model`绑定标签的输入值被输入。

`is-draggable`设置为`true`使标签可拖动。

![](img/f6db4fe83a70f4bbd7df81ab58ecfcef.png)

Photo by [Srecko Skrobic](https://unsplash.com/@sreckoskrobic?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 结论

SimplebarVue 是一个用于添加工具栏的 Vue 组件。

vue-konva 是一个让我们在 vue 应用程序中使用画布的包。

vue-table-component 让我们可以轻松地添加带有排序和过滤的表格。

vue-tags-input 允许我们向应用程序添加输入。

## 简单英语的 JavaScript

喜欢这篇文章吗？如果是这样，通过 [**订阅解码获得更多类似内容，我们的 YouTube 频道**](https://www.youtube.com/channel/UCtipWUghju290NWcn8jhyAw) **！**