# Nx、React 和 Tailwind CSS —变得简单

> 原文：<https://javascript.plainenglish.io/nx-react-tailwind-css-made-simple-7ab71d39e678?source=collection_archive---------5----------------------->

![](img/2e24b64f87fd9925a9f08d11ae70d629.png)

[Image from: https://unsplash.com/@sigmund](https://unsplash.com/@sigmund)

**更新(2021 年 2 月 17 日)**:新的、更简单的方法可用——见[https://kuccello . medium . com/NX-react-tail wind-made-simple-New-and-improved-v2-d 79 F8 b 173622](https://kuccello.medium.com/nx-react-tailwind-made-simple-new-and-improved-v2-d79f8b173622)

**原帖**

我写这篇文章是因为我做了一些有用的事情，然后忘记了我做了什么。我试图寻找一个类似的解决方案来帮助我回忆起我忘记我做了什么，并发现那里的信息可能不是最好的方法。本质上，这些只是一些开发人员关于使用 react 应用程序将 tailwind 与我的 Nx 托管工作区集成的方法的说明。

好了，先来点背景知识。我是顺风 CSS 的粉丝。多年与 Ant、材料设计、Bootstrap 等等的斗争，让我对 React 中的“UI 框架”产生了恶趣味。在过去的一年里，我一直在修补 Tailwind，并开始相信它是 web 用户界面的未来。我将不再对这个问题发表进一步的意见，并假设既然你在这里阅读这篇文章，你已经知道我在说什么了。

我也是最近转换到 Nx 的。Nx 诞生于 Angular world，是我一直在寻找的管理大型 React 项目的缺失部分，这些项目涉及更大的后端，并且需要将应用程序的各个部分分成库。再说一遍，既然你听到了这篇文章，我会假设你知道 Nx 是什么，为什么它很好。现在说说好东西…

我用 api 和 react 应用程序创建了一个新的 Nx 工作区。很好，没什么特别的。然后我想加上顺风。有很多方法可以做到这一点。暂时假设在 html 中使用 CDN 链接是不可能的(我的意思是谁真的想冒这个安全问题的风险——当然，除非你正在管理自己的 CDN 设置，但那完全是另一回事)。对我来说，添加 tailwind 意味着控制 css 的生成。

# **我做了什么**

## **安装依赖项**

```
npm i \
  tailwindcss \
  autoprefixer@9.8.6 \
  postcss \
  postcss-cli \
  postcss-nested -D
```

*我使用的是 autoprefixer@9.8.6，因为在撰写本文时最新版本存在已知问题。*

## **编辑您的 react 应用程序 style.css**

*路径:<工作空间目录>/apps/your-app/src/styles . CSS*

```
/* For base styles (check out normalize.css) */
@tailwind base;
/* Simple reusable components provided by tailwind */
@tailwind components;
/* utility classes generated based on our tailwind.config.js */
@tailwind utilities;
```

## **创建顺风配置**

```
cd workspace/apps/your-appnpx tailwind init
```

## **创建 postcss.config.js**

*路径:<工作空间目录>/apps/your-app/postcss . config . js*

```
module.exports = {
  plugins: [
    require('postcss-nested'),
    require("tailwindcss")("./tailwind.config.js"),
    require("autoprefixer")
  ]
}
```

## **编辑 React 应用的 workspace . JSON**

将以下目标添加到 React 应用程序定义中:
(*workspace . JSON>projects>YOUR-PROJECT>architect>build-tailwind-CSS*)

```
…
“build-tailwind-css”: {
  “builder”: “@nrwl/workspace:run-commands”,
  “outputs”: [],
  “options”: {
    “command”: “node ../../node_modules/.bin/postcss src/styles.css -o src/app/tailwind.css”,
    “cwd”: “apps/your-app”
  }
},
…
```

然后你只需要运行:

```
nx run your-app:build-tailwind-css
```

现在，您的 react 应用程序 src 目录中将有一个 tailwind.css 文件。如果您需要重新生成它，只需再次运行上面的命令。

最后一步是再次修改 workspace.json，在构建选项的“styles”数组中添加对新的 tailwind.css 文件的引用。

*workspace.json >项目> YOUR-PROJECT >架构师>构建>选项>样式*

```
...
styles": [
  "apps/your-app/src/styles.css",
  "apps/your-app/src/app/tailwind.css"
],
...
```

这是一种简单、可控的方法，可以将 tailwind 包含在 Nx workspace managed React 项目中。

## **网上找到的其他方法**

[问题:如何在 React App 中集成 Tailwind](https://github.com/nrwl/nx/issues/3480)

[不能在 Nx 下一个项目中使用 Tailwindcss](https://github.com/nrwl/nx/issues/2298)

[sébastien Dubois](https://dsebastien.medium.com/?source=post_page-----bf890ea882e--------------------------------)->[通过 Angular 和 Storybook 为 Nrwl NX 工作区添加顺风支持](https://medium.com/javascript-in-plain-english/adding-tailwind-support-to-a-nrwl-nx-workspace-with-angular-and-storybook-bf890ea882e)