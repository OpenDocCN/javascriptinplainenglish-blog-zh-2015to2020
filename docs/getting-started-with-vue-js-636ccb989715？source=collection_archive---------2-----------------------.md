# Vue.js 入门

> 原文：<https://javascript.plainenglish.io/getting-started-with-vue-js-636ccb989715?source=collection_archive---------2----------------------->

![](img/68b7c2e61aa3ed31b801c9ee4753e66f.png)

Photo by [Cristina Gottardi](https://unsplash.com/@cristina_gottardi?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

Vue.js 是一个易于使用的 web 应用框架，我们可以用它来开发交互式前端应用。

在本文中，我们将了解如何将 Vue.js 框架添加到我们的应用程序中，并构建我们的第一个应用程序。

# 脚本标签

我们可以包含带有`script`标签的 Vue.js 框架。

为此，我们可以编写:

```
<script src='https://vuejs.org/js/vue.js'></script>
```

在我们的应用程序代码加载之前。上面的 URL 有开发版本，有所有的警告，但没有缩小。

为了包含生产版本，我们可以编写:

```
<script src='[https://vuejs.org/js/vue.min.js](https://vuejs.org/js/vue.min.js)'></script>
```

要将版本修正到 2.6.11，我们可以写:

```
<script src="https://cdn.jsdelivr.net/npm/vue@2.6.11"></script>
```

如果我们不需要支持 Internet Explorer，也可以在浏览器中导入模块，如下所示:

```
<script type="module">   
  import Vue from 'https://cdn.jsdelivr.net/npm/vue@2.6.11/dist/vue.esm.browser.js' </script>
```

`vue.esm.browser.js`文件已经针对浏览器进行了优化，因此可以在不创建太多请求的情况下使用它。

# 例子

为了创建我们的第一个应用程序，我们可以如下创建`index.html`文件:

```
<!DOCTYPE html>
<html>
  <head>
    <title>Parcel Sandbox</title>
    <meta charset="UTF-8" />
    <script src="[https://vuejs.org/js/vue.js](https://vuejs.org/js/vue.js)"></script>
  </head> <body>
    <div id="app"></div>
    <script src="./src/index.js"></script>
  </body>
</html>
```

然后在`src/index.js`中，我们可以添加:

```
new Vue({
  template: "<div>{{ 'hello' }}</div>",
  el: "#app"
});
```

然后我们应该在屏幕上看到`hello`，因为我们有一个 ID 为`app`的`div`，Vue.js 使用它来呈现我们的`Vue`实例。

Vue.js 编译器负责将模板转换成 HTML。

在上面的模板中，`'hello'`是一个 JavaScript 字符串，它在 Vue 编译器运行时被转换成 HTML 文本。

我们指定模板有一个包含文本`'hello'`的`div`。

# Vue.js 框架的不同版本

Vue.js 框架有几种不同的版本。他们是:

*   完整版—包含编译器和运行时的版本。这就是我们在上面的例子中使用的。
*   编译器——负责将模板字符串编译成 JavaScript 渲染函数的代码
*   运行时——负责创建 Vue 实例、渲染和更新虚拟 DOM 等的代码。
*   UMD —一个可以直接包含在`script`标签中的模块。它包括运行时和编译器
*   CommonJS —一个可以与 Browserify 或 Webpack 1 等较旧的捆扎机一起使用的模块。
*   ES 模块—2.6 版的新增功能。它可以通过`<script type='module'>`直接由浏览器使用，也可以与 Webpack 2 或 Rollup 等捆绑器一起使用。

# 运行时和编译器

运行时用于通过`new Vue`创建`Vue`实例。

编译器用于编译由`template`选项指定的模板。

这意味着:

```
new Vue({
  template: "<div>{{ 'hello' }}</div>",
  el: "#app"
});
```

需要编译器和:

```
new Vue({
  el: "#app",
  render(h) {
    return h("div", "hello");
  }
});
```

不会。

当我们的应用程序变得更复杂时，模板语法会更方便。

当我们使用`vue-loader`或`vueify`时，`.vue`文件中的模板被预编译成 JavaScript，所以我们不需要最终包中的编译器。

当我们不需要使用编译器时，我们应该只使用运行时编译，因为它要小 30%。

根据我们使用的捆绑器，我们可以包含不同的构建。例如，在包裹中，我们可以写:

```
"alias": {     
  "vue" : "./node_modules/vue/dist/vue.common.js"   
}
```

在`package.json`里。

![](img/c0e56a0fc3086b2b4deb9d8f11099507.png)

Photo by [Paul Gilmore](https://unsplash.com/@paulgilmore_?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# Vue CLI

我们可以通过 Vue-CLI 自动创建并构建我们的应用，这样我们就不必担心要选择哪些包了。

要安装它，我们可以运行:

```
npm install -g @vue/cli
```

或者:

```
yarn global add @vue/cli
```

然后，我们可以通过运行以下命令来创建一个项目:

```
vue create project
```

或者运行:

```
vue ui
```

显示 GUI，让我们在浏览器中创建一个 Vue 项目。

此外，我们可以通过运行以下命令使用`npx`直接运行 Vue CLI:

```
npx vue create project
```

运行`vue create`将为我们呈现一个具有各种选项的向导，例如:

```
Vue CLI v3.11.0
┌───────────────────────────┐
│  Update available: 4.1.2  │
└───────────────────────────┘
? Please pick a preset: Manually select features
? Check the features needed for your project: (Press <space> to select, <a> to toggle all, <i> to invert selection)
>(*) Babel
 ( ) TypeScript
 ( ) Progressive Web App (PWA) Support
 ( ) Router
 ( ) Vuex
 ( ) CSS Pre-processors
 (*) Linter / Formatter
 ( ) Unit Testing
 ( ) E2E Testing
```

当我们选择`Manually Select Features`时，该屏幕出现。如果我们不想从这个屏幕中选择任何东西，我们可以选择`Default`。

要运行 Vue UI，我们可以运行:

```
npx vue ui
```

然后我们的浏览器会自动转到[http://localhost:8000/project/select](http://localhost:8000/project/select)，在这里我们会看到一个创建项目的创建链接。

单击“create”选项卡后，我们可以通过单击“Create a new project”来创建项目。

# 结论

我们可以创建一个新的 Vue.js 应用程序，方法是通过`script`标签或使用 Vue CLI 或 Vue UI web 应用程序来包含它。

首先，我们添加了带有`script`标签的 Vue.js，然后创建一个新的`Vue`实例来创建一个应用程序。

Vue.js 框架有各种版本。如果我们需要模板，我们需要完整的构建，除非它是用`vue-loader`或`vueify`预构建的。

否则，我们可以使用仅运行时构建来减少它的大小。