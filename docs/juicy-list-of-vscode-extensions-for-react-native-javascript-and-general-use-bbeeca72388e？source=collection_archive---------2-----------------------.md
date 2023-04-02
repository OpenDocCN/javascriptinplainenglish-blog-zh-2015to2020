# React、React Native、JavaScript 和 Productivity 的顶级 VSCode 扩展

> 原文：<https://javascript.plainenglish.io/juicy-list-of-vscode-extensions-for-react-native-javascript-and-general-use-bbeeca72388e?source=collection_archive---------2----------------------->

![](img/2a13a728f15bfcf6fc209139c3a74b18.png)

我从 2016 年开始全职使用 VSCode。在我使用 Visual Studio 之前，我一直在寻找更轻便的编辑器。我想与你分享我目前安装的扩展。我是一名 React 本地开发人员，所以以 JavaScript 为中心的部分将是最大的部分，但不是唯一的部分！不管你使用什么技术，有一堆通用的扩展会增加你编码时间的快乐。

# 必须拥有—我最喜欢的 3 个 VSCode 扩展

## [设置同步](https://marketplace.visualstudio.com/items?itemName=Shan.code-settings-sync)

> *使用 GitHub Gist 在多台机器上同步设置、片段、主题、文件图标、启动、按键、工作区和扩展。*·单于

第一个也是唯一的扩展我安装后改变工作空间(新电脑，新帐户，编辑器重新安装或磁盘格式化)。我在 Github Gist 上安全地存储了我所有的扩展和设置。与队友分享来比较和定制他们的编辑器也是有用的。

## [ESLint](https://marketplace.visualstudio.com/items?itemName=dbaeumer.vscode-eslint)

> *将 ESLint JavaScript 集成到 VS 代码中。德克·鲍默*

在我看来，linter 是编写 JavaScript 代码时最重要的工具。

我的职业生涯始于 C#和其他具有丰富特性的 ide 的强类型语言，对我来说，当某些东西被红色下划线时，它将无法工作，这是很自然的。如果没有配置 linter，你可以自由地编写有语法错误的代码，你会在浏览器中得到通知*或者反应稍早一点*——在我看来已经太晚了。

这个扩展在写作的时候给你很好的反馈。

在我的团队中，我们配置了 git 挂钩，不允许任何人用 linter 错误推送项目，这使得项目更加稳定。这也避免了在拉取请求上浪费时间。没有更多的搜索错别字或错误，很容易消除与林特规则。

## [GitLens — Git 增压](https://marketplace.visualstudio.com/items?itemName=eamodio.gitlens)

> *增强 Visual Studio 代码中内置的 Git 功能——通过 Git 责备注释和代码透镜使代码作者一目了然，无缝导航和探索 Git 存储库，通过强大的比较命令获得有价值的见解，等等*Eric Amodio

我从事生命周期长的项目。因此，我经常在我或我的队友很久以前写的文件中工作，然后编辑多次以满足业务需求。这里是 git 通过文件的整个历史展示其威力的地方。这个扩展为我提供了可视化工具来浏览文件提交的历史。

它还显示每一行的最后一次提交，以及日期、消息和作者。我经常使用它来获取我正在查看的代码的上下文。当你理解为什么有人用这种特殊的方式写那句话时，建立心态会快得多。

# React Native、React 和 JavaScript 的 VSCode 扩展

## [自动关闭标签](https://marketplace.visualstudio.com/items?itemName=formulahendry.auto-close-tag)

> *自动添加 HTML/XML 关闭标记，与 Visual Studio IDE 或 Sublime 文本相同*韩军

当我打开一些新标签时，它会自动为它创建关闭标签。例如，我输入`<Component>`，这个扩展会立即添加`</Component>`。它适用于 JSX TSX 律所。

## [自动重命名标签](https://marketplace.visualstudio.com/items?itemName=formulahendry.auto-rename-tag)

> *自动重命名成对的 HTML/XML 标签*韩军

它类似于前面的扩展。当我编辑一个标签时，配对的标签也会被编辑，所以我不需要重新访问它。它适用于 JSX TSX 律所。

## [自动导入](https://marketplace.visualstudio.com/items?itemName=steoates.autoimport)

> *自动查找、解析并提供所有可用导入的代码动作和代码完成。与打字稿和 TSX 一起工作*

## [颜色高亮](https://marketplace.visualstudio.com/items?itemName=naumovs.color-highlight)

> 在你的编辑器中突出显示网页颜色

每当我打开的文件中有十六进制颜色时，该颜色将被设置为字符串的背景。例如 `{myColour: '#AA0000',}`将用红色背景突出显示`#AA0000`。

## [ES7 React/Redux/graph QL/React-Native 片段](https://marketplace.visualstudio.com/items?itemName=dsznajder.es7-react-js-snippets)

