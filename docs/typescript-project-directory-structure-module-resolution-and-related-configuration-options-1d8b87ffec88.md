# 打字稿项目结构指南

> 原文：<https://javascript.plainenglish.io/typescript-project-directory-structure-module-resolution-and-related-configuration-options-1d8b87ffec88?source=collection_archive---------0----------------------->

## 目录结构、模块解析和相关配置选项

![](img/cf4f38107d885b2053d54873646c2e5a.png)

TypeScript 项目的目录结构和模块解析并不复杂，直到您开始将事物组织成单独的单元，将它们放入不同的目录，并尝试这里讨论的一些配置选项。

本文提供了对影响 TypeScript 项目目录结构和模块解析的机制和配置选项的总体理解，以及一些注意事项，希望能避免读者陷入某些陷阱。但是，它不是教程，没有给出详细的例子。要了解更多详细信息，您可以找到配置选项首次出现时 TypeScript 手册相关部分的链接，以及本文末尾演示这里讨论的特性和配置选项的测试项目。

本文假设读者了解 TypeScript 和 Node.js 包以及模块解析的基础知识。除非另有说明，所有讨论的配置选项都是 TypeScript 选项，它们作为命令行选项传递给编译器，或者在“tsconfig.json”中设置。

本文只讨论常见的用例。这里讨论的 TypeScript 项目输出。js 文件放入一个目录(“outDir”)，而不是一个输出文件(“outFile”)，编译器使用“节点”模块解析策略。以便讨论不会因为不常见的用例以及仅为向后兼容而保留的选项而分散读者的注意力。

本讨论中的“实现文件”是指 TypeScript 源文件(。ts，。tsx ),它将被编译器编译成。js 文件。“项目根目录”是指“tsconfig.json”所在的目录。

# **通用 tsconfig.json 选项**

