# 使用 Vue 路由器定义嵌套路由

> 原文：<https://javascript.plainenglish.io/defining-nested-routes-with-vue-router-2ce874cf2ca0?source=collection_archive---------2----------------------->

![](img/048fe2f316783ec8d1ae7b03f192b03e.png)

Photo by [chuttersnap](https://unsplash.com/@chuttersnap?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

Vue.js 是一个易于使用的 web 应用框架，我们可以用它来开发交互式前端应用。

在本文中，我们将了解如何使用 Vue 路由器定义嵌套路由。

# 为什么我们需要嵌套路由？

我们需要嵌套路由来组织组件嵌套多层的路由。

例如，我们必须找到一种组织路线的方法，如:

```
/user/bar/profile                     
/user/bar/posts
```

变成一个连贯的地方。

# 定义嵌套路线

我们可以通过在路由条目中添加一个`children`数组来定义嵌套路由，然后在父路由的模板中，我们必须添加`router-view`来显示子路由。

例如，我们可以这样写:

`src/index.js`:

```
const Profile = { template: "<p>{{$route.params.user}}'s profile</p>" };
const Posts = { template: "<p>{{$route.params.user}}'s posts</p>" };
const User = {
  template: `
    <div>
      <p>user - {{$route.params.user}}</p>
      <router-view></router-view>
    </div>
`
};
const routes = [
  {
    path: "/user/:user",
    component: User,
    children: [
      { path: "posts", component: Posts },
      { path: "profile", component: Profile }
    ]
  }
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
        <router-link to="/user/foo">Foo User</router-link>
        <router-link to="/user/foo/posts">Foo Post</router-link>
        <router-link to="/user/foo/profile">Foo Profile</router-link>
      </div>
      <router-view></router-view>
    </div>
    <script src="src/index.js"></script>
  </body>
</html>
```

在上面的代码中，我们有:

```
const routes = [
  {
    path: "/user/:user",
    component: User,
    children: [
      { path: "posts", component: Posts },
      { path: "profile", component: Profile }
    ]
  }
];
```

定义子路由。

`Posts`和`Profile`组件就像我们通常定义的任何组件一样。

然而，`User`组件有`router-view`来显示`Posts`和`Profile`组件的内容。

在模板中，我们有 3 条通向根路由和子路由的路由器链路:

```
<router-link to="/user/foo">Foo User</router-link>
<router-link to="/user/foo/posts">Foo Post</router-link>
<router-link to="/user/foo/profile">Foo Profile</router-link>
```

那么我们应该看到:

```
Foo User Foo Post Foo Profile
user - foofoo's posts
```

当我们转到`/#/user/foo/posts`或点击`Foo Post`时。

我们应该看到:

```
Foo User Foo Post Foo Profileuser - foofoo's profile
```

当我们转到`/#/user/foo/profile`或点击`Foo Profile`时。

当我们转到`/#/user/foo`或点击`Foo User`时，我们会看到:

```
Foo User Foo Post Foo Profileuser - foo
```

任何以`/`开头的嵌套路径都将被视为根路径。这让我们可以使用组件嵌套，而不必使用嵌套的 URL。

此外，父路由和子路由都可以访问相同的路由参数。

例如，我们可以将一个`UserHome`组件及其路由添加到上面的示例中，如下所示:

`src/index.js`:

```
const Profile = { template: "<p>{{$route.params.user}}'s profile</p>" };
const Posts = { template: "<p>{{$route.params.user}}'s posts</p>" };
const UserHome = { template: "<p>{{$route.params.user}} home</p>" };
const User = {
  template: `
    <div>
      <p>user - {{$route.params.user}}</p>
      <router-view></router-view>
    </div>
`
};
const routes = [
  {
    path: "/user/:user",
    component: User,
    children: [
      { path: "", component: UserHome },
      { path: "posts", component: Posts },
      { path: "profile", component: Profile }
    ]
  }
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
        <router-link to="/user/foo">Foo User</router-link>
        <router-link to="/user/foo/posts">Foo Post</router-link>
        <router-link to="/user/foo/profile">Foo Profile</router-link>
      </div>
      <router-view></router-view>
    </div>
    <script src="src/index.js"></script>
  </body>
</html>
```

现在，当我们转到`/user/foo`或点击`Foo User`，我们会看到:

```
Foo User Foo Post Foo Profileuser - foofoo home
```

其他一切都保持不变。

![](img/08955c99b998dd6c74a3bee30f618b72.png)

Photo by [D A V I D S O N L U N A](https://unsplash.com/@davidsonluna?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 结论

我们可以通过添加一个带有路由数组的`children`属性来添加嵌套路由。

然后我们必须将`router-view`添加到父路由中，这样我们就可以看到子路由的内容。

父路由和子路由都可以访问相同的路由参数。

## **用简单英语写的 JavaScript 笔记**

我们已经推出了三种新的出版物！请关注我们的新出版物:[**AI in Plain English**](https://medium.com/ai-in-plain-english)[**UX in Plain English**](https://medium.com/ux-in-plain-english)[**Python in Plain English**](https://medium.com/python-in-plain-english)**——谢谢，继续学习！**

**我们也一直有兴趣帮助推广高质量的内容。如果您有一篇文章想要提交给我们的任何出版物，请发送电子邮件至[**submissions @ plain English . io**](mailto:submissions@plainenglish.io)**，并附上您的 Medium 用户名，我们会将您添加为作者。另外，请让我们知道您想加入哪个/哪些出版物。****