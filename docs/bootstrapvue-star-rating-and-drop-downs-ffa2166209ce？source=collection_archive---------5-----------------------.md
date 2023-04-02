# BootstrapVue —星级和下拉列表

> 原文：<https://javascript.plainenglish.io/bootstrapvue-star-rating-and-drop-downs-ffa2166209ce?source=collection_archive---------5----------------------->

![](img/3d59307259ec293b2e94988c7ccac76b.png)

Photo by [Lily Banse](https://unsplash.com/@lvnatikk?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

为了制作好看的 Vue 应用，我们需要设计组件的样式。

为了使我们的生活更容易，我们可以使用内置样式的组件。

在本文中，我们将了解如何定制星级输入和添加下拉菜单。

# 清除按钮

通过`show-clear`按钮，我们可以添加一个清除按钮来重置所选的评级。

例如，我们可以写:

```
<template>
  <div id="app">
    <b-form-rating id="rating-inline"  show-clear show-value  value="4"></b-form-rating>
  </div>
</template><script>
export default {
  name: "App",
  data() {
    return {
      value: 4
    };
  }
};
</script>
```

左边有一个“x”按钮，我们可以点击它来清除评级。

# 核标准情报中心

分级中的图标可以更改。

因此，我们可以显示除了星星以外的东西。

我们必须注册`IconsPlugin`来改变图标。

所以我们要写:

```
import Vue from "vue";
import App from "./App.vue";
import { BootstrapVue, IconsPlugin } from "bootstrap-vue";
import "bootstrap/dist/css/bootstrap.css";
import "bootstrap-vue/dist/bootstrap-vue.css";Vue.use(BootstrapVue);
Vue.use(IconsPlugin);
Vue.config.productionTip = false;new Vue({
  render: h => h(App)
}).$mount("#app");
```

在`main.js`。

然后在我们的组件中，我们编写:

```
<template>
  <div id="app">
    <b-form-rating
      icon-empty="heart"
      icon-half="heart-half"
      icon-full="heart-fill"
      icon-clear="x-square"
      precision="2"
      show-clear
      show-value
      v-model='value'
    ></b-form-rating>
  </div>
</template><script>
export default {
  name: "App",
  data() {
    return {
      value: 3.5
    };
  }
};
</script>
```

我们设置了`icon-empty`、`icon-half`、`icon-full`和`icon-clear`道具。来改变图标。

`icon-empty`用于未填充的图标。

`icon-half`用于半填充图标。

`icon-full`用于完全填充的图标。

`icon-clear`是清除按钮的图标。

# 国际化

按钮的区域设置可以更改。

我们可以设置`locale`属性来设置区域设置。

例如，我们可以写:

```
<template>
  <div id="app">
    <b-form-rating :locale="locale" v-model="value"></b-form-rating>
  </div>
</template><script>
export default {
  name: "App",
  data() {
    return {
      locale: "en-US"
    };
  }
};
</script>
```

设置区域设置字符串。

# 选择下拉菜单

BootstrapVue 为我们提供了一个`b-form-select`组件，让我们向表单中添加下拉菜单。

例如，我们可以写:

```
<template>
  <div id="app">
    <b-form-select v-model="selected" :options="options"></b-form-select>
    <p>{{ selected }}</p>
  </div>
</template><script>
export default {
  name: "App",
  data() {
    return {
      selected: null,
      options: [
        { value: null, text: "Select a Fruit" },
        { value: "apple", text: "Apple" },
        { value: "orange", text: "Orange" },
        { value: "grape", text: "Grape", disabled: true }
      ]
    };
  }
};
</script>
```

创建下拉菜单。

我们有一个`options`数组，它有`value`和`text`属性。

显示的是`text`属性。

`value`是设置为状态值的值。

如果是`true`，则`disabled`会阻止选项被选中。

我们将数组设置为`options`属性的值。

选定的值显示在 p 元素中。

尺寸可以像其他部件一样用`size`支柱设定。

# 选择

我们可以用`b-form-select-option`组件单独添加选项元素。

例如，我们可以写:

```
<template>
  <div id="app">
    <b-form-select v-model="selected" :options="options">
      <b-form-select-option :value="null">Select a Fruit</b-form-select-option>
      <b-form-select-option value="apple">Apple</b-form-select-option>
      <b-form-select-option value="orange">Orange</b-form-select-option>
      <b-form-select-option value="grape" disabled>Grape</b-form-select-option>
    </b-form-select>
    <p>{{ selected }}</p>
  </div>
</template><script>
export default {
  name: "App",
  data() {
    return {
      selected: null
    };
  }
};
</script>
```

我们有`b-form-select-option`组件。

`value`属性是绑定到`selected`状态的值。

`disabled`停止选择选项。

# 选项组

`b-form-select-option-group`让我们添加选项组。

例如，我们可以写:

```
<template>
  <div id="app">
    <b-form-select v-model="selected" :options="options">
      <b-form-select-option :value="null">Select a Fruit</b-form-select-option>
      <b-form-select-option value="apple">apple</b-form-select-option>
      <b-form-select-option-group label="orange">
        <b-form-select-option :value="{orange: 'juice'}">orange juice</b-form-select-option>
        <b-form-select-option :value="{ orange: 'ice cream' }">orange ice cream</b-form-select-option>
      </b-form-select-option-group>
    </b-form-select>
    <p>{{ selected }}</p>
  </div>
</template><script>
export default {
  name: "App",
  data() {
    return {
      selected: null
    };
  }
};
</script>
```

添加选项组。

我们有`b-form-select-option-group`和`b-form-select-option`组件。

`label`道具显示为组的标题。

![](img/8089a694007e2e36597964e30492707d.png)

Photo by [Mike Benna](https://unsplash.com/@mbenna?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 结论

我们可以通过添加图标和改变大小来自定义星级组件。

## 简单英语的 JavaScript

你知道我们有四份出版物和一个 YouTube 频道吗？在 [**plainenglish.io**](https://plainenglish.io/) 和 [**找到它们订阅我们的 YouTube 频道**](https://www.youtube.com/channel/UCtipWUghju290NWcn8jhyAw) **！**