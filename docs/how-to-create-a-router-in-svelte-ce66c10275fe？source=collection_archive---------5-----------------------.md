# 如何创建自己的苗条路由器

> 原文：<https://javascript.plainenglish.io/how-to-create-a-router-in-svelte-ce66c10275fe?source=collection_archive---------5----------------------->

## 苗条的

## 我们可以用几行代码避免不必要的包

![](img/76dc182f460f58fd5bba5b9fb29b993e.png)

Photo by [Javier Allegue Barros](https://unsplash.com/@soymeraki?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

在单页面应用程序中，例如当我们使用 JavaScript 框架时(就像我们钟爱的 Svelte 一样)，我们需要为用户提供一种方法，让他们根据需要在网站的各个部分之间导航。

因为在 SPA 中所有的网页请求都被重定向到根页面，所以我们需要模拟导航页面的“标准”方式。这就是路由库帮助我们的地方，虽然它看起来是一个复杂的任务，但它的基本功能非常简单，只需几行代码就可以完成。

在文章的最后，我们可以使用自己的路由器，也许是为了我们的下一个项目。

# 为什么要实施定制路由器

*   我们不需要当前路由器包的所有功能。SPA 的捆绑包大小是需要考虑的最重要的事情之一，因为性能与用户下载必要的脚本所需的时间成正比。
*   **我们可以定制路由器功能，以适应您的需求。**也许我们想要在页面之间添加一些过渡，或者在页面变化时触发一个功能，嗯……我们拥有代码的控制权，我们正在编写我们自以为是的规则。
*   **我们可以学习新的 Javascript 技能。**在本文中，例如，我们将使用 [*HTML5 历史 API*](https://developer.mozilla.org/en-US/docs/Web/API/History_API) ，一个本地 API 来与浏览器的历史进行交互。你会惊讶于有这么多的可能性。

# 路由器如何工作

让我们开始学习我们最喜欢的路由器是如何工作的。例如，让我们以*细长路由* npm 包为例。

```
<Router {url}>
    <Route path="blog/:id" component={Post}/>
    <Route path="blog" component={Blog}/>
    <Route path="about" component={About}/>
    <Route path="/" component={Home} />
</Router><script>
  **import** {Router, Route} **from** "svelte-routing";
  **import** Home **from** "./routes/Home.svelte";
  **import** About **from** "./routes/About.svelte";
  **import** Blog **from** "./routes/Blog.svelte";
  **import** Post **from** "./routes/Post.svelte"; **export let** url = "";
</script>
```

在这个包中，我们声明初始路径( *url* 变量)，将它传递给路由器组件，并声明所有的路由，每个路由都包含要加载的路径和组件。我们将在 main *App.svelte* 中编写这段代码，因此当用户请求特定页面时，我们总是传入根页面并返回他选择的组件/页面。

为此，路由器将读取当前的*窗口。位置*并将在声明的路由中搜索匹配相同模式的路由。因此，路由器将呈现正确的组件。

# 我们将构建的定制路由器示例

在本文中，我们将构建一个路由器组件，您可以这样使用它:

```
<Router {routes}/><script>
  **import** Router **from** "@/components/Router.svelte";
  **import** Home **from** "./routes/Home.svelte";
  **import** About **from** "./routes/About.svelte";
  **import** Blog **from** "./routes/Blog.svelte";
  **import** Post **from** "./routes/Post.svelte"; const routes = {
    "blog/:id": () => Post,
    "blog": () => Blog,
    "about": () => About,
    "/": () => Home
  };
</script>
```

# 如何实现路由器

那么让我们开始:创建一个新组件，将其命名为 *Router.svelte* 。这里我们需要呈现所选的页面并搜索正确的路线。

## 基本路由

假设您有以下路线:

*   /博客
*   /关于
*   /用户/帖子
*   /

首先，我们需要一个呈现当前页面的位置:

```
<**svelte:component** this={***component***}/>
```

我们还需要声明*组件*变量，并接受 *routes* 对象作为属性:

```
**let** component**;
export let** routes = {};
```

然后，我们必须找到我们之间的正确路线。让我们创建一个函数 LoadRoute:

```
**const** LoadRoute = *path* => {
  ***component*** = ***routes***[*path*]();
};
```

最后，在挂载时调用这个函数，传递当前页面:

```
**onMount**(() => {
  **LoadRoute**(*location.pathname*);
});
```

并使用以下函数拦截反向动作:

```
**window.onpopstate** = () => LoadRoute(*location.pathname*);
```

完整代码:

```
<**svelte:component** this={***component***}/><script>
  **import** {onMount} **from** "svelte"; **let** component**;
  export let** routes = {}; **onMount**(() => {
    **LoadRoute**(*location.pathname*);
  });
</script><script context="module">
 **const** LoadRoute = *path* => {
    ***component*** = ***routes***[*path*]();
  }; **window.onpopstate** = () => LoadRoute(*location.pathname*);
</script>
```

## 导航功能

为了在我们的网站中导航，我们需要实现一个将在整个项目中使用的功能，这样我们就可以在不重新加载的情况下更改页面。

将此添加到您的代码中:

```
<script context="module">
  // previous code **export const** navigate = *path* => {
    window.history.pushState(null, null, path);
    LoadRoute(path);
  };
</script>
```

现在，您可以在每个组件中以这种方式使用函数:

```
<**button** on:click={() => **navigate**("/about")}>
  About
</**button**>
```

## 路径变量

我们在路由器中发现的另一个常见的东西是带参数的路径，假设我们有一篇博客文章必须在 */blog/my-post* 处响应，其中 *my-post* 将是动态的。在这种情况下，路由器还必须搜索带有动态分段的路由。

例如，我们可以将以下路线添加到我们的路线中:

*   /blog/:id

在我们的路由器中，变量将由一个开始的“:”定义，就像瘦路由一样。

我们必须将所有可用的路径分割成段( *getRouteSegments* 函数)，然后将每个段与当前路径进行比较( *getRoute* )。

我们显然需要提取当前路径中的所有变量作为 props ( *getProps* 函数)。

将以下代码添加到我们的

```
**let** _routes = {};**const** getRouteSegments = *routes* =>
  Object.entries(routes).map(([path, component]) => ({
    path,
    component,
    segments: path
      .replace(/^\/+|\/+$/g, '')
      .split('/')
      .map(segment => ({
        name: segment.replace(':', ''),
        variable: segment.startsWith(':')
      }))
  }));**const** getRoute = *path* => {
  **const** segments = path.replace(/^\/+|\/+$/g, '').split('/'); **return** _routes.find(route => {
    if(route.segments.length !== segments.length) return false; **return** segments.every((*s*, *i*) =>
      route.segments[i].name === s ||
      route.segments[i].variable
    );
  });
};**const** getProps = (*path*, *routeSegments*) => {
  **let** props = {}; **const** segments = path.replace(/^\/+|\/+$/g, '').split('/');
  segments.map((*s*, *i*) =>
    routeSegments[i].variable &&
    (props[routeSegments[i].name] = s)
  ); **return** props;
};
```

现在，我们在<脚本>中声明*道具*变量:

```
**let** props = {};
```

我们编辑 onMount 函数:

```
onMount(() => {
  _routes = getRouteSegments(routes);
  LoadRoute(location.pathname);
});
```

我们的 LoadRoute 函数:

```
**const** LoadRoute = async(path) => {
  **const** current = getRoute(path); component = (**await** current.component()).default;
  props = getProps(path, current.segments);
};
```

最后，我们必须将道具传递给当前组件:

```
<**svelte:component** this={***component***} {***...props***}/>
```

## 完整的 Router.svelte 代码

```
<**svelte:component** this={***component***} {***...props***}/><script>
  **import** {onMount} **from** "svelte"; **let** component**;
  export let** routes = {}; **onMount**(() => {
    _routes = getRouteSegments(routes);
    LoadRoute(*location.pathname*);
  });
</script><script context="module">
 **let** _routes = {};
  **let** props = {}; **const** LoadRoute = async(*path*) => {
    **const** current = getRoute(path); component = (**await** current.component()).default;
    props = getProps(path, current.segments);
  }; **const** getRouteSegments = *routes* =>
    Object.entries(routes).map(([path, component]) => ({
      path,
      component,
      segments: path
        .replace(/^\/+|\/+$/g, '')
        .split('/')
        .map(segment => ({
          name: segment.replace(':', ''),
          variable: segment.startsWith(':')
        }))
    })); **const** getRoute = *path* => {
    **const** segments = path.replace(/^\/+|\/+$/g, '').split('/'); **return** _routes.find(route => {
      if(route.segments.length !== segments.length) return false; **return** segments.every((*s*, *i*) =>
        route.segments[i].name === s ||
        route.segments[i].variable
      );
    });
  }; **const** getProps = (*path*, *routeSegments*) => {
    **let** props = {}; **const** segments = path.replace(/^\/+|\/+$/g, '').split('/');
    segments.map((*s*, *i*) =>
      routeSegments[i].variable &&
      (props[routeSegments[i].name] = s)
    ); **return** props;
  }; **export const** navigate = *path* => {
    window.history.pushState(null, null, path);
    LoadRoute(path);
  }; **window.onpopstate** = () => LoadRoute(*location.pathname*);
</script>
```

# 结论

这是我写的第一篇关于 Medium 的文章，希望能写很多其他的文章，对你有所帮助。我将写下我作为一名 web 开发人员的经历以及我将使用的新技术。

如果我会犯一些语法错误，我很抱歉，但英语不是我的母语。

如果我帮了你，或者你有任何问题，请联系我，或者跟随我获取其他建议。谢谢大家！

**以下是我的一些其他文章:**

[](https://medium.com/javascript-in-plain-english/diy-multilanguage-in-javascript-project-65f8aa91a506) [## JavaScript 项目中的 DIY 多语言

### 让我们用这个简单的方法来国际化我们的 webapp

medium.com](https://medium.com/javascript-in-plain-english/diy-multilanguage-in-javascript-project-65f8aa91a506) [](https://medium.com/javascript-in-plain-english/create-reusable-svelte-components-for-your-page-layouts-9593b1a60b08) [## 为你的页面布局创建可重用的苗条组件

### 了解如何构建一个可以跨页面使用的组件

medium.com](https://medium.com/javascript-in-plain-english/create-reusable-svelte-components-for-your-page-layouts-9593b1a60b08) 

## **用简单英语写的 JavaScript**

喜欢这篇文章吗？如果有，通过 [**订阅获取更多类似内容解码，我们的 YouTube 频道**](https://www.youtube.com/channel/UCtipWUghju290NWcn8jhyAw) **！**