# JavaScript 静态代码分析初学者指南

> 原文：<https://javascript.plainenglish.io/beginners-guide-to-javascript-static-code-analysis-5a219bc46a12?source=collection_archive---------16----------------------->

# JavaScript 中静态代码分析的所有内容、原因、时间和方式的答案

![](img/013b857e80accdc18389c097e358300d.png)

你是否苦于代码写得不好？你的代码库是否充满了不一致？每次你的代码被审查时，你会感到焦虑吗？如果你对这些问题中的任何一个回答“是”，静态代码分析可能会有所帮助。

[静态代码分析](https://deepsource.io/blog/introduction-static-code-analysis/?utm_source=medium&utm_medium=jsinplainenglish&utm_campaign=contentdistribution&utm_term=jsstaticanalysis)是在执行之前分析代码*的过程。它为开发人员提供了许多优势，集成静态代码分析器可以增强开发人员的工作流程。*

让我们深入了解什么是静态代码分析，为什么应该在什么时候开始使用它，以及如何在项目中快速设置它。

# 什么是静态代码分析？

在我们刚刚提出的所有问题中，这可能是最容易回答的。顾名思义，静态代码分析就是对处于静态或非执行状态的代码进行分析。这相当于另一个开发人员自动阅读和审查您的代码，除了计算机提供的额外的效率、速度和一致性，这是任何人都无法比拟的。

# 和测试有什么不同？

您可能会想，“如果我在系统级别编写了所有单元的详细测试和功能测试，并且它们都通过了，那么我的代码就没有错误，对吗？”是的，它是。恭喜你。但是无 bug 代码并不等同于好代码；这里面还有很多东西。这是静态分析大放异彩的领域。

所有类型的测试，无论是单元测试、功能测试、集成测试、可视化测试还是回归测试，都要运行代码，然后将结果与已知的预期状态输出进行比较，看是否一切正常。测试确保您的代码按预期运行。它把你的代码当作一个黑盒，给它输入并验证输出。

另一方面，静态代码分析分析其各个方面，如可读性、一致性、错误处理、类型检查以及与最佳实践的一致性。[静态分析](https://deepsource.io/glossary/static-analysis/?utm_source=medium&utm_medium=jsinplainenglish&utm_campaign=contentdistribution&utm_term=jsstaticanalysis)主要关注的不是你的代码是否提供了预期的输出，而是代码本身是如何编写的。这是对源代码质量的分析，而不是对其功能的分析。

总而言之，测试检查你的代码是否有效，而静态分析检查它是否写得好。测试和静态分析是相互补充的，理想情况下，您应该在您的项目中采用两者的健康组合。

# 为什么要使用静态代码分析？

任何读取源代码、解析源代码并提出改进建议的工具都是静态代码分析器。有许多工具属于静态代码分析器的总称，从 linters 和 formatters 到漏洞扫描器和 PR 检查器。让我们回顾一下在工作流程中使用这些工具的主要原因。

# 深度代码扫描

问任何一个开发人员，他们都会证实代码审查是必不可少的。第二双眼睛可以发现你代码中的问题，而你可能永远也发现不了。他们也很可能会提出更好的方法来完成任务。有时阅读其他人的代码可以让评审者了解到一些已经嵌入到项目中的晦涩的有用的功能。评审者和被评审者(可能不是一个真实的词，但我还是会用)都在这个过程中学到了一些东西。

但是有什么比一个人审查你的代码更好的呢？每个开源开发者审查一下怎么样！静态分析器由一个庞大的开源规则库提供支持，这意味着对该工具做出贡献的每个人都间接审查了您的代码。这使得很难发现一些人类评审员可能会忽略的错误。

人都会犯错。只有 15%安装了 JSHint(一种流行的 JavaScript 代码审查工具)的代码库顺利通过测试。这也显示了让计算机来审查你的代码是多么的重要。

示例:

考虑这个程序让用户挑选他们最喜欢的水果。不选的话，默认为‘芒果’。

```
**let** fruits **=** ['Apple', 'Banana', 'Cherry', 'Mango']
**function** getFruit(index) {
	index **=** index **||** 3 *// Everybody likes mangoes* 	**return** fruits[index]
}
```

这个代码是有效的。对于除`0`之外的所有输入，即。如果你不是很彻底，你的测试也会顺利通过。

```
getFruit()  *// Mango* getFruit(2) *// Cherry* getFruit(0) *// Mango (expected Apple!)*
```

结果是，你不能在这个程序中选择一个苹果，因为`0`，像`null`和`undefined`是一个虚假值。你应该使用零合并操作符(`??`)来代替，一个 linter 会告诉你。

```
**let** fruits **=** ['Apple', 'Banana', 'Cherry', 'Mango']
**function** getFruit(index) {
	index **=** index **??** 3 *// Everybody likes mangoes* 	**return** fruits[index]
}
```

# 护栏和辅助轮

每个开发人员都以自己的风格编写不同的代码。但是当许多开发人员一起工作时，他们以一致的方式编写代码是很重要的。这就是风格指南的用武之地。设置一个是编写一致代码的第一步，在与其他开发人员合作时，它的实施极其重要。

实施一个风格指南不是一个手工的壮举。不能指望开发人员记住数百条规则，并对照每条规则检查每一行。为什么不让计算机来做呢？

我用过的每一种语言都有一个短评。 [JavaScript 有 ESLint](https://eslint.org/)； [Python 有黑](https://black.readthedocs.io/en/stable/)， [Ruby 有 RuboCop](https://github.com/rubocop-hq/rubocop) 。这些 linters 做的简单工作是确保您的代码遵循规定的样式规则。像 RuboCop 这样的一些 linters 也实施了一些好的实践，比如原子函数和更好的变量名。这种提示通常有助于在 bug 导致生产问题之前检测并修复它们。

示例:

考虑下面的 JavaScript 代码片段，其中打印了列表中的水果名称。该列表在整个程序中保持不变。

```
**var** fruits **=** ['Apple', 'Banana', 'Cherry', 'Mango']
console.log(fruits[0])
```

如果这样配置，ESLint 可以确保尽可能使用常量，以避免代码中的副作用。这是一个很好的做法，但如果你没有棉绒，很容易错过。

```
**const** fruits **=** ['Apple', 'Banana', 'Cherry', 'Mango']
console.log(fruits[0])
```

在`var`之上强制使用`const`和`let`，这是块范围的，导致程序更容易调试，并且通常被认为是一个好的实践。

# 立即发现问题…

开发人员喜欢的另一件事是测试他们的代码，确保它支持各种输入。像测试驱动开发这样的实践强调测试您编写的代码的重要性。但是编写测试需要时间和精力。很难衡量每一个可能的输入，并确保您的代码符合这一点。最终，测试变得太多，并且在较大的代码库上需要几个小时才能完成。

静态代码分析器不会遇到这个问题。您不需要编写测试；您可以导入整个预置库。此外，静态分析器运行速度非常快，因为不涉及代码执行！事实上，许多 linters 与编辑器集成在一起，并在您键入时实时突出代码的问题。

示例:

![](img/cb087c31d5b5ce6e5e02a674d0ee70bc.png)

Source — [https://i.redd.it/uwc7cddddcp51.png](https://i.redd.it/uwc7cddddcp51.png)

有时候，实时太快了。

# …同样快速地修复它们

大多数静态分析器，尤其是 linters 和 formatters，不仅会指出问题，还会为您修复大部分问题。像 Python 的[Black](https://black.readthedocs.io/en/stable/)和 JavaScript 的[ESLint](https://eslint.org/)这样的 Linters 与 ide 集成在一起，一旦你保存了编辑过的文件，就可以自动修复它们。

这非常方便，因为现在，您的代码质量提高了，而您甚至不必有意识地考虑它。作为开发人员，我们被便利惯坏了，不是吗？

示例:

ESLint 有`--fix`标志，可以修复不必要的分号、尾随空格和悬空逗号等常见问题。

考虑过去几个例子中的相同代码片段。(这里的代表一个空格。)

```
**var** fruits **=** [
	'Apple',
  'Banana',
  'Cherry',··
	'Mango'
];
```

运行带有`--fix`标志的 ESLint，片刻之后您就有了这个。

```
**const** fruits **=** [
	'Apple',
	'Banana',
	'Cherry',
	'Mango',
]
```

好多了！

# 材料清单

物料清单通常用在供应链管理中，作为任何产品的原材料成本。软件也需要类似的材料清单。

当你开发一个应用时，你不可避免地会使用其他开发者开发的框架和工具。反过来，这些框架使用其他开发人员构建的框架。而且不知不觉中，设置一个简单的 Vue.js app 就能把成千上万的包放到你的`node_modules/`目录里。

这就是我们生活的可怕现实。构建在包之上的包。每个巨人都站在另一个巨人的肩膀上。你的应用程序的强度取决于它最弱的依赖关系。漏洞扫描器是另一组静态分析器，它根据漏洞和利用的广泛数据库检查依赖关系树中的每个依赖关系。所有具有已知漏洞的软件包都会被报告，并且可以通过一个命令进行更新。

示例:

GitHub 用 Dependabot 提供依赖扫描。`npm`还使用`npm audit`命令提供漏洞扫描。Dependabot 和`npm audit`都提供了将易受攻击的包自动更新到补丁版本的能力。

# 自动化枯燥的东西

手动代码审查浪费了大量时间。进行评审的人必须从他们自己的工作中抽出时间来进行评审，仔细检查代码，指出所有可以改进的不同地方，既有逻辑上的，也有微小的细节上的，例如不正确的格式或与惯例和风格指南的偏差。然后，审核者必须做出所有建议的更改，并重复该过程。

添加一些 linters、formatters 和拼写检查器使整个过程更加简化。你会问，怎么会这样？首先，预提交挂钩将确保代码在被签入 VCS 之前得到正确的链接和格式化。其次，构建管道或 GitHub 工作流形式的项目级自动化将在每次提交时测试代码质量，并突出 PR 本身的问题。第三，审查者将被解放出来关注大局，因为在 PR 进行人工审查之前，所有较小的事情都已经被处理了。

由软件进行的任何数量的代码审查都不能完全取代人工审查。但是在手动评审之前的静态扫描可以很容易地增加评审者的经验，减少他们的工作量，并且通过迭代更小的问题更快更彻底地评审开发人员的代码。

# 什么时候

现在。是的，没错。我说的是现在。现在再晚就太晚了。如果我没有说服你，你可能已经进行到“如何做”的第二步了。

# 怎么做

设置很容易。因为我们已经在这里反复讨论了 ESLint，所以让我们在一个示例项目中设置它。

# 创建新项目

为您的项目创建一个新目录。进入目录并初始化目录中的 Node.js 包。向导会问你一系列问题。完成后，您就有了一个新的 Node.js 包。

```
$ mkdir wuphf.com
$ cd wuphf.com
$ npm init
```

# 安装 ESLint

安装 ESLint。太简单了。

```
$ npm install eslint
```

# 配置 ESLint

运行以下命令，打开 ESLint 向导。

```
$ ./node_modules/.bin/eslint --init
```

这个向导问了很多关于你将如何在项目中使用 ESLint 的问题。确保选择 Airbnb 规则集。当设置完成时，目录中会有一个文件`.eslintrc.js`。

该文件定义了该项目将在 Node.js 上运行，并且它将建立在 Airbnb 样式指南中定义的规则之上。由于我们正在编写一个控制台应用程序，我可以自定义规则并关闭警告它们的规则。

```
module.exports **=** {
  env**:** {
    es2021**:** **true**,
    node**:** **true**,
  },
  **extends:** [
    'airbnb-base',
  ],
  parserOptions**:** {
    ecmaVersion**:** 12,
  },
  overrides**:** [
		{
      files**:** ['*.js'],
      rules**:** {
        'no-console'**:** 'off',
      },
    },
	],
};
```

将此文件提交到版本控制中。

你有它！ESLint 现在将持续扫描项目中的所有 JavaScript 文件。我还建议安装 [Husky](https://typicode.github.io/husky/#/) 在每次提交前运行 lint 作业，这样你就永远不会将糟糕的代码签入到你的 [VCS](https://deepsource.io/glossary/version-control-system/?utm_source=medium&utm_medium=jsinplainenglish&utm_campaign=contentdistribution&utm_term=jsstaticanalysis) 中。

# 结束了

仅此而已。静态分析您的代码真的很简单，好处是如此之多，没有理由不这样做。

享受编写更干净、更安全、更可读、更易维护(简单地说，更好)的代码的乐趣，我们将在下一期再见。

*原载于* [*DeepSource 博客*](https://deepsource.io/blog/static-analysis-javascript/?utm_source=medium&utm_medium=jsinplainenglish&utm_campaign=contentdistribution&utm_term=jsstaticanalysis) *。*

*更多内容请看*[*plain English . io*](http://plainenglish.io/)