# 使用 Vue 3 开始使用 Vue 路由器 4

> 原文：<https://javascript.plainenglish.io/getting-started-with-vue-router-with-vue-3-b66ee486c398?source=collection_archive---------9----------------------->

![](img/ff9e4cf8f51ec546b15e9e43ebaa21f6.png)

Photo by [Paul Gilmore](https://unsplash.com/@paulgilmore_?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

Vue 3 处于测试阶段，可能会有变化。

为了轻松地构建一个单页面应用程序，我们需要添加路由，以便将 URL 映射到呈现的组件。

在这篇文章中，我们将看看如何使用 Vue 路由器 4 与 Vue 3。

# 入门指南

首先，我们可以通过在`VueRouter`对象中包含脚本和访问所需的方法来创建我们的应用程序。

例如，我们可以写:

```
<!DOCTYPE html>
<html lang="en">
  <head>
    <script src="https://unpkg.com/vue@next"></script>
    <script src="https://unpkg.com/vue-router@4.0.0-beta.7/dist/vue-router.global.js"></script>
    <title>App</title>
  </head>
  <body>
    <div id="app">
      <p>
        <router-link to="/foo">Go to Foo</router-link>
        <router-link to="/bar">Go to Bar</router-link>
      </p>
      <router-view></router-view>
    </div>
    <script>
      const Foo = { template: "<div>foo</div>" };
      const Bar = { template: "<div>bar</div>" }; const routes = [
        { path: "/foo", component: Foo },
        { path: "/bar", component: Bar }
      ]; const router = VueRouter.createRouter({
        history: VueRouter.createWebHistory(),
        routes
      }); const app = Vue.createApp({});
      app.use(router);
      app.mount("#app");
    </script>
  </body>
</html>
```

在`head`标签中，我们有 Vue 3 和 Vue 路由器 4 的脚本标签。

在`body`中，我们用 ID 为`app`的 div 来保存模板。

`router-link`和`router-view`组件分别用于链路和路由器视图。

然后我们调用`VueRouter.createRouter`来创建路由器并传入路由。

`VueRouter.createWebHistory`方法打开历史模式，让我们使用没有散列符号的 URL。

然后我们调用`createApp`来创建我们的应用程序。

而且我们包括了带`app.use`的路由器。

最后，我们安装应用程序。

现在，我们可以单击链接并查看相应组件的内容。

`routes`具有`path`和`component`属性来将路径映射到组件。

我们可以用`this.$router.currentRouter.value.params`属性访问路由器数据。

例如，我们可以写:

```
<!DOCTYPE html>
<html lang="en">
  <head>
    <script src="https://unpkg.com/vue@next"></script>
    <script src="https://unpkg.com/vue-router@4.0.0-beta.7/dist/vue-router.global.js"></script>
    <title>App</title>
  </head>
  <body>
    <div id="app">
      <p>
        <router-link to="/foo/1">Go to Foo</router-link>
        <router-link to="/bar">Go to Bar</router-link>
      </p>
      <p>{{id}}</p>
      <router-view></router-view>
    </div>
    <script>
      const Foo = { template: "<div>foo</div>" };
      const Bar = { template: "<div>bar</div>" }; const routes = [
        { path: "/foo/:id", component: Foo },
        { path: "/bar", component: Bar }
      ]; const router = VueRouter.createRouter({
        history: VueRouter.createWebHistory(),
        routes
      }); const app = Vue.createApp({
        computed: {
          id() {
            return (
              this.$router.currentRoute.value.params &&
              this.$router.currentRoute.value.params.id
            );
          }
        }
      });
      app.use(router);
      app.mount("#app");
    </script>
  </body>
</html>
```

路由器链接中有`/foo/1` URL。

`:id`是名为`id`的 URL 参数的占位符。

我们可以用`this.$router.current.value.params`对象得到它的值。

这里有所有的 URL 参数键和值。

# 结论

我们可以用 Vue 路由器 4 和 Vue 3 添加基本路由。

喜欢这篇文章吗？如果有，通过 [**订阅我们的 YouTube 频道**](https://www.youtube.com/channel/UCtipWUghju290NWcn8jhyAw?sub_confirmation=true) **获取更多类似内容！**