# BootstrapVue —表单标记

> 原文：<https://javascript.plainenglish.io/bootstrapvue-form-tags-ba604c00cb7d?source=collection_archive---------9----------------------->

![](img/7bf85fe3b2bfae26565b264eb0df3c01.png)

Photo by [Jakub Kapusnak](https://unsplash.com/@foodiesfeed?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

为了制作好看的 Vue 应用，我们需要设计组件的样式。

为了使我们的生活更容易，我们可以使用内置样式的组件。

在本文中，我们将研究如何在表单中添加表单标签。

# 表单标签

表单标记是自定义标记的输入表单控件。

我们可以定制它，并允许重复的标签选择和标签验证。

要添加它，我们可以使用`b-form-tags`组件。

例如，我们可以写:

```
<template>
  <div id="app">
    <b-form-tags v-model="value"></b-form-tags>
    <p>Value: {{ value }}</p>
  </div>
</template><script>
export default {
  name: "App",
  data() {
    return {
      value: ["banana", "grape"]
    };
  }
};
</script>
```

显示允许我们输入标签的输入。

`v-model`将把输入的值绑定到一个数组，正如我们从 p 元素中看到的。

当我们按回车键时，我们将添加值。

我们可以添加`no-add-on-enter`道具来禁止在进入时添加新标签。

我们可以用`add-on-change`道具添加标签。

# 使用分隔符创建标签

标签创建可以添加分隔符。

我们可以添加`separator`标签和所有我们想让用户使用的分隔符。

例如，我们可以写:

```
<template>
  <div id="app">
    <b-form-tags v-model="value" separator=" ,"></b-form-tags>
    <p>Value: {{ value }}</p>
  </div>
</template><script>
export default {
  name: "App",
  data() {
    return {
      value: ["banana", "grape"]
    };
  }
};
</script>
```

然后我们可以使用空格和逗号作为分隔符。

当我们键入它们时，我们会看到一个新的标签。

# 使用退格删除最后一个标签

默认情况下，当我们按退格键时，`b-form-tags`组件不会移除标签。

为了让用户按 backspace 来删除最后一个标签，我们可以使用`remove-on-delete` prop。

例如，我们可以写:

```
<template>
  <div id="app">
    <b-form-tags v-model="value" remove-on-delete></b-form-tags>
    <p>Value: {{ value }}</p>
  </div>
</template><script>
export default {
  name: "App",
  data() {
    return {
      value: ["banana", "grape"]
    };
  }
};
</script>
```

然后我们可以用退格删除最后一项。

# 式样

我们可以用各种道具来设计标签。

`tag-pills`用药丸的外观呈现标签。

`tag-variant`应用 Bootstrap 内置的样式变量。

`size`设置组件外观的尺寸。

`placeholder`让我们为输入元素设置占位符文本。

`state`让我们设置验证状态。我们将其设置为`true`有效、`false`无效或`null`。

`disabld`让我们禁用输入。

例如，我们可以写:

```
<template>
  <div id="app">
    <b-form-tags v-model="value" tag-pills size="lg"></b-form-tags>
    <p>Value: {{ value }}</p>
  </div>
</template><script>
export default {
  name: "App",
  data() {
    return {
      value: ["banana", "grape"]
    };
  }
};
</script>
```

然后标签更圆，尺寸更大。

# 标签验证

我们可以使用`tag-validator`来验证标签。

例如，我们可以写:

```
<template>
  <div id="app">
    <b-form-tags v-model="value" :tag-validator="tagValidator"></b-form-tags>
    <p>Value: {{ value }}</p>
  </div>
</template><script>
export default {
  name: "App",
  data() {
    return {
      value: ["banana", "grape"]
    };
  },
  methods: {
    tagValidator(tag) {
      return tag.length > 2;
    }
  }
};
</script>
```

我们将`tag-validator`属性设置为`tagValidator`方法。

我们检查标签的长度是否大于 2。

如果标签不满足该条件，我们将看到一条错误消息。

# 检测新的、无效的和重复的标签

我们可以听听`tag-state`事件。

每当新的标签被输入到新的标签输入元素中时，它就被发出。

当标签未通过验证或检测到重复标签时，也会发出此消息。

例如，我们可以写:

```
<template>
  <div id="app">
    <b-form-tags v-model="tags" :tag-validator="validator" @tag-state="onTagState"></b-form-tags>
    <p>Value: {{ tags }}</p>
    <p>Invalid: {{ invalidTags }}</p>
    <p>Duplicate: {{ duplicateTags }}</p>
  </div>
</template><script>
export default {
  name: "App",
  data() {
    return {
      tags: [],
      validTags: [],
      invalidTags: [],
      duplicateTags: []
    };
  },
  methods: {
    onTagState(valid, invalid, duplicate) {
      this.validTags = valid;
      this.invalidTags = invalid;
      this.duplicateTags = duplicate;
    },
    validator(tag) {
      return tag.length > 2 && tag.length < 6;
    }
  }
};
</script>
```

我们将`onTagState`方法设置为`tag-state`事件处理程序的值。

同样，我们有`validator`方法来设置`tag-validator`道具。

`v-model`用于将输入值绑定到`tags`。

我们显示无效和重复的标签。

然后，当我们输入无效或重复的标签时，它们将显示在 p 元素中。

![](img/d71511578bdf1736bbf0e3bc7740ae37.png)

Photo by [Srecko Skrobic](https://unsplash.com/@sreckoskrobic?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 结论

我们可以添加`b-form-tags`组件来添加一个标签输入。

它具有验证和检查重复的特性。

# 简单英语的 JavaScript

你知道我们有四份出版物和一个 YouTube 频道吗？在 [**plainenglish.io**](https://plainenglish.io/) 和 [**找到它们订阅解码，我们的 YouTube 频道**](https://www.youtube.com/channel/UCtipWUghju290NWcn8jhyAw) **！**