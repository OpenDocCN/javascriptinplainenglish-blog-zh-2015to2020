# TypeScript 项目设置—热重装、库和构建目标

> 原文：<https://javascript.plainenglish.io/typescript-project-setup-hot-reloading-libraries-and-build-targets-99863e746aca?source=collection_archive---------3----------------------->

![](img/9f37807fb0b21068ae470d9f202afa5e.png)

Photo by [Eaters Collective](https://unsplash.com/@eaterscollective?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

TypeScript 是 JavaScript 的自然扩展，在许多项目中取代了 JavaScript。

然而，并不是每个人都知道它实际上是如何工作的。

在本文中，我们将研究如何创建一个进行热重载的 TypeScript 项目。

我们还将了解如何选择要包含在项目中的类型定义，以便我们可以使用我们想要的特性。

# 使用观察模式

为了让我们的生活更轻松，我们应该确保每次保存代码时，我们的项目都会重新编译和重新加载。

这样，我们就不必在每次修改代码时重新编译和重启我们的应用程序。

在 watch 模式下，我们会观察发生变化的源代码文件，然后编译器会自动重新构建应用。

我们的 TypeScript 项目应该已经安装了 TypeScript 编译器。

然后我们运行:

```
tsc --watch
```

监视代码更改并重新编译代码。

现在，如果我们更改代码，我们应该在编译时看到消息，如果发现任何编译器错误，就会显示出来。

# 编译后自动执行代码

只重新编译文件，不运行它们。

要运行它们，我们必须使用另一个程序。

`ts-watch`包以监视模式启动编译器，观察 9 的输出，并根据编译结果运行命令。

例如，我们可以写:

```
npx tsc-watch --onsuccess "node dist/index.js"
```

在没有安装的情况下运行`tsc-watch`，并在节点成功的情况下运行`dist/index.js`。

# 使用 NPM 启动编译器

我们可以在`package.json`中添加一个`start`脚本来运行命令来启动项目。

所以我们可以写:

```
{
  //...
  "scripts": {  
    "start": "tsc-watch --onsuccess \"node dist/index.js\""  
  },
  //...
}
```

# 版本定位

有了 TypeScript 编译器，我们可以在最终构建中针对不同版本的 JavaScript。

我们可以从低至 ES3 的版本升级到最新版本。

这样，我们可以轻松地支持传统浏览器。

例如，我们可以写:

```
{  
  "compilerOptions": {  
    "target": "es5",
    //...
  }
}
```

支持像 Internet Explorer 这样的浏览器，这些浏览器不支持内置的最新 JavaScript 特性。

支持以下版本。

`es3`–该语言的第三版。当没有指定`target`时，这是默认值。它是在 1999 年定义的

`es5`—2009 年 12 月发布的 JavaScript 第五版

第六版 JavaScript，增加了许多创建复杂应用的特性，如类、模块、箭头函数、承诺等。

`es2015` —同`es6`

`es2016`–JavaScript 规范的第七版包括了数组的`includes`方法和取幂操作符

`es2017`–JavaScript 规范的第八版。它增加了检查对象和异步/等待的特性

`es2018`–JavaScript 规范的第 9 版。它将 spread 和 rests 操作或对象添加到字符串处理和异步操作中。

`esNext` —目标特性将包含在下一版本的 JavaScript 规范中。

从 ES6 开始，JavaScript 版本由年份表示。

ES6 与 ES2015 相同。然后 JavaScript 开始每年递增地发布新特性。

所以版本改成了年份。

保存文件更改后，将编译并运行代码。

生成的代码在`dist`文件夹中。

![](img/6daf74e1badeae677cce80a0db529927.png)

Photo by [Christine Siracusa](https://unsplash.com/@christine_siracusa?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 为编译设置库文件

要启用目标版本中没有的特性，我们可以更改目标。

我们可以用以下选项设置`lib`编译器选项。

`es5`、`es2015`、`es2016`、`es2017`、`es2018`改变对应 JavaScript 版本的类型定义文件。

`esnext`让我们将提议的附加 JavaScript 包含到代码中。

`dom`包含 DOM 库。

`dom.iterable`为 DOM API 的添加提供类型信息，并允许在 HTML 元素上迭代。

`webworker`包含 web worker 特性。

`es2015.core`包含 ES2015 推出的主要功能的类型信息。

`es2015.collection`设置包括`Map`和`Set`构造器的类型信息。

`es2015.generator`和`es2015.iterable`包括发生器和可重复特征的类型信息。

`es2015.promise` —包括承诺的类型信息。

`es2015.reflect` —包括反映特征的类型信息，以访问属性和原型

`es2015.symbol`、`es2015.symbol.wellknown` —包括符号的类型信息

我们在`compilerSections`部分包含了`lib`设置。

例如，我们可以写:

```
{
  "compilerOptions": {
    "target": "es5",
    "outDir": "./dist",
    "rootDir": "./src",
    "noEmitOnError": true,
    "lib": ["es2015", "dom", "es2015.collection"]
   }
}
```

# 结论

为了使用我们想要的 JavaScript 特性，我们必须在 protect 中包含 TypeScript 类型定义。

我们还可以为我们的 TypeScript 项目选择我们想要的 JavaScript 版本。