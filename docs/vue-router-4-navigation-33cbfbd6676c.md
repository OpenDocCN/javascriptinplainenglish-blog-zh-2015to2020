# Vue 路由器 4 —导航

> 原文：<https://javascript.plainenglish.io/vue-router-4-navigation-33cbfbd6676c?source=collection_archive---------16----------------------->

![](img/6d7bcca37e4566ab231ac3f6d6982cbe.png)

Photo by [Joseph Barrientos](https://unsplash.com/@jbcreate_?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

**Vue Router 4 处于测试阶段，可能会有变化。**

为了轻松地构建一个单页面应用程序，我们需要添加路由，以便将 URL 映射到呈现的组件。

在这篇文章中，我们将看看如何使用 Vue 路由器 4 与 Vue 3。

# 航行

我们可以用`this.$router`的方法导航到一条路线。

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
        <a @click="goBack" href="#">Go Back</a>
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
      }); const app = Vue.createApp({
        methods: {
          goBack() {
            window.history.length > 1
              ? this.$router.go(-1)
              : this.$router.push("/foo");
          }
        }
      });
      app.use(router);
      app.mount("#app");
    </script>
  </body>
</html>
```

创建一个`goBack`方法，当我们点击返回链接时调用它。

在方法中，我们检查浏览器历史中是否有任何内容。

如果有，那么我们回到上一页。

否则，我们去`/foo`页面。

# 对参数变化做出反应

我们可以用几种方式对路由参数的变化做出反应。

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
        <router-link to="/foo/1">Foo 1</router-link>
        <router-link to="/foo/2">Foo 2</router-link>
      </p>
      <router-view></router-view>
    </div>
    <script>
      const Foo = {
        template: "<div>foo</div>",
        watch: {
          $route(to, from) {
            console.log(to, from);
          }
        }
      }; const routes = [{ path: "/foo/:id", component: Foo }]; const router = VueRouter.createRouter({
        history: VueRouter.createWebHistory(),
        routes
      }); const app = Vue.createApp({});
      app.use(router);
      app.mount("#app");
    </script>
  </body>
</html>
```

我们有映射到`Foo`组件的`/foo/:id`路由。

在`Foo`组件中，我们有`$route`观察器来观察路线。

`to`有路线可去。有我们出发的路线。

它们都是带有路径、路由元数据、路由参数、查询参数等的对象。

此外，我们可以使用`beforeRouteUpdate`方法来观察路线变化:

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
        <router-link to="/foo/1">Foo 1</router-link>
        <router-link to="/foo/2">Foo 2</router-link>
      </p>
      <router-view></router-view>
    </div>
    <script>
      const Foo = {
        template: "<div>foo</div>",
        beforeRouteUpdate(to, from, next) {
          console.log(to, from);
          next();
        }
      }; const routes = [{ path: "/foo/:id", component: Foo }]; const router = VueRouter.createRouter({
        history: VueRouter.createWebHistory(),
        routes
      }); const app = Vue.createApp({});
      app.use(router);
      app.mount("#app");
    </script>
  </body>
</html>
```

我们没有观察`$route`对象，而是使用了`beforeRouteUpdate`钩子，它可以通过`app.use(router)`调用获得。

现在，当我们单击链接时，我们将看到与使用观察器时相同的数据。

唯一的区别是我们调用`next`移动到下一条路线。

# 结论

我们可以用 watchers 或者 hooks 用 Vue Router 4 来看导航。

喜欢这篇文章吗？如果有，通过 [**订阅我们的 YouTube 频道**](https://www.youtube.com/channel/UCtipWUghju290NWcn8jhyAw?sub_confirmation=true) **获取更多类似内容！**