**选项** [***延伸***](https://www.typescriptlang.org/tsconfig#extends)

**选项“extends”指定了另一个要继承的配置文件，这可能是您的配置的一个很好的起点。有些社区维护的基本配置针对特定的运行时环境进行了调整，您可以在项目中安装和继承这些配置。见项目[**@ ts config/bases**](https://github.com/tsconfig/bases/)。**

****选项**[***rootDir***](https://www.typescriptlang.org/tsconfig#rootDir)**

****“rootDir”指定预期包含所有实现文件的根。“rootDir”应该包含所有实现文件，即将要编译的源文件。****

****默认情况下，“rootDir”被推断为“所有非声明输入文件中最长的公共路径”。这意味着如果你所有的输入源文件都在下面。/src，"，"/src”被推断为“rootDir”。如果你的源文件都在两者之下。”/src "和"。/test "，那么"。/”将是“rootDir”。****

****不要混淆“rootDir”和“rootDir”。他们是不同的。****

# ******输出选项******

****这些选项设置发出的 JavaScript 是什么样子的(以便它们与不同的运行时环境兼容)，生产什么其他产品，以及它们输出到哪里。****

******选项** [**胜过**](https://www.typescriptlang.org/tsconfig#outDir)****

******如果指定，输出`.js`(以及`.d.ts`、`.js.map`等。)文件将被发送到此目录中。在大多数类型脚本项目中，应该设置“升级”。否则，除了源之外，还会发出输出，污染源文件夹。******

******“rootDir”下的目录结构将保留在“outDir”中。这意味着从“rootDir/path a/path b/module EC . ts”编译的 JavaScript 将被发送到“outDir/path a/path b/module EC . js”。******

******总之，规则是:“rootDir”包含所有要编译的实现文件(源文件)，而“outDir”是发出输出的地方。“rootDir”下的相同目录结构保留在“outDir”中。本文中讨论的一些选项允许源文件采用非常灵活的目录结构。然而，在任何情况下，这条规则都不会改变。******

********选项** [**目标**](https://www.typescriptlang.org/tsconfig#target)******

******旨在根据发出的 JS 代码将在其中执行的运行时环境进行设置。此选项确定源代码中的哪些功能(如 arrow function ()=>{})在发出的 JS 中被降级，即在较新版本的 Javascript 语言中实现的哪些功能不应在输出 Javascript 文件中发出，而应使用较旧版本的语法和功能来实现，以便与较旧的运行时兼容。******

******选项“目标”更改选项“lib”、“module”的默认值，并通过选项“module”，选项“moduleResolution”。******

********选项** [**模块**](https://www.typescriptlang.org/tsconfig#module)******

******此选项确定发出的 JavaScript 中的模块导入和导出代码(在运行时使用)，而不是编译时 TypeScript 的模块解析。默认值取决于选项“目标”。******

******由于该选项影响发出的 Javascript，因此该值应该取决于发出的 Javascript 代码将被使用的运行时环境的模块系统。如今，最著名的模块系统是“CommonJS”和“ES 模块”。******

******如果您将发出的代码与 Node 一起使用，那么模块系统取决于您将发出的 Javascript 代码放入的包的类型。包的类型由 package.json 中的“类型”字段决定，如果最近的父 package.json 缺少“类型”字段或包含“类型”:“commonjs”，则使用 CommonJS 模块系统。如果包含“类型”:“模块”，则使用“ES 模块”系统。关于如何设置该选项的更多信息，请点击阅读[。](/how-to-correctly-use-typescript-module-import-syntax-and-settings-in-various-circumstances-e98bfa87f70f)******

********选项**[申报](https://www.typescriptlang.org/tsconfig#declaration)******

******如果此选项设置为“true”，编译器将生成类型声明“. d.ts”文件。如果其他代码要使用该项目，则应打开该选项。******

********选项** [**声明映射**](https://www.typescriptlang.org/tsconfig#declarationMap)******

******如果打开了“声明”，选项“declarationMap”会使编译器发出类型声明(. d.ts)的源映射，将定义映射回原始。ts”源文件。效果是当用户点击一个定义时，编辑器(比如 VS 代码)可以转到原来的。使用“转到定义”等功能时，ts 文件。******

# ********“package . JSON”选项********

******这些选项通过导出类型、函数、值、对象以及导入包时执行的入口点来定义包的公开接口。******

********选项** [**【类型】**](https://www.typescriptlang.org/docs/handbook/declaration-files/publishing.html#including-declarations-in-your-npm-package)******

****指示包的类型—是 CommonJS 包还是 es 模块？缺少“类型”字段或“类型”字段的值为“commonjs”表示是 CommonJS 包。值“模块”表示 es 模块。****

****如果您使用 Node.js 运行代码，则该字段确定 Node.js 为您的代码提供什么模块。对于 CommonJS，“require()”是提供的并且可用，ES 模块“import”“export”语句则不是。对于 ES 模块，情况正好相反。****

******选项** [**【类型】【打字员】**](https://www.typescriptlang.org/docs/handbook/declaration-files/publishing.html#including-declarations-in-your-npm-package)****

****指示包的类型声明文件(. d.ts)。“打字员”与“类型”同义，两者都可以使用。如果“index.d.ts”位于包的根，则可以省略“types ”,尽管仍然建议这样做。****

****如果包装将由外部代码使用，应注明“类型”。****

******选项** [**主**](https://docs.npmjs.com/cli/v6/configuring-npm/package-json#main)****

******这是包的主要 JavaScript 入口点。******

******通常，“types”指向声明文件，“main”指向。从“outDir”中的 TypeScript 入口点发出的 js。******

********选项** [**出口**](https://nodejs.org/api/packages.html#packages_exports)******

******作为“主”选项的替代，该选项支持[子路径导出](https://nodejs.org/api/packages.html#packages_subpath_exports)和[条件导出](https://nodejs.org/api/packages.html#packages_conditional_exports)。到目前为止，在 Node.js v15.0.1 下，这两个特性仍然是实验性的，因此不再进一步讨论。******

# ********初始实施文件********

******实现文件是源文件()。ts。tsx)将由 tsc 编译，并且输出(。js)的实现文件被发射到“outDir”中。默认情况下，编译项目根目录下的所有源文件。******

******文件包含选项(“包含”、“排除”、“文件”)可以明确指定最初*包含在实现文件中的文件。然后，在编译期间，编译器可以通过模块解析(在“import”语句中)递归地包括由初始实现文件引用的进一步的实现文件。这将在讨论 TypeScript 模块解析之后进行讨论。*******

*******选项** [**包括**](https://www.typescriptlang.org/tsconfig#include)*****

*******指定相对于项目根目录的文件名或文件名模式列表。*******

*********选项** [**排除**](https://www.typescriptlang.org/tsconfig#exclude)*******

*******指定在解析“include”时要跳过的文件名或文件名模式的列表。注意:“排除”仅在解析“包含”时起作用。即使某个文件是由“exclude”指定的，如果编译器解析了对它的模块引用，它仍然可以包含在实现文件中。*******

*********选项** [**文件**](https://www.typescriptlang.org/tsconfig#files)*******

*******指定要作为实现文件包含的文件列表。*******

# *********模块参考基础知识*********

*******模块引用是将“导入”语句中的路径解析为表示模块的文件。代表性文件可以是源文件(。ts，。tsx)或类型声明(. d.ts)。*******

*********通用选项*********

*******选项** [**模块解析**](https://www.typescriptlang.org/tsconfig#moduleResolution) **(模块解析策略)*******

*****选项“moduleResolution”设置 TypeScript 编译器在编译时如何解析模块。不要与选项“模块”混淆，该选项设置输出的模块系统。js 文件并影响发出的代码的运行时行为。*****

*****选项“moduleResolution”的值可以是“经典”或“节点”。“classic”只是为了向后兼容而保留的，不在本文中讨论。值“节点”是现代代码中使用的节点。这里所有的讨论都是基于“节点”的。*****

*****由于选项“moduleResolution”的默认值会受到选项“target”或“module”的值的影响，因此最好将选项“moduleResolution”显式设置为“node”。*****

*******选项“trace resolution”*******

*****此选项使编译器能够输出模块解析的每一步以供诊断。*****

# *******相对和非相对模块分辨率*******

*****根据 TypeScript 源文件中“import”语句中的路径，模块引用是相对模块引用(以/、)开始。/或者../)和非相对(绝对)模块引用。*****

# *******相对模块分辨率和相关选项*******

*****相对模块解析解析相对于导入文件的模块。它用于解析包中的内部模块。*****

*****以下是影响 TypeScript 相对模块分辨率的选项:*****

*******选项**[**rootDirs**](https://www.typescriptlang.org/tsconfig#rootDirs)*****

*******配置了相对于项目根目录(tsconfig.json 所在位置)的目录列表，当编译器解析相对模块引用时，编译器会将这些目录视为合并到一个根目录中。*******

*********重要提示:**阅读下面关于路径和目录结构不一致的“警告”。*******

# *******非相关模块分辨率及相关选项*******

*****非相关模块被解析(1)相对于选项 **baseUrl** 并通过**路径映射**，如果配置的话，(2)通过模仿 Node.js 模块解析机制，或(3)到[环境模块](https://www.typescriptlang.org/docs/handbook/modules.html#ambient-modules)声明。它通常用于导入外部依赖项。但是“baseUrl”也允许它用于导入内部模块。*****

*******步骤 1:尝试解析相对于“**[**baseUrl**](https://www.typescriptlang.org/tsconfig#baseUrl)**”和“** [**路径**](https://www.typescriptlang.org/tsconfig#paths)**”*******

*****此步骤仅在设置了选项“baseUrl”时有效。“baseUrl”(相对于项目根目录)指向一个基目录，编译器基于该目录尝试解析非相对模块引用。*****

*****使用“baseUrl ”,同一个包中的两个内部模块可以使用非相对模块引用来相互引用，而不是必须使用相对引用，相对引用有时非常麻烦，如“../../../../pathA/pathB/pathC "。*****

*****选项“paths”在非相对模块解析中启用“baseUrl”上的路径映射。它将路径模式映射到相应的目录位置数组(一个模式映射到一个位置数组)。目录位置是相对于“baseUrl”指定的。*****

*****为一个模式在数组中提供多个目录位置允许“后退”映射，这意味着尝试依次在多个位置解析一个模块。*****

*******重要提示:**阅读下面关于路径和目录结构不一致的“警告”。*****

*******第二步:模仿 Node.js 模块解析机制*******

*****假设“/root/pathA/pathB/pathC/modulea . ts”包含语句“import {whatever} from 'moduleB '”，TypeScript 编译器在模拟 Node.js 模块解析时，(1)在“/root/pathA/pathB/pathC”、“/root/pathA/pathB”、“/root”、“/root”和“/”下依次查找文件夹“node_modules”，一次向上前进一个目录，并且(2)如果找到文件夹“node_modules”，则依次查找文件如果步骤(2)在“node_modules”文件夹中失败，编译器返回到步骤(1)继续处理目录，直到“/”。*****

*******步骤 3:解析为环境模块声明*******

*****环境模块是类型声明(. d.ts)中模块的类型声明。详见打字手册中的[环境模块](https://www.typescriptlang.org/docs/handbook/modules.html#ambient-modules)。*****

# *******使用“baseUrl”、“paths”和“rootDirs”的注意事项*******

*****当“baseUrl”、“paths”和“rootDirs”都没有使用时，源文件的目录结构以及发出的 JavaScript 的目录结构(回想一下,“rootDir”中的目录结构保留在“outdir”中)与 Node.js 模块解析机制一致。在相对模块引用的情况下，可以按照导入语句中的路径在目录结构中解析模块。在非相对模块引用的情况下，该模块应该是外部模块，由诸如 npm 之类的包管理器安装在目录“node_modules”下。最后，编译后的输出可以直接由 Node.js 执行，或者由下游的构建工具使用。*****

*****选项“baseUrl”、“paths”和“rootDirs”可以配置 TypeScript 编译器在模块解析期间将“import”语句中的模块路径映射到文件系统中的非常规目录。“baseUrl”和“paths”允许基于不在“node_modules”下的路径解析非相关模块，而“outDirs”允许解析虚拟合并成一个的多个根中的相关模块。现在，“rootDir”下的目录结构仍然保留在“outDir”中，不管映射如何，这意味着输出目录结构与发出的。因此，不能通过 Node.js 或类似工具直接解析 js 文件。*****

*****这不是打字稿的问题。假设在与运行时可执行文件的最终布局不匹配的源目录布局中，开发人员有责任通过包括构建步骤(例如将依赖项从不同位置复制到最终位置)来解决这个问题。需要注意的是，编译器不会执行这些步骤。*****

# *******通过模块解析包含引用的实现文件*******

*****回想一下，模块解析将模块引用解析为表示该模块的文件。该文件可以是源文件(例如 ts，。tsx)或类型声明(. d.ts)。模块引用可以是相对模块引用，也可以是非相对模块引用。相对模块引用是相对于导入模块的源文件来解析的。通过以下方式解析非相对模块引用:(1)相对于 baseUrl 和路径映射，(2)模仿 Node.js 策略在“node_modules”文件夹中搜索，以及(3)解析到环境模块。*****

*****下面是一些规则，关于什么时候一个代表被引用模块的文件被包含在实现文件中，意味着一个输出。js 将被编译并发送到 outDir:*****

*****(1)在相对和非相对模块解析中，如果模块解析为类型声明文件(. d.ts)或环境模块，则不包括在内。编译器很乐意使用该文件中包含的类型信息，而不需要进一步查看。这包括模块解析为包含“类型”属性的 package.json 的情况。*****

*****(2)在非相对模块解析中，如果编译器模仿 Node.js 模块解析，解析为“node_modules”文件夹中的文件，则该文件不包括在实现文件中。该模块被假定在一个外部包中，单独构建，并在运行时通过一个包管理器(如 npm)集成。*****

*****(3)否则该文件(源文件)包含在实现文件中，编译器将编译该文件并发出一个. js。*****

# *******增量编译和项目引用*******

*****增量编译通过跳过那些已经编译过并且没有改变的源文件来节省时间。项目引用允许一起构建两个或更多相关的项目，而被引用的项目是增量编译的。有了这两个特性，一个完整的项目可以分解成更小的项目，以便更好地组织和更快地构建。*****

*****假设项目 a 和项目 b 都在开发中。ProjectA 在 ProjectB 中导入并使用代码。当 ProjectA 被编译时，开发人员也希望编译 ProjectB，但只是在需要的时候。*****

*****为了实现这一点，两个项目都需要在它们的项目根中包含 tsconfig.json。然后，在他们的配置中，ProjectA 需要使用选项“引用”来引用 ProjectB，ProjectB 必须启用选项“复合”。*****

*******选项** [**参考**](https://www.typescriptlang.org/tsconfig#references)*****

*******在 ProjectA 中，选项“reference”是用对象数组配置的。对象的“path”属性引用被引用项目(ProjectB)的项目根(包含 tsconfig.json)。对象还有一个“prepend”属性，但它只适用于“outFile ”,不在讨论之列。*******

*******请注意，此选项仅在使用“- -build”调用 tsc 时有效。*******

*********选项“** [**复合**](https://www.typescriptlang.org/tsconfig#composite)**”:*********

*****默认情况下，该选项的值为“true”。对于被另一个项目(ProjectB)引用的项目，必须将其设置为“true”。它强制实施以下约束，使构建工具能够快速确定项目是否需要重新构建，而无需查看源代码:*****

*   *****“rootDir”默认为项目根目录(包含 tsconfig.json)*****
*   *****所有要编译的实现文件必须由“include”和“files”显式匹配，而不是由模块引用包含。这样，构建工具不需要检查源文件来确定需要构建哪些文件。*****
*   *****“declaration”默认为 true，以生成类型声明(. d.ts)。您可能还希望启用“declarationMap ”,以便当用户在代码编辑器(如具有“转到定义”功能的 VS Code)中单击定义时，编辑器会转到源文件(。ts)而不是类型声明(. d.ts)。*****

*******增量编译选项** [**增量**](https://www.typescriptlang.org/tsconfig#incremental) **和构建模式(-b，- - build)*******

*****增量编译依赖于构建信息文件来确定哪些源代码需要重新构建。文件的位置可以由“tsBuildInfoFile”选项控制。如果文件丢失，tsc 将进行全面编译。*****

*****Tsc 命令行选项“-b”、“- -build”使 tsc 构建被引用的项目。由于被引用的项目必须打开选项“复合”，而“复合”又强制“增量”，所以被引用的项目总是增量编译的。“复合”还打开“声明”，因此发出被引用项目的类型声明(. d.ts)。那么对被引用项目的模块引用将被解析为类型声明，而不是实现。*****

*****有一些标志可以与"-b "或"- - build "一起使用。它们是"- - dry "用于模拟运行，"- - clean "用于清理构建输出，"- - force "用于构建，好像所有项目都过期了一样，而"- - watch "用于在监视模式下构建。*****

*****请注意，增量编译根据编译信息文件确定编译哪个源代码，而不检查输出。如果上次生成的生成信息文件在那里，并且没有源文件被更改，即使发出了。js 文件被删除，增量编译不会重新创建它们。目前，在 Github 上有一个关于这个的[公开问题](https://github.com/microsoft/TypeScript/issues/30602)。此外，如果构建信息文件被删除，当调用' tsc -b - -clean '时，将发出。js 文件不会被删除。为了避免这种情况，请始终将构建信息文件(tsconfig.tsbuildinfo)与发出的. js 一起移除。*****

*******将引用的项目作为模块导入*******

*****选项“reference”仅生成被引用的项目，不会更改 TypeScript 的模块引用的任何行为。要将引用的项目作为模块导入，请参考上面讨论的模块引用。*****

*******使用项目参考的注意事项*******

*****被引用项目的类型声明文件(. d.ts)由编译器生成。这意味着，在构建之前，引用被引用项目的导入语句将在代码编辑器(如 VS 代码)中显示错误，因为缺少声明。有时候，VS 代码中的错误直到重新启动才会消失。*****

# *****结论*****

*****理解了这些机制和配置选项，您应该能够以更灵活的方式组织您的 TypeScript 项目。如果您想尝试本文中讨论的特性和配置选项，您可以查看这个[演示项目](https://github.com/bingtimren/ts-proj-structure-demo)并研究它的构建过程。*****

## *****进一步阅读*****

*****[](https://bit.cloud/blog/sharing-types-between-your-frontend-and-backend-applications-l5qih48g) [## 在前端和后端应用程序之间共享类型

### 您的后端 API 已经更新，可以返回新类型的数据。必须通知前端团队进行更新…

比特云](https://bit.cloud/blog/sharing-types-between-your-frontend-and-backend-applications-l5qih48g)***** 

******更多内容请看*[***plain English . io***](https://plainenglish.io/)*。报名参加我们的* [***免费周报***](http://newsletter.plainenglish.io/) *。关注我们关于*[***Twitter***](https://twitter.com/inPlainEngHQ)[***LinkedIn***](https://www.linkedin.com/company/inplainenglish/)*[***YouTube***](https://www.youtube.com/channel/UCtipWUghju290NWcn8jhyAw)*[***不和***](https://discord.gg/GtDtUAvyhW) *。对增长黑客感兴趣？检查* [***电路***](https://circuit.ooo/) *。********