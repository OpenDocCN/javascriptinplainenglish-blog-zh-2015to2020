# 为什么你应该开始使用 Snowpack

> 原文：<https://javascript.plainenglish.io/why-you-should-start-to-use-snowpack-86a9cf999800?source=collection_archive---------1----------------------->

## 面向现代 web 应用的快速简单的免捆绑构建工具

![](img/891b28e37174b12d68bd4ca03f4760e7.png)

Photo by Polina Zimmerman in Pexels

今天，我们相信我们需要一个像 Webpack 这样的模块捆绑器来构建我们的应用程序，开发简单的应用程序，或者在开发期间，但是这不是真的。

在过去的某个时候，JavaScript 模块捆绑器成为 web 开发的任何阶段的必要部分，增加了任何开发的复杂性。

现代模块捆扎机的一个例子是 [Webpack](https://webpack.js.org/) 。Webpack 是一个强大的工具，可以做很多事情，但同时，可能很难配置，并且有很大的学习曲线。Webpack 是在 2012 年推出的，当时 ES 模块还不存在。

如今，我们有了其他替代产品，如无模块捆绑包的 Snowpack，2.0 版本将于本月晚些时候发布，但我们现在可以尝试一下。

注意:*这些例子在 Snowpack 的 2.0 版本和之前的版本*中都有效。

Snowpack 模块是在 2019 年推出的，当时现代浏览器已经支持 ES 模块。也就是说，我认为 Snowpack 在许多方面更容易使用，在许多情况下更实用，在本文中，我将向您展示一个简单的示例。

首先，创建一个 [npm](https://www.npmjs.com/) 项目(如果您想使用 yarn，情况类似):

*这将创建一个 package.json 文件，您将使用它来添加项目依赖项:*

```
$npm init
```

安装 Snowpack:

*我觉得还是本地安装比较好，不过随你喜欢。*

```
//Install locally, in your project:
$npm install --save-dev snowpack**@next**//Install glaobaly:
$npm install -g snowpack
```

注:添加“@ *下一个”尝试新的 2.0 版本。*

安装“js-storage”依赖项:

*在这个例子中，我将使用“*JS-Storage*”:*JS Storage 是一个简化存储访问的插件(HTML5)。

js-存储

```
$npm install --save-dev js-storage
```

前面的命令在/node_modules/文件夹中安装“js_storage”库。但是请注意，您不能直接使用它，因为它不符合 ESM 模块规范。

```
import js_storage from 'js-storage'; //No a valid ESM.
```

*虽然我们大多数人写的是“* js-storage *”，而不是“* js-storage *。如果没有文件扩展名，浏览器就无法理解它，因为它不是一个有效的 URL。Snowpack 允许您将 npm 包捆绑成单个文件，因此它可以生成那个* js-storage *。浏览器能理解的 js。*

运行 SnowPack:

我们将使用 npx 来执行 snowpack 二进制文件，因此我们需要安装 npx(执行 npm 包二进制文件)。

```
$ npm install -g npx//Execute
$ npx snowpack
```

什么也没发生:

⠼积雪场正在安装
✖没什么可安装的。

要解决这个问题，请打开 package.json 文件，并将您的依赖项添加到 Snowpack config:

```
{
  "name": "example",
  "version": "1.0.0",
  "description": "",
  "main": "index.js",
  "scripts": {
    "test": "echo \"Error: no test specified\" && exit 1"
  },
  "author": "",
  "license": "ISC",
  "devDependencies": {
    "js_storage": "^1.1.0",
    "snowpack": "^2.0.0-beta.9"
  },"snowpack": {
    "knownEntrypoints": [
      "js_storage"
    ]
  }
}
```

注意:*在以前版本的 SnowPack( < 2)中，“knownEntrypoints”被称为“webDependencies”。*

再次运行:

```
npx snowpack
```

✔扫雪包安装完成。[0.77 秒]

⦿网 _ 模块/大小 gzip
└─ js-storage.js

Snowpack 安装您的 npm 依赖项，以便它们可以直接在现代浏览器中运行(通过原生 ESM 导入)。

Snowpack 将您的软件包安装在“/web_modules”目录中(您可以配置路径*)。现在，您可以直接在浏览器中导入和使用它们，而无需额外的捆绑阶段。

*请注意，这种无需使用模块捆绑器就能在浏览器中导入 npm 包的能力是所有无捆绑开发和 Snowpack 的基础。*

直接在浏览器上使用依赖关系:

```
<script type="module">
   import js-storage 
       from "/web_modules/js_storage.js";
</script>
```

或者在 js 文件中使用依赖关系:

```
import js-storage
       from '/web_modules/js_storage.js';
```

* *在 package.json(可选)文件中配置输出路径:

```
...
"snowpack": {
    ...
    "installOptions": {
      "dest": "/**my_web_modules**"
    }
  }
...
```

这就是全部。直截了当地说，您可以在浏览器或 js 文件中使用 npm 依赖项。

Snowpack 还有一个“开发服务器”(一个用于 web 应用程序的即时开发环境)。如果执行以下命令，将启动一个静态服务器:

```
$snowpack dev
```

在“无捆绑”开发中，每个文件都是根据浏览器的请求构建的，所以启动时不需要做任何工作，也不需要在每次更改单个文件时再次“捆绑”。

您还可以部署您的应用程序，并通过执行以下命令为生产生成您的站点的静态副本:

```
$snowpack build
```

最后，您可以通过为生产环境“捆绑”您的最终部署(js 文件、css 文件)来优化您的站点，方法是执行以下命令:

```
$snowpack build -bundle
```

我们还可以使用 Snowpack 来驱动整个构建管道。Babel、TypeScript 和其他构建工具可以通过行转换直接连接到 Snowpack。这些转换被称为“构建脚本”

例如，创建 snowpack.config.json 文件:

```
{
  "scripts": {
   //Pipe every .js file through Babel CLI
   "build:js": "babel --no-babelrc"    
  }
}
```

## 结论

现在，现代浏览器已经支持 ES 模块，Snowpack 是一个完美的工具:如果您需要从头开始，它可以直接使用，并且您不需要编写大量的配置。它也很快，您可以以简单的方式在 dev o prod 模式下使用它。Webpack 配置更强大，但是也许你不需要这么多的个性化。

我正在一个大项目中使用 Snowpack，到目前为止，我没有遇到任何问题，而且，我的性能也有所提高。

## 参考

[积雪场网站](https://www.snowpack.dev/)

我希望你喜欢这篇文章。非常感谢你阅读我！