# 管理。React 应用程序中的环境变量

> 原文：<https://javascript.plainenglish.io/managing-env-variables-in-react-app-4c48da162887?source=collection_archive---------6----------------------->

## 设置多个暂存环境的分步指南

![](img/1e633551f96d5f3b097a4c5ced6b57f3.png)

Photo by [Caspar Camille Rubin](https://unsplash.com/@casparrubin?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) on [Unsplash](https://unsplash.com/s/photos/javascript?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)

当使用 Create React App 开发 web 应用程序时，默认情况下，开发人员在本地环境中获得`NODE_ENV=development`，在生产版本中获得`NODE_ENV=production`。并且，禁止修改`NODE_ENV`。根据 Create React 应用程序，这是一个有意的设置，以保护`production`环境免受错误/意外部署的影响。

让我们在一个新创建的 react 应用程序中进行实验，并如下修改 **app.js**

```
import React from "react";
const App = ()=>(
 <div>
  <p>The App is reading  - {process.env.NODE_ENV} .env </p>
 </div>
)
export default App;
```

在浏览器中，我们可以看到类似这样的内容:

```
The App is reading - development .env
```

这是因为:

```
scripts: {
  "start": "react-scripts start", // "NODE_ENV" is equal to       //"development".
  "build": "react-scripts build", // "NODE_ENV" is equal to "production".
}
```

现在我们要把**分开。像 app 根目录中的 **.env.development、**和 **.env.test** 这样的 env** 。以下是要遵循的步骤:

# 第一步

现在安装这个软件包:

```
npm install env-cmd --save
```

# 第二步

修改 **package.json** 如下:

add these in package.json inside scripts

# 第三步

创建我们的**。env** 文件:

[.env.development](https://gist.github.com/TunvirRahman/c3ad916b4a9a3bbc8b9ef8034150aa2e)

[.env.production](https://gist.github.com/TunvirRahman/9b386c991341a157f3504e855577a476)

[.env.test](https://gist.github.com/TunvirRahman/4408c8d29bd07383af46024bbc04b142)

*注意:创建自定义环境变量时需要前缀* `*REACT_APP_*` *。*

# 最后

修改 **app.js** 文件如下:

```
import React from "react";
const App = ()=>(
<div>
  <p>The App is reading  - {process.env.REACT_APP_STAGE} .env </p>        </div>
)
export default App;
```

现在如果我们跑:

```
yarn run start:stage:prod
```

输出将类似于:

```
The App is running on — Production // In Your Browser
```

就这样，我们现在为 React 应用程序提供了 3 个暂存环境！

# 测试

```
 /// package.json add these"jest": {
  "setupFiles": [
    "<rootDir>/jest/setEnvVars.js"
  ]
}
```

创建`./jest/setEnvVars.js`并放置您的模拟环境变量

```
REACT_APP_STAGE = 'mock';
```

# 感谢阅读！🍻

## 简单英语的 JavaScript

喜欢这篇文章吗？如果有，通过 [**订阅我们的 YouTube 频道**](https://www.youtube.com/channel/UCtipWUghju290NWcn8jhyAw) **获取更多类似内容！**