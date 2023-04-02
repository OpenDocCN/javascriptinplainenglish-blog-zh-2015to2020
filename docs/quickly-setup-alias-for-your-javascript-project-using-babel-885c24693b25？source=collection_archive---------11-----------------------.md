# 用 Babel 为您的 JavaScript 项目设置别名

> 原文：<https://javascript.plainenglish.io/quickly-setup-alias-for-your-javascript-project-using-babel-885c24693b25?source=collection_archive---------11----------------------->

![](img/2a8fe60479fd7e196e4da7887cd56f8f.png)

Photo by [Emile Perron](https://unsplash.com/@emilep?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

你曾经导入过这样的文件吗？这叫*进口地狱*和**都是废话！**它不仅使代码看起来很难看，而且有时(尤其是对于像我这样的新手)很难找到正确的路径，然后当林挺时，我们以`eslint (import/no-unresolved)`警告结束。

**让我们通过创建 alias 让生活变得简单一点。**

## 先决条件

带有 Webpack 和 Babel 设置的 JavaScript 项目。查看我的帖子[如何用 Webpack，Babel + SCSS 建立一个 JavaScript 项目](/quickly-setup-a-javascript-project-with-webpack-babel-scss-6c5db2bebab7)

# 第一步

*   打开你的终端和`cd`进入你的项目
*   `npm i babel-plugin-module-resolver`或者如果你喜欢`yarn add babel-plugin-module-resolver`

# 第二步

*   转到您的`babelrc`文件。如果你还没有，在你的项目的根目录下创建一个，这样它就和你的`webpack`文件一致了。要创建一个，只需在您的终端中键入以下内容`touch .babelrc`
*   添加以下内容:

```
{
  ...
  "plugins": [
    ["babel-plugin-module-resolver",
      {
        "alias": {
          "@pages": "./src/app/pages",
          "@helpers": "./src/app/helpers",
          "@services": "./src/app/services",
          "@styles": "./src/styles",
        }
      }
    ]
  ]
}
```

你可以称你的别名为“任何你喜欢的东西”,并且可以随意创建。我更喜欢在我的前面加一个`@`的标志。你不需要。

**就这样！**你现在所要做的就是将`import file from "../../../file";`改为你命名的别名，例如，如果`file.js`存储在助手文件夹中，只需这样做:`import file from "@helpers/file";`

感谢阅读！

如果这是有帮助的，如果你给这篇文章一个👏如果你还没有分享，跟随的**将会很棒。**

[也请考虑订阅有我推荐链接的媒体。Medium 是一个很好的学习平台，我用它来了解技术领域的最新动态，并学习其他开发人员的经验。](https://mazaher-muraj.medium.com/membership)

你的订阅将直接支持我和许多其他媒体作家。

[](https://mazaher-muraj.medium.com/membership) [## 通过我的推荐链接加入媒体

### 阅读 Mazaher Muraj(以及媒体上成千上万的其他作家)的每一个故事。您的会员费直接支持…

mazaher-muraj.medium.com](https://mazaher-muraj.medium.com/membership)