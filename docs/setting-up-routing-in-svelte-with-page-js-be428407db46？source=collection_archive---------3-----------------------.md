# 使用 Page.js 在 Svelte 中设置路由

> 原文：<https://javascript.plainenglish.io/setting-up-routing-in-svelte-with-page-js-be428407db46?source=collection_archive---------3----------------------->

本文是关于使用 Svelte 系列文章的第二部分。我们已经(或将)涵盖的主题和文章如下:

1.  [设置苗条&安装顺风 CSS 和后置 CSS](https://jackwhiting.co.uk/posts/setting-up-svelte-and-integrating-tailwind-css)
2.  [用 Page.js 设置路由](https://jackwhiting.co.uk/posts/setting-up-routing-in-svelte-with-pagejs/#)
3.  [重构和优化我们的路由器](https://jackwhiting.co.uk/posts/refactoring-our-router-within-svelte)
4.  *..还有更多*

在上一篇文章中，我们简要地看了一下旋转苗条。这篇文章不要求你这样做，但是如果你没有一个苗条的安装，我建议你遵循苗条网站上的[快速入门指南](https://svelte.dev/blog/the-easiest-way-to-get-started)。

![](img/f974d22742e64bdc7e69f45bdbd199cb.png)

我们可以通过几种方式来查看 Svelte 中的路由，第一选择可能是 [Sapper](https://sapper.svelte.dev/) (目前处于测试阶段)，这是由 Svelte 团队构建的一个迷你框架。还有几个包可以用来集成路由，比如[svelet-routing](https://github.com/EmilTholin/svelte-routing)或 [Page.js](https://visionmedia.github.io/page.js/) 。

对于这篇文章，我们将利用令人敬畏的库 [Page.js](https://visionmedia.github.io/page.js/) 。我们选择了 Page.js，因为它对我们的单个路线提供了大量的粒度控制，并且不需要任何定制组件就可以处理我们站点中的任何锚(`<a>`)链接。我发现这样做效果更好，因为当内容从 WYSIWYG 或 Markdown 文件中呈现时，所有内部锚链接都可以完美地工作。

# 设置我们的环境

首先，我们需要安装 Page.js。在站点的根目录下打开您的终端并运行以下命令:

```
yarn add page *# or npm install page*
```

我们还需要对 package.json 文件进行调整，以确保如果我们在已经导航到的页面上重新加载，例如， */blog* ，我们不会收到服务器错误，它会加载适当的路径。打开 package.json 并编辑下面一行:

```
"start"**:** "sirv public"
```

追加`--single`选项，如下所示。

```
"start"**:** "sirv public --single"
```

这应该设置好一切，以便我们以后尽可能少地遇到错误。

# 编写路由器

当我们考虑如何将 Page.js 集成到 Svelte 中时，我想解释一下事情是如何工作的，并展示几个例子。Page.js 具有极强的可扩展性，因此您可以在每个路由的基础上执行许多逻辑。

## 设置路由器

在`App.svelte`文件的顶部包含来自 Page.js 包的路由器。我们还想为我们的 Home route 创建一个新文件，所以在下面的位置`pages/Home.svelte`创建一个新文件。

```
<script>
  **import** router from "page"

  *// Include our Routes
*  **import** Home from './pages/Home.svelte'
</script>
```

确保您用一些虚拟文本填充了`pages/Home.svelte`文件，这样您就可以区分它是否加载了路线，如下所示:

```
<h1>Home Page</h1>
```

接下来，回到我们的`App.svelte`文件中，我们将需要创建几个变量，我们将把它们传递给子组件，并告诉 Svelte 加载什么路径。

```
<script>
  ...   **let** page
  **let** params
</script>
```

为了查看 Page.js，并根据我们所在的 URL 更改组件，我们需要调用`router`包。我们可以这样做，将 URL 作为第一个属性，然后将我们的函数(更新组件)作为第二个属性。

```
*// Set up the pages to watch for* router('/', () => page **=** Home)
```

在上面的代码中，我们在"/"处观察对站点根目录的任何请求，然后将`page`变量设置为我们之前从`./routes/Home.svelte`文件导入的`Home`组件。

然后我们需要确保路由器一直在观察变化，这可以通过运行`router.start()`来实现。这将如下所示:

```
*// Set up the router to start and actively watch for changes* router.start()
```

最后，为了让路由器加载正确的组件，我们可以利用`[svelte:component](https://svelte.org/)`，这将允许我们在任何页面加载时重新创建和销毁组件(这意味着当您导航到任何锚点/浏览器历史更改时，将加载不同的路由)。在结束的`</script>`标签后添加以下内容:

```
<svelte:component this**=**{page} params**=**{params} />
```

你的`App.svelte`放在一起应该看起来像下面这样。

```
<script>
  **import** router from "page"

  *// Include our Routes
*  **import** Home from './pages/Home.svelte'

  *// Variables
*  **let** page
  **let** params

  *// Set up the pages to watch for
*  router('/', () => page **=** Home) *// Set up the router to start and actively watch for changes
*  router.start()
</script><svelte:component this**=**{page} params**=**{params} />
```

如果您从应用程序的根目录运行`yarn dev`，您现在应该看到主页加载并显示您添加到`Home.svelte`文件的任何内容。

## 扩展我们的路由器

为了让我们的路由器实际上路由东西，我们需要设置更多的 routes 实例，并将它们添加到我们的代码中。在 routes 目录中为`Blog.svelte`和`SingleBlog.svelte`创建两个新文件。

## 博客.苗条

如果你不完全理解这里发生的一切，不要担心。我在`Blog.svelte`的例子中包含了一些伪代码，因为我想展示我们如何基于路由参数动态加载内容。总结一下正在发生的事情——我们利用 Svelte 的`onMount`从一个虚拟 API 获取一系列 JSON 博客文章，然后遍历所有文章并显示每个文章的链接。

```
<script>
  **import** { onMount } from 'svelte' **const** apiUrl **=** 'https://jsonplaceholder.typicode.com/posts/'
  **let** data **=** [] onMount(async () => {
    **const** response **=** await fetch(apiUrl)
    data **=** await response.json()
  });
</script><h1>Blog</h1>{#each data as item }
  <div>
    <h5><a href**=**"/blog/{item.id}">{item.title}</a></h5>
  </div>
{/each}
```

## 单身博客

对于单个博客路径，我们将依靠我们在`App.svelte`中设置的`params`变量，然后从具有该 ID 的虚拟 API 中获取单个帖子。

```
<script>
  **import** { onMount } from 'svelte' **export** **let** params **const** apiUrl **=** 'https://jsonplaceholder.typicode.com/posts/'
  **let** data **=** [] onMount(async () => {
    **const** response **=** await fetch(apiUrl **+** params.id)
    data **=** await response.json()
  });
</script><h1>{data.title}</h1>
<p>{data.body}</p>
```

## 修改我们的原始代码

现在我们已经创建了两个新文件，我们需要返回并编辑我们的`App.svelte`来包含它们。打开文件并导入两条新路线。

```
**import** Blog from "./pages/Blog.svelte"
**import** SingleBlog from "./pages/SingleBlog.svelte"
```

添加两个新的`router`实例，这样我们可以观察新的页面。要监视访问*/博客*的用户，添加以下内容:

```
router('/blog', () => page **=** Blog);
```

当添加一个动态路由时，我们看到了另一个属性`router()`,它允许我们在到达路由端点之前加载或操作任何数据。我们将在代码中利用这一点，这样我们就可以确保将任何查询参数传递给最终组件。

在`router()`的其他实例下添加以下代码:

```
router(
  '/blog/:id', 

  *// Before we set the component
*  (ctx, next) => {
  	params **=** ctx.params
  	next()
  },   *// Finally set the component
*  () => page **=** SingleBlog
);
```

在这段代码中，我们在中间有另一个函数，它允许我们传递`ctx`(上下文)和一个`next`函数调用。在这个例子中，在继续更新路由组件之前，我们将 URL 中定义的`params`赋给代码中的`params`变量。

如果你回头看一下`routes/SingleBlog.svelte`文件的代码，你会看到我们使用了这些参数来允许我们从远程 API 获取我们正在访问的文章的 ID。

为了能够导航到这些路线，我们需要在我们的应用程序中添加一些样板文件，将`App.svelte`文件中结束`</script>`标记后的代码更新为:

```
<nav>
  <a href**=**"/">Home</a>
  <a href**=**"/blog">Blog</a>
</nav><main>
 <svelte:component this**=**{page} params**=**{params} />
</main>
```

我们的`App.svelte`文件现在应该看起来有点像这样:

```
<script>
  **import** router from "page"

  *// Include our Routes
*  **import** Home from "./routes/Home.svelte"
  **import** Blog from "./routes/Blog.svelte"
  **import** SingleBlog from "./routes/SingleBlog.svelte"

  *// Variables
*  **let** page
  **let** params

  *// Set up the pages to watch for
*  router('/', () => page **=** Home)
  router('/blog', () => page **=** Blog)
  router(
    '/blog/:id', 
    (ctx, next) => {
     params **=** ctx.params
     next()
    }, 
    () => page **=** SingleBlog
  ); *// Set up the router to start and actively watch for changes
*  router.start()
</script><nav>
  <a href**=**"/">Home</a>
  <a href**=**"/blog">Blog</a>
</nav><main>
 <svelte:component this**=**{page} params**=**{params} />
</main>
```

在您的浏览器中测试一下，您应该会看到一切正常，没有任何页面重新加载或错误。

# 进一步的例子

结合使用 Page.js 和 Svelte 可以做很多事情。虽然我觉得这篇文章有点长，但我确实想涵盖几个要点，这对任何构建更复杂应用程序的人来说都非常方便。

## 证明

我们可以利用`router()`来检查是否存储了用户变量，或者用户是否通过了身份验证，如果没有，则重定向到登录页面。这对于创建锁定区域非常有用。

```
<script>
  ...
  **import** AuthedRoute from './pages/AuthedRoute.svelte' 
  **import** Login from './pages/Login.svelte' *// any logic you have for getting the user here, or more to come on this later.
*  **let** user **=** **null**  
  router("/login", () => page **=** Login)
  router('/private', () => {
    *// If the user is not set, redirect to login 
*    **if** (**!** user) {
      router.redirect('/login')
    } page **=** PrivateRoute
  })
  ...
</script>
```

## 捕捉所有页面/错误页面

当用户点击一个我们没有设置路线的页面时会发生什么？嗯，目前他们会看到一个几乎空白的页面。我们可以通过在`router()`上设置一个通配符来捕捉这一点，并显示一个错误路由。

```
<script>
  ...
  **import** ErrorPage from "./pages/ErrorPage.svelte"

  router('/*', () => page **=** Error)
  ...
</script>
```

# 结论

这篇文章中有很多内容讨论了如何将 Page.js 实现为 Svelte 来创建一个动态路由器，我希望它是有用的。我们构建文档的方式目前还不是最干净的，因为这意味着我们必须在我们的`router()`调用和主`App.svelte`文件中重复很多逻辑。在以后的文章中，我想谈谈我们如何重构这一点，并更有效地设置路由。

*这篇文章对你有帮助吗？考虑通过* [*请我喝咖啡*](https://ko-fi.com/jackabox) *来支持我，帮助我制作更多优质的内容和未来的视频。* *原载于*[*https://jackwhiting.co.uk*](https://jackwhiting.co.uk/posts/setting-up-svelte-and-integrating-tailwind-css/)*。你可以在我的网站上找到更多类似的文章或者关于 PHP、JavaScript、Vue、Svelte 或者自由职业的文章。*