> *JS/TS 中 React、Redux 和 Graphql 的简单扩展，带有 ES7 语法*dsznajder

基本片段集。例如，我写`rafcp`，它用 ES7 模块系统的反应箭头功能组件自动完成我的文件。基本上这里的`$1`是文件名。

```
import React from 'react'
import PropTypes from 'prop-types'const $1 = props => {
  return <div>$0</div>
}$1.propTypes = {}export default $1
```

这是一个很大的集合，并且在不断改进。

## [高亮匹配标记](https://marketplace.visualstudio.com/items?itemName=vincaslt.highlight-matching-tag)

> *突出显示匹配的结束和开始标记*

非常有用的扩展，可以用下划线突出成对的标签。这有助于浏览 JSX 树。

## [命名颜色](https://marketplace.visualstudio.com/items?itemName=guillaumedoutriaux.name-that-color)

> *从十六进制颜色表示中获取友好名称。*纪尧姆·杜特里奥

我不喜欢浪费时间去思考:“我应该只给这种颜色命名为`secondaryColor`还是给它起一个合理的名字，比如`blueSky`？”。这个扩展命名每一种十六进制颜色。例如它可以告诉你:`#B10000 is Bright Red or even #bright-red.`

## [更漂亮—代码格式化程序](https://marketplace.visualstudio.com/items?itemName=esbenp.prettier-vscode)

> *使用更漂亮的代码格式化程序*es Ben Petersen

自动格式化您的 React 本机代码，以避免团队中耗时且不必要的讨论。我们在每次提交时都进行格式化。我也有经常按“格式化文档”快捷键的倾向…

记得用更漂亮的文档设置`.prettierrc`配置文件来定制你的格式。

## [React 属性智能感知](https://marketplace.visualstudio.com/items?itemName=OfHumanBondage.react-proptypes-intellisense)

> *PropTypes intellisense 对人类束缚的组件*做出反应

该扩展查找 React PropTypes 并将它们添加到建议列表中。

## [分类-进口](https://marketplace.visualstudio.com/items?itemName=amatiasq.sort-imports)

> *自动分拣 ES6 进口。*VSC 分类进口

我喜欢把我的库放在文件的顶部，而本地文件用换行符隔开。这个扩展使它成为半自动化的。

## [导出自动完成功能](https://marketplace.visualstudio.com/items?itemName=capaj.vscode-exports-autocomplete)

> *从项目中自动完成 javascript 模块导出*capaj

## [React 原生工具](https://marketplace.visualstudio.com/items?itemName=msjsdiag.vscode-react-native)

> *React Native 的调试和集成命令*微软

React Native 的调试器和多个助手函数。如果你喜欢在编辑器中做任何事情，而不是打开几个终端，这对你有好处。

## [vs code-style-components](https://marketplace.visualstudio.com/items?itemName=jpoissonnier.vscode-styled-components)

> *样式化组件的语法高亮显示*

Styled Components 在 React(和 React Native)世界拥有众多粉丝。但是使用带标签的模板文字作为没有颜色的字符串是……丑陋的？不错吧。着色可以提高你的生产力，让代码更具可读性，我非常喜欢可读性。

# 其他扩展

# 与团队合作

## [吉拉和比特水桶(官方)](https://marketplace.visualstudio.com/items?itemName=atlassian.atlascode)

> *将吉拉和 Bitbucket 的强大功能引入 VS 代码——使用 Atlassian for VS 代码，您可以创建和查看问题、开始处理问题、创建拉请求、进行代码审查、开始构建、获取构建状态等等！*

它让我在不离开代码编辑器的情况下获得了很好的拉请求体验。比在浏览器里做评论好多了。

我不用离开编辑器就可以用吉拉门票制作新的分支。我真的很喜欢这个扩展，我把它推荐给所有的 Bitbucket 用户。如果你和团队一起工作的话，这是非常有用的。

## [GitLab 工作流程](https://marketplace.visualstudio.com/items?itemName=fatihacet.gitlab-workflow)

> *GitLab VSCode 集成*

它提供了创建或分配给您的问题和合并请求的预览。它还扩展了 VSCode 命令面板和状态栏，以提供有关项目的更多信息。

## [VS 代码的 Marp](https://marketplace.visualstudio.com/items?itemName=marp-team.marp-vscode)

> *在 VS 代码上创建用 Marp Markdown 编写的幻灯片。*
> 
> *Marp，Markdown Presentation 生态系统，提供了制作精美幻灯片的绝佳体验。你只需要专注于在 Markdown 文档中写你的故事。*·Marp 团队

我是 Markdown 的超级粉丝。我还需要不时地做一个演示，所以我决定在 markdown 制作我的套牌。额外利润——幻灯片很容易转换成文档，因此我一举两得。

这个扩展在 VSCode 中引入了 Marp 生态系统。例如甲板预览。如果你正在做演示，我鼓励你在他们的网站上多读一些。

