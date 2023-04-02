# 如何使用 React、AWS 和 Bitbucket 设置部署管道

> 原文：<https://javascript.plainenglish.io/how-to-set-up-a-deployment-pipeline-with-react-aws-and-bitbucket-aeaa9b0bdd9c?source=collection_archive---------1----------------------->

## 建立一个新的 React 项目的快速指南，该项目具有部署到 HTTPS 的 S3/云形成站点的管道。

![](img/6dbdf1220e3eba1ec5aab34fd25f35db.png)

Made by the author in [Canva](https://www.canva.com/)

建立一个新的 React 项目不是开发人员通常会做的事情，所以很容易忘记所有涉及的步骤。本文旨在指导您开始一个新的 React 项目，在 Bitbucket 中为部署设置一个管道，并在 AWS 中设置一个静态站点，通过 HTTPS 为您的项目提供服务。

在本例结束时，您将拥有一个完全可以开发的项目。我们的项目将满足以下标准:

*   在版本控制中设置
*   每次提交时自动运行构建和单元测试
*   能够按下按钮部署到 S3 铲斗
*   作为静态网站托管在 S3 的应用程序
*   该站点安全地服务于 HTTPS 上空的云生成

# 先决条件

在这个示例中，您需要遵循一些事项:

*   一个[比特桶](https://bitbucket.org/product/)账户
*   [创建 React App](https://create-react-app.dev/) 已安装
*   一个 [AWS](https://aws.amazon.com/) 账户

# 位存储库

我们的第一步是在 Bitbucket 中建立一个新的代码库。你要做的第一件事是用你的 Bitbucket 账户登录，打开你的 Bitbucket 仪表盘。

![](img/1428399803d7a3400373548bdb08657a.png)

Screenshot by the author

从控制面板中选择“创建存储库”。这将弹出“新建存储库”对话框，询问您一些信息。

![](img/9272ac39801ab6ff0cd78d9728dbb22f.png)

Screenshot by the author

输入存储库名称，然后单击“创建存储库”。这将为我们创建一个新的空存储库。

![](img/9305dc9756e624662c4031c415895dc2.png)

Screenshot by the author

接下来，我们将需要单击右上角的“Clone ”,在您的机器上克隆项目。

![](img/637ea6124c1eec207723f640bdf65c40.png)

Screenshot by the author

在本地克隆了存储库之后，我们的下一步将是创建一个新的 React 应用程序。我们不会写任何实际的应用程序代码，但我们需要一个空白的项目，基本上只是为了证明我们的设置工作。

在项目目录中，在终端中运行以下命令来生成 React 项目。

```
$ create-react-app .
```

接下来，我们将提交变更并推回到我们存储库的主分支。

```
$ git add .
$ git commit -m "commit create-react-app"
$ git push origin master
```

# S3 静态网站

现在我们已经准备好了一个项目，我们需要在 AWS 中做一些工作来建立我们的基础设施。

登录您的 AWS 仪表板，我们将从那里打开 S3 服务。

![](img/172c901dd851c73f50474275f5525518.png)

Screenshot by the author

![](img/4d568c1d9e3261fcff08b0dc43d229db.png)

Screenshot by the author

从 S3 服务控制面板中，我们将选择“创建存储桶”来创建一个新存储桶来托管我们的站点。

您可以为您的存储桶提供任何您喜欢的名称。我将对默认设置进行的唯一更改是取消选中“阻止所有公共访问”选项。我们需要这个残疾人公开为我们的网站服务。

![](img/7394af6d499e7ae16494d19fba3c3129.png)

Screenshot by the author

![](img/45ae3f0631def75d5d16b592ca9b8815.png)

Screenshot by the author

这是创建表单中唯一需要的更改。创建完成后，我们将从 S3 服务控制面板的列表中选择新创建的存储桶。

![](img/41247a993618b37ccf1e65229e77e723.png)

Screenshot by the author

我们希望在 bucket 中选择 Permissions 选项卡，我们需要将以下 JSON 对象添加到 bucket 策略中，以使这个 Bucket 完全公开可用。

***注:*** *更改资源属性，为自己的 S3 桶匹配 ARN。*

![](img/4404e61ec8fb3f3b79be796056aba1b3.png)

Screenshot by the author

![](img/614190877dd2111e5aece63298661fbb.png)

Screenshot by the author

![](img/215d8ae2cde703a9251ca125225d1e24.png)

Screenshot by the author

我们还需要为这个桶更改一个设置，以使它准备好托管我们的站点。切换到“属性”标签，向下滚动到底部的“静态网站托管”部分，点击该部分的“编辑”按钮。

![](img/e5bf7a9a1bb755dcf9e3c6b538620220.png)

Screenshot by the author

![](img/193e4895eaaa647db214075dff67d05f.png)

Screenshot by the author

在下一页，我们希望启用静态网站托管。然后，我们为索引和错误文档指定“index.html ”,以指向 React 应用程序。

![](img/873ad12774ebadd3ea5557560e67ca6b.png)

Screenshot by the author

# 云锋

下一步是为我们的 S3 站点创建一个 CloudFront 发行版。切换到 CloudFront 服务并点击“创建发行版”

![](img/d5fb1f4ec65d7b794718fdd789046fef.png)

Screenshot by the author

在交付方式屏幕上，选择“Web”下的“Get Started”。

![](img/64ca3a2ae510edc7f6942db1e4ca800f.png)

Screenshot by the author

我们需要复制在上一节中创建的静态 S3 站点的 URL，并将该值粘贴到“Origin Domain Name”字段中。

![](img/e03f531149b1b840726df18fd0584cb5.png)

Screenshot by the author

![](img/ae823626e07b316c32eeea328a149098.png)

Screenshot by the author

现在，我将其他所有内容保留为默认设置，并单击该表单底部的“Create”。

![](img/ff467e54af57c04b7e6e51d1faa69919.png)

Screenshot by the author

这将需要几分钟来生成我们的分布。同时，我们现在可以设置我们的 Bitbucket 构建和部署管道了。

# 比特桶流水线

我们跳回到我们的 Bitbucket 仪表板，从那里我们将希望拉起管道部分。

![](img/4a43041834108e6e03285624a032d0d9.png)

Screenshot by the author

如果您向下滚动，将会出现一个用于创建您的第一个管道的模板。我们需要“将 React 应用程序部署到 S3”选项。

![](img/a0f0b78895ad0806c3218c6bdc2dbe8b.png)

Screenshot by the author

这将弹出一个编辑器，为我们的管道提供一个启动脚本。请注意，我们将需要向 Bitbucket 提供 AWS 访问密钥，作为管道运行时使用的变量。

![](img/5ad4e444fb4bb221dbd36a395a3b5ea8.png)

Screenshot by the author

回到 AWS，我们还需要为 Bitbucket 设置一个 IAM 用户，或者您可以为现有用户提供访问密钥，前提是他们的配置文件中有 S3 和 CloudFront 访问权限。

![](img/bd6fefd485af71e09b4151f8981a08d1.png)

Screenshot by the author

![](img/c949ecd1e0702dc7bb41bb5398031dd7.png)

Screenshot by the author

![](img/fa2f5e2fcc86346a03eb871f1869511a.png)

Screenshot by the author

![](img/0a1c43d7119b42cc53ae103c1dd5c055.png)

Screenshot by the author

创建后，您将获得 Bitbucket 中所需的访问密钥。

回到 Bitbucket，在右边有一个“添加变量”的部分。扩展一下，我们需要在这里为以下变量提供 3 个值:`AWS_ACCESS_KEY_ID`、`AWS_SECRET_ACCESS_KEY`和`AWS_DEFAULT_REGION`

![](img/7810badb55933e878896738d75bf8a50.png)

Screenshot by the author

![](img/7d8e90e375b3a3c6044366f476290f3a.png)

Screenshot by the author

在 bitbucket-pipelines.yml 文件中，我们只需要更改 2 行。第一个是“部署到 S3”部分中的 S3_BUCKET 属性。该值当然是我们为静态站点创建的存储桶的名称。

我们需要更改的第二个值是 CloudFront 失效部分的 DISTRIBUTION_ID。作为参考，我的完整管道文件如下。

接下来，我们单击按钮将文件提交到存储库。现在，如果我们回到管道部分，我们应该看到至少有一条管道在运行。

![](img/630ca3fb48cd935c160085d8ce1818d5.png)

Screenshot by the author

如果我们进入最新管道运行的详细信息部分，如果管道运行成功，我们应该会看到一个部署按钮。单击 Deploy 按钮开始部署过程。

![](img/612ea4563ecc3ed8b97e368012a75cbf.png)

Screenshot by the author

单击“Deploy to Production”步骤下的 Deploy 按钮，这将弹出一个确认对话框，我们在其中再次单击 Deploy 以开始该过程。

![](img/9918f4d2f455e37251b1adc55a6dc022.png)

Screenshot by the author

在成功运行之后，如果我们调出 CloudFront 发行版的域名，我们应该会看到 React 徽标。请注意，每次推送至 master 时，该管道都会运行，在成功运行后，您可以选择手动启动生产部署。

![](img/8ac6a4bb0414fcd2430b004bd172af69.png)

Screenshot by the author

# 自定义域名和 SSL 证书

我们的管道现在已经完全设置好了，如果你不需要将你的站点设置为自定义域，那么你可以跳过这最后一步。我们将打开 Route 53 服务并进入注册域名部分。您可以使用现有的域或选择“注册域”来创建一个新的域。

***注意:*** *如果您在不同的提供商下注册了域名，您还可以选择转移到您的 AWS 帐户。*

![](img/04c51789c512523f5c0cafe5259221d3.png)

Screenshot by the author

如果您正在注册一个新域，这可能需要一点时间才能完全完成。完成后，我们将转到 AWS 证书管理器来提供 SSL 证书。

![](img/fc12bdc0df3444e62a791291ebaceda3.png)

Screenshot by the author

单击“配置证书”下的“开始”。我们选择我们需要一个公共证书，然后提供该证书的域名。

![](img/d0a93477a50c692994bc967a51d1e64f.png)![](img/91058cd440f6c4da34d14cee103dbe8a.png)

Screenshot by the author

在下一页上，我们选择 DNS 验证，AWS 将为我们处理验证，因为我们在我们的 AWS 帐户中托管域。

![](img/6c28fd4d2bef1cd5737f21ddf79e1eda.png)

Screenshot by the author

在我们完成验证屏幕后，您将可以选择“在 Route 53 中创建记录”,这是您想要为添加到此证书的每个域执行的操作。

![](img/003874cedced18418cd693fa43aae4b4.png)

Screenshot by the author

![](img/5b0f884c179a2e061554caa35185407e.png)

Screenshot by the author

完成后，AWS 需要几分钟时间来完成新证书的验证。

![](img/d9368c182ef876564fa879e64cd40477.png)

Screenshot by the author

![](img/72c0051409ee2530d41f95c228823ece.png)

Screenshot by the author

一旦您获得批准，验证就完成了。我们现在准备将这个证书与我们的 CloudFront 发行版关联起来。

转到 Route53，我们希望为我们的 CloudFront 发行版创建一个域名别名，以自定义域名。从列表中选择域名，然后在下一个屏幕上选择“创建记录”和“简单路由”。

![](img/22a76c5f7e04bc382687dc70b3c22120.png)

Screenshot by the author

![](img/06be504345e5499ded6782062df054d0.png)

Screenshot by the author

这里我们选择“CloudFront 发行版的别名”,并将 CloudFront 域名粘贴到值中。记录类型值为“A”。

![](img/112cb228804e1d407e8f646136219f6a.png)

Screenshot by the author

回到 CloudFront 服务，选择我们之前创建的发行版。在这里，我们需要选择“行为”选项卡，并编辑已创建的默认值。

![](img/68a591798f7fbdeeb676876b683d70c5.png)

Screenshot by the author

对于“查看器协议策略”，我们需要选择“将 HTTP 重定向到 HTTPS”。

![](img/adc536138d52aa0d30a6e542ee6426c4.png)

Screenshot by the author

确认更改，然后在“常规”选项卡下选择“编辑”。

![](img/b39008662593e16e39048986f2ac5b4f.png)

Screenshot by the author

设置完成后，我们需要将“SSL 证书”更新为“自定义”选项，因为我们已经将它与 Route53 中的域名相关联，所以我们的新证书应该显示为 select 中的一个选项。我们需要做的另一个改变是将我们的域名添加到“备用域名”列表中。

![](img/59b84cc6120db2a11d47b905c9a3f2b8.png)

Screenshot by the author

![](img/efa398c2276b0a8b179aa4c0516649bd.png)

Screenshot by the author

确认了这些更改后，如果我们直接转到 URL，我们应该会看到 React 应用程序，并看到我们的网站受到新 SSL 证书的保护。

![](img/4d1b2d8dce4e2552f1ccf3b1c3516ffa.png)

Screenshot by the author

*以上是使用 AWS 和 Bitbucket 建立生产就绪站点和开发管道的指南。这是一个非常便宜的设置，为开发提供了出色的性能和低摩擦。感谢阅读！*

[](https://medium.com/swlh/manage-nodejs-production-processes-with-pm2-2e77ad57898e) [## 与 PM2 一起管理节点生产流程

### 使用 PM2 管理生产节点简介

medium.com](https://medium.com/swlh/manage-nodejs-production-processes-with-pm2-2e77ad57898e) [](https://medium.com/swlh/clean-up-your-react-code-with-custom-hooks-57f7f4410593) [## 用定制钩子清理你的 React 代码

### 用定制挂钩清理功能组件。

medium.com](https://medium.com/swlh/clean-up-your-react-code-with-custom-hooks-57f7f4410593)