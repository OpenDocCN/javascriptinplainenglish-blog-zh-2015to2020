# BootstrapVue —文本区域

> 原文：<https://javascript.plainenglish.io/bootstrapvue-text-areas-746cab338261?source=collection_archive---------8----------------------->

![](img/feb789830ddd41c1c80e20d4a936027b.png)

Photo by [Olav Tvedt](https://unsplash.com/@olav_tvedt?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

为了制作好看的 Vue 应用，我们需要设计组件的样式。

为了使我们的生活更容易，我们可以使用内置样式的组件。

在这篇文章中，我们将看看如何添加一个文本区域。

# 文本区域

我们可以在表单中添加一个文本区域，以便添加多行文本输入。

它可以支持自动调整高度、最小和最大行数以及显示验证状态。

要添加一个，我们使用`b-form-textarea`组件。

例如，我们可以写:

```
<template>
  <div id="app">
    <b-form-textarea v-model="text" placeholder="Enter text" rows="3" max-rows="6"></b-form-textarea>
    <p>{{text}}</p>
  </div>
</template><script>
export default {
  name: "App",
  data() {
    return {
      text: ""
    };
  }
};
</script>
```

我们添加了`b-form-textarea`输入框。

`v-model`将`text`绑定到输入值。

`placeholder`有占位符。

`rows`设置为 3，但我们可以用`max-rows`输入多达 6 行文本。

显示输入的值。

# 胶料

我们可以改变输入的大小。

要改变大小，我们可以使用`size`道具。

例如，我们可以写:

```
<template>
  <div id="app">
    <b-form-textarea v-model="text" placeholder="Enter text" size="sm"></b-form-textarea>
    <p>{{text}}</p>
  </div>
</template><script>
export default {
  name: "App",
  data() {
    return {
      text: ""
    };
  }
};
</script>
```

缩小文本区域。

可能的值是`'sm'`或`'lg'`。`'sm'`是给小的。`'lg'`代表大。

# 显示的行

我们可以用`rows`道具改变行。

例如，我们可以写:

```
<template>
  <div id="app">
    <b-form-textarea v-model="text" placeholder="Enter text" rows="8"></b-form-textarea>
    <p>{{text}}</p>
  </div>
</template><script>
export default {
  name: "App",
  data() {
    return {
      text: ""
    };
  }
};
</script>
```

那么我们的文本区域有 8 行。

# 禁用调整大小

`no-resize`属性禁止调整文本区域的大小。

例如，我们可以写:

```
<template>
  <div id="app">
    <b-form-textarea v-model="text" placeholder="Enter text" rows="3" no-resize></b-form-textarea>
    <p>{{text}}</p>
  </div>
</template><script>
export default {
  name: "App",
  data() {
    return {
      text: ""
    };
  }
};
</script>
```

`no-resize`道具禁用调整大小手柄。

# 自动高度

默认情况下，`b-form-textarea`将调整其高度以适应内容。

如果输入的行数减少，我们可以使用`no-auto-shrink`属性禁用收缩。

`sticky`会让它永不缩水。

例如，我们可以写:

```
<template>
  <div id="app">
    <b-form-textarea v-model="text" placeholder="Enter text" rows="3" max-rows="8" no-auto-shrink></b-form-textarea>
    <p>{{text}}</p>
  </div>
</template><script>
export default {
  name: "App",
  data() {
    return {
      text: ""
    };
  }
};
</script>
```

# 验证状态

我们可以设置`state`属性来显示输入的验证状态。

例如，我们可以写:

```
<template>
  <div id="app">
    <b-form-textarea v-model="text" placeholder="Enter text" :state="text.length >= 5"></b-form-textarea>
    <p>{{text}}</p>
  </div>
</template><script>
export default {
  name: "App",
  data() {
    return {
      text: ""
    };
  }
};
</script>
```

我们有`state`属性来检查`text`的`length`是否大于或等于 5 才被认为是有效的。

如果无效，该框显示为红色。

否则，它显示为绿色。

# 文本格式化程序

我们可以按照我们想要的方式格式化输入的文本。

为此，我们可以设置`formatter`道具。

例如，我们可以写:

```
<template>
  <div id="app">
    <b-form-textarea v-model="text" placeholder="Enter text" :formatter="formatter"></b-form-textarea>
    <p>{{text}}</p>
  </div>
</template><script>
export default {
  name: "App",
  data() {
    return {
      text: ""
    };
  },
  methods: {
    formatter(value) {
      return value.toUpperCase();
    }
  }
};
</script>
```

我们用`formatter`方法返回大写的文本。

# 使文本区域显示为纯文本

`plaintext`道具使文本区域显示为纯文本。

例如，我们可以写:

```
<template>
  <div id="app">
    <b-form-textarea v-model="text" placeholder="Enter text" plaintext></b-form-textarea>
    <p>{{text}}</p>
  </div>
</template><script>
export default {
  name: "App",
  data() {
    return {
      text: ""
    };
  }
};
</script>
```

现在我们没有显示边框。

# 将其设为只读

属性使它成为只读的。

![](img/3e828a8dbcf9dd09bf3cc899e67f164c.png)

Photo by [Dwayne Hills](https://unsplash.com/@dhillssr?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 结论

我们可以使用`b-form-textarea`向我们的应用程序添加一个文本区域。

它可以在各种定制，像改变大小或显示为纯文本。

## 简单英语的 JavaScript

你知道我们有四份出版物和一个 YouTube 频道吗？在 [**plainenglish.io**](https://plainenglish.io/) 和 [**找到它们订阅我们的 YouTube 频道**](https://www.youtube.com/channel/UCtipWUghju290NWcn8jhyAw) **！**