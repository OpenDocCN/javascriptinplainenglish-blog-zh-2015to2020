# Vue.js 路由简介

> 原文：<https://javascript.plainenglish.io/introduction-to-vue-js-routing-122b7cce00b4?source=collection_archive---------12----------------------->

![](img/c9fc68c41dcb28e5d40873b5417b6238.png)

Photo by [Sawyer Bengtson](https://unsplash.com/@sawyerbengtson?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

Vue.js 是一个易于使用的 web 应用框架，我们可以用它来开发交互式前端应用。

为了用 Vue.js 创建一个单页应用程序，我们必须将 URL 映射到组件，这样当用户访问一个 URL 时，就会显示相应的组件。

在本文中，我们将了解如何使用 Vue 路由器创建一些简单的路由。

# 入门指南

我们可以通过添加一个带有 Vue 路由器库 URL 的`script`标签来使用 by Vue Router。

我们可以做一个简单的 app 如下:

`src/index.js`:

```
const Foo = { template: "<div>foo</div>" };
const Bar = { template: "<div>bar</div>" };const routes = [
  { path: "/foo", component: Foo },
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

在上面的代码中，我们在`src/index.js`中定义了两个组件`Foo`和`Bar`。

然后，我们用以下方式将它们映射到路线:

```
const routes = [
  { path: "/foo", component: Foo },
  { path: "/bar", component: Bar }
];const router = new VueRouter({
  routes
});
```

然后，我们用以下内容创建了一个新的 Vue 实例:

```
new Vue({
  el: "#app",
  router
});
```

在模板中，我们用`router-links`将路由映射到`a`标签，URL 对应于我们定义的路由:

```
<router-link to="/foo">Foo</router-link>
<router-link to="/bar">Bar</router-link>
```

然后我们用`router-view`来显示映射到路由的组件:

```
<router-view></router-view>
```

最后，我们应该得到:

```
Foo Link Bar Linkfoo
```

当我们点击`Foo Link`时。

并且:

```
Foo Link Bar Linkbar
```

当我们点击`Bar Link`时。

一旦我们注入了路由器，我们也可以访问`this.$router`。

我们可以用它来导航路线。例如，我们可以这样写:

`src/index.js`:

```
const Foo = { template: "<div>foo</div>" };
const Bar = { template: "<div>bar</div>" };const routes = [
  { path: "/foo", component: Foo },
  { path: "/bar", component: Bar }
];const router = new VueRouter({
  routes
});new Vue({
  el: "#app",
  router,
  methods: {
    goBack() {
      window.history.length > 1 ? this.$router.go(-1) : this.$router.push("/");
    }
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
    <script src="[https://unpkg.com/vue/dist/vue.js](https://unpkg.com/vue/dist/vue.js)"></script>
    <script src="[https://unpkg.com/vue-router/dist/vue-router.js](https://unpkg.com/vue-router/dist/vue-router.js)"></script>
  </head>
  <body>
    <div id="app">
      <div>
        <router-link to="/foo">Foo Link</router-link>
        <router-link to="/bar">Bar Link</router-link>
        <a href="#" @click="goBack">Go Back</a>
      </div>
      <router-view></router-view>
    </div>
    <script src="src/index.js"></script>
  </body>
</html>
```

然后，当我们点击几次`Foo Link`和`Bar Link`链接，然后点击`Go Back`时，我们会看到它会回到我们之前导航到的路线。

![](img/9df6d8d29f9e3b470f03c9000b156702.png)

Photo by [Justin Lawrence](https://unsplash.com/@thisisjlaw?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

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

`“/user/:username”`中的`:username`是参数，所以使用`this.$route.params.username`可以得到`/user/`之后传入的内容。

我们添加了一个返回`this.$route.params.username`的计算属性`username`，因此我们可以使用:

```
`<div>{{username}}</div>`
```

显示`username` URL 参数。

# 结论

我们可以通过使用 Vue 路由器将 URL 映射到组件。

一旦包含了它，我们就可以定义将 URL 映射到组件的路径。他们还可以获取参数，这些参数可以通过将 Vue 路由器注入到我们的应用程序中而获得的`this.$route`对象来检索。

为了显示链接到 Vue 路由器路由的链接，我们使用`router-link`，为了显示映射到路由的组件，我们使用`router-view`。

## **用简单英语写的 JavaScript 笔记**

我们已经推出了三种新的出版物！请关注我们的新出版物: [**AI in Plain English**](https://medium.com/ai-in-plain-english) ，[**UX in Plain English**](https://medium.com/ux-in-plain-english)，[**Python in Plain English**](https://medium.com/python-in-plain-english)**——谢谢，继续学习！**

**我们也一直有兴趣帮助推广高质量的内容。如果您有一篇文章想要提交给我们的任何出版物，请发送电子邮件至[**submissions @ plain English . io**](mailto:submissions@plainenglish.io)**，使用您的 Medium 用户名，我们会将您添加为作者。另外，请让我们知道您想加入哪个/哪些出版物。****