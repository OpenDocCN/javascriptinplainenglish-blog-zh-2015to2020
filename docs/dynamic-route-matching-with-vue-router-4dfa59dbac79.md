# 与 Vue 路由器的动态路由匹配

> 原文：<https://javascript.plainenglish.io/dynamic-route-matching-with-vue-router-4dfa59dbac79?source=collection_archive---------4----------------------->

![](img/ec68fa102ace4246b6889e376bd31118.png)

Photo by [Toa Heftiba](https://unsplash.com/@heftiba?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

Vue.js 是一个易于使用的 web 应用框架，我们可以用它来开发交互式前端应用。

在本文中，我们将了解如何使用 Vue 路由器动态匹配路由。

# 路线参数

我们可以用`this.$route.params`对象获得路线参数。

例如，我们可以编写一个应用程序，显示如下传入的 URL 参数:

`src/index.js`:

```
const User = {
  computed: {
    username() {
      return this.$route.params.username;
    }
  },
  template: `<div>{{username}}</div>`
};const routes = [{ path: "/user/:username", component: User }];const router = new VueRouter({
  routes
});new Vue({
  el: "#app",
  router
});
```

`index.html`:

```
<!DOCTYPE html>
<html>
  <head>
    <title>App</title>
    <meta charset="UTF-8" />
    <script src="[https://unpkg.com/vue/dist/vue.js](https://unpkg.com/vue/dist/vue.js)"></script>
    <script src="[https://unpkg.com/vue-router/dist/vue-router.js](https://unpkg.com/vue-router/dist/vue-router.js)"></script>
  </head>
  <body>
    <div id="app">
      <div>
        <router-link to="/user/foo">Foo</router-link>
        <router-link to="/user/bar">Bar</router-link>
      </div>
      <router-view></router-view>
    </div>
    <script src="src/index.js"></script>
  </body>
</html>
```

`“/user/:username”`中的`:username`是参数，所以我们可以通过使用`this.$route.params.username`得到`/user/`之后传入的内容。

我们添加了一个返回`this.$route.params.username`的计算属性`username`，因此我们可以使用:

```
`<div>{{username}}</div>`
```

显示`username` URL 参数。

我们也可以在一个路由中有多个参数。

例如，我们可以这样写:

`src/index.js`:

```
const Name = {
  template: `<div>
    {{$route.params.firstName}} 
    {{$route.params.lastName}}
  </div>`
};const routes = [{ path: "/name/:firstName/:lastName", component: Name }];const router = new VueRouter({
  routes
});new Vue({
  el: "#app",
  router
});
```

`index.html`:

```
<!DOCTYPE html>
<html>
  <head>
    <title>App</title>
    <meta charset="UTF-8" />
    <script src="[https://unpkg.com/vue/dist/vue.js](https://unpkg.com/vue/dist/vue.js)"></script>
    <script src="[https://unpkg.com/vue-router/dist/vue-router.js](https://unpkg.com/vue-router/dist/vue-router.js)"></script>
  </head>
  <body>
    <div id="app">
      <div>
        <router-link to="/name/Jane/Doe">Jane</router-link>
        <router-link to="/name/Mary/Smith">Mary</router-link>
      </div>
      <router-view></router-view>
    </div>
    <script src="src/index.js"></script>
  </body>
</html>
```

然后，当我们单击链接时，我们会获得传入 URL 参数的名字和姓氏。

`:firstName`和`:lastName`按位置与参数匹配。

# 对参数变化做出反应

我们可以观察`$route`对象来观察参数的变化。名称相同但参数不同的布线使用相同的元件。它们不会从头开始重新渲染，所以不会调用生命周期挂钩。

例如，我们可以这样写:

`src/index.js`:

```
const Name = {
  data() {
    return {
      oldName: "",
      newName: ""
    };
  },
  watch: {
    $route(to, from) {
      this.oldName = from.params.name;
      this.newName = to.params.name;
    }
  },
  template: `<div>
    Old: {{oldName}} 
    New: {{newName}}
  </div>`
};const routes = [{ path: "/name/:name", component: Name }];const router = new VueRouter({
  routes
});new Vue({
  el: "#app",
  router
});
```

`index.html`:

```
<!DOCTYPE html>
<html>
  <head>
    <title>App</title>
    <meta charset="UTF-8" />
    <script src="[https://unpkg.com/vue/dist/vue.js](https://unpkg.com/vue/dist/vue.js)"></script>
    <script src="[https://unpkg.com/vue-router/dist/vue-router.js](https://unpkg.com/vue-router/dist/vue-router.js)"></script>
  </head>
  <body>
    <div id="app">
      <div>
        <router-link to="/name/Jane">Jane</router-link>
        <router-link to="/name/Mary">Mary</router-link>
      </div>
      <router-view></router-view>
    </div>
    <script src="src/index.js"></script>
  </body>
</html>
```

在上面的代码中，我们将`Name`组件映射到采用`name` route 参数的路由。

此外，`Name`有一个针对`$route`对象的观察器，它接受`to`和`from`参数。它们分别是以前的和当前的路线。

因此，当我们在链接之间来回点击时，我们会看到显示的旧参数和新参数。

![](img/372487e2c7f7847a341fd825d34c984f.png)

Photo by [Matt Duncan](https://unsplash.com/@foxxmd?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 全部捕获/ 404 未找到路径

我们可以添加`*`作为路由的通配符。

例如，我们可以使用:

```
path: '*'
```

匹配所有路线和:

```
path: '/user-*'
```

来匹配任何以`user-`开头的东西。

例如，我们可以使用通配符，如下所示:

`src/index.js`:

```
const Foo = { template: "<p>foo</p>" };
const Bar = { template: "<p>bar</p>" };
const NotFound = { template: "<p>not found</p>" };
const routes = [
  { path: "/foo", component: Foo },
  { path: "/bar", component: Bar },
  { path: "*", component: NotFound }
];const router = new VueRouter({
  routes
});new Vue({
  el: "#app",
  router
});
```

`index.html`:

```
<!DOCTYPE html>
<html>
  <head>
    <title>App</title>
    <meta charset="UTF-8" />
    <script src="[https://unpkg.com/vue/dist/vue.js](https://unpkg.com/vue/dist/vue.js)"></script>
    <script src="[https://unpkg.com/vue-router/dist/vue-router.js](https://unpkg.com/vue-router/dist/vue-router.js)"></script>
  </head>
  <body>
    <div id="app">
      <div>
        <router-link to="/foo">Foo</router-link>
        <router-link to="/bar">Bar</router-link>
      </div>
      <router-view></router-view>
    </div>
    <script src="src/index.js"></script>
  </body>
</html>
```

当我们转到除`/foo`或`/bar`之外的任何 URL 时，我们会看到“未找到”消息。

我们可以在其他文本中使用通配符，如下所示:

`src/index.js`:

```
const Foo = { template: "<p>foo</p>" };
const Bar = { template: "<p>bar</p>" };
const routes = [
  { path: "/foo*", component: Foo },
  { path: "/bar", component: Bar }
];const router = new VueRouter({
  routes
});new Vue({
  el: "#app",
  router
});
```

`index.html`:

```
<!DOCTYPE html>
<html>
  <head>
    <title>App</title>
    <meta charset="UTF-8" />
    <script src="[https://unpkg.com/vue/dist/vue.js](https://unpkg.com/vue/dist/vue.js)"></script>
    <script src="[https://unpkg.com/vue-router/dist/vue-router.js](https://unpkg.com/vue-router/dist/vue-router.js)"></script>
  </head>
  <body>
    <div id="app">
      <div>
        <router-link to="/foo">Foo</router-link>
        <router-link to="/bar">Bar</router-link>
      </div>
      <router-view></router-view>
    </div>
    <script src="src/index.js"></script>
  </body>
</html>
```

然后，当我们转到任何以`foo`开头的网址时，就会看到`foo`。

# 正则表达式

我们可以使用正则表达式来匹配路由参数。例如，我们可以写:

`src/index.js`:

```
const Foo = { template: "<p>foo {{$route.params.id}}</p>" };
const Bar = { template: "<p>bar</p>" };
const routes = [
  { path: "/foo/:id(\\d+)", component: Foo },
  { path: "/bar", component: Bar }
];const router = new VueRouter({
  routes
});new Vue({
  el: "#app",
  router
});
```

`index.html`:

```
<!DOCTYPE html>
<html>
  <head>
    <title>App</title>
    <meta charset="UTF-8" />
    <script src="[https://unpkg.com/vue/dist/vue.js](https://unpkg.com/vue/dist/vue.js)"></script>
    <script src="[https://unpkg.com/vue-router/dist/vue-router.js](https://unpkg.com/vue-router/dist/vue-router.js)"></script>
  </head>
  <body>
    <div id="app">
      <div>
        <router-link to="/foo">Foo</router-link>
        <router-link to="/bar">Bar</router-link>
      </div>
      <router-view></router-view>
    </div>
    <script src="src/index.js"></script>
  </body>
</html>
```

然后当我们走到`/foo/1`时，我们看到`foo 1`。如果`/foo/`后面的不是一个数字，那么我们什么也看不到。

# 结论

我们可以通过在想要传入的东西前面加上冒号来传入路由参数，然后我们可以从`this.$route.params`对象中获取它。

星号是一个通配符，匹配任何与列出的路由不匹配的 URL。我们还可以将它与其他文本结合起来，以匹配我们想要匹配的模式。

最后，我们可以使用正则表达式来匹配路由参数。

## **用简单英语写的 JavaScript 笔记**

我们已经推出了三种新的出版物！为我们的新出版物献上一点爱心吧，请跟随他们:[**AI in Plain English**](https://medium.com/ai-in-plain-english)，[**UX in Plain English**](https://medium.com/ux-in-plain-english)，[**Python in Plain English**](https://medium.com/python-in-plain-english)**——谢谢，继续学习！**

**我们也一直有兴趣帮助推广高质量的内容。如果您有一篇文章想要提交给我们的任何出版物，请发送电子邮件至:[**submissions @ plain English . io**](mailto:submissions@plainenglish.io)**，使用您的 Medium 用户名，我们会将您添加为作者。另外，请让我们知道您想加入哪个/哪些出版物。****