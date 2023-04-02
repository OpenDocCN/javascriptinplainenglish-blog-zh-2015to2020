# BootstrapVue —表单基础

> 原文：<https://javascript.plainenglish.io/bootstrapvue-form-basics-712096587908?source=collection_archive---------4----------------------->

![](img/06ab3b7af143e3345b36239a40827ff6.png)

Photo by [Dan Gold](https://unsplash.com/@danielcgold?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

为了制作好看的 Vue 应用，我们需要设计组件的样式。

为了使我们的生活更容易，我们可以使用内置样式的组件。

在本文中，我们将了解如何使用 BootstrapVue 创建基本表单。

# 形式

BootstrapVue 有很多组件可以按照我们的方式创建表单。

为了创建一个表单，我们可以使用`b-form`、`b-form-group`和`b-form-input`组件。

例如，我们可以通过编写以下内容来创建一个简单的表单:

```
<template>
  <div id="app">
    <b-form @submit="onSubmit" @reset="onReset">
      <b-form-group id="input-group-1" label="name" label-for="input-1" description="your name">
        <b-form-input id="input-1" v-model="form.name" type="text" required placeholder="name"></b-form-input>
      </b-form-group>
    </b-form>
  </div>
</template><script>
export default {
  name: "App",
  data() {
    return {
      form: {}
    };
  },
  methods: {
    onSubmit() {},
    onReset() {}
  }
};
</script>
```

我们使用这些组件创建了一个带有输入框的简单表单。

`b-form-input`是一个呈现输入元素的文本输入框。

`v-model`将输入的值绑定到模型。

它还使用了`@submit`和`@reset`处理程序来允许我们处理表单上触发的提交和重置事件。

# 内嵌表单

我们可以通过给`b-form`添加`inline`属性来制作一个内嵌输入的表单。

例如，我们可以写:

```
<template>
  <div id="app">
    <b-form inline>
      <label class="sr-only" for="name">Name</label>
      <b-input id="name" placeholder="name"></b-input>
    </b-form>
  </div>
</template><script>
export default {
  name: "App"
};
</script>
```

我们只需将`inline`属性添加到`b-form`中，使我们的表单成为内联的，而不是块元素。

我们可以通过编写以下代码并排添加多个表单输入元素:

```
`<template>
  <div id="app">
    <b-form inline>
      <label class="sr-only" for="name">name</label>
      <b-input class="mb-2 mr-sm-2 mb-sm-0" id="name" placeholder="name"></b-input><label class="sr-only" for="email">email</label>
      <b-input-group prepend="@" class="mb-2 mr-sm-2 mb-sm-0">
        <b-input id="email" placeholder="email"></b-input>
      </b-input-group>
    </b-form>
  </div>
</template><script>
export default {
  name: "App"
};
</script>
```

现在我们有两个并排的输入框。

它们反应灵敏，因此它们的大小会根据视窗宽度进行调整。

# 表单文本助手

我们可以添加一个`b-form-text`元素来为我们的输入字段添加解释。

例如，我们可以写:

```
<template>
  <div id="app">
    <b-form inline>
      <label for="name">name</label>
      <b-input type="text" id="name"></b-input>
      <b-form-text id="name">
        10 or more characters
      </b-form-text>
    </b-form>
  </div>
</template><script>
export default {
  name: "App"
};
</script>
```

我们用`b-form-text`在输入框下面添加一个描述。

# 反馈助手

BootstrapVue 为我们提供了`b-form-invalid-feedback`组件来显示表单验证错误。

如果表单输入值有效，它还有`b-form-valid-feedback`来显示一条消息。

例如，我们可以通过编写以下内容来显示表单验证错误:

```
<template>
  <div id="app">
    <b-form inline>
      <label for="feedback-user">name</label>
      <b-input v-model="name" :state="validation" id="name"></b-input>
      <b-form-invalid-feedback :state="validation">please enter a name</b-form-invalid-feedback>
    </b-form>
  </div>
</template><script>
export default {
  name: "App",
  data() {
    return {
      name: ""
    };
  },
  computed: {
    validation() {
      return this.name.length > 0;
    }
  }
};
</script>
```

我们有一个下面带有`b-form-invalid-feedback`的`b-input`组件。

`state`属性采用一个表示表单值验证状态的表达式。

`b-form-invalid-feedback`也采用相同的`state`道具。

现在，如果我们键入一些内容，我们会看到一个绿色边框和一个复选标记。

如果我们什么也没输入，我们会看到一个红色边框和一个感叹号，带有表单验证消息。

我们只有`validation` computed 属性来返回输入验证状态。

要在我们输入有效内容时添加一条消息来显示内容，我们可以这样写:

```
<template>
  <div id="app">
    <b-form inline>
      <label for="feedback-user">name</label>
      <b-input v-model="name" :state="validation" id="name"></b-input>
      <b-form-invalid-feedback :state="validation">please enter a name</b-form-invalid-feedback>
      <b-form-valid-feedback :state="validation">good job</b-form-valid-feedback>
    </b-form>
  </div>
</template><script>
export default {
  name: "App",
  data() {
    return {
      name: ""
    };
  },
  computed: {
    validation() {
      return this.name.length > 0;
    }
  }
};
</script>
```

我们补充道:

```
<b-form-valid-feedback :state="validation">good job</b-form-valid-feedback>
```

当我们在输入框中输入内容时显示一条消息。

![](img/1420b7ba77c8062e9ee0222aaa9032e6.png)

Photo by [Kouji Tsuru](https://unsplash.com/@pafuxu?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 结论

BootstrapVue 为我们提供了许多表单组件来添加输入框和验证指示器。

它可以自动将输入值绑定到模型字段。

## 简单英语的 JavaScript

你知道我们有四份出版物和一个 YouTube 频道吗？在 [**plainenglish.io**](https://plainenglish.io/) 和 [**找到他们订阅我们的 YouTube 频道**](https://www.youtube.com/channel/UCtipWUghju290NWcn8jhyAw) **！**