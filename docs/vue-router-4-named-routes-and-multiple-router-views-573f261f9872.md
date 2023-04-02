# Vue 路由器 4–命名路由和多个路由器视图

> 原文：<https://javascript.plainenglish.io/vue-router-4-named-routes-and-multiple-router-views-573f261f9872?source=collection_archive---------6----------------------->

![](img/3cc217d5ba4a8a686881d9c8fad373e2.png)

Photo by [Justin Lawrence](https://unsplash.com/@thisisjlaw?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

Vue Router 4 处于测试阶段，可能会有变化。

为了轻松地构建一个单页面应用程序，我们需要添加路由，以便将 URL 映射到呈现的组件。

在这篇文章中，我们将看看如何使用 Vue 路由器 4 与 Vue 3。

# 命名路线

我们可以命名我们的路线，用名称而不是路径来标识我们的路线。

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
        <router-link :to="{ name: 'bar', params: { id: '123' }}"
          >Bar</router-link
        >
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
      }); const app = Vue.createApp({});
      app.use(router);
      app.mount("#app");
    </script>
  </body>
</html>
```

创建一个采用 URL 参数`:id`的路由`bar`。

它还有一个带有路由名称的`name`属性。

然后我们有一个`router-link`，它有一个带有对象的`to`道具。

该对象将路线名称作为`name`属性的值。

`params`有路线采用的路线参数。

# 命名视图

我们可以用 Vue 路由器命名视图。

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
      <router-view></router-view>
      <router-view name="a"></router-view>
      <router-view name="b"></router-view>
    </div>
    <script>
      const Foo = {
        template: "<div>foo</div>"
      };
      const Bar = {
        template: "<div>bar</div>"
      };
      const Baz = {
        template: "<div>baz</div>"
      };
      const routes = [
        {
          path: "/",
          components: {
            default: Foo,
            a: Bar,
            b: Baz
          }
        }
      ];
      const router = VueRouter.createRouter({
        history: VueRouter.createWebHistory(),
        routes
      });
      const app = Vue.createApp({});
      app.use(router);
      app.mount("#app");
    </script>
  </body>
</html>
```

我们在`routes`数组中有一个对象。

它具有带有 URL 路径的`path`属性。

`components`属性有一个对象，其属性名称是`router-view`的名称属性值。

`default`用于没有名称的路由器视图。

在模板中，我们用`router-view`来显示路线内容。

然后我们看到:

```
foo
bar
baz
```

已显示。

# 嵌套命名视图

我们还可以在子路由中使用命名视图。

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
      <router-view></router-view>
    </div>
    <script>
      const Parent = {
        template: `<div>
          parent
          <router-view></router-view>
          <router-view name="a"></router-view>
          <router-view name="b"></router-view>
        </div>`
      }; const Foo = {
        template: "<div>foo</div>"
      };
      const Bar = {
        template: "<div>bar</div>"
      };
      const Baz = {
        template: "<div>baz</div>"
      };
      const routes = [
        {
          path: "/parent",
          component: Parent,
          children: [
            {
              path: "child",
              components: {
                default: Foo,
                a: Bar,
                b: Baz
              }
            }
          ]
        }
      ];
      const router = VueRouter.createRouter({
        history: VueRouter.createWebHistory(),
        routes
      });
      const app = Vue.createApp({});
      app.use(router);
      app.mount("#app");
    </script>
  </body>
</html>
```

我们有带多个`router-view`的`Parent`路线。

根模板有一个`router-view`。

然后`Parent`组件拥有子组件`router-view`。

在`routes`数组中，我们有一个带有映射到`Parent`组件的`/parent`路由的对象。

我们有一条`child`路线，上面有`router-view` s 的零部件

然后我们看到:

```
parent
foo
bar
baz
```

当我们转到`/parent/child`时显示。

# 结论

我们可以用 Vue 路由器 4 拥有多个`router-view`组件。

此外，我们可以命名我们的路线，这样我们就可以使用导航。

喜欢这篇文章吗？如果有，通过 [**订阅我们的 YouTube 频道**](https://www.youtube.com/channel/UCtipWUghju290NWcn8jhyAw?sub_confirmation=true) **获取更多类似内容！**