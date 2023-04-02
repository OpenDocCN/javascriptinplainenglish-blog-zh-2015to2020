# Vue 路由器路由器和组件防护以及导航解析流程

> 原文：<https://javascript.plainenglish.io/vue-router-router-and-component-guards-and-navigation-resolution-flow-f4a407eb7197?source=collection_archive---------4----------------------->

![](img/9924ce4f47d6dade6e26d2f173720d36.png)

Photo by [Linda Finkin](https://unsplash.com/@lwitham?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

Vue.js 是一个易于使用的 web 应用框架，我们可以用它来开发交互式前端应用。

Vue 路由器是一个 URL 路由器，它将 URL 映射到组件。

在本文中，我们将研究定义路径和组件内防护，并研究完整的导航解析流程。

# 每条路线的警卫

我们还可以定义每条路线的导航保护。例如，我们可以将其定义如下:

`src/index.js`:

```
const Login = { template: "<div>login</div>" };
const Profile = { template: "<div>profile</div>" };
const routes = [
  {
    path: "/",
    component: Login
  },
  {
    path: "/profile",
    component: Profile,
    beforeEnter: (to, from, next) => {
      if (!localStorage.getItem("token")) {
        next("/");
      } else {
        next();
      }
    }
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
      <router-view></router-view>
    </div>
    <script src="src/index.js"></script>
  </body>
</html>
```

`/profile`的`beforeEnter`钩子检查`localStorage.token`是否被定义。如果是的话，我们就让他们通过。否则，我们回到`/`。

因此，如果定义了`localStorage.token`，那么我们可以转到`/#/profile`并看到显示的`profile`。否则，我们会被重定向回`/#/`并看到登录。

签名与全局`beforeEach`回调相同。

# 组件内防护装置

我们可以在组件内部定义导航保护，挂钩如下:

*   `beforeRouteEnter`
*   `beforeRouteUpdate`
*   `beforeRouteLeave`

例如，我们可以如下使用它们:

## 在 RouteEnter 之前

如果我们想在继续路由之前检查令牌是否存在，我们可以编写一些类似下面的代码:

`src/index.js`:

```
const Login = { template: "<div>login</div>" };
const Profile = {
  data() {
    return { user: "foo" };
  },
  template: "<div>profile</div>",
  beforeRouteEnter(to, from, next) {
    if (!localStorage.getItem("token")) {
      return next("/");
    }
    next(vm => {
      alert(`You're logged in as ${vm.user}`);
    });
  }
};
const routes = [
  {
    path: "/",
    component: Login
  },
  {
    path: "/profile",
    component: Profile
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
      <router-view></router-view>
    </div>
    <script src="src/index.js"></script>
  </body>
</html>
```

在上面的代码中，我们使用了`beforeRouteEnter`钩子来检查令牌是否存在于本地存储中。如果是的话，我们继续。否则，我们回到`/`。

如果我们继续，那么用一个回调函数调用`next`，回调函数是作为参数的组件对象。因此，我们可以在`next`回调中从`vm`获取`user`属性。

最后，如果定义了一个`localStorage.token`，我们应该会看到显示“您以 foo 身份登录”的警告框。

只有`beforeRouteEnter`接受对`next`的回调，因为导航完成前组件不可用。

## 在 RouteUpdate 之前

我们可以使用`beforeRouteUpdate`如下:

`src/index.js`:

```
const Profile = {
  data() {
    return {
      id: undefined
    };
  },
  template: "<div>id: {{id}}</div>",
  beforeRouteUpdate(to, from, next) {
    this.id = to.params.id;
    next();
  }
};
const routes = [
  {
    path: "/profile/:id",
    component: Profile
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
      <router-view></router-view>
    </div>
    <script src="src/index.js"></script>
  </body>
</html>
```

然后当我们去`/#/profile/1`，然后`/#/profile/2`，我们会看到`id: 2`显示。

`beforeRouteUpdate`监视路由更新，因此当路由首次加载时，它什么也不做。

![](img/cfe5ec20a689c4f59f52556d3478e5db.png)

Photo by [Aaron Greenwood](https://unsplash.com/@onevibe?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

## beforeRouteLeave

我们可以使用`beforeRouteLeave`在用户导航离开某条路线之前运行代码。

例如，我们可以如下使用它:

`src/index.js`:

```
const Profile = {
  template: "<div>profile</div>",
  beforeRouteLeave(to, from, next) {
    const answer = window.confirm("Are you sure you want to leave?");
    if (answer) {
      next();
    } else {
      next(false);
    }
  }
};
const routes = [
  {
    path: "/profile",
    component: Profile
  }
];const router = new VueRouter({
  routes
});new Vue({
  el: "#app",
  router
});
```

`index.html:`

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
      <router-view></router-view>
    </div>
    <script src="src/index.js"></script>
  </body>
</html>
```

在上面的代码中，我们有一个在用户试图离开一条路线之前运行的`beforeRouteLeave`钩子。

我们有:

```
const answer = window.confirm("Are you sure you want to leave?");
```

询问用户是否想离开。如果他点击 OK，导航将通过调用`next()`继续。否则，调用`next(false)`并取消导航。

# 全导航分辨率流程

导航解析的完整流程如下:

1.  导航触发
2.  `beforeRouteLeave`叫做
3.  全局`beforeEach`警卫打来电话
4.  `beforeRouterUpdate`在组件中称为
5.  `beforeEnter`在路由配置中调用
6.  解析异步路由组件
7.  `beforeEnter`在激活的组件中称为
8.  全球`beforeResolve`警卫来电
9.  导航确认
10.  全局`afterEach`钩子调用
11.  DOM 更新已触发
12.  回调传递给被调用的`beforeRouteEnter`中的`next`

# 结论

我们可以在单独的路线和组件中设置路线防护。它们有自己的挂钩，但它们可以为每个路线或组件提供相同的用途。

唯一的例外是在`next`函数中接受回调的`beforeRouteEnter`。

全球导航卫星系统通常在本地导航卫星系统之前被调用。

## **用简单英语写的 JavaScript 笔记**

我们已经推出了三种新的出版物！请关注我们的新出版物:[**AI in Plain English**](https://medium.com/ai-in-plain-english)，[**UX in Plain English**](https://medium.com/ux-in-plain-english)，[**Python in Plain English**](https://medium.com/python-in-plain-english)**——谢谢，继续学习！**

**我们也一直有兴趣帮助推广高质量的内容。如果您有一篇文章想要提交给我们的任何出版物，请发送电子邮件至[**submissions @ plain English . io**](mailto:submissions@plainenglish.io)**，使用您的 Medium 用户名，我们会将您添加为作者。另外，请让我们知道您想加入哪个/哪些出版物。****