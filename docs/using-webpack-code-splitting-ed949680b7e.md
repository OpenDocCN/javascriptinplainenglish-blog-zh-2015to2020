# 使用 Webpack 代码分割

> 原文：<https://javascript.plainenglish.io/using-webpack-code-splitting-ed949680b7e?source=collection_archive---------12----------------------->

## 在本教程中，我们将研究一个叫做代码分割的 Webpack 特性。

![](img/1d55d1f682e6097aa621a0a3ba46e62d.png)

代码分割将允许您通过将主 JavaScript 文件分割成不同的文件来减小其大小，并在需要时延迟加载这些文件。

在构建单页面应用程序时，这是一个非常重要的特性，因为您的 JavaScript 文件可能非常大，这意味着您的页面需要等待文件下载完毕才能启动。

使用代码分割，您可以将代码分割成多个文件，并且只下载应用程序显示页面所需的文件。例如，如果您只在单个页面上使用一个组件，您不需要在应用程序的每个页面上加载该组件，使用代码分割 Webpack 可以仅在需要时延迟加载该组件。

为了能够动态加载脚本，你需要使用`import`的 Javascript 特性，而不是关键字，因为这是静态的，函数`import()`。

首先你需要安装一个动态导入的 babel 插件。

```
npm install babel-plugin-syntax-dynamic-import --save-dev
```

安装完成后，你需要添加一个`.babelrc`文件到你的项目的根目录，并添加如下配置。

```
{
    "presets": [
        "babel-preset-env"
    ],
    "plugins": [
        "babel-plugin-syntax-dynamic-import"
    ]
}
```

用 Javascript 导入文件非常简单，只需使用`import Module from './location-of-module.js'`。

使用`import()`语法，它将接受您想要导入的内容的单个参数，这将返回一个允许您解析模块内容的承诺。

例如，要使用它来导入 Vue 组件，您将使用以下语法。

```
Vue.component('search', (resolve) => {
    import('./components/Search.vue')
      .then((Search) => {
        resolve(Search.default)
      })
  })
```

当你使用类似于`npm run dev`的东西来构建你的 Javascript 时，你会看到你的普通主 Javascript 被构建，并且会有一个新的 0.js 文件。

```
DONE  Compiled successfully in 871ms

     Asset     Size  Chunks                    Chunk Names
      0.js  75.9.8 kB       0  [emitted]  [big]
/js/app.js   900.7 kB       1  [emitted]         /js/app
```

这个`0.js`文件将包含您的搜索组件，只有当组件需要显示在您的应用程序上时，Webpack 才会获取这个文件。

# Vue 单一文件组件

要在你的 Vue 单个文件组件中使用它，语法甚至更简单，你只需要在你定义组件的地方使用导入功能。

```
const app = new Vue({
    el: '#app',
    components: {
      Search: () => import('./components/Search.vue'),
    }
});
```

# 与 vue 路由器一起使用

如果你有一个使用 Vue Router 来定义在页面上显示什么组件的单页面应用程序，你仍然可以使用与上面相同的技术进行代码分割。

```
export function createRouter () {
  return new Router({
    mode: 'history',
    routes: [
      { path: '/', component: () => import('./components/Home.vue') },
      { path: '/item/:id', component: () => import('./components/Item.vue') }
    ]
  })
}
```

# 改变公共道路

我发现使用代码分割的一个问题是，在不同的环境中，JavaScript 文件存储在不同的位置。

要解决这个问题，你可以告诉 Webpack 你的公共文件夹在哪里，这样你就可以根据环境来改变它。

```
const ASSET_PATH = process.env.ASSET_PATH || '/';
mix.webpackConfig({
  output: {
    publicPath: ASSET_PATH
  }
})
```

# 预取模块

如果你有一个组件要延迟加载到你的应用中，你可以通过告诉它预取这个文件来加速浏览器的处理。

例如，假设您有一个登录按钮，它会弹出一个登录模式，这个登录模式可以在它自己的组件文件中，您可以使用上述技术对它进行代码拆分。当您为登录模式定义操作时，您将把它放在登录按钮组件中，并导入登录模式。

使用 Webpack 注释，您可以使用下面的代码告诉 Webpack 预取它。

```
import(/* webpackPrefetch: true */ "LoginModal");
```

然后，Webpack 会自动将`<link rel="prefetch" href="login-modal.js">`附加到页面的头部，这将告诉浏览器预取这个脚本，因为如果用户单击登录按钮，将需要它。

现在，您可以在项目中使用这一功能来拆分代码，并仅在需要时自动延迟加载这些组件。

*原载于【https://paulund.co.uk】[](https://paulund.co.uk/webpack-code-splitting)**。***