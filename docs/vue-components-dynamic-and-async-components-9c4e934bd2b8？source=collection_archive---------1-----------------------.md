# Vue 组件—动态和异步组件

> 原文：<https://javascript.plainenglish.io/vue-components-dynamic-and-async-components-9c4e934bd2b8?source=collection_archive---------1----------------------->

![](img/1460251001050bb450e5937ca2f72a94.png)

Photo by [Joshua Hibbert](https://unsplash.com/@joshnh?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

Vue.js 是一个易于使用的 web 应用框架，我们可以用它来开发交互式前端应用。

在本文中，我们将研究如何定义和使用动态和异步组件。

# 通过动态组件保持活力

当我们在组件之间动态切换时，出于性能原因，我们有时希望保持状态或避免重新呈现。

例如，当在组件之间切换时，我们可以如下使用它来保持组件的状态:

`src/index.html`:

```
Vue.component("post", {
  data() {
    return {
      showDetails: false
    };
  },
  template: `
    <div>
      <h1>Title</h1>
      <button [@click](http://twitter.com/click)='showDetails = !showDetails'>
        Toggle Details
      </button>
      <div v-if='showDetails'>
        Lorem ipsum dolor sit amet.
      </div>
    </div>
  `
});Vue.component("archive", {
  data() {
    return {
      showDetails: false
    };
  },
  template: `
    <div>
      <h1>Archive</h1>
    </div>
  `
});new Vue({
  el: "#app",
  data: {
    tabName: "post"
  }
});
```

`index.html`:

```
<!DOCTYPE html>
<html>
  <head>
    <title>App</title>
    <meta charset="UTF-8" />
    <script src="[https://cdn.jsdelivr.net/npm/vue/dist/vue.js](https://cdn.jsdelivr.net/npm/vue/dist/vue.js)"></script>
  </head>
  <body>
    <div id="app">
      <button [@click](http://twitter.com/click)='tabName = "post"'>Post</button>
      <button [@click](http://twitter.com/click)='tabName = "archive"'>Archive</button>
      <keep-alive>
        <component v-bind:is="tabName"></component>
      </keep-alive>
    </div>
    <script src="src/index.js"></script>
  </body>
</html>
```

然后，当我们单击“切换细节”来显示文章选项卡中的文本时，我们会看到在切换到存档选项卡并返回之后，它仍会显示。

这是因为我们用`component`元素包裹了`keep-alive`。

`keep-alive`要求被切换的组件都有名字。

# 异步组件

我们可以创建只在异步组件需要时才从服务器加载的组件。

为此，我们没有传入一个对象作为第二个参数，而是传入一个返回承诺的函数。例如，我们可以这样写:

`src/index.js`:

```
Vue.component("async-component", (resolve, reject) => {
  setTimeout(() => {
    resolve({
      template: "<div>Async component</div>"
    });
  }, 1000);
});new Vue({
  el: "#app"
});
```

`index.html`:

```
<!DOCTYPE html>
<html>
  <head>
    <title>App</title>
    <meta charset="UTF-8" />
    <script src="[https://cdn.jsdelivr.net/npm/vue/dist/vue.js](https://cdn.jsdelivr.net/npm/vue/dist/vue.js)"></script>
  </head>
  <body>
    <div id="app">
      <async-component></async-component>
    </div>
    <script src="src/index.js"></script>
  </body>
</html>
```

1 秒钟后，我们应该会在屏幕上看到“异步组件”。

我们看到上面的函数从参数中获取了一个`resolve`函数，并在我们想要从服务器中检索定义时调用它。

要指示加载失败，我们可以调用`reject(reason)`来完成。

另一个例子是将它与 Webpack 代码分割一起使用，如下所示:

```
Vue.component('async-webpack', function (resolve) {
  require(['./async-component'], resolve)
})
```

我们也可以使用`import`功能导入一个组件，如下所示:

```
Vue.component(
  'async-webpack',
  () => import('./async-component')
)
```

如果我们想在本地注册一个组件，我们可以写:

```
new Vue({
  // ...
  components: {
    'component': () => import('./async-component')
  }
})
```

![](img/f36e14daafc29fdd4bf992a54957e7e4.png)

Photo by [Chris Lawton](https://unsplash.com/@chrislawton?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 处理装载状态

我们可以返回一个承诺，它有一个对象来引用加载和出错的组件。

例如，我们可以通过运行`npx vue create app`将它与 Vue CLI 生成的应用程序一起使用，其中`app`是应用程序名称。然后，我们可以在向导中选择默认选项。

然后我们编写如下代码:

`App.vue`:

```
<template>
  <div id="app">
    <AsyncComponent/>
  </div>
</template><script>
import Loading from "./components/Loading";
import Error from "./components/Error";
import HelloWord from "./components/HelloWorld";const AsyncComponent = () => ({
  component: new Promise(resolve => {
    setTimeout(() => {
      resolve(HelloWord);
    }, 1000);
  }),
  loading: Loading,
  error: Error,
  delay: 0,
  timeout: 3000
});export default {
  name: "App",
  components: {
    AsyncComponent
  }
};
</script><style>
</style>
```

`components/HelloWorld.vue`:

```
<template>
  <div>Hello</div>
</template>
```

`components/Error.vue`:

```
<template>
  <div>Error</div>
</template>
```

`component/Loading.vue`:

```
<template>
  <div>Loading</div>
</template>
```

在`App.vue`中，下面的代码:

```
component: new Promise(resolve => {
    setTimeout(() => {
      resolve(HelloWord);
    }, 1000);
  })
```

延迟加载`HelloWorld`组件一秒钟。`delay`是显示加载组件之前的毫秒数。

`timeout`是 Vue 停止加载组件之前的毫秒数。

因此，当`HelloWorld`组件正在加载时，我们将看到来自`Loading`组件的加载。

这种组件定义从 Vue 2.3.0 开始就有了。如果我们想对路由组件使用上述语法，应该使用 Vue Router 2.4.0。

# 结论

我们可以使用`keep-alive`来保持组件的当前状态，因为它是用`component`元素动态呈现的。

我们可以通过传入一个返回承诺的函数来定义带有`Vue.component`的异步组件。

在 Vue 2.3.0 或更高版本中，我们可以通过创建一个函数来定义一个异步组件，该函数返回一个对象，该对象承诺将组件解析为呈现的组件。

除了加载加载组件的延迟和 Vue 停止尝试加载组件之前的超时之外，我们还可以指定加载和错误组件。