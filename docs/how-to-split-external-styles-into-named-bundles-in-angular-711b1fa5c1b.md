# 如何在 angular 中将外部样式分割成命名束

> 原文：<https://javascript.plainenglish.io/how-to-split-external-styles-into-named-bundles-in-angular-711b1fa5c1b?source=collection_archive---------7----------------------->

![](img/5d7d82e183eefb0a696804c6244df292.png)

Photographer: [Damir Spanic](https://unsplash.com/@spanic) | Source: [Unsplash](https://unsplash.com/)

有时候，在构建我们的 angular 应用程序时，我们希望将第三方外部样式或遗留样式拆分成一个单独的 CSS 包。这很容易做到。继续阅读以了解更多。

## 包括外部样式

众所周知，我们可以通过将任何外部样式添加到 angular.json 文件中的 styles 数组来将它包含在我们的 angular 应用程序中:

```
"styles": [
  "styles/bootstrap.scss",
  "styles/app.scss",
  "styles/some-other-style.scss"
]
```

这将在 ng-cli 构建完成后生成一个 CSS 文件(ng build):

距离/样式。<some-random-hash>。钢性铸铁</some-random-hash>

在生产构建中，angular 会生成一个随机散列，用于浏览器缓存破坏。如果我们的包中发生了变化，就会生成一个新的散列并添加到文件名中，这样我们就可以使以前缓存的文件无效，并迫使浏览器从我们的服务器中检索新文件。

**快速提示:**如果你想在`ng-serve, as well as ng build,`中提取 CSS，你可以编辑你的`angular.json`文件并将`"extractCss": true`添加到`architect -> build -> options`中。我们以前可以将`--extract-css`标志传递给我们的 ng build 命令，但不幸的是，Angular 6+之后就不支持了

## 使用对象符号

我们还可以使用[这里定义的对象符号](https://angular.io/guide/workspace-config#style-script-config)将配置对象传递给样式数组，以便在我们的 angular 应用程序中包含外部样式:

```
"styles": [
  {
    "input": "styles/bootstrap.scss",
    "bundleName": "bootstrap"
  },
  {
    "input": "styles/app.scss",
    "bundleName": "app"
  },
  {
    "input": "styles/legacy.scss",
    "bundleName": "legacy"
  },
]
```

这将输出 3 个单独的 CSS 文件:

*   `dist/bootstrap.<some-random-hash>.css`
*   `dist/app.<some-random-hash>.css`
*   `dist/legacy.<some-random-hash>.css`

我们学习了如何轻松地将外部包分割成单独的命名包。这对于跟踪包的状态很有用。

## 结论

我们学习了如何轻松地将 angular 中的外部样式拆分成单独的命名包。这对于跟踪包的状态很有用。

[**加入我的邮件列表，获得更多这样的提示和技巧**](https://successful-creator-8115.ck.page/ff2106eb5d)