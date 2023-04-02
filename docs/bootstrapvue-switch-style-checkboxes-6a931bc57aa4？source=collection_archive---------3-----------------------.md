# BootstrapVue —切换样式复选框

> 原文：<https://javascript.plainenglish.io/bootstrapvue-switch-style-checkboxes-6a931bc57aa4?source=collection_archive---------3----------------------->

![](img/bbbd98d565d2465a68a5ccf9e9c827b7.png)

Photo by [Dan Tuykavin](https://unsplash.com/@slow_d?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

为了制作好看的 Vue 应用，我们需要设计组件的样式。

为了使我们的生活更容易，我们可以使用内置样式的组件。

在本文中，我们将了解如何在 BootstrapVue 中创建开关样式的复选框。

# 成组的开关样式复选框

要制作一组 os 切换样式的复选框，我们可以将`switches`属性添加到`b-form-checkbox-group`中。

例如，我们可以写:

```
<template>
  <div id="app">
    <b-form-checkbox-group v-model="selected" :options="options" switches></b-form-checkbox-group>
    {{selected}}
  </div>
</template>
<script>
export default {
  name: "App",
  data() {
    return {
      selected: [],
      options: ["apple", "orange", "grape"]
    };
  }
};
</script>
```

因为`b-form-checkbox-group`上的`switches`道具，我们有一组看起来像拨动开关的复选框。

`options`设置复选框选项。

然后，当我们打开和关闭开关时，我们会看到所选的选项显示在其下方。

# 交换机规模

`size`道具让我们调整大小。

所以我们可以写:

```
<template>
  <div id="app">
    <b-form-checkbox switch size="sm">Small</b-form-checkbox>
  </div>
</template>
<script>
export default {
  name: "App"
};
</script>
```

我们可以看到一个特别小的开关，因为我们有`size='sm'`。

或者，我们可以写`size='lg'`来做一个超大的开关。

# 普通支票输入

我们可以使用`plain`属性禁用复选框的引导样式。

例如，我们可以写:

```
<template>
  <div id="app">
    <b-form-checkbox-group v-model="selected" :options="options" plain></b-form-checkbox-group>
  </div>
</template>
<script>
export default {
  name: "App",
  data() {
    return {
      selected: [],
      options: ["apple", "orange", "grape"]
    };
  }
};
</script>
```

我们在`b-form-checkbox-group`上有`plain`属性，所以我们显示了浏览器本地复选框。

# 上下文状态

我们可以添加一个`state`道具来向用户显示验证状态。

道具应添加到`b-form-checkbox-group`、`b-form-invalid-feedback`和`b-form-valid-feedaback`组件中。

例如，我们可以写:

```
<template>
  <div id="app">
    <b-form-checkbox-group
      v-model="selected"
      :options="options"
      :state="state"
      name="checkbox-validation"
    >
      <b-form-invalid-feedback :state="state">please select 1 or more</b-form-invalid-feedback>
      <b-form-valid-feedback :state="state">looks good</b-form-valid-feedback>
    </b-form-checkbox-group>
  </div>
</template>
<script>
export default {
  name: "App",
  data() {
    return {
      selected: [],
      options: ["apple", "orange", "grape"]
    };
  },
  computed: {
    state() {
      return this.selected.length > 0;
    }
  }
};
</script>
```

我们将`state`道具添加到所有 3 个组件中。

通过检查`this.state.length`来计算`state`值。

如果我们没有选择任何内容，我们会看到所有内容都显示为红色，并且我们会看到“请选择 1 个或更多”。

否则，我们会看到“看起来不错”,所有东西都显示为绿色。

# 三态支持

我们可以在复选框中添加一个不确定的状态。

例如，我们可以写:

```
<template>
  <div id="app">
    <b-form-checkbox v-model="checked" :indeterminate="indeterminate">click me</b-form-checkbox>
    <b-button @click="indeterminate  = !indeterminate ">toggle indeterminate</b-button>
    <p>{{checked}}</p>
  </div>
</template>
<script>
export default {
  name: "App",
  data() {
    return {
      checked: false,
      indeterminate: false
    };
  }
};
</script>
```

我们有设置到`indeterminate`字段的`indeterminate`属性。

如果我们单击“切换不确定”，我们会看到复选框中显示一个破折号。

指示复选框处于不确定状态。

![](img/d4f9e7e62c9571b948a62330bb6a0b50.png)

Photo by [Brooke Lark](https://unsplash.com/@brookelark?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 结论

我们可以用`indeterminate`道具制作一个三态复选框。

此外，我们可以制作一组开关样式的复选框。

## 简单英语的 JavaScript

你知道我们有四份出版物和一个 YouTube 频道吗？在 [**plainenglish.io**](https://plainenglish.io/) 和 [**找到他们订阅我们的 YouTube 频道**](https://www.youtube.com/channel/UCtipWUghju290NWcn8jhyAw) **！**