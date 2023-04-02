# Vue 路由器 4–程序导航

> 原文：<https://javascript.plainenglish.io/vue-router-4-programmatic-navigation-c142fa88180e?source=collection_archive---------14----------------------->

![](img/e4c750ba72e7b962f1b2aad8962b013e.png)

Photo by [Markus Spiske](https://unsplash.com/@markusspiske?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

Vue Router 4 处于测试阶段，可能会有变化。

为了轻松地构建一个单页面应用程序，我们需要添加路由，以便将 URL 映射到呈现的组件。

在这篇文章中，我们将看看如何使用 Vue 路由器 4 与 Vue 3。

# 程序导航

我们可以用 Vue Router 4 有计划地浏览路线。

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
        <router-link to="/foo">Foo</router-link>
        <a href="#" @click.stop="go">Bar</a>
      </p>
      <router-view></router-view>
    </div>
    <script>
      const Foo = {
        template: "<div>foo</div>"
      }; const Bar = {
        template: "<div>bar</div>"
      };
      const routes = [
        { path: "/foo", component: Foo },
        { path: "/bar", component: Bar }
      ]; const router = VueRouter.createRouter({
        history: VueRouter.createWebHistory(),
        routes
      }); const app = Vue.createApp({
        methods: {
          go() {
            this.$router.push("bar");
          }
        }
      });
      app.use(router);
      app.mount("#app");
    </script>
  </body>
</html>
```

我们有一个`a`元素要转到`bar`路线。

将`stop`修改器添加到`@click`以防止传播。

在 root Vue 实例中，我们有`this.$router.push`方法来到达给定的路线。

我们也可以写:

```
this.$router.push({ path: "bar" });
```

我们可以对命名路由这样做:

```
router.push({ name: 'bar', params: { id: '123' } })
```

所以我们可以写:

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
        <router-link to="/foo">Foo</router-link>
        <a href="#" @click.stop="go">Bar</a>
      </p>
      <p>{{ $router.currentRoute.value.params.id }}</p>
      <router-view></router-view>
    </div>
    <script>
      const Foo = {
        template: "<div>foo</div>"
      }; const Bar = {
        template: "<div>bar</div>"
      };
      const routes = [
        { path: "/foo", component: Foo },
        { path: "/bar/:id", component: Bar, name: "bar" }
      ]; const router = VueRouter.createRouter({
        history: VueRouter.createWebHistory(),
        routes
      }); const app = Vue.createApp({
        methods: {
          go() {
            this.$router.push({ name: "bar", params: { id: "123" }       });
          }
        }
      });
      app.use(router);
      app.mount("#app");
    </script>
  </body>
</html>
```

我们添加了`:id` URL 参数占位符，并为其添加了`name`属性。

所以我们可以用`name`值代替路径调用`push`。

URL 参数在`id`属性中。

# `$router.replace(location, onComplete?, onAbort?)`

`$router.replace`方法与`$router.push`相似，只是它覆盖了之前的历史记录条目。

我们可以将`replace`道具添加到`router-link`中，如下所示:

```
<router-link :to="..." replace>
```

这与以下内容相同:

```
router.replace(...)
```

# `$router.go(n)`

`$router.go`方法接受一个整数，并让我们在浏览器历史中前进或后退给定的步数。

负整数意味着我们在后退。

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
        <router-link to="/foo">Foo</router-link>
        <router-link to="/bar">Bar</router-link>
        <a href="#" @click.stop="goBack">Back</a>
      </p>
      <router-view></router-view>
    </div>
    <script>
      const Foo = {
        template: "<div>foo</div>"
      }; const Bar = {
        template: "<div>bar</div>"
      };
      const routes = [
        { path: "/foo", component: Foo },
        { path: "/bar", component: Bar }
      ]; const router = VueRouter.createRouter({
        history: VueRouter.createWebHistory(),
        routes
      }); const app = Vue.createApp({
        methods: {
          goBack() {
            this.$router.go(-1);
          }
        }
      });
      app.use(router);
      app.mount("#app");
    </script>
  </body>
</html>
```

我们有带`this.$router.go`调用的`goBack`方法来返回到先前的路线。

该方法的约定遵循历史 API。

# 结论

我们可以使用 Vue Router 4 以编程方式导航到不同的路由，它与 Vue 3 一起工作。

喜欢这篇文章吗？如果有，通过 [**订阅我们的 YouTube 频道**](https://www.youtube.com/channel/UCtipWUghju290NWcn8jhyAw?sub_confirmation=true) **获取更多类似内容！**