## [现场分享](https://marketplace.visualstudio.com/items?itemName=ms-vsliveshare.vsliveshare)

> *使用您喜欢的工具进行实时协作开发。微软*

这对结对编程来说非常好。对于远程工作和在家与团队进行头脑风暴特别有用。

# 改善编码体验

## [全部自动完成](https://marketplace.visualstudio.com/items?itemName=Atishay-Jain.All-Autocomplete)

> *在 VSCode 中从打开的文件创建自动完成项目。*·Atishay Jain

内置的 autocompleter 并不包含所有内容。有时编辑在写作时不可能理解上下文。当这种情况发生时，有一个使用过的单词的列表是很有用的——变量名、函数名等等。

## [IntelliJ IDEA 按键绑定](https://marketplace.visualstudio.com/items?itemName=k--kato.intellij-idea-keybindings)

> *IntelliJ IDEA key bindings 的端口，包括用于 WebStorm、PyCharm、PHP Storm 等。*加藤圭佑

我曾经和 PyCharm 一起工作过一段时间，然后是 IntelliJ，一开始使用相同的快捷方式更容易。不幸的是，它没有完全映射，所以有一堆来自 IntelliJ 和一些来自 Visual Studio 的键绑定。

通常当我需要在 JetBrains 工具中做一些事情时，对我来说更容易，因为我知道许多快捷方式。如果有人不从这些工具中迁移，我不知道我是否会推荐它。我认为最好使用有基本设置的工具，除非你能熟练使用其他工具，否则过渡会耗费太多精力。

## [Lorem ipsum](https://marketplace.visualstudio.com/items?itemName=Tyriar.lorem-ipsum)

> *生成并插入 lorem ipsum 文本*Daniel Imms

原型制作的有用工具。我只是输入一个命令`lorem: Insert a paragraph`或`lorem: Instert a line`，而不是从网上复制粘贴一些虚拟文本。

# 值得检查的主题

## [支架灯 Pro](https://marketplace.visualstudio.com/items?itemName=eryouhao.brackets-light-pro)

> *一个主题基本款的支架更轻更漂亮。*·二又好

## [一个 Monokai 主题](https://marketplace.visualstudio.com/items?itemName=azemoh.one-monokai)

> 莫诺凯和黑暗主题的结合约书亚·阿泽莫

我最喜欢的深色主题。是*那个*:)

# 其他语言支持

## [Excel 查看器](https://marketplace.visualstudio.com/items?itemName=GrapeCity.gc-excelviewer)

> *在 Visual Studio 代码工作区中查看 Excel 电子表格和 CSV 文件。葡萄的品质*

当我处理这些文件时，我用它来获得更好的 CSV 预览。

## [外壳格式](https://marketplace.visualstudio.com/items?itemName=foxundermoon.shell-format)

> *shellscript、Dockerfile、properties、gitignore、dotenv、hosts、JVM options…document format*fox under moon

好像更漂亮，但不是为了`bash`。

## [外壳检查](https://marketplace.visualstudio.com/items?itemName=timonwong.shellcheck)

> *在 vscode 中使用 shellcheck 的扩展*Timon Wong

ShellCheck 是一个开源的静态分析工具，可以自动发现你的 shell 脚本中的 bug。

我在`bash`中编写脚本，对一些项目文件进行转换。`bash`也写词。拥有这种语言的现代编辑经验是很有用的——我不经常写脚本，它的语法和我所知道的任何东西都不一样。它不原谅错误。

## [YAML](https://marketplace.visualstudio.com/items?itemName=redhat.vscode-yaml)

> *Red Hat 支持 YAML 语言，内置 Kubernetes 和 Kedge 语法支持*Red Hat

如果你用 CI 做任何事情，那么你的配置文件通常在 YAML。这个扩展为你提供了强大的语言支持。

## [XML 工具](https://marketplace.visualstudio.com/items?itemName=DotJoshJohnson.xml)

> *用于 Visual Studio 代码的 XML 格式化、XQuery 和 XPath 工具*Josh Johnson

我安装它是为了拥有漂亮的 XML 格式化特性。我喜欢自动格式化:)

**如果你学到了新东西，请:**

**→鼓掌👏按钮** below️这样更多的人可以看到故事
**→** [**在 Twitter 上关注我**(@ koprowski _ it)](https://twitter.com/Koprowski_it)永远不会错过一个新故事

*原载于*[*https://koprowski . it*](https://koprowski.it/2020/vscode-extensions-for-react-native-javascript/)*。*

## 简单英语的 JavaScript

喜欢这篇文章吗？如果是这样，通过 [**订阅解码获得更多类似内容，我们的 YouTube 频道**](https://www.youtube.com/channel/UCtipWUghju290NWcn8jhyAw) **！**