# 最简单的 Node.js 设置:第 2 部分

> 原文：<https://javascript.plainenglish.io/a-minimal-node-js-setup-part-2-eslint-prettier-vs-code-7963768dbb30?source=collection_archive---------0----------------------->

## ESLint，更漂亮+ VSCode

![](img/1f0384a88526abd967d10aa3a726edc8.png)

这是我关于使用 Babel 建立一个最小的 Node.js & Express 项目的系列文章的第 2/2 部分。点击这里查看《T2》第一部:

[](https://medium.com/javascript-in-plain-english/a-minimal-node-js-express-babel-setup-part-1-6be7b3f3bb55) [## 一个最小的 Node.js、Express 和 Babel 设置

### 让我们设置一个使用 Babel 的基本 Node.js / Express.js API。巴别塔将“移植”我们的 ES2015+代码和模块…

medium.com](https://medium.com/javascript-in-plain-english/a-minimal-node-js-express-babel-setup-part-1-6be7b3f3bb55) 

在 **Part 1** 中，我在一个 fresh Node / Express 项目中设置了 Babel，谈到了节点版本化，设置脚本使用 Babel 进行开发和生产，考虑 Babel 是否还值得(*tldr；我觉得两个方向都公平*。

在这篇文章中，我们将通过在我们的节点项目中设置 [ESLint](https://eslint.org/) 和[beautiful](https://prettier.io/)来工作，然后具体看看我在 [VS Code](https://code.visualstudio.com/) 中使用的几个设置选项。为什么这个设置在我的“最小设置”系列中？我认为用一些自动样式化和静态检查来启动项目是很关键的，这样可以发现开箱即用的最佳实践/ bug 检测。

我们要解决的一个关键问题是“*我们如何让 ESLint 和 Prettier 更好地合作？”*幸运的是，这是现代节点开发中非常常见的工作流。

整个设置的代码(我的文章第 1 和第 2 部分)可以在这里找到:

[](https://github.com/neightjones/node-babel-template) [## 邻居/节点巴别塔模板

### 该模板使用 babel 创建了一个基本的 Node.js / Express.js API。它还为 eslint 和…设置了很好的默认值

github.com](https://github.com/neightjones/node-babel-template) 

# 背景:ESLint & Prettier

**ESLint**

ESLint 直接从他们的网站静态地分析你的代码以快速发现问题。ESLint 将检查您的代码，看您是否违反了它的任何规则(有许多默认值，您也可以自定义规则集)。例如，[这里有一个规则叫做](https://eslint.org/docs/rules/no-dupe-keys) `[no-dupe-keys](https://eslint.org/docs/rules/no-dupe-keys)`。这一点非常简单——如果你创建了一个类似于`const x = { hello: 'world', hello: 'Nate' }`的对象，ESLint 会认为这是你代码中的一个错误。像这样的规则非常有用，因为它们可以防止你犯错误。其他规则不太关注明显的错误，而更关注最佳实践。

**更漂亮**

正如他们提到的那样，Prettier 是“一个固执己见的代码格式化程序”这意味着他们不想给你太多的选择，而是希望你接受他们帮助你维护的代码风格。不过，我们将讨论如何进行一些基本的样式覆盖。我不介意它的强烈意见——样式在很大程度上被开发人员社区所接受，自动格式化对于开发的一致性和易用性来说是一大胜利。

**那么有什么区别呢？有重叠吗？**

同时使用这两种工具一定有意义吗？绝对的。[漂亮点的说最好，这里。](https://prettier.io/docs/en/comparison.html)本质上，Prettier 负责所有与样式相关的规则(箭头函数中的空格、逗号、分号、括号)，ESLint 负责所有与样式无关的事情，比如未使用的变量、对象文字中的重复键等等。

但是，事实上存在重叠，因为 ESLint 也包含一些与样式相关的规则。因此，我们的目标是设置一些东西，以确保 ESLint 的风格相关规则不会与我们的更漂亮的设置相冲突，因为我们选择使用更漂亮的作为我们的权威风格。

让我们从代码开始…第一步，让我们做一些基本的 ESLint 设置。

# 步骤 1:基本 ESLint 设置

[上面链接的回购](https://github.com/neightjones/node-babel-template)有此设置的所有最终代码。

从以下安装开始:

```
npm install --save-dev eslint eslint-config-airbnb-base eslint-plugin-import
```

这些是干什么用的？

*   `eslint`是主 ESLint 包
*   `eslint-config-airbnb-base` : ESLint 运行您或第三方可以定义的各种规则集，包括选择哪些语法规则是错误的，或者只是警告等。Airbnb 规则集是一个非常受欢迎的选项，可以自动建立一套可靠的规则
*   `eslint-plugin-import` [是一个](https://www.npmjs.com/package/eslint-plugin-import)包，帮助林挺处理 ES 模块风格`import` / `export`语句以及文件路径等。事实上，我们的 Airbnb 包指出我们也需要这个包来使用它的规则

现在，我们创建一个名为`.eslintrc.js`的文件，其中包含我们的 ESLint 配置。首先，它看起来像这样:

*   首先，[我们指定环境](https://eslint.org/docs/user-guide/configuring#specifying-environments)，它定义了一组全局变量。我们用现代的`es2021`
*   其次，我们`extend``airbnb-base`——现在我们使用 Airbnb 林挺规则
*   最后，我们在`parserOptions`中设置`ecmaVersion`和`sourceType`——`ecmaVersion`实际上是由我们的`es2021` `env`值自动设置的(es2021 === es12)，但我们将把它留在这里，我们使用 es 模块，因此使用`module`

让我们检查一下到目前为止它是否工作。将这一行作为新脚本添加到您的`package.json`中:

```
"scripts": {
  // ...other scripts,
  "lint": "eslint \"src/**/*.js\""
}
```

继续从项目的根目录运行`npm run lint`,看看会弹出什么。当本文发布时，我将修复示例回购中的林挺错误，但这里有一些我看到的示例错误(也有警告):

*   错误 import 语句后应有 1 个空行，后面没有另一个 import import/newline-after-import `命令
*   `Expected 1 empty line after import statement not followed by another import` —这是关于导入后空行的样式相关规则
*   `'next' is defined but never used` —这是一个未使用的变量错误，与 Express 端点中的`next`参数有关(在我的设置的第 1 部分中生成)
*   `Unable to resolve path to module '#routes/index'` —这个很有趣……[在第 1 部分](https://medium.com/javascript-in-plain-english/a-minimal-node-js-express-babel-setup-part-1-6be7b3f3bb55)中，我们用别名巴别塔建立了绝对进口 ESLint 应该如何理解这一点？我们将在稍后的帖子中处理它

这是一个好的开始——现在我们知道我们的 linter 可以正常工作，并且是按照 Airbnb 规则正确设置的。我们已经可以看到 ESLint 在抱怨一个样式项目(导入后的空行)，所以接下来我们将设置 prettle 并确保 ESLint *不关心样式规则，因为我们将这一任务专门委托给 prettle。*

# 第二步:添加更漂亮的

接下来，让我们安装更漂亮的:

```
npm install --save-dev prettier 
```

让我们再给我们的`package.json`添加一个脚本:

```
"scripts": {
  // ... other scripts,
  "format": "prettier --write \"src/**/*.js\""
}
```

继续运行`npm run format`，它会告诉 prettle 自动为我们检查并格式化代码(也就是说，如果你的文件还没有达到 prettle 的标准，它实际上会改变你的文件)。

有用！但是有些东西对我来说并不理想…例如，你可以在示例项目中看到，我使用单引号。在运行完`npm run format`之后，除了其他更新之外，Prettier 更新了我所有的代码，使用双引号。我们可以在你的项目的顶部定制一个`.prettierrc`文件，如下所示:

该配置强制使用分号，在所有对象、数组等后面添加逗号。，强制使用单引号，并设置每行最多 80 个字符。再运行一次`npm run format`，你会自动看到这些变化。

很酷…但是 ESLint 和 Pretty 现在完全没有交流。目前，将会有一堆 ESLint 和 Prettier 规则**相冲突的情况，使得不可能同时满足两者。**例如，你可能运行一个更漂亮的格式，它会以一种产生 ESLint /Airbnb 错误的方式改变代码…但是你不能把它改回来，因为更漂亮的会报告错误。

让我们让他们一起工作。添加这两个包:

```
npm install --save-dev eslint-plugin-prettier eslint-config-prettier
```

*   `eslint-plugin-prettier` [是一个 ESLint 插件](https://www.npmjs.com/package/eslint-plugin-prettier)，它在后台运行得更漂亮，并报告结果为 ESLint 错误。我们在`.eslintrc.js`文件中配置这个，因为它是一个 ESLint 插件，结果是 ESLint 将在后台使用 Prettier 作为工具来驾驶这艘船
*   `eslint-config-prettier` [是一个 ESLint 配置](https://www.npmjs.com/package/eslint-config-prettier)，它简单地*关闭*任何与漂亮风格冲突的规则。正如他们提到的，因为他们只是关闭规则，不做任何其他事情，所以你想将它与另一个规则集(即我们的 Airbnb 规则集)结合使用，关闭会冲突的规则

现在，将您的`.eslintrc.js`更新成这样:

以下是更新内容:

*   在`extends`键中，我们保留了`airbnb-base`，但是在之后添加了`prettier`*Airbnb 规则，上面写着“使用 Airbnb 规则，但是除此之外，禁用`eslint-config-prettier`中指定的任何规则”*
*   在`rules`中，我们将通过更漂亮(在`eslint-plugin-prettier`中)显示的问题标记为 ESLint `error` s。
*   在`plugins`中，我们必须指定我们确实在使用`eslint-plugin-prettier`

现在发生了什么？ESLint 现在已经关闭了所有可能与 prettle**冲突的规则**，所以我们知道当我们运行 prettle(`npm run format`正如我们所设置的那样)，*它不会再产生 ESLint 会认为是错误的格式化代码。*

其次，让我们仔细检查一下“将更漂亮的问题报告为 ESLint 错误”的概念在示例 repo 的`app.js`文件中，第一行如下所示:

```
import createError from 'http-errors';
```

将它改为像这样使用双引号，并删除分号:

```
import createError from "http-errors"
```

记住，在我们的`.prettierrc`中，我们指定了单引号并要求分号。我们希望 ESLint 在后台使用更漂亮的，并报告更漂亮的错误(感谢`eslint-plugin-prettier` )…运行`npm run lint`，看看会发生什么:

```
error: Replace `"http-errors"` with `'http-errors';` prettier/prettier
```

酷！正如我们所希望的，我们有一个由更漂亮的支票驱动的 ESLint 错误。

我们的设置在这一点上非常顺利，但还有一个主要的林挺错误…

# 第三步:修复巴别塔绝对进口林挺错误

在本系列的第 1 部分中，我们添加了一个名为`babel-plugin-module-resolver`的 Babel 插件来帮助我们定义别名，从而在代码中实现绝对导入。[这是该设置产生的导入结果](https://github.com/neightjones/node-babel-template/blob/main/src/app.js#L7):

```
import indexRouter from '#routes/index';
```

因为 Babel 在构建过程中会处理这些别名，所以 ESLint 不理解我们在这里所指的就不足为奇了。谢天谢地，我们可以用`[eslint-import-resolver-babel-module](https://www.npmjs.com/package/eslint-import-resolver-babel-module)`。这有助于 ESLint 理解它可以查看 Babel 别名路径来确认导入的文件是否存在。

```
npm install --save-dev eslint-import-resolver-babel-module
```

然后我们将添加一个`settings`键到我们的`.eslintrc.js`文件中，就像插件在它的文档中指定的那样…

```
// ... the rest of your .eslintrc.js
  settings: {
    'import/resolver': {
      'babel-module': {},
    },
  },
```

准备就绪后，您可以再次运行`npm run lint`，您将看到这类错误消失了！

# 步骤 4:VS 代码中的工具(可选)

无论你是否使用 VS 代码，之前的一切都是好的。这一节只是一个如何使这个过程更加顺利的快速纲要。我们不会运行`npm`命令来进行林挺和格式化，而是添加 VS 代码插件来持续监视我们的代码。

您将安装这些 2 VS 代码扩展:

*   埃斯林特公司
*   [更漂亮的](https://marketplace.visualstudio.com/items?itemName=esbenp.prettier-vscode)(由更漂亮的)

现在，你的代码已经在你输入的时候被 ESLint 插件链接了。例如，在任意一行键入以下内容:

```
const x = 'hi';
```

很快，您会看到一条红线，显示`no-unused-vars`的林挺错误，因为这个变量没有做任何事情。现在你不必运行`npm run lint`来查看发生了什么(尽管有时这样做来查看你的项目整体上发生了什么也是不错的)。

有了 pretty，你可以格式化代码块，但是我们将设置它在你每次保存文件时自动运行 pretty*。*

*运行`cmd+shift+p`，搜索`Preferences: Open Settings (JSON)`。您可能会看到一个以`"[javascript]"`为键的块(如果不是，用那个键创建一个空块)。然后将`"editor.formatOnSave": true`添加到`"[javascript]"`部分。因为我有一些默认的东西，我现在有:*

```
*// ... other stuff
    "[javascript]": {
        "editor.defaultFormatter": "esbenp.prettier-vscode",
        "editor.formatOnSave": true
    },*
```

*然后我为`json`格式添加了一个类似的块(你也可以为`typescript`等做其他的)。):*

```
*// ... other stuff
    "[json]": {
        "editor.formatOnSave": true
    }*
```

*现在，当你输入和保存文件的时候，漂亮将会和你一起工作。让我们回到之前在`app.js`中的例子…同样，将第一个导入行改为使用双引号而不是单引号，并删除分号:*

```
*import createError from "http-errors"*
```

*现在保存文件…更漂亮的修复它！随着你的发展，这是如此方便。我经常会写一些我知道可能需要修复的代码(例如，我的代码行太长了)，依靠 pretty 立即为我修复它是很棒的。这为我节省了大量考虑格式化的时间。*

*感谢跟随。一旦你有了我的教程和这篇文章的第 1 部分**中的代码，你就可以开始开发你的应用程序代码了。这两部分的所有代码都在示例 repo 中，您可以放心地将它作为项目的起点。***