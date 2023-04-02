# Node.js 最佳实践—项目和异步

> 原文：<https://javascript.plainenglish.io/node-js-best-practices-project-and-async-3db755a12402?source=collection_archive---------7----------------------->

![](img/0bbb7037dee0e01f9fe7f073ae0ead4b.png)

Photo by [Abbie Love](https://unsplash.com/@misslove?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

像任何类型的应用程序一样，JavaScript 应用程序也必须写得很好。

否则，我们以后会遇到各种各样的问题。

在本文中，我们将看看在编写节点应用程序时应该遵循的一些最佳实践。

# 以`npm init`开始所有项目

我们应该以`npm init`开始 will 项目，以便在我们的应用程序项目中获得`package.json`文件。

这将让我们添加项目的元数据，如项目名称、版本和所需的包。

# 设置`.npmrc`

我们应该添加`.npmrc`来添加节点包存储库的 URL，我们必须在常规的 NPM 存储库之外使用它。

此外，我们可以使每个人的软件包版本相同。

为了让大家用同一个版本，我们可以写。

```
npm config set save=true
npm config set save-exact=true
```

我们制作的`npm install`也安装了相同的版本。

`package-lock.json`也将存储库锁定到相同的版本。

# 将脚本添加到我们的`package.json`

`package.json`可以持有项目相关的脚本。

都在`scripts`文件夹里。

这样，我们可以只使用一个命令来完成启动项目的所有工作。

我们可以用`npm start`给我们的项目添加一个`start`脚本。

例如，我们可以写:

```
"scripts": {
  "start": "node app.js"
}
```

然后只需运行`npm start`来运行我们的 app，而不是`node app.js`。

此外，我们可以添加更多命令:

```
"scripts": {
  "postinstall": "bower install && grunt build",
  "start": "node app.js",
  "test": "node ./node_modules/jasmine/bin/jasmine.js"
}
```

`postinstall`在`package.json`中的包安装后运行。

`npm test`运行运行测试的`test`脚本。

这让我们不用自己输入多个命令就可以运行测试。

我们还可以在`package.json`中添加自定义脚本。

我们可以用`npm run script {name}`运行它们，其中`name`是脚本的关键。

# 使用环境变量

我们应该为运行在不同环境中的东西设置环境变量。

例如，我们可以使用 dotenv 库从`.env`文件或环境变量中读取它们，这样我们就可以在我们的应用程序中使用它们。

可以在我们的节点应用程序中使用`process.env`访问它们。

# 使用样式指南

如果我们在风格指南中采用风格，那么我们就以一致的风格编写代码。

拥有一致的代码更容易理解。

我们可以用像 ESLint 这样的棉绒自动检查。

它包含内置节点应用程序的规则。

其他风格指南包括:

*   Airbnb—【https://github.com/airbnb/javascript 
*   谷歌—[https://google.github.io/styleguide/javascriptguide.xml](https://google.github.io/styleguide/javascriptguide.xml)
*   jQuery—[https://contribute.jquery.org/style-guide/js/](https://contribute.jquery.org/style-guide/js/)
*   标准 JS—[http://standardjs.com/](http://standardjs.com/)

# 拥抱异步

我们必须对节点应用程序使用异步代码。

这样，他们就不会在等待某件事情完成的时候耽误整个应用程序。

为了让我们的生活更容易，我们可以使用承诺来编写我们的异步代码。

为了使它们更短，我们可以使用 async 和 await。

我们可以用`util`模块将一些节点异步回调转换成承诺。

像`fs`这样的模块也有返回承诺的方法版本。

![](img/398586d7381e05cee46ed5909872cecb.png)

Photo by [Katie Bernotsky](https://unsplash.com/@pupscruffs?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 结论

我们应该充分利用`package.json`和`npm`项目提供的功能。

此外，我们应该尽可能使用异步。

一致的编码风格也很好。

## **简单英语的 JavaScript**

喜欢这篇文章吗？如果有，通过 [**订阅解码，我们的 YouTube 频道**](https://www.youtube.com/channel/UCtipWUghju290NWcn8jhyAw) **获取更多类似内容！**