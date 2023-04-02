# Vuelidate —自定义验证器

> 原文：<https://javascript.plainenglish.io/vuelidate-custom-validators-f99132e40189?source=collection_archive---------7----------------------->

![](img/7efe781dc703c35511bbde09d2039ef1.png)

Photo by [Gijs Coolen](https://unsplash.com/@gijsparadijs?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

默认情况下，Vue.js 没有任何表单验证功能。

因此，我们需要添加自己的表单验证库或自己的代码。

在本文中，我们将看看如何用 Vuelidate 创建定制的验证器。

# **自定义验证器**

我们可以用 Vuelidate 创建自己的定制验证器。

例如，我们可以写:

```
<template>
  <div id="app">
    <div>
      <label>name</label>
      <input v-model.trim="$v.name.$model">
      <div v-if="!$v.name.required">name is required.</div>
      <div v-if="!$v.name.mustBeCool">name is invalid.</div>
    </div>
  </div>
</template>
<script>
import { required } from "vuelidate/lib/validators";
const mustBeCool = value => value.includes("cool");export default {
  name: "App",
  data() {
    return {
      name: ""
    };
  },
  validations: {
    name: {
      required,
      mustBeCool
    }
  }
};
</script>
```

我们创建了`mustBeCool`函数来验证`name`字段。

`value`有输入值。

我们只是返回我们期望在函数中有效的内容。

# 可选验证器

我们还可以用自定义验证器来验证可选字段。

为此，我们使用 Vuelidate 的一个助手函数。

例如，我们可以写:

```
<template>
  <div id="app">
    <div>
      <label>name</label>
      <input v-model.trim="$v.name.$model">
      <div v-if="!$v.name.mustBeCool">name is invalid.</div>
    </div>
  </div>
</template>
<script>
import { helpers } from "vuelidate/lib/validators";
const mustBeCool = value => !helpers.req(value) || value.includes("cool");export default {
  name: "App",
  data() {
    return {
      name: ""
    };
  },
  validations: {
    name: {
      mustBeCool
    }
  }
};
</script>
```

使`name`成为可选字段。

我们只验证`value`是否为非空字符串。

`helpers.req(value)`检查是否需要该值。

否认这一点意味着我们使字段可选。

# 额外参数

我们可以在字段中添加额外的参数。

例如，我们可以写:

```
<template>
  <div id="app">
    <div>
      <label>name</label>
      <input v-model.trim="$v.name.$model">
      <div v-if="!$v.name.mustBeCool">name is invalid.</div>
    </div>
  </div>
</template>
<script>
import { helpers } from "vuelidate/lib/validators";
const contains = param => value => !helpers.req(value) || value.includes(param);
export default {
  name: "App",
  data() {
    return {
      name: ""
    };
  },
  validations: {
    name: {
      mustBeCool: contains("cool")
    }
  }
};
</script>
```

向代码中添加一个参数进行验证。

我们创建了接受一个`param`参数的`contains`函数。

然后我们可以用`includes`方法检查`param`，看看我们输入的内容是否包含在内。

`value`有输入值。

# $道具支持

同样，我们可以使用`withParams`方法来创建带有参数的验证器。

如果我们使用`withParams`，我们将获得关于带有`$params`属性的有效性规则的信息。

例如，我们可以写:

```
<template>
  <div id="app">
    <div>
      <label>name</label>
      <input v-model.trim="$v.name.$model">
      <div v-if="!$v.name.mustBeCool">name is invalid.</div>
    </div>
  </div>
</template>
<script>
import { helpers } from "vuelidate/lib/validators";
const contains = param =>
  helpers.withParams(
    { type: "contains", value: param },
    value => !helpers.req(value) || value.includes(param)
  );export default {
  name: "App",
  data() {
    return {
      name: ""
    };
  },
  validations: {
    name: {
      mustBeCool: contains("cool")
    }
  }
};
</script>
```

我们调用了`helpers.withParams`方法来获取`contains`函数的参数。

属性让我们设置我们接受的属性的类型。

# 结论

我们可以用 Vuelidate 创建定制的验证器。

喜欢这篇文章吗？如果有，通过 [**订阅我们的 YouTube 频道**](https://www.youtube.com/channel/UCtipWUghju290NWcn8jhyAw?sub_confirmation=true) **获取更多类似内容！**