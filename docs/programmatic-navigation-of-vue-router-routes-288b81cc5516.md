# Vue 路由器路线的程序化导航

> 原文：<https://javascript.plainenglish.io/programmatic-navigation-of-vue-router-routes-288b81cc5516?source=collection_archive---------3----------------------->

![](img/27c3aa6b3ee15cc04857d94980b4e8d7.png)

Photo by [Richie Nolan](https://unsplash.com/@richienolan?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

Vue.js 是一个易于使用的 web 应用框架，我们可以用它来开发交互式前端应用。

在本文中，我们将了解如何动态浏览 Vue 路由器路由。

# 程序导航

除了使用`router-link`创建一个链接让用户导航路线之外，我们还可以通过编程导航路线。

# router.push(位置，onComplete？，奥纳博特？)

为此，我们可以使用组件可用的`$router`实例以编程方式导航路线。

点击`router-link`链接时会调用`router.push(…)`。我们可以自己调用它，以编程方式导航到路线。

例如，我们可以定义路线并以编程方式在路线中导航，如下所示:

`src/index.js`:

```
const Foo = { template: "<p>foo</p>" };
const Bar = { template: "<p>bar</p>" };const routes = [
  {
    path: "/foo",
    component: Foo
  },
  {
    path: "/bar",
    component: Bar
  }
];const router = new VueRouter({
  routes
});new Vue({
  el: "#app",
  router,
  methods: {
    goTo(route) {
      this.$router.push(route);
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
        <a href="#" @click='goTo("foo")'>Foo</a>
        <a href="#" @click='goTo("bar")'>Bar</a>
      </div>
      <router-view></router-view>
    </div>
    <script src="src/index.js"></script>
  </body>
</html>
```

在上面的代码中，我们有一个`goTo`方法，它接受一个字符串来表示我们想要去的路线。

在方法中，我们调用`this.$router.push(route);`去我们想要到达的路线。

因此，当我们点击`Foo`时，我们会看到`foo`，当我们点击`Bar`时，我们会看到`bar`。

我们也可以传入一个对象，如下所示:

```
this.$router.push({ path: route });
```

此外，我们可以在`$router.push`时前往指定路线。为此，我们写道:

`src/index.js`:

```
const Foo = { template: "<p>foo</p>" };
const Bar = { template: "<p>bar</p>" };const routes = [
  {
    name: "foo",
    path: "/foo",
    component: Foo
  },
  {
    name: "bar",
    path: "/bar",
    component: Bar
  }
];const router = new VueRouter({
  routes
});new Vue({
  el: "#app",
  router,
  methods: {
    goTo(name) {
      this.$router.push({ name });
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
        <a href="#" @click='goTo("foo")'>Foo</a>
        <a href="#" @click='goTo("bar")'>Bar</a>
      </div>
      <router-view></router-view>
    </div>
    <script src="src/index.js"></script>
  </body>
</html>
```

在上面的代码中，我们通过编写以下代码将`name`属性添加到我们的路由中来定义命名路由:

```
const routes = [
  {
    name: "foo",
    path: "/foo",
    component: Foo
  },
  {
    name: "bar",
    path: "/bar",
    component: Bar
  }
];
```

然后我们可以在`goTo`方法中按如下名称进入一条路线:

```
this.$router.push({ name });
```

我们可以按如下方式传入路由参数:

```
router.push({ name: 'user', params: { userId: '123' } })
```

以下**不会**工作:

```
router.push({ path: 'user', params: { userId: '123' } })
```

我们可以使用如下查询字符串来访问路径:

```
router.push({ path: 'user', query: { userId: '123' } })
```

或者:

```
router.push({ name: 'user', query: { userId: '123' } })
```

我们也可以使用如下的路径参数:

```
router.push({ path: `/user/123` });
```

例如，我们可以如下使用它们:

```
const Foo = { template: "<p>foo {{$route.query.id}}</p>" };
const Bar = { template: "<p>bar</p>" };const routes = [
  {
    path: "/foo",
    component: Foo
  },
  {
    path: "/bar",
    component: Bar
  }
];const router = new VueRouter({
  routes
});new Vue({
  el: "#app",
  router,
  methods: {
    goTo(path, query) {
      this.$router.push({ path, query });
    }
  }
});
```

然后，当我们单击`Foo`时，我们会看到`foo 1`，因为我们获取了一个以`id`为关键字的查询字符串。

和浏览器里去`/#/foo?id=1`是一样的。

同样的规则也适用于组件`router-link`的`to`属性。

在 Vue Router 2.2.0 或更高版本中，我们可以选择提供一个`onComplete`和`onAbort`回调到`router.push`或`router.replace`作为第二个和第三个参数。

在 Vue 路由器 3.1.0+中。`router.push`和`router.replace`将返回承诺，我们不需要传递第二个和第三个参数来处理这些情况。

如果我们的目的地与当前路线相同，只有参数在变化，比如从`/users/1`到`/users/2`，那么我们必须使用`beforeRouteUpdate`钩子对变化做出反应。

![](img/1ca120fc3f593fa430487a9d5f5cf937.png)

Photo by [George Hiles](https://unsplash.com/@hilesy?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# router.replace(位置，onComplete？，奥纳博特？)

`router.replace`的作用类似于`router.push`,只是没有添加新的历史条目。

`router.replace(…)`和`<router-link :to=”…” replace>`一样。

# 路由器. go(n)

我们可以使用`router.go`前进或后退，方法是为前进或后退的步数传递一个整数。负是向后，正是向前。

例如，我们可以如下使用它:

`src/index.js`:

```
const Foo = { template: "<p>foo</p>" };
const Bar = { template: "<p>bar</p>" };const routes = [
  {
    path: "/foo",
    component: Foo
  },
  {
    path: "/bar",
    component: Bar
  }
];const router = new VueRouter({
  routes
});new Vue({
  el: "#app",
  router,
  methods: {
    forward() {
      this.$router.go(-1);
    },
    back() {
      this.$router.go(1);
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
        <router-link to="foo">Foo</router-link>
        <router-link to="bar">Bar</router-link>
        <a href="#" @click="forward">Forward</a>
        <a href="#" @click="back">Back</a>
      </div>
      <router-view></router-view>
    </div>
    <script src="src/index.js"></script>
  </body>
</html>
```

我们必须以`forward`和`back`方法分别前进和后退。

如果没有这样的历史记录，将会自动失败。

# 结论

我们有`router.push`方法来使用不同的名称、路径、查询字符串或参数进入路径。

同样，我们可以用`router.replace`做同样的事情，但是不添加新的历史条目。

它们都接受一个用于路由的字符串或对象，以及一个`onComplete`和`onAbort`处理程序。

`router.go`让我们在浏览器历史中前进和后退。前进或后退需要许多步骤。

## **用简单英语写的 JavaScript 笔记**

我们已经推出了三种新的出版物！请关注我们的新出版物:[**AI in Plain English**](https://medium.com/ai-in-plain-english)，[**UX in Plain English**](https://medium.com/ux-in-plain-english)，[**Python in Plain English**](https://medium.com/python-in-plain-english)**——谢谢，继续学习！**

**我们也一直有兴趣帮助推广高质量的内容。如果您有一篇文章想要提交给我们的任何出版物，请发送电子邮件至[**submissions @ plain English . io**](mailto:submissions@plainenglish.io)**，使用您的 Medium 用户名，我们会将您添加为作者。另外，请让我们知道您想加入哪个/哪些出版物。****