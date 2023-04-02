# 基本 Webpack 配置

> 原文：<https://javascript.plainenglish.io/basic-webpack-configuration-9e9d066ccc14?source=collection_archive---------2----------------------->

## 使用 Webpack 启动节点项目的简单步骤。

![](img/7c185edd6ca0c08a04999774e6b2ceb2.png)

Photo by [Max Nelson](https://unsplash.com/@maxcodes?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

如果你是初学者，想了解 node js，了解一下 Webpack 是很好的。配置 Webpack 可能是一项令人沮丧的任务。随着每一个版本日复一日的到来，插件中断和混乱的例子曲线学习网络包可以是粗糙的。

直接冲向配置好的样板文件可能会有帮助，但有时开发人员应该编写不带框架或任何花哨东西的普通 javascript。因此，让我们在接下来的两分钟内消除这些疑虑。

# 入门指南

[这里的](https://github.com/chitru/webpack-basic-configuration)就是我们要做的。浏览一遍，有个基本概念。

## 项目结构

看项目结构。

```
your_project(root folder)
|--fonts
|--images
|--sass
|--src
  -index.js
|--index.html
```

## 安装 Webpack

使用`npm init`命令在项目文件夹中创建 package.json 文件，开始使用 Nodejs 项目。然后我们可以用`npm i --save-dev webpack webpack-cli`安装 Webpack 作为开发依赖。

## 创建条目

Webpack 从一个名为 entry point 的 JavaScript 文件开始工作。在`src`文件夹中创建`index.js`。

## 正在创建 webpack.config.js

Webpack 的魔力始于以下几点:

## 编辑 package.json 中的 npm 脚本以运行 Webpack

要运行 Webpack，我们必须使用带有简单的`webpack`命令的`npm`脚本和我们的配置文件作为配置选项。
删除此`“test”: “echo \”Error: no test specified\” && exit 1"`增加`“build”: “webpack — config webpack.config.js”`

而`package.json`文件中的`scirpts`应该是这样的:

## 运行 Webpack

用基本设置运行`npm run build`命令。它的作用是:

*   查找条目文件，解析其中的导入模块依赖关系，并将其捆绑到名为 *dist 的新文件夹中的单个 *bundle.js* 文件中。*

> 到目前为止还能期待什么？
> 
> 你应该预料到当你运行`npm run build` 命令时，你会在根文件夹中有一个 *dist* 文件夹，里面有 *bundle.js* 。如果你想检查，你可以在你的文件夹里创建一个 HTML 文件。在 *index.js* 中创建 add `console.log(“Hello world”);` 并将`<script src=”dist/bundle.js”></script>`添加到您的 HTML 文件中，并在浏览器*控制台*中查看 Hello world 是否已打印。你应该会看到“你好，世界”的字样。

现在我们捆绑了标准的 JavaScript。那么现在我们来谈谈装载机。如果你想冷却 ES6(和未来版本)的功能，并兼容浏览器，该怎么办？如果你想把 ES6 代码转换成浏览器兼容的呢？这就是*装载机*发挥作用的地方。加载器是 Webpack 的一个主要特性，它可以转换我们的代码。让我们将`module.rules`添加到`webpack.config.js`中，如下所示。

## 使用 Babel-loader 安全地避开 ES6

Babel 是最好的 JavaScript transpiler，我们在捆绑它之前用它来转换现代 JavaScript 代码。基本上，它所做的是在捆绑之前，将我们的现代 JavaScript 代码转换成浏览器兼容的 JavaScript 代码。安装它与

`npm i --save-dev babel-loader @babel/core @babel/preset-env`

现在将规则添加到 JavaScript 文件中。

*   `test`是文件 JavaScript 文件扩展名的*正则表达式*。
*   `exclude`告诉 Webpack 在传输时排除`node_modules`文件夹。
*   `use`是我们设置加载器的主要规则，加载器将应用于与`test` regexp(我们文件中的 Javascript)相对应的文件。
*   `options`因装载机而异。在我们的文件中，我们已经为 Babel 设置了默认预设，以考虑它应该转换哪些 ES6 功能，而不应该转换哪些功能。如果你想学的话，这本身就是一个很大的话题，但是为了简单起见，我们将保持原样。

现在 ES6 可以安全地放在我们的代码中了。它将传输浏览器理解的 JavaScript。

如果你的代码还没有工作，这里有一个自我提醒。

![](img/a535a63fb2c1ef322fa7042bfc8528f9.png)

Photo by [Jordan Whitfield](https://unsplash.com/@whitfieldjordan?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

## 使用 CSS-loader、SASS-loader 和 MiniCSSExtractPlugin 添加 CSS 功能

SASS 预处理程序是一种新的样式化方法。它支持各种简单的 HTML 组件样式化方法。之后，我们可以使用自动前缀或缩小等。所以 Webpack 完全可以做到我们想要的。

如果您在入口点文件`index.js`中使用`import ‘./sass/style.scss`，它会给出一个错误，因此我们可以将 SASS-loader 添加到我们的 Webpack 配置中。

现在我们可以在`webpack.config.js`文件中为 SASS 文件添加一个新规则。同时，我们希望将所有转换后的 CSS 提取到单独的 *bundle.css* 文件中，为此，我们有一个名为*minicsextractplugin 的特殊插件。*安装时使用:

`npm i --save-dev mini-css-extract-plugin`

现在。*萨斯曼。scss，。css* 文件可以在你的 *index.js* 文件中安全使用。加载器的 Webpack 数组将被逐个应用。这是我们的 webpack.config.js 文件的样子。

## 使用文件加载器

首先，用`npm i — save-dev file-loader`安装`file-loader`，并给`webpack.config.js`添加一条新规则。

如果 Webpack 可以成功解析`dist`文件夹中的`images`文件夹中的文件，如果你的 CSS 文件中有图片。

现在最后运行`npm run build`,你将有你的上传准备好的文件供网络服务器使用。

享受吧。如果你需要任何帮助，请留言。感谢您的阅读。