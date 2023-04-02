# 如何发布 JavaScript 库

> 原文：<https://javascript.plainenglish.io/how-to-publish-javascript-libraries-68b9dd22dbda?source=collection_archive---------2----------------------->

![](img/a6404453a64d51b7af13647489fc229a.png)

Photo by [James Pond](https://unsplash.com/@jamesponddotco?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

你写了一些代码，很有用。当然，您想在不同的项目中使用它，或者您甚至想发布它让更多的人使用它。你可以把源代码上传到某个地方，并写下如何使用它的说明。但是有一个更标准更简单的方法:**将其发布为库**。

当您发布您的库时，您或其他人可以通过键入以下内容轻松获得它:

```
npm install your-super-library
```

并将其用于:

```
import { somethingUseful } from “your-super-library”;
```

这使得高效的代码重用和基于更小的可重用片段的项目创建成为可能。

# **哪里出版**

目前 [npmjs](https://www.npmjs.com/) 是事实上的标准，它允许公共和私有包，每个 OS JavaScript 库都在那里发布。 [Yarn](https://github.com/yarnpkg/yarn/issues/5891) 资源库基本上是 npmjs 的镜像。

有两个主要的客户端访问存储库(也称为包管理器):npm 和 yarn。两者都很棒，都允许开发人员轻松地:

*   从存储库中获取库
*   处理可传递依赖关系
*   随着新版本的发布更新依赖关系
*   发布您自己的库
*   在机器之间复制依赖关系树(例如 dev 和 CI)
*   …

也有一些替代方案来托管您自己的回购，如 [Artifactory](https://jfrog.com/artifactory/) 或 [Nexus](https://www.sonatype.com/product-nexus-repository) 。在本指南中，我假设您将使用 npmjs，但发布到 Artifactory 或 Nexus 应该基本相同。

# **package.json**

package.json 是描述您的包的文件(在本指南中，我将库和包作为同义词使用)。

这是一个示例 package.json

如您所见，它包括一些描述性数据**如名称、版本、描述、作者、许可证、存储库或主页。**

名称和版本是必需的。package.json 中的附加属性是合法的，因此项目通常向 package.json 添加属性，以存储 CI 服务器、测试库等使用的一些配置。

在选择**名称**时，您应该考虑使用[作用域包](https://docs.npmjs.com/misc/scope)，这不仅是因为更容易找到自由名称，还因为它允许您更改特定作用域的存储库或认证数据，这在使用私有包时非常有用。

对于**版本**你必须遵循[永远](https://semver.org/)。Semver 说，当更新你的包依赖关系时，你可以决定改变是补丁还是次要的，但是因为这会对使用你的库的项目(例如包大小)产生很大的影响，所以我建议把这些改变作为次要的来发布。

还有一些数据会影响其他项目如何使用我们的包:

*   **main** —指向我们库的主 js 文件
*   **模块** —指向我们库的主 ES6 模块
*   **jsnext:main** —类似模块，但已过时
*   **类型** —指向 typescript 主 d.ts 文件
*   **依赖关系** —使用该库所需的其他包
*   **副作用**—nodejs 编译器优化 ES6 模块的提示

# **模块**

发布我们的库的主要原因是为了便于重用。包存储库使下载库变得容易，但我们也希望代码在项目中易于使用。

这就是**模块**概念发挥作用的地方。一个模块可以被看作是一个乐高积木，它的底部有一些“管子”,顶部有一些“螺柱”,这个简单的契约可以很容易地将一些模块连接在一起。模块内部可以是任何东西，模块的内部不会影响其他模块的内部。

这个使所有模块都可以连接到其他模块的契约被称为**模块定义**。目前我们有很多模块定义，所以我们需要理解它们以便为我们的库选择一个(或多个)模块定义。

为了理解不同的模块定义，我推荐你阅读我以前的文章 [JavaScript 模块定义](https://medium.com/javascript-in-plain-english/javascript-module-definitions-a5a467a7f202)

## **我应该选择哪个模块定义**

库的最佳模块定义取决于库的使用方式:

*   如果你的库只能在节点中使用(因为它使用文件系统),你应该使用 CJS
*   如果您的库将用于新的 web 应用程序，您应该使用 ESM
*   如果你想让你的库在遗留的网络应用中使用，你可能需要 AMD 或者 IIFE
*   如果您正在为一个特定的框架创建一个组件，并且它推荐一个特定的模块定义，那么就去使用它

但是，如果我的库可以在浏览器和节点上使用，并且我想向所有人开放，我该怎么办呢？

如果你还记得，我们在 package.json 中有属性 *main* 和 *module* ，它们指向库的主 js 文件和使用 es 模块的库的主 js。

这允许我们用 ESM 和 bundle/trans file 对我们的库进行两次编码，并生成例如 *dist/index.js* 和 *dist/index.es.js* 文件。然后我们可以将*主*和*模块*指向这 2 个文件。

当一个项目依赖于我们的库时，它的捆绑者将选择最适合它需要的文件。这样，新的应用程序将选择 ESM 文件。

但是旧的应用程序会怎么样呢？如果我们选择 **UMD** 作为主文件，nodejs 应用程序、使用 requireJS 的应用程序甚至不使用捆绑器或加载器的 web 应用程序都可以使用它。

**所以 main=UMD 和 module=ES6 几乎支持我们库的每一个用户。**

今天很少有人在没有捆绑器的情况下建立网站，所以如果你对支持传统网站不感兴趣，你可能更喜欢 main=CJS 和 module=ES6。

# **要不要捆绑？**

捆绑意味着将所有文件连接成一个 js 文件。这是一种对 web 应用程序非常有用的技术。但是我们的图书馆可以捆绑或不捆绑出版。

如果您的库是从一个节点应用程序中使用的，那么它是否是捆绑的就无关紧要了。使用捆绑器的 web 应用程序也会发生同样的情况。除非您的库使用别名(或其他方法)来缓解导入中的“相对路径地狱”。在这种情况下，您不能依赖使用您的库的应用程序来定位文件，最好进行捆绑。

对于不使用 bundler 的 web 应用程序，通常最好进行捆绑，因为浏览器很难通过 HTTP 找到导入的文件。即使这样做了，下载>解析>查找导入>下载>解析>查找导入……的周期也比一次下载整个库要慢。

所以是的，**通常捆绑你的库更安全**。

# 我们应该在捆绑包中包含依赖关系吗？

我们的捆绑包不得包含依赖代码。我们已经在 package.json 中列出了依赖项，因此使用我们库的项目将会下载依赖项本身。

一些开发者(包括我)在 bundler 配置文件中导入 package.json，将 package.json 依赖项作为外部使用。这样，我们可以确保捆绑包中不包含任何依赖项。

对于不使用加载器或捆绑器的遗留 web 应用程序，包含一个包含依赖项的捆绑包可能是有用的，但是您永远不应该将它用作主文件。

# 我们应该转换到旧的 EcmaScript 吗？

当然，当我们生成 UMD、AMD 或 IIFE bundles 时，我们必须这样做，因为我们的库可能会像一样在浏览器*中使用。*

但是我们也需要将 ESM 包移植到 ES5。这很有趣，因为 ESM 是在 ES6 中定义的。这是必要的，因为现代的捆绑器理解 ES 模块，但默认情况下不转换依赖关系。这会产生应用程序包，其中包含一些浏览器无法运行的代码。

# **poly fill 会怎样？**

如果我们将代码移植到旧版本，浏览器也可能需要一些聚合填充来运行我们的库。

但是提供聚合填充的责任总是由应用程序承担，而不是由它的依赖项承担，因为只有应用程序的创建者知道目标浏览器，并且这是防止几个依赖项的唯一方法，以提供几个等价的聚合填充。

因此，**我们的库必须在运行时检查所需特性的存在，如果不存在，它将抛出一个错误来帮助应用程序开发人员添加 polyfill。**

此外，在库文档中提及所需的 polyfills 也是很好的。

# **打字稿会怎么样？**

作为库开发人员，我们希望其他开发人员能够轻松使用我们的库，因此我们将提供 typescript 定义文件(. d.ts)。这些文件解释了我们的模块期望和导出的类型。所以用 typescript(现在相当流行)编写的应用程序可以使用我们的库，而不需要自己编写. d.ts 文件。

如果我们用 typescript 编写代码，那么我们就不需要自己编写. d.ts，因为 typescript 会自动生成它。Typescript 还帮助我们在模块内部正确处理类型，因此它将提高我们的代码质量。

在模块化架构中，一些部分可能由不同的团队使用不同的技术创建，因此这些部分按照预期工作并尽可能少地出现错误非常重要。

正如我们可以配置本库的 **main** 文件一样，在 package.json 中，我们也可以告知我们的包的 main .d.ts 位于何处。为此，我们将使用属性**类型。**

**注意** *:* 如果我们的库依赖于其他包，我们需要导入额外的类型来使用它们，这些**类型依赖就不是 dev-dependencies** 。因为使用我们库的应用程序将需要过渡性地使用它们。

# **package-lock.json，yarn.lock 还是 npm-shrinkwrap.json？**

锁文件让我们复制完全相同的依赖关系树实例，包括直接依赖关系和传递依赖关系。这对于在不同开发人员的机器、CI 中拥有相同的环境，甚至是回到过去生成构建的开发环境，都是非常有用的。

现在我们正在构建一个用于其他应用程序的库，我们有一个不同的需求:

> 如果我们的库有一个依赖项，并且使用我们的库的应用程序有相同的依赖项，我们希望应用程序包只包含一次依赖项。

这就是为什么我们在 package.json 中用通配符(^，~，*，>，…)定义依赖项，如果有一个版本的依赖项与我们的库和应用程序都兼容，那么它应该只包含一次。

当构建依赖于它的应用程序时，我们库中的 **package.lock** 和 **yarn.lock** 都将被忽略。但是 **npm-shrinkwrap.json** 是可传递的，所以应用程序将被迫使用相同版本的公共依赖项，它们可能会被添加两次。

**对于库，建议不要添加 npm-shrinkwrap.json** 。

对于服务器端应用程序或 CLI 应用程序，添加它是可以的，但可能 **package.lock** 也是可以的。

# **将要发布的文件**

默认情况下，除了名为. npmignore 的文件中列出的文件之外，项目文件夹中的所有文件都将被发布。gitignore 事实上如果没有。不要理会。将使用 gitignore 文件。

在 package.json 中，我们还可以配置[文件属性](https://docs.npmjs.com/files/package.json#files)。它是一个文件模式数组，描述当您的库发布时要上传到存储库的条目。如果您的 package.json 有一个 **files** 属性，那么只有匹配的文件才会包含在包中。

无论您的配置如何，还有一些文件总是包含在内:package.json、README、CHANGELOG、LICENSE 和 main 属性中的文件。

您可以通过运行以下命令来检查哪些文件将包含在您的包中

```
npm pack
```

# 出版过程

我们终于准备好出版我们的图书馆了。要创建我们的库，我们应该完成:

*   配置软件版本控制(git init)
*   创建我们的 package.json 并配置所有需要的属性(npm init)
*   编写代码和测试
*   配置捆绑器并生成捆绑包
*   创建一个自述文件，至少解释这个库的用途以及如何使用它

## **检查库是否工作**

由于我们正在生成一个库，所以我们希望将其作为实际应用程序的依赖项进行测试。

我建议您在您的库中创建一个名为 **example** 的目录，其中包含一个使用该库的演示应用程序。

示例文件夹中的以下 package.json 将允许您依赖您的库包，然后像使用外部依赖项一样使用它，而无需发布它:

```
{ “name”: “@jacarma/my-awesome-library-example”, “version”: “0.0.0”, “license”: “MIT”, “private”: true, “dependencies”: { “@jacarma/my-awesome-library”: “file:..” }, “scripts”: { “start”: “dev-server” }}
```

或者，你可以 **npm 在你的库文件夹中链接**，然后在另一个文件夹的目录中， **npm 链接你的库名**。这将在 node_modules 中创建一个链接，让您可以将它用作依赖项。

## **预发行支票**

在发布之前，我们希望确保我们会做得正确，以下是一些对我来说有意义的检查:

*   我们是在主服务器上还是在发布分支上
*   我们的工作目录与回购中的代码完全匹配
*   我们的依赖关系已更新
*   捆绑内容可以。依赖项不包含在包中，生成的包使用预期的模块定义…
*   我们不包括私人或不需要的文件( **npm 包**)

## **发布**

首先，如果你没有 npm 账户，你可以在[npmjs.com/signup](https://www.npmjs.com/signup)创建一个

*   从您的终端登录 npmjs，它会询问您的用户名和密码

```
npm login
```

*   删除你的库版本并创建一个新的 git 标签

```
npm version
```

*   推送碰撞代码和新标签

```
git push --follow-tags
```

*   将库发布到 npmjs

```
npm publish --access public
```

*   庆祝一下！您的库已经可以使用了，您可以创建一个新的应用程序并添加库作为依赖项来证明这一点

![](img/881345f897add66d00aab81de3545dd1.png)

Photo by [Jason Dent](https://unsplash.com/@jdent?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

## **可选**

*   写发布说明( [Github 发布](https://help.github.com/en/github/administering-a-repository/creating-releases))
*   更新网站( [gh-pages](https://github.com/tschaub/gh-pages)

## 替代:np

有一个工具可以简化发布过程，它叫做 [np](https://github.com/sindresorhus/np) 。

# 作为操作系统库维护者的职责

如果你想把这个库作为开源来发布，你将获得一些责任。

*   回答问题，bug 和 PR
*   保持相关性最新
*   保护您的图书馆安全
*   注意安全警报
*   不维护时发出警告

没有什么是强制性的，但是如果你不履行它们，有人会分叉你的代码，开发者更喜欢维护良好的代码。

# 结论

在发布过程中没有什么困难:npm 版本和 npm 发布应该足够了。

所有的“复杂性”都来自于生成易于在应用程序中使用的包:

*   一些应用程序运行在浏览器上，一些运行在节点上
*   有些应用程序使用捆绑器，有些不使用
*   有些应用程序使用 typescript，有些不使用
*   浏览器支持不同版本的 ES，缓解这种情况的方法(transfile+poly fill)在模块化架构中很复杂或不明确
*   模块定义太多
*   开发人员并不完全理解传递依赖是如何解决的(对等依赖，。锁定文件、通配符)
*   开发人员没有完全区分包管理器、捆绑器、传输器和模块加载器的职责，因为通常它们中的几个都包含在一个工具中
*   …

我希望这篇文章能帮助你弄清楚如何发布 JS 库。请记住，这个生态系统每天都在发展，本文首次发表于 2019 年 12 月，一些提示可能在未来无效，因此请谨慎使用。

我很想知道这篇文章是否对你出版一个库有所帮助，你可以通过 [@jacarma](https://twitter.com/jacarma) 联系我

# 追踪

*   [JavaScript 模块简史](https://medium.com/sungthecoder/javascript-module-module-loader-module-bundler-es6-module-confused-yet-6343510e7bde)
*   [为摇树优化 JavaScript 包](https://madewithlove.be/optimizing-javascript-packages-for-tree-shaking/)
*   [排版出版](https://www.typescriptlang.org/docs/handbook/declaration-files/publishing.html)