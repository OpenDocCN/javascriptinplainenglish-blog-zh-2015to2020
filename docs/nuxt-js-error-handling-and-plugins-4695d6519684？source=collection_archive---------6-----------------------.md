# Nuxt.js —错误处理和插件

> 原文：<https://javascript.plainenglish.io/nuxt-js-error-handling-and-plugins-4695d6519684?source=collection_archive---------6----------------------->

![](img/a55652287fda7265bdf3cb4552700607.png)

Photo by [Markus Winkler](https://unsplash.com/@markuswinkler?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

Nuxt.js 是一个基于 Vue.js 的应用框架。

我们可以用它来创建服务器端渲染应用和静态站点。

在本文中，我们将看看如何用 Nuxt.js 处理异步数据错误和插件。

# 处理错误

当我们得到异步数据时，我们可以处理页面中的错误。

为此，我们写道:

```
<template>
  <div>{{ title }}</div>
</template><script>
import axios from "axios";export default {
  async asyncData({ params, error }) {
    try {
      const { data } = await axios.get(
        `https://jsonplaceholder.typicode.com/posts/${params.id}`
      );
      return { title: data.title };
    } catch {
      error({ statusCode: 404, message: "Post not found" });
    }
  },
};
</script>
```

我们有一个带有对象的`asyncData`方法，该对象具有`error`属性。

`error`是一个我们可以用来渲染误差的函数。

# 资产

Nuxt 使用 vue 加载器、文件加载器和 url 加载器 Webpack 加载器进行资产服务。

此外，我们可以将`static`文件夹用于静态资产。

要添加资产，我们可以写:

```
<template>
  <div class="container">
    <img src="~/assets/kitten.jpg" />
  </div>
</template><script>
export default {};
</script>
```

`~`是 Nuxt 根文件夹的简写。

# 静态

我们还可以提供`static`文件夹中的资产。

例如，我们可以将文件移动到`static`文件夹中，并写下:

```
<template>
  <div class="container">
    <img src="/kitten.jpg" />
  </div>
</template><script>
export default {};
</script>
```

`/`是`static` 文件夹的简写。

# 插件

我们可以在应用程序中添加插件。

它们可能是像 Axios 这样的外部包。

或者我们可以使用 Vue 插件。

要使用 Vue 插件，我们必须手动添加它们。

例如，如果我们想给我们的应用程序添加一个工具提示，我们通过运行以下命令来安装`v-tooltip`包:

```
npm install --save v-tooltip
```

然后我们创建`plugins/vue-tooltip.js`并添加:

```
import Vue from 'vue'
import VTooltip from 'v-tooltip'Vue.use(VTooltip)
```

然后在`nuxt.config.js`中，我们添加:

```
plugins: [
  '@/plugins/vue-tooltip.js'
],
```

然后我们可以像往常一样使用它，写道:

```
<template>
  <div class="container">
    <button v-tooltip="'hello world'">hello</button>
  </div>
</template><script>
export default {};
</script>
```

# 在\$root & context 中注入

我们还可以在所有应用程序中提供功能。

为此，我们可以调用`inject`函数。

我们可以通过添加`plugins/hello.js`来创建自己的插件:

```
export default (context, inject) => {
  const hello = msg => console.log(`hello ${msg}!`)
  inject('hello', hello)
}
```

然后我们可以在`nuxt.config.js`中添加我们的插件:

```
plugins: [
  '@/plugins/hello.js'
],
```

然后我们可以通过编写以下代码来调用我们的`hello`函数:

```
<template>
  <div class="container"></div>
</template><script>
export default {
  mounted() {
    this.$hello("james");
  },
};
</script>
```

我们还需要:

```
context.$hello = hello
```

在我们使用 Nuxt 2.12 或更早版本导出的函数中。

我们也可以通过编写以下代码将`$hello`函数与`asyncData`函数一起使用:

```
<template>
  <div class="container"></div>
</template><script>
export default {
  mounted() {
    this.$hello("james");
  },
  asyncData({ $hello }) {
    $hello("asyncData");
  },
};
</script>
```

`$hello`是`asyncData`上下文参数的一部分。

# 结论

我们可以在获得数据时处理错误。

此外，我们可以创建自己的插件和功能，并将其注入我们的应用程序。