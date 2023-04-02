# 如何用 Webpack，Babel + SCSS 建立一个 JavaScript 项目

> 原文：<https://javascript.plainenglish.io/quickly-setup-a-javascript-project-with-webpack-babel-scss-6c5db2bebab7?source=collection_archive---------1----------------------->

![](img/85143d23647616812ca00b1e85e19b50.png)

Photo by [Max Nelson](https://unsplash.com/@maxcodes?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) on [Unsplash](https://unsplash.com/s/photos/webpack?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)

对于初学者来说，用 Webpack 和 Babel 建立一个 JavaScript 项目可能是一个令人望而生畏的步骤。我知道这是为了我，正因为如此，我害怕了很长一段时间。但久而久之，我明白了，其实也没那么难。然而，**为您的项目重复创建一个设置会很耗时。**

***TL；dr:*** *在本文中，我将向您展示快速设置的步骤。但如果你只是想进行回购，那么请点击* [*点击这里*](https://github.com/mazaherm/project-webpack-boilerplate) *。但是如果你想继续下去，那就继续读下去吧！*

# 第一步

打开终端 cd，找到您想要创建项目的位置。我喜欢在我的桌面上有一个名为 dev 的项目文件夹，所以在我的情况下，我将使用`cd desktop/dev``。

*   通过执行`mkdir <name of project folder>`创建您的项目文件夹。我打算把我的名字叫做`project-webpack-boilerplate`。
*   您可以使用 yarn 或 npm 创建一个`package.json`文件。我喜欢纱线，所以我会跑`yarn init`。在每一个提示符下按回车键。
*   像这样创建文件结构:

```
project-webpack-bolierplate
  --src
    --app
    --public
    --styles
  .babelrc
  package.json
  webpack.config.js
```

你`.babelrc, package.json and webpack.config.js`档应该和`src`在一个级别。

# 第二步

打开您的`webpack.config.js`文件并插入以下内容:

```
const HtmlWebpackPlugin = require('html-webpack-plugin')const ExtractTextPlugin = require('extract-text-webpack-plugin')require("babel-polyfill")module.exports = { entry: ['babel-polyfill', __dirname + '/src/app/index.js'], output: { path: __dirname + '/dist', filename: 'bundle.js', publicPath: '/' }, module: { rules: [ { test: /\.js*$*/, use: 'babel-loader', exclude: [/node_modules/]},{ test: /\.(sa|sc|c)ss*$*/, use: [{ loader: 'style-loader' }, { loader: 'css-loader' }, { loader: 'sass-loader' }] }, { test: /\.css*$*/, use: ExtractTextPlugin.extract({ fallback: 'style-loader', use: [ { loader: 'css-loader'}, { loader: 'sass-loader'} ], }) } ]}, plugins: [ new HtmlWebpackPlugin({ template: __dirname + '/src/public/index.html', inject: 'body' }), new ExtractTextPlugin('main.css') ], devServer: { contentBase: './src/public', port: 8000 }}
```

打开您的`.babelrc`文件并插入以下内容:

```
{ "presets": ["@babel/preset-env"],}
```

现在您应该已经用 webpack、babel 和 scss 建立了一个项目。所以我们来测试一下。

# 第三步

在 src 文件夹中创建以下内容:

```
--app
 index.js
--public
 index.html
--styles
 main.scss
```

# **第四步**

向 package.json 添加/安装软件包

```
"devDependencies": {"@babel/core": "^7.6.0","@babel/preset-env": "^7.6.0","babel-core": "^6.26.3","babel-loader": "^8.0.6","babel-plugin-module-resolver": "^3.2.0","babel-preset-env": "^1.7.0","babel-polyfill": "^6.26.0","css-loader": "^3.2.0","extract-text-webpack-plugin": "^4.0.0-beta.0","html-webpack-plugin": "^3.2.0","node-sass": "^4.12.0","sass-loader": "^8.0.0","style-loader": "^1.0.0","webpack": "^4.40.2","webpack-cli": "^3.3.8","webpack-dashboard": "^3.2.0","webpack-dev-server": "^3.8.0"}
```

您可以在 devDependencies 上方添加一个脚本来启动项目

```
"scripts": {"start": "webpack-dev-server --history-api-fallback --inline --progress"}
```

现在您应该有一个完整的 JavaScript 项目，包括 webpack、babel 和 scss 设置。

您想要将 React 添加到您的 JavaScript 项目中吗？看看我写的这个帖子！

您可能感兴趣的其他帖子:

[](/the-17-git-commands-you-should-know-as-a-junior-developer-9bd439adf5cc) [## 初级开发人员应该知道的 17 个 Git 命令

### 记住 git 命令可能很棘手。以下是你需要的最常见的一种

javascript.plainenglish.io](/the-17-git-commands-you-should-know-as-a-junior-developer-9bd439adf5cc) [](/how-to-call-and-use-third-party-apis-in-vanilla-javascript-with-a-full-example-da44b095da03) [## 如何在普通 JavaScript 中调用和使用第三方 API，并附有完整示例

### 学习如何调用和使用 API 和普通 JavaScript，包括完整的例子

javascript.plainenglish.io](/how-to-call-and-use-third-party-apis-in-vanilla-javascript-with-a-full-example-da44b095da03) [](/build-a-palindrome-checker-in-vanilla-javascript-3470304100a5) [## 用普通 JavaScript 构建一个回文检查器

### 学习如何用简单的 JavaScript 通过几个简单的步骤构建一个回文检查器。

javascript.plainenglish.io](/build-a-palindrome-checker-in-vanilla-javascript-3470304100a5) 

你可以参考 Github repo(链接[这里](https://github.com/mazaherm/project-webpack-boilerplate))来看看我是怎么实现的。我甚至在 babelrc 中添加了别名来逃避../../../../..地狱`。

或者看看我的帖子[用巴别塔为你的 JavaScript 项目快速设置别名](https://medium.com/@mazaher.muraj/quickly-setup-alias-for-your-javascript-project-using-babel-885c24693b25)

感谢阅读！

如果这是有帮助的，如果你给这篇文章一个👏如果你还没有分享，跟随的**将会很棒。**

[请考虑订阅我的推荐链接。Medium 是一个很好的学习平台，我用它来了解技术领域的最新动态，并学习其他开发人员的经验。](https://mazaher-muraj.medium.com/membership)

你的订阅将直接支持我和许多其他媒体作家。

[](https://mazaher-muraj.medium.com/membership) [## 通过我的推荐链接加入媒体

### 阅读 Mazaher Muraj(以及媒体上成千上万的其他作家)的每一个故事。您的会员费直接支持…

mazaher-muraj.medium.com](https://mazaher-muraj.medium.com/membership)