# React 组件构建和发布工作流

> 原文：<https://javascript.plainenglish.io/react-component-building-and-publishing-workflow-c7bb815dd2c?source=collection_archive---------4----------------------->

![](img/ffc12cc26db58d83be30a09832237385.png)

Credit: [Cheesecake Labs](https://s3.amazonaws.com/ckl-website-static/wp-content/uploads/2017/07/Banner_css-1280x680.png)

React 开发人员每天都在使用类似的组件代码。但并不是所有的都**简化了 React 组件构建和发布工作流程**。

# **在我们讨论构建和发布工作流之前，这里是我们需要它的原因。**

**#原因 1:** 我们的大部分 React 代码都是可重用的，并且我们很有可能在另一个项目中需要相同的 React 组件。

**#原因 2:** 为了在我们所有的 React 项目中保持代码的一致性。

**#理由 3:** 为开源做贡献的机会。我们可以发布组件，并让其他人也使用它！

你知道 NPM 的包装是怎么做的。然而，当涉及到发布我们在项目中使用的 React 组件时，我们犹豫了。

# 你犹豫的原因

*   构建一个可在 NPM 上发布的新 React 组件需要付出很多努力。
*   你觉得浪费时间在你的项目中安装和更新已发布的包是不值得的。
*   您希望您的组件位于 components 文件夹中，并且当组件代码被更改时，您希望您的项目“实时重新加载”。

# 当我还是个新手的时候，我曾经发布过这样一个 React 组件…

我曾经创建了一个新的 Git 存储库。

然后设置所有的`babel`和`webpack`的东西。

然后我会编写 React 组件代码。

然后我会手动编译它，并用`npm publish`发布到`npm`。

为了测试 React 组件，我将使用`npm i -S that-react-component`在我的项目目录中手动安装该组件。

没有“实时重新加载”…我必须更新组件库中的包，并更新我的项目中的模块。

## 那个过程有多糟糕。我知道这很糟糕。所以，我停止开源 React 组件。我让它们存在于我的项目目录中。如果我需要一个组件，我会将组件文件复制粘贴到另一个项目中。就这样继续下去。直到我了解到…

`create-react-library`和`npm link`！

我刚刚在 GitHub 搜索框里闲逛，输入一些奇怪的东西，然后我发现了`create-react-library`。

![](img/e33fecc35603929920bf2aac061c87d6.png)

Credit: [SAPYard](https://sapyard.com/wp-content/uploads/2017/10/SAP-Workflow-SAPYard.png)

# 这里有一个更好的方法。

## 第一步

所以，每当你知道你正在为正在进行的项目构建一个组件时，想想…

*   如果您可能需要其他项目的组件。
*   如果这个组件对其他开发者有用。
*   如果您的组件逻辑和用例不是非常特定于项目的。
*   或者出于理智的考虑，您希望您的项目文件不那么杂乱。

**如果您认为这些理由中的任何一个是正确的，那么您应该考虑将组件构建为一个可重用的组件，该组件被打包为一个模块。**

如果是，则转到步骤 2。

## 第二步

通过终端访问存储所有项目的主文件夹，并运行以下命令👇

```
$ npx create-react-library <COMPONENT_NAME> # then answer few basic prompts
$ cd <COMPONENT_NAME> # enter in that directory
```

**瞧！您得到了 React 组件样板代码设置。**

## 第三步

在终端中执行以下操作👇

```
$ npm i # this will install your component modules
$ cd example && npm i # install CRA modules
$ npm start
```

运行上述命令将安装 React 组件和创建 React 应用程序示例所需的模块。

## 第四步

让我们浏览一下项目的文件结构。

```
project
│   README.md
│   package.json
|   ...   
│
└───example
│   │   package.json
│   │   ...
│
└───src
    │   package.json
    │   ...
```

现在，无论何时你在`src/`中对你的库或者对示例应用的`example/src`做出改变，`create-react-app`都会实时重新加载你的本地开发服务器，这样你就可以实时迭代你的组件了！

![](img/b4db418f9392d7e80b48f1be61c90aac.png)

Credit: [CRL Repository](https://github.com/transitive-bullshit/create-react-library)

## **第五步**

那么，如果**您希望组件与您正在进行的项目**一起工作，而不必在`npm`上更新和发布，该怎么办呢？

这里，`npm link`来救援了！

在你的组件根目录下，运行`$ npm link`。这使得您的组件对所有项目都可用！

## **第六步**

现在通过终端，**到达正在进行的项目目录**，运行
`$ npm link <COMPONENT_NAME>`。

就是这样。你连接了两个世界！

**让你的组件目录和正在进行的项目都实时观看和编译代码。**

## **第七步**

是时候在您的项目中使用该组件了！
您可以以正常方式导入和使用该组件。

```
import ThatAwesomeComponent from '<COMPONENT_NAME>'
```

现在，当您在浏览器中更新组件代码和刷新项目时，您会看到事情变得生动起来！

## 走吧。你自己试试！

# 发布 React 组件

既然您想要发布/部署/发布您的 React 组件…

*   确保你有好的文档来支持你的代码。
*   此外，检查您的版本号。如果你觉得还行，还能用，就该发表了！

```
$ npm publish
```

就是这样。您的文件将被建立并公布在 NPM 注册！

如果你想让你的构建不那么臃肿，用下面的 `**.npmignore**` **文件发布组件。**

```
# it is also configured for TypeScript Components
src
example
.vscode
.eslintrc
.prettierrc
package-lock.json
tsconfig.json
tsconfig.test.json
.gitignore
node_modules
.travis.yml
rollup.config.js
```

# 你忘了一件事！

不要忘记`npm unlink`正在进行的项目目录中的组件，这样你就可以使用“真实的”和“发布的”组件。当开发组件时，您可以再次链接它。

要取消链接，从项目目录中执行`$ npm unlink <COMPONENT_NAME>`。

# 链接

[https://github.com/transitive-bullshit/create-react-library](https://github.com/transitive-bullshit/create-react-library)
https://codurance.com/2016/12/21/how-to-use-npm-link

# 结论

如果以正确的方式构建和发布，React 组件会更棒。

## 关于我

**我是 Kumar Abhirup，一名来自印度的 16 岁 JavaScript React 开发人员**，每天都在学习新东西。

[在 Twitter 上与我联系](https://twitter.com/kumar_abhirup)🐦
[我的个人网站和作品集](https://kumar.now.sh) 🖥️

请在下面评论你更好的方法，以及改进这篇文章的建议。:)