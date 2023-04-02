# 用 HackMyResume & FRESH 自动化我的简历

> 原文：<https://javascript.plainenglish.io/automating-my-resume-with-hackmyresume-fresh-6b99d655b1a?source=collection_archive---------0----------------------->

![](img/146e3f83a3700ea57be45df42ed6689b.png)

Photo by [Helloquence](https://unsplash.com/@helloquence?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

作为我的数字化春季大扫除的一部分，我决定将我的各种简历、履历表和在线简介合并成一个易于管理的文档。我还想尽可能多地自动化我的简历，使其更容易更新和分发，因为这比我目前使用 Adobe Illustrator 和上传 PDF 的过程更容易。

在对可用于管理、生成和分析简历的不同格式和工具做了一些研究后，我选定了 [HackMyResume](https://github.com/hacksalot/HackMyResume) 和 [FRESH](https://github.com/fresh-standard/fresh-resume-schema) 。HackMyResume 支持 JSONResume 和 FRESH，但是我决定使用 FRESH，因为它支持更多的属性。

# 什么是 HackMyResume？

HackMyResume 是一个节点。JS 工具允许你创建、转换、验证和分析 JSONResume 和 FRESH 格式的简历，它还允许你使用 JSONResume 主题或 FRESH 主题生成各种格式的简历。

要开始使用 HackMyResume，您可以使用`new`命令或指向一个现有的文档。然后，你可以运行`build`使用其中一个主题将简历制作成人工制品，或者使用`analyze`获得文档中关键词的统计数据以及你工作经历中的任何空白。

对我简历的分析最初吸引我使用 HackMyResume，因为我可以用它来确保我的就业历史中没有空白，并使用关键字分析来确保我的简历突出了我希望潜在雇主看到的要点。

对我来说，一个额外的好处是，我可以使用`convert`命令获取我最初用来整理简历的工具的输出，该工具获取我的 LinkedIn 个人资料并将其转换为 JSONResume，然后创建一个新文档。

此外，由于 HackMyResume 可以同时使用 JSONResume 和 HackMyResume 主题，这使得它成为使用这两种格式的理想工具。

# 为什么 FRESH over JSONResume？

JSONResume 可能是这两种格式中最广为人知的。有很多关于它的博客文章，也有很多加强它的主题。

当你访问 JSONResume 网站时，你会被建议将你的简历发布到他们的 web 服务上，但这个 web 服务以及 JSONResume 生态系统中的许多工具似乎都不起作用，因为它们要么运行在旧的节点版本上，要么网站不断给你超时错误。

HackMyResume 在网站前端也好不到哪里去，因为 fluentdesk 网站也从来没有解决过；然而，实际的工具确实可以工作，所以这对我来说是远离 JSONResume 的第一步。

其次，如果您比较 JSONResume 和 FRESH 模式，就会发现 FRESH 模式提供了更多。两者都提供:

*   人员的基本信息
*   在线个人资料
*   工作经历
*   志愿工作
*   教育
*   奖金；奖品
*   出版作品
*   技能
*   语言
*   兴趣
*   参考
*   项目
*   版本控制元数据

然而，对于 JSONResume，FRESH 有以下内容(至少在撰写本文时)

*   技能可以表示为一个分类列表，但单独的技能也有自己的条目(更详细的技能列表)
*   “出版作品”分为写作和演讲，所以你可以添加任何你在会议上发言的内容
*   你可以加上你对旅行和重新安置以及远程工作的倾向
*   你可以添加推荐书和推荐信，这样 LinkedIn 的推荐书就不会和你的潜在雇主可以联系的人的名单混在一起了
*   项目和样本是分开的，这样你就可以有一个定义更好的样本集来使用，而不仅仅是那些与雇佣关系相关的样本
*   FRESH 将志愿工作分为服务和附属，这使得俱乐部和聚会的录音工作变得更加容易
*   FRESH 中的几乎所有东西都有高亮和关键字，而 JSONResume 中的高亮是为少数属性保留的

亮点对我来说很重要。在英国，我们倾向于使用单页文件向潜在雇主概述我们的就业历史，因此能够记录重点内容使得项目列表的生成容易得多。

# 自动化我的简历

> 我选择一个懒惰的人去做艰苦的工作。因为一个懒惰的人会找到一个简单的方法去做。—比尔·盖茨

我之前设置的最大问题之一是，一旦我决定了我希望在简历中包含的技能，我就必须更新我的简历的 Adobe Illustrator 文档以及我简历的网络版本，然后将它们上传到我的网站。

通过结合使用 [Github](https://github.com) 和 [Travis CI](https://travis-ci.org) ，我能够自动验证、分析和生成我的简历，并上传它们，只需对 JSON 文档进行修改。

## 使用 Git & Github 控制我的简历

我每天都在工作中使用 Git，所以 Git 和 Github 是我对非二进制文件进行版本控制的首选工具。

我也是特性分支开发的爱好者，因为用于将所有东西合并回主分支或开发分支的 Pull 请求是一个很好的提醒，可以用来评论正在进行的更改。

为了更改我的简历，我创建了一个功能分支，进行我想用来更新我的简历的更改，然后发出一个拉请求。然后，我让与 Github(在这种情况下是 Travis CI)集成的“检查”工具在合并到我的主分支之前指示更改是否会降低我的简历质量，如果一切正常的话。

如果我决定发布一个新版本的简历，我会为这个版本创建一个 git 标签。这是因为我对 master branch 的更改不会影响最终产品，也不会触发重新上传，这让我可以更好地控制简历的发布。

## 使用 Travis CI 创建和发布我的简历

Travis CI 是我的首选 CI 平台。我更喜欢它，因为它有一个简单的界面，它使用的 YAML 格式允许很多灵活性(例如，我的 CV 需要 Ubuntu 字体系列，可以使用`apt-get`命令安装)。

我让 Travis 在每次提交时运行 HackMyResume 的验证和分析工具，这样我就可以快速发现问题并纠正它们。

我计划最终添加一个测试套件，该套件将使用分析的输出来检查我的就业历史记录中没有添加天数，并确保我的关键字以我希望的方式描述了我的经历。

我还可以让 Travis 运行许多其他工具，比如拼写和语法检查器、阅读年龄检查器和简历语气检查器。

当一个新的 git 标签被创建时，Travis 将运行一个脚本来生成我的简历的 HTML 版本(这是一个单独的更加简化的 JSON 文件)和我的简历的 HTML 版本。

我为我的简历使用了一个自定义主题，该主题针对单张 A4 纸进行了优化，因此我很难使用 HackMyResume 的默认打印选项，所以我使用一个单独的脚本，使用 Puppeteer 从我的 CVs HTML 文档中生成 PDF。

一旦我的简历和履历表的 PDF 和 HTML 格式都生成了，我就用一个脚本把它们上传到我的网站。如果任何步骤失败，那么 Travis 构建将变成红色，这允许我清楚地看到我需要手动干预。

# 构建 HackMyResume 主题

我的简历使用了一个自定义主题，该主题基于我在过去 6 年中使用的现有 Adobe Illustrator 文档。为了保持一致性，我在使用 Puppeteer 打印 PDF 时使用了相同的 CSS 布局和排版规则，因为它比 WKHTMLtoPDF 或 PhantomJS 对最终 PDF 的外观有更多的控制。

构建自定义的 HackMyResume 主题非常简单。Github 上有一个[新鲜主题库，你可以使用它，一旦你复制了基础主题，你就可以黑掉你心中的内容。](https://github.com/fresh-standard/fresh-themes)

这里有一些建议:

*   为主题生成 PDF 和 HTML 输出有单独的 HTML 和 CSS 文档，所以请确保更新正确的文档
*   您可以覆盖部分模板，方法是将它们放在`partials`文件夹中，并更新主模板以移除模板调用的`sections/`部分(这将使用您的`partials`文件夹中的部分)
*   您可以使用`{{{`让把手不转义字符，这对于组织名称中的撇号很有用
*   最好对任何图像进行 base64 编码，而不是链接到它们
*   您可以在句点后提供`dateRange`函数使用的日期格式，例如`{{dateRange . 'YYYY'}}`强制它只显示年份(默认情况下它显示年份和月份

## PDF 打印

对我来说，打印可能是 HackMyResume 中最难弄清楚的部分。因为我需要将我的简历模板保存在一张 A4 纸上，所以我必须有更高级的选项来控制打印设置。不幸的是，HackMyResume 目前提供的 [WKHTMLtoPDF](https://wkhtmltopdf.org/) 和 [PhantomJS](http://phantomjs.org/) 选项未能实现，所以我使用[木偶师](https://pptr.dev/)来控制一个无头 Chrome 浏览器。

我将在这里记录我使用不同打印选项的发现，以防对其他人有所帮助。

为了控制打印设置，您需要创建一个 JSON 文档，该文档将设置传递给 HackMyResume 的选项。在本文档中，您可以通过在`pdf`键下声明`wkhtmltopdf`或`phantomjs`来定义用于打印的引擎。

如果您使用`wkhtmltopdf`引擎，那么您可以在`wkhtmltopdf`键下定义一个新对象，它将包含传递给 WKHTMLtoPDF 的参数。这些可以包括:

*   利润
*   纸张大小
*   纸张方向
*   用于 PDF 的标题

即使您可以定义的选项提供了所有的灵活性，我发现 WKHTMLtoPDF 和 PhantomJS 并不支持我在 CSS 中定义的许多 flex-box 规则和字体。

我选择了木偶师，因为它允许你使用 Chrome，Chrome 已经有很好的 PDF 打印支持，并提供了许多选项来控制输出，我最终的木偶师脚本如下所示:

```
const puppeteer = require("puppeteer");
const path = require("path");
const filePath = path.resolve(__dirname, '../out/cv.html'); 
(async () => {
  const browser = await puppeteer.launch();
  const page = await browser.newPage();
  await page.goto(`file:///${filePath}`);
  await page.pdf({
    path: "out/CV.pdf",
    format: "A4",
    printBackground: true,
    displayHeaderFooter: false,
    margin: {
      top: "10mm",
      left: "10mm",
      right: "10mm",
      bottom: "10mm"
    }
  });
   await browser.close();
})();
```

这个脚本实际上是告诉 Chrome 打开我的简历的 HTML 文件，打印不带页眉或页脚的 PDF 文件，并打印背景图像(因为我使用这些内容作为一组自定义列表项目点)

# 我想做的改进

自动生成我的简历和履历表只是我实现目标的第一步，我的目标是让那些工作的人更容易更新他们的简历，而不是每三年换一次工作。

我目前使用每月提醒来记住坐下来记录我最新的项目、技能和工作中的亮点，但这可以通过聊天机器人等对话界面来处理得更好(这是我积极寻求建立的东西)。

聊天机器人可以利用我正在使用的功能分支系统，在我与它交谈后发出拉请求，让我在查看 Github 通知时接受更改。这让我可以更好地控制输入的内容，通过 Travis 对提交进行检查，我可以看到回归。

一旦我足够信任聊天机器人的更新，我会直接从主分支发布，而不是 git 标签，这样我就可以有一个连续的简历和简历发送渠道。如上所述，虽然我希望有更多的检查发生这种情况。

另一个我想做的整合是更新我的 LinkedIn，Stack Overflow Developer Story 和其他网站，作为发布过程的一部分，这样一切都保持同步和一致。

我也希望建立一个交互式的 HTML 简历模板。到目前为止，我看到的所有主题都非常倾向于提供静态的可打印网站。我的用例有点不同，所以能够使用 React 和 D3.js 这样的工具来可视化我的简历数据将有助于我脱颖而出。

# 摘要

保持信息更新是一件痛苦的事情，所以自动化分析和生成我的简历和履历让我可以专注于修改文本。

将更改代码的需求抽象到对话界面中可以使更新变得轻而易举，并且不会感觉像我在更新我的简历，而只是聊聊工作和我的项目。

虽然 HackMyResume 比 JSONResume 好，但打印选项并不完美，因此您可能需要运行自己的打印脚本来确保获得想要的输出。