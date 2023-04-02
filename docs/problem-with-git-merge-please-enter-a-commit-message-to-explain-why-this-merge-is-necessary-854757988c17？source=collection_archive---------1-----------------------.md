# 修复 Git 合并的一个常见问题

> 原文：<https://javascript.plainenglish.io/problem-with-git-merge-please-enter-a-commit-message-to-explain-why-this-merge-is-necessary-854757988c17?source=collection_archive---------1----------------------->

## "请输入一条提交消息来解释为什么需要合并"

我最近完成了这篇关于如何用 Netlify 作为 CMS 创建博客的文章。在撰写本文时，我试图进行一次合并，但最终失败了。我收到了这条信息:

```
Merge branch 'master' of [https://github.com/marieqg/netlify-cms-medium](https://github.com/marieqg/netlify-cms-medium)
# Please enter a commit message to explain why this merge is necessary,
# especially if it merges an updated upstream into a topic branch.
#
# Lines starting with '#' will be ignored, and an empty message aborts
# the commit.
~                                                                                                                                                                  
~                                                                                                                                                                                                                                                                                                                                
"~/Documents/Projets/medium/netlify-cms-medium/.git/MERGE_MSG" 6L, 297C
```

这不是我第一次得这种病，但最后一次是肯定的。我又被困在那里一段时间。根据您使用的编辑器，有几种方法可以解决这个问题。

# 第一步:了解原因

首先，让我们试着理解为什么我们会得到这个信息。

有几个选项:

*   当在提取变更(git pull)之前对您正在处理的分支进行了提交(您尝试推送提交:git push)时，就会发生这种情况
*   您更新了您的 git 客户端
*   你以前从未有过领先于遥控器的本地分支
*   您的 git 配置最近已更改
*   你应该做`git rebase`或`git pull --rebase`而不是合并

![](img/99f470d062af39b709a26c60aa71aca0.png)

Most of the time when I get this message, it’s because I forgot to pull before I pushed and my branches get confused. Photo by [Maddy Baker](https://unsplash.com/@maddybakes?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) on [Unsplash](https://unsplash.com/s/photos/branches-git?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)

# 步骤 2:完成合并

然后，让我们解决它来完成我们的合并。解决这个问题的方法将取决于我们的编辑。所以:

## 1.对于 Vi 或 Vim

如果您正在使用 vi 或 vim，要设法退出，您必须:

1.  按“I”(I 代表插入)
2.  撰写您的合并邮件
3.  按“esc”(退出)
4.  写“:wq”(写并退出)
5.  然后按回车键

就我个人而言，这是我正在使用的一个编辑器(让我们不要陷入开发人员关于使用哪个编辑器的争论，对吗？)，这对我来说效果很好。

## 2.适用于 Pico、Nano 或 Emac

如果您使用的是 Nano，您必须:

*   CTRL + X
*   然后，CTRL + C

对于 nano，CTRL + C 可能就足够了。

就像这个一样简单，但是我总是纠结于这个。

# 🕵️‍♂️资源走得更远

[](https://stackoverflow.com/questions/19085807/please-enter-a-commit-message-to-explain-why-this-merge-is-necessary-especially) [## 请输入一条提交消息来解释为什么此合并是必要的，尤其是如果它合并了一个…

### 这里 nice 的意思是你喜欢的编辑器，或者你觉得它对用户更友好。潜在的问题是 Git…

stackoverflow.com](https://stackoverflow.com/questions/19085807/please-enter-a-commit-message-to-explain-why-this-merge-is-necessary-especially) [](https://appuals.com/fix-please-enter-commit-message-explain-merge-necessary/) [## 修复:请输入一个提交消息来解释为什么这个合并是必要的-Appuals.com

### 使用 git 开发中心时，最令人尴尬的错误消息之一可能是提交…

appuals.com](https://appuals.com/fix-please-enter-commit-message-explain-merge-necessary/) [](https://www.derekgourlay.com/blog/git-when-to-merge-vs-when-to-rebase/) [## git——什么时候合并，什么时候重组

### 这个(乱七八糟的)分支历史你看着眼熟吗？在 git 中保持干净的历史可以归结为知道何时使用…

www.derekgourlay.com](https://www.derekgourlay.com/blog/git-when-to-merge-vs-when-to-rebase/) 

👏如果你喜欢这篇文章，请随时关注我，将来会收到类似的内容。

📚还有，如果你想了解更多，可以看看那些文章:

[](https://medium.com/@mariequittelier/jamstack-how-to-do-a-contact-form-step-by-step-with-gatsby-js-netlify-and-mailgun-52d26432a5c4) [## 如何用 Gatsby.js，Netlify，Mailgun 一步步做一个联系表单

### 在这篇文章中，我将一步一步地向你展示如何用 Gatsby.js，Netlify 函数一步一步地做一个联系人表单…

medium.com](https://medium.com/@mariequittelier/jamstack-how-to-do-a-contact-form-step-by-step-with-gatsby-js-netlify-and-mailgun-52d26432a5c4) [](https://medium.com/@mariequittelier/how-to-connect-your-gatsby-js-landing-page-to-google-analytics-and-deploy-to-netlify-step-by-step-8352467583df) [## 如何将您的 Gatsby.js 登录页面连接到 Google Analytics 并逐步部署到 Netlify

### 使用谷歌分析很重要，因为它可以让你跟踪你的网站的使用情况。让我们来学习如何使用…进行设置

medium.com](https://medium.com/@mariequittelier/how-to-connect-your-gatsby-js-landing-page-to-google-analytics-and-deploy-to-netlify-step-by-step-8352467583df) [](https://medium.com/@mariequittelier/how-to-create-a-newsletter-with-mailchimp-gatsby-js-netlify-d48778d5c774) [## 如何用 Mailchimp，Gatsby.js & Netlify 创建时事通讯

### 在填写了联系表格并连接了 Google Analytics 之后，我想继续我的系列文章，讲述如何…

medium.com](https://medium.com/@mariequittelier/how-to-create-a-newsletter-with-mailchimp-gatsby-js-netlify-d48778d5c774)