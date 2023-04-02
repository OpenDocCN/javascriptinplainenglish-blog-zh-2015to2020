# 超简单的语言翻译与 AWS 放大预测和 Vue.js

> 原文：<https://javascript.plainenglish.io/super-easy-language-translator-with-aws-amplify-predictions-and-vue-js-9b40226924c0?source=collection_archive---------8----------------------->

![](img/a1319abdee869d1c187ddccc9c3e1451.png)

Dictionary Page

在过去的几周里，我爱上了 AWS Amplify。我是 Firebase 的超级粉丝，因为它比 AWS 更容易使用。但有了 Amplify，AWS 让使用其核心产品变得简单了。在本教程中，我们将看看 Amplify 的预测 API，并使用它来建立一个语言翻译器。

# AWS Amplify CLI 设置

如果您还没有帐户，请前往 [AWS](https://aws.amazon.com/) 创建一个。然后打开您的终端并运行以下命令:

```
npm i -g @aws-amplify/cli
```

# Vue 应用程序创建

要创建 Vue 应用程序，请打开您的终端，将 cd 放入您选择的目录，然后运行以下命令:

```
npx vue create language-translator
```

然后只需选择应用程序的默认设置:
1。请选择一个预设:**默认(babel，eslint)**

# 放大设置

## 扩大账户

现在，在您选择的代码编辑器中打开新创建的 Vue 项目(我使用的是 VS 代码)。然后，在项目的根目录中打开一个新的终端(在 VS 代码中，终端->新终端)。如果您从未使用过 Amplify，请运行以下命令:

```
amplify configure
```

这将引导您完成在 AWS 中设置新 IAM 帐户的步骤。这个帐户将通过 CLI 编程生成我们告诉 Amplify 的所有内容。

## 初始化 Vue 项目中的放大器

然后，我们需要在我们的项目中初始化 Amplify。为此，请在终端中运行以下命令:

```
amplify init
```

**选择以下选项:**

1.  输入项目名称:
    **回车选择默认**
2.  输入环境的名称: **dev**
3.  选择您的默认编辑器:
    **Visual Studio 代码**
4.  选择你正在构建的应用类型:
    **javascript**
5.  你用的是什么 javascript 框架:
    **vue**
6.  源目录路径:
    **回车选择默认** (src)
7.  分发目录路径:
    **回车选择默认** (dist)
8.  构建命令:
    **回车选择默认的** (npm.cmd run-script build)
9.  开始命令:
    **回车选择默认** (npm.cmd run-script serve)
10.  是否要使用 AWS 配置文件？:
    **Y**
11.  选择您在最后一步中设置的帐户

## 添加放大预测

要将放大预测添加到我们的项目中，请在终端中运行以下命令:

```
amplify add predictions
```

**选择以下选项:**

1.  请从以下类别中选择一个:
    **转换**
2.  您需要将 auth (Amazon Cognito)添加到您的项目中，以便为用户文件添加存储。您想现在添加身份验证吗？:**Y
    Y**
3.  您想使用默认的身份验证和安全配置吗？
    **默认配置**
4.  您希望用户如何登录？
    **用户名**
5.  您想配置高级设置吗？
    **不，我完成了**
6.  您想转换什么？
    **将文本翻译成不同的语言**
7.  为您的资源提供一个友好的名称
    **点击回车键选择默认的**
8.  源语言是什么？你可以选择你喜欢的任何东西。我选择英语
9.  目标语言是什么？你可以选择你喜欢的任何东西。我选择西班牙语
10.  谁应该有访问权限？
    **Auth 和 Guest 用户**

## 发布到 AWS

要让 Amplify 创建我们需要的所有资源，请在终端中运行以下命令:

```
amplify push
```

确认您想要继续，然后在 Amplify 为我们的项目提供服务时静观其变。

# Vue 项目设置

现在，让我们安装一些将在我们的 Vue 项目中使用的包。打开终端并运行:

```
npm i bootstrap-vue aws-amplify aws-amplify-vue
```

## 在 Vue 中配置包

接下来，在 Vue 应用程序中打开 src/main.js，并将以下代码放入其中:

main.js

## 翻译者

现在打开 src/App.vue，将以下代码放入其中:

App.vue

# 结论

这就是全部了。我打赌你认为会比这更难。现在你可以开始明白我为什么喜欢使用 AWS Amplify 了。请在评论中告诉我你的想法。