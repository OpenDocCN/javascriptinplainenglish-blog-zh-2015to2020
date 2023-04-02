# 可维护的 JavaScript —存储配置数据

> 原文：<https://javascript.plainenglish.io/maintainable-javascript-storing-config-data-3630d580bb2?source=collection_archive---------10----------------------->

![](img/9bcfe4e6db26c9a68ceef7e736f8186e.png)

Photo by [Fikri Rasyid](https://unsplash.com/@fikrirasyid?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

如果想继续使用代码，创建可维护的 JavaScript 代码很重要。

在本文中，我们将通过查看存储配置数据的方式来了解创建可维护的 JavaScript 代码的基础。

# 存储配置数据

有各种方法来存储我们的应用程序的配置数据。

像 React、Vue 和 Angular 这样的前端框架有它们自己存储特定于环境的配置数据的方式。

我们用来创建、运行和构建项目的 CLI 程序可以从中读取数据，并将正确的配置值放入我们的构建工件中。

例如，使用 Create React App，我们可以创建自己的配置文件

```
REACT_APP_NOT_SECRET_CODE=abcdef
```

在我们的`.env`文件中。

然后我们可以在我们的应用程序代码中读取带有`process.env.REACT_APP_NOT_SECRET_CODE`的值。

然后，当我们运行`npm start`来启动应用程序，运行`npm run build`来构建应用程序时，该值将被添加。

每个环境可以有一个文件。

我们可以:

*   `.env`:默认配置文件。
*   `.env.local`:本地超控
*   `.env.development`、`.env.test`、`.env.production`:环境特定设置
*   `.env.development.local`、`.env.test.local`、`.env.production.local` —本地超控。

我们检查 ib `.env`并忽略本地的。

只要密钥以`REACT_APP_`开头，那么就可以读取。

Vue CLI 也有类似的东西。

它需要以下文件:

*   `.env` —默认文件
*   `.env.local` —本地覆盖
*   `.env.[mode]` —针对特定环境的覆盖
*   `.env.[mode].local` —特定环境的本地覆盖

然后在`.env`文件中，我们可以写:

```
VUE_APP_NOT_SECRET_CODE=some_value
```

只要 config 键的前缀是`VUE_APP_`，Vue CLI 就会读取。

对于节点应用，我们可以使用 dotenv 包。

我们可以通过运行以下命令来安装它:

```
npm i dotenv
```

然后，我们可以通过编写以下内容来创建我们的配置:

```
DB_HOST=localhost
DB_USER=root
DB_PASS=s1mpl3
```

然后我们可以通过书写来使用它:

```
require('dotenv').config()
```

然后，我们可以通过编写以下内容来读取配置:

```
const db = require('db')
db.connect({
  host: process.env.DB_HOST,
  username: process.env.DB_USER,
  password: process.env.DB_PASS
})
```

从`process.env`获取配置值。

我们也可以用 JSON 格式存储这些值。

例如，我们可以写:

```
{
  "MESSAGE_INVALID_VALUE": "Invalid value",
  "URL_INVALID": "/errors/invalid"
}
```

来存储 JSON 文件。

然后我们可以用`fetch`、jQuery 的`getJSON`或者任何我们想要的东西来读取 JSON 文件。

如果我们使用像 Webpack、Rollup 或 package 这样的模块捆绑器，JSON 也可以用`import`直接导入。

例如，我们可以这样写:

```
import config from './config.json'
```

导入我们的 JSON 文件。

它将像一个普通的 JavaScript 对象一样被导入，我们可以像这样使用它们。

在所有示例中，我们将配置存储在应用程序之外。

这样，我们只需更改配置文件，这些值就会在引用它们的任何地方得到反映。

![](img/5e3072f90b87bb59b2e388fba692cd62.png)

Photo by [Christiann Koepke](https://unsplash.com/@christiannkoepke?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 结论

我们可以在外部存储配置数据，这样我们可以在一个地方更改值，它们将在任何地方得到反映。

我们可以为每个环境创建单独的配置文件，以使我们的应用程序在所有环境中都能工作。

![](img/787be6c671be8d345dc786dad8729ce5.png)

[Subscribe to Decoded, our official YouTube channel!](https://www.youtube.com/channel/UCtipWUghju290NWcn8jhyAw)