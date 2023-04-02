# BootstrapVue —微调按钮定制

> 原文：<https://javascript.plainenglish.io/bootstrapvue-spin-button-customization-6a53d799ff0a?source=collection_archive---------3----------------------->

![](img/6a4b6e5f4c1dd99c697d03bb6fa8a91a.png)

Photo by [Srecko Skrobic](https://unsplash.com/@sreckoskrobic?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

为了制作好看的 Vue 应用，我们需要设计组件的样式。

为了使我们的生活更容易，我们可以使用内置样式的组件。

在这篇文章中，我们将看看如何自定义一个微调按钮。

# 什么是旋转按钮？

数值调节钮是一种数值范围表单控件。

我们可以将它包含在`b-form-spinbutton`组件中。

例如，我们可以写:

```
<b-form-spinbutton v-model="value" min="1" max="100"></b-form-spinbutton>
```

我们用`min`和`max`道具设置最小和最大允许值。

`v-model`将输入值绑定到`value`。

然后我们看到一个表单控件，左边有一个减号按钮，右边有一个加号按钮。

输入的数字将显示在中间。

# 禁用它

我们可以禁用微调按钮，这意味着它是非交互式的。

比如说。我们可以写:

```
<template>
  <div id="app">
    <b-form-spinbutton disabled v-model="value" min="1" max="100"></b-form-spinbutton>
    <p>Value: {{ value }}</p>
  </div>
</template>
<script>
export default {
  name: "App",
  data() {
    return {
      value: 0
    };
  }
};
</script>
```

我们添加了`disabled`道具，使其不可交互。

# 只读

我们也可以将其设置为只读。

这意味着我们可以关注它，但我们不能选择一个值。

例如，我们可以用`readonly`道具来做这件事:

```
<template>
  <div id="app">
    <b-form-spinbutton readonly v-model="value" min="1" max="100"></b-form-spinbutton>
    <p>Value: {{ value }}</p>
  </div>
</template>
<script>
export default {
  name: "App",
  data() {
    return {
      value: 0
    };
  }
};
</script>
```

# 验证状态

我们可以显示验证状态。

为此，我们设置了`state`道具。

例如，我们可以写:

```
<template>
  <div id="app">
    <b-form-spinbutton :state='isValid' v-model="value" min="1" max="10"></b-form-spinbutton>
    <p>Value: {{ value }}</p>
  </div>
</template>
<script>
export default {
  name: "App",
  data() {
    return {
      value: 0
    };
  },
  computed: {
    isValid(){
      return !!this.value;
    }
  }
};
</script>
```

我们将`state`属性设置为计算出的`isValid`属性。

当我们设定一个数字时，一切都是绿色的。

否则，一切都是红色的。

# 事件

`b-form-spinbutton`组件发出一些事件。

当用户按下递增或递减按钮释放鼠标按钮时，它发出`change`事件。

例如，我们可以写:

```
<template>
  <div id="app">
    <b-form-spinbutton wrap @change="value2 = $event" v-model="value" min="1" max="10"></b-form-spinbutton>
    <p>Value 1: {{ value }}</p>
    <p>Value 2: {{ value2 }}</p>
  </div>
</template>
<script>
export default {
  name: "App",
  data() {
    return {
      value: 0,
      value2: 0
    };
  }
};
</script>
```

`$event`变量具有所选数字的值。

我们将它赋给了`value2`，这样我们就可以在底部的 p 元素中显示它。

![](img/cc763473cc2d8d46393f9609a5c5be22.png)

Photo by [Behzad Ghaffarian](https://unsplash.com/@behz?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 结论

我们可以监听由`b-form-spinbutton`发出的事件，并用发出的值做一些事情。

此外，我们可以将表单控件设置为禁用或只读。

只读意味着我们可以专注但不能做任何事情。

禁用意味着它根本不是交互式的。

我们还可以用`state`属性显示控件的验证状态。

## 简单英语的 JavaScript

你知道我们有四份出版物和一个 YouTube 频道吗？在[**plain English . io**](https://plainenglish.io/)和 [**找到它们订阅解码，我们的 YouTube 频道**](https://www.youtube.com/channel/UCtipWUghju290NWcn8jhyAw) **！**