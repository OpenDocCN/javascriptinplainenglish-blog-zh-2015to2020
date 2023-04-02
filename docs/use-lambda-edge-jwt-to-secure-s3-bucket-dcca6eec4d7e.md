# 使用λ@ Edge+JWT 来保护 S3 铲斗

> 原文：<https://javascript.plainenglish.io/use-lambda-edge-jwt-to-secure-s3-bucket-dcca6eec4d7e?source=collection_archive---------2----------------------->

在 CloudFront 后面保护您的 S3 资产是一个常见的用例，因此这些资产只能通过 CloudFront 访问。通常可以通过将 OAI 添加到 CloudFront 发行版中，并使用 CloudFront 签名的 URL 来访问资产。Lambda@Edge 提供了新的思路，可以灵活地实现相同的目标。

顾名思义，Lambda@Edge 在 CloudFront edge 位置运行 Lambda 函数，因此它更接近用户，运行速度也更快。最强大的武器是你可以编程控制你想要如何访问 SF 发行版和 S3！

这个架构有三个关键点。

*   资产元数据用 JWT 编码，JWT 令牌作为参数成为资产 URL 的一部分。
*   Lambda@Edge 与 ViewerRequest 相关联，它在请求到来时验证 JWT 令牌。
*   根据 JWT 令牌验证，继续进行不同的决策。

![](img/493b81adc8c2e41bc559ed144cc02e89.png)

# >应用

您的应用程序代码需要生成一个 JWT 令牌。出于演示目的，我使用 jwt.io 生成令牌，私有签名密钥是`i-like-beijing-duck`，有效负载有两个密钥，`key: beijing-duck.jpg`是 S3 桶中的资产密钥，`iat`表示令牌生成的时间。分发资产时，URL 类似于[*https://public.mianio.com/chinese-food?token = eyjhbgcioijiuzi 1 NII SINR 5 CCI 6 ikpxvcj 9 . eyjrzxkioijizwlqaw 5 nlwr 1y 2 suanbniiiawf 0 ijoxnte 2 JM 5 mdiyfq . h6po-2 oybuxx-xenhlvneyrrctfekndokyvkvpkc*](https://public.mianio.com/chinese-food?token=eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJrZXkiOiJiZWlqaW5nLWR1Y2suanBnIiwiaWF0IjoxNTE2MjM5MDIyfQ.H6PO-2OYbUxX-XenHLvneyrrctFekOZKNdokyVKVPKc)

![](img/4da314443fb0a520c6523c3dea372d4f.png)

# >λ@ Edge

lambda 代码非常简单:

1.  从 URL 中提取令牌
2.  使用私钥验证令牌，以确保它是来自您的发行者(即您的应用程序代码)的真实令牌
3.  如果令牌是真实的，使用有效载荷中的密钥继续余下的请求旅程，即点击 CloudFront -> S3

虽然它被称为 Lambda@Edge，但它与普通 Lambda 函数有所不同，记住这一点非常重要。

1.  不支持环境变量。这就是为什么 JWT 签名令牌是从参数存储中检索，而不是从环境变量中检索。您可能会认为这样不好，因为它需要从参数存储中获取秘密，这会减慢这个过程。嗯，这是真的，但实际上并没有那么糟糕，因为正如你在演示中看到的，`let secret`被声明为句柄外的全局变量，以便我们可以使用 lambda 函数缓存，只有当它是冷启动时，`secret`是未定义的，然后代码需要从参数存储中获取。
2.  函数内存大小只有 128MB！所以你最好保持你的功能精简，不要做繁重的工作。
3.  不支持 VPC。
4.  5 秒超时。

这些限制限制了 Lambda@Edge 做新奇的事情。但幸运的是，这足以满足我们对 JWT 的需求。

如果 JWT 令牌丢失，我们需要做的就是放弃请求，并将其发送到错误页面，在本例中为`[www.mianio.com/40](http://www.mianio.com/401)3`。

如果 JWT 令牌无效，我们需要做的就是放弃请求，并将其发送到错误页面，在本例中为`[www.mianio.com/401](http://www.mianio.com/401)`。

如果 JWT 令牌有效，我们需要从有效负载中提取密钥，并通过设置`request.uri=key`将密钥传递给请求。然后，请求将到达 CloudFront，如果有命中，则为资产提供服务；如果有未命中，则请求将到达 S3。

# >上传

您只能为 us-east-1 地区的函数添加触发器，因此我们必须将函数上传到 us-east-1。

首先，我们需要创建部署包。

```
zip -r viewerRequestFunc.zip .
```

然后我们可以上传包来创建函数。

```
aws lambda create-function --function-name viewerRequest-Mianio --zip-file fileb://viewerRequestFunc.zip --region us-east-1 --runtime nodejs12.x --role arn:aws:iam::xxxxxxxxxxx:role/service-role/viewerRequestRole --handler index.handler
```

稍后当我们需要更新函数时。

```
aws lambda update-function-code --function-name viewerRequest-Mianio --zip-file fileb://viewerRequestFunc.zip --region us-east-1
```

你看，我需要给这个函数一个执行 IAM 角色，这很重要，因为这个函数需要与参数存储对话，还需要记录到 CloudWatch。

![](img/1e96bf88c1a9850daa2368be4419e8a2.png)

要添加触发器，与该功能相关联的 IAM 执行角色必须由服务主体`lambda.amazonaws.com`和`edgelambda.amazonaws.com`来承担。

![](img/9288d7be47f921f8a696e1b2d5a753e6.png)

一旦它被上传，你可以在美国东部一台看到它

![](img/f1d62a0696d3b195bf2d99c1bc908374.png)

# >云锋

由于访问控制现在在 lambda@edge 中，我们需要在 CloudFront 配置中关闭`Restrict Viewer Access`。

![](img/a669ef844474fd4fd1758a42f63dbab6.png)

# >部署

现在是时候把 lambda@edge 和 CloudFront 联系起来了。

1.  发布新版本。请记住，每当功能代码或配置发生变化时，都需要发布新的版本。

![](img/da7349dba08fb85b32b25d358ee7ce9c.png)

2.在新版本的设计器窗口中，点击`+ Add triger`，从列表中选择 CloudFront，并选择与该功能关联的 CF 发行版。

![](img/e6838f3af14489a892a821f7ee096182.png)

将函数部署到发行版需要几分钟时间。

# >日志

我花了一段时间才找到木头在哪里。因为这些功能是在边缘位置触发和执行的，所以我们需要转到边缘位置区域来查看日志。例如，在 ap-southeast-2 上发出一个请求，日志将在 ap-southeast-2 的 CloudWatch 上创建。日志组名称以`/aws/lambda/us-east-1`开头

有了四种类型的 CloudFront 事件，ViewerRequest、OriginRequest、OriginResponse 和 ViewerResponse，您可以将 Lambda@Edge 与许多潜在事件联系起来。但是请记住[的限制](https://docs.aws.amazon.com/AmazonCloudFront/latest/DeveloperGuide/lambda-requirements-limits.html)，因为根据需求的不同，这可能不是一个理想的解决方案。

源代码【https://github.com/crespowang/lambda-edge-auth 

# **用简单英语写的便条**

你知道我们有四份出版物和一个 YouTube 频道吗？你可以在我们的主页 [**plainenglish.io**](https://plainenglish.io/) 找到所有这些内容——关注我们的出版物并 [**订阅我们的 YouTube 频道**](https://www.youtube.com/channel/UCtipWUghju290NWcn8jhyAw) **来表达你的爱吧！**