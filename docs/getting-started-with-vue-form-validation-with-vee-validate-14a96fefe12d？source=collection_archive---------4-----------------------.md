# 使用 Vee-Validate 开始 Vue 表单验证

> 原文：<https://javascript.plainenglish.io/getting-started-with-vue-form-validation-with-vee-validate-14a96fefe12d?source=collection_archive---------4----------------------->

![](img/8832e8c8343a59114c1176483fd1ce66.png)

Photo by [Jordan Rowland](https://unsplash.com/@yakimadesign?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

表单验证是 Vue.js 没有内置的功能。

但是，我们还是非常需要这个功能。

在本文中，我们将看看如何用 Vee-Validate 库添加基本的表单验证。

# 入门指南

我们可以从安装包开始。

要安装它，我们运行:

```
npm install vee-validate --save
```

然后我们可以通过编写以下代码来注册`ValidationProvider`插件:

```
import Vue from "vue";
import App from "./App.vue";
import { ValidationProvider } from "vee-validate";Vue.component("ValidationProvider", ValidationProvider);
Vue.config.productionTip = false;new Vue({
  render: h => h(App)
}).$mount("#app");
```

在`main.js`里。

这样，我们可以在整个应用程序中使用它。

我们也可以导入`extend`来创建我们自己的验证规则:

```
import Vue from "vue";
import App from "./App.vue";
import { ValidationProvider, extend } from "vee-validate";extend("secret", {
  validate: value => value === "secret",
  message: "wrong word"
});Vue.component("ValidationProvider", ValidationProvider);
Vue.config.productionTip = false;new Vue({
  render: h => h(App)
}).$mount("#app");
```

`value`有输入的值，如果有效，我们返回`true`，否则返回`false`。

`message`是我们将要显示的表单验证错误消息。

```
Vue.component(“ValidationProvider”, ValidationProvider);
```

将使`ValidationProvider`组件全球可用。

# 基本用法

我们可以通过编写以下代码将`ValidationProvider`组件添加到我们的应用程序中:

```
<template>
  <div>
    <ValidationProvider rules="secret" v-slot="{ errors }">
      <input v-model="email" type="text">
      <span>{{ errors[0] }}</span>
    </ValidationProvider>
  </div>
</template>
<script>
export default {
  data(){
    return {
      email: ''
    }
  }
};
</script>
```

如果我们键入“secret ”,那么我们将什么也看不到。

否则，我们会看到我们之前设置的错误消息“单词错误”。

`errors`拥有我们之前拥有的验证器返回的错误消息。

# 组件注册

如果我们只想在一个组件中使用`ValidationProvider`组件，我们可以写:

```
import { ValidationProvider } from 'vee-validate';export default {
  components: {
    ValidationProvider
  }
};
```

在组件中注册`ValidationProvider`。

# 规则参数

我们可以给规则添加参数。

例如，我们可以写:

```
extend("minLength", {
  validate(value, args) {
    return value.length >= args.length;
  },
  params: ["length"],
  message: "too short"
});
```

我们有一个带有`args`参数的`validate`方法。

`args`可以具有由`params`属性指定的任何属性。

因此，在这种情况下，我们可以拥有`length`属性 ion `args`。

要使用它，我们可以写:

```
<template>
  <div>
    <ValidationProvider rules="minLength:5" v-slot="{ errors }">
      <input v-model="value" type="text">
      <span>{{ errors[0] }}</span>
    </ValidationProvider>
  </div>
</template>
<script>
export default {
  data() {
    return {
      value: ""
    };
  }
};
</script>
```

我们有:

```
minLength:5
```

将`args.length`设置为 5。

所以如果`value`的长度小于 5，我们会得到一个错误信息。

# 多个参数

在我们的`validate`方法中，我们可以有多个参数。

例如，我们可以写:

```
extend("length", {
  validate(value, { min, max }) {
    return value.length >= min && value.length <= max;
  },
  params: ["min", "max"],
  message: "too short"
});
```

然后我们可以通过写来使用它:

```
<template>
  <div>
    <ValidationProvider rules="length:5,10" v-slot="{ errors }">
      <input v-model="value" type="text">
      <span>{{ errors[0] }}</span>
    </ValidationProvider>
  </div>
</template>
<script>
export default {
  data() {
    return {
      value: ""
    };
  }
};
</script>
```

我们在论证中将`args`重组为`min`和`max`。

然后我们传入用逗号分隔的参数。

然后，如果我们输入少于 5 个字符或多于 10 个字符的内容，我们会看到一条错误消息。

Vee-Validate 会自动传入任意数量的参数，并将其放入`args`。

但是将属性名放在`params`中会让我们通过键获取，而不是以数组的形式获取。

所以我们可以这样写:

```
rules="one_of:1,2,3,4,5,6,7,8"
```

如果我们为`one_of`规则定义了一个`validate`方法，那么`args`将会有`[1, 2, 3, 4, 5, 6, 7, 8]`。

# 信息

我们可以在验证函数中返回消息，作为在`message`中设置它们的替代方法。

例如，我们可以写:

```
import Vue from "vue";
import App from "./App.vue";
import { ValidationProvider, extend } from "vee-validate";extend("positive", value => {
  if (value >= 0) {
    return true;
  }
  return "please enter a positive number";
});
Vue.component("ValidationProvider", ValidationProvider);
Vue.config.productionTip = false;new Vue({
  render: h => h(App)
}).$mount("#app");
```

然后我们可以写:

```
<template>
  <div>
    <ValidationProvider rules="positive" v-slot="{ errors }">
      <input v-model="value" type="text">
      <span>{{ errors[0] }}</span>
    </ValidationProvider>
  </div>
</template>
<script>
export default {
  data() {
    return {
      value: ""
    };
  }
};
</script>
```

如果我们输入的不是正数，我们会看到“请输入一个正数”。

![](img/45c0ab4a4a0ce0988be0a3181ea54a96.png)

Photo by [Melanie Medina](https://unsplash.com/@beholdherlife?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 结论

Vee-Validate 是一个易于使用的包，用于向 Vue 应用程序添加表单验证功能。

这比用我们自己的代码验证输入要容易得多。

## 简单英语的 JavaScript

你知道我们有四份出版物和一个 YouTube 频道吗？在 [**plainenglish.io**](https://plainenglish.io/) 和 [**找到它们订阅我们的 YouTube 频道**](https://www.youtube.com/channel/UCtipWUghju290NWcn8jhyAw) **！**