# Angular:如何支持 IE11

> 原文：<https://javascript.plainenglish.io/angular-how-to-support-ie11-d3b8c3d3494e?source=collection_archive---------2----------------------->

![](img/d577f8fc3ce7bfff2242ad83bbffa822.png)

在这篇文章中，我将向你展示我用 Angular 支持 Internet Explorer 11 的步骤。前半部分将快速显示您需要采取的步骤，后半部分将对这些步骤进行更详细的分解，以便任何希望了解更多信息的人使用。最后，我将添加一些在实际应用中可能出现的额外技巧。

# 💪让我们完成它

# 🎯步骤 1 —瞄准 ES5

IE11 最多只支持 ES5。因此，我们必须更新我们的`tsconfig.json`。
更新`compilerOptions`中的`target`属性，以匹配以下内容(如果尚未匹配):

```
"compilerOptions": {
    "target": "es5"
}
```

# 🌐步骤 2 —更新`broswerlist`

打开你的`browserlist`文件，并更改行`not IE 9-11`以匹配:

```
not IE 9-10
IE 11
```

# 🔧步骤 3-聚合填充

如果您或您的任何依赖项使用 ES6+的特性，您将需要填充这些特性。CoreJS 包含在 Angular install 中，可用于您需要的大多数聚合填充。

打开您的`polyfills.ts`文件，将以下内容放在顶部的`BROWSER POLYFILLS`下:

如果需要速战速决 ***(不推荐)*** :

```
import 'core-js';
```

否则，请尝试辨别您需要什么聚合填充。我发现这些涵盖了我的用例:

```
*import* 'core-js/es6/symbol';
*import* 'core-js/es6/object';
*import* 'core-js/es6/function';
*import* 'core-js/es6/parse-int';
*import* 'core-js/es6/parse-float';
*import* 'core-js/es6/number';
*import* 'core-js/es6/math';
*import* 'core-js/es6/string';
*import* 'core-js/es6/date';
*import* 'core-js/es6/regexp';
*import* 'core-js/es6/map';
*import* 'core-js/es6/weak-map';
*import* 'core-js/es6/set';
*import* 'core-js/es6/array';
*import* 'core-js/es7/array'; *// for .includes()*
```

我们需要做的下一部分是在`polyfills.ts`顶部附近找到下面的行:

```
/** IE10 and IE11 requires the following for NgClass support on SVG elements */ 
// import 'classlist.js'; // Run `npm install --save classlist.js`.
```

按照指示运行:
`npm install --save classlist.js`

然后取消对导入的注释:

```
/** IE10 and IE11 requires the following for NgClass support on SVG elements */ 
import ' classlist.js ' ; // Run `npm install --save classlist.js`.
```

如果使用角形材料或`@angular/platform-browser/animations`中的`AnimationBuilder`,则找到以下线条:

```
// import 'web-animations-js'; 
// Run `npm install --save web-animations-js`.
```

取消 import 语句的注释并运行`npm install --save web-animations-js`。

您的最终`polyfills.ts`文件应该类似于:

# ✅完成

就是这样！你应该可以走了！🚀🚀

你很可能会遇到更多的问题。其中一些将在本文的后半部分讨论。

# 🤯但是为什么呢？

在我们深入探讨可能出现的其他问题之前，让我们快速浏览一下上面每个步骤的原因。

*   目标 ES5:非常简单，IE11 只支持 ES5 或更低版本。因此，TypeScript 需要将您的代码转换成 ES5 兼容代码。
*   Browserlist:这是一个有趣的问题。我们需要声明我们支持 IE 11，但是如果我们不支持 IE 9 或 ie10，同样重要的是明确声明我们不支持它们，否则 differential loader 会包含很多废话。_(谢谢[@威斯科普兰德 _](https://twitter.com/wescopeland_) 的建议)_
*   poly fills——我们使用的一些库或编写的代码依赖于 IE11 不支持的 ECMAScript 版本的功能，因此我们需要使用变通方法手动为 ES5 提供这一功能。这将允许使用现代功能的代码继续正确工作。*(注意:每个聚合填充都会增加束的大小，因此在选择要导入的聚合填充时要小心)*

# 💡一些附加提示

好了，写这篇文章的动机来自于在我们的绿色应用中支持 IE11 的任务。尤其令人痛苦的是，这是一个事后想法，然后强调了与支持 IE11 的兼容性问题:

## 第三方依赖需要支持 ES5

这一点很快就变得很明显，因为错误很容易被显示到控制台中。但这确实凸显了一个有趣的问题。

现在，如果我们想在我们的应用程序中包含一个新的依赖项或库，我们需要确保它构建并支持 ES5，否则，我们必须跳过它。这可能会限制我们未来的选择，这从来都不是理想的。

## IE11 不支持 CSS 自定义属性

这很快变成了地狱。IE11 不支持 CSS 自定义属性，比如`--primary-color: blue;`，这意味着我们的主题化解决方案可能会失败。

经过大量的调查，我发现*可以被聚合填充，但是，我发现的聚合填充很慢，对束的大小有很大的影响，并且不完全完美。缺少功能，例如在一行中包含多个自定义属性等问题。*

它们也不适用于我们的特定用例以及我们的主题化解决方案，后者依赖于自定义属性的运行时设置。

我的解决方案来自于 [css-vars-ponyfill](https://www.npmjs.com/package/css-vars-ponyfill) ，它允许在运行时设置全局定制属性。可怕的🔥🔥

## 在 IE11 中设置`style`属性

IE11 只允许用它支持的 CSS 属性设置 DOM 元素的`style`属性。
例如，进行如下操作:

```
document.body.style = '--primary-color: blue; font-size: 18px';
```

结果在接下来的 IE11 上，输了`--primary-color: blue`。

```
<body style="font-size: 18px"></body>
```

## flexbox 支持引起的样式问题

IE11 确实支持 flexbox，但它对如何支持非常挑剔。我注意到，如果我想使用`flex: 1;`来允许一个元素填充剩余的空间，在 IE11 上我必须设置完整的 flex 属性:`flex: 1 0 auto;`或类似的东西。

## 在 IE11 中运行 DevTools 与 zone.js 冲突

没错。出于某种原因，当你在 IE11 上运行`ng serve`的同时打开开发工具会导致与`zone.js`的冲突；

要解决这个问题，你需要为 zone 添加一个全局`ZONE FLAG`来执行一些额外的代码。

你在`polyfills.ts`做这个。找到`zone.js`导入并添加如下内容，如下所示:

```
(window as any).__Zone_enable_cross_context_check = true; import 'zone.js/dist/zone'; *// Included with Angular CLI.*
```

# 😭结论

在这一周里，我并不喜欢尝试让它工作。现在我有了它的支持；我觉得很有成就💪。
希望这篇文章能在将来给某人省点痛苦！

希望你从阅读这篇文章中有所收获，也许是你以前不知道的花絮。

如果有任何问题，欢迎在下面提问或在 Twitter 上联系我: [@FerryColum](https://twitter.com/FerryColum) 。

*最初发布于 2020 年 1 月 10 日*[*https://dev . to*](https://dev.to/coly010/angular-how-to-support-ie11-4924)*。*