# Nuxt.js —错误页面、异步和请求数据

> 原文：<https://javascript.plainenglish.io/nuxt-js-error-pages-async-and-request-data-cc58452c8a8c?source=collection_archive---------3----------------------->

![](img/6a471833600e96d99d9ae8e38aaa646b.png)

Photo by [Aniket Bhattacharya](https://unsplash.com/@aniket940518?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

Nuxt.js 是一个基于 Vue.js 的应用框架。

我们可以用它来创建服务器端渲染应用和静态站点。

在本文中，我们将了解如何使用 Nuxt.js 添加错误页面、获取异步数据以及获取请求数据。

# 错误页面

我们可以通过将 pages 文件放在`layouts`文件夹中，向 Nuxt 应用程序添加一个错误页面。

例如，如果我们想在得到 404 错误时显示一个页面，我们添加一个`layouts/error.js`文件:

```
<template>
  <div>
    <h1 v-if="error.statusCode === 404">Page not found</h1>
  </div>
</template><script>
export default {
  props: ["error"],
};
</script>
```

它需要一个`error`属性，我们可以检查`statusCode`属性以获得错误状态代码。

# 页

页面具有特殊的属性和功能，使我们的通用应用程序的开发。

例如，我们的页面可能看起来像:

```
<template>
  <div>hello {{ name }}</div>
</template><script>
export default {
  asyncData(context) {
    return { name: "world" };
  },
  fetch() {
    //...
  },
  head() {
    return { foo: "bar" };
  },
};
</script>
```

`asyncData`方法让我们在加载组件之前设置状态。

返回的对象将与我们的数据对象合并。

`fetch`方法让我们在呈现页面之前填充存储。

让我们为页面设置元标签。

`loading`防止页面在我们进入时自动调用`this.$nuxt.$loading.finish()`，在我们离开时自动调用`this.$nuxt.$loading.start()`。

这让我们可以手动控制行为。

`transition`为页面定义一个特定的过渡。

`scrollToTop`是一个布尔值，它指定我们是否要在呈现页面之前滚动到顶部。

`validate`是动态路线的验证器功能。

`middleware`定义页面的中间件。

# 异步数据

我们可以使用`asyncData`方法获取数据。

如果我们在代码中使用 Axios 拦截器，那么我们必须首先创建它的一个实例。

例如，我们可以写:

```
import axios from 'axios'
const myAxios = axios.create({
  // ...
})
myAxios.interceptors.response.use(
  function (response) {
    return response.data
  },
  function (error) {
    // ...
  }
)
```

在我们的`asyncData`方法中，我们可以用它来返回一个承诺。

例如，我们可以写:

```
<template>
  <div>{{ title }}</div>
</template><script>
import axios from "axios";export default {
  async asyncData({ params }) {
    const { data } = await axios.get(
      `https://jsonplaceholder.typicode.com/posts/${params.id}`
    );
    return { title: data.title };
  },
};
</script>
```

我们有一个`asyncData`方法，它接受一个具有`params`属性的对象。

它有我们页面的 URL 参数。

然后我们可以使用`axios`来获取我们想要的数据并返回解析后的值。

可以从我们的组件中检索解析的对象。

# 背景

`asyncData`的上下文参数也具有`req`和`res`属性。

`req`有请求对象。

而`res`有了回应。

在使用请求数据之前，我们使用`process.server`检查页面是否是服务器端呈现的。

为此，我们写道:

`pages/hello.vue`

```
<template>
  <div class="container">{{host}}</div>
</template><script>
export default {
  async asyncData({ req, res }) {
    if (process.server) {
      return { host: req.headers.host };
    }
    return {};
  },
};
</script>
```

如果`process.server`是`true`，那么我们就可以使用`req`对象来获取请求数据。

# 结论

我们可以在我们的页面中使用 Nuxt 获取请求数据。

此外，我们还可以创建自己的错误页。

我们可以用异步数据初始化我们的页面。

喜欢这篇文章吗？如果是这样的话，通过 [**订阅我们的 YouTube 频道**](https://www.youtube.com/channel/UCtipWUghju290NWcn8jhyAw?sub_confirmation=true) **就能获得更多类似的内容！**