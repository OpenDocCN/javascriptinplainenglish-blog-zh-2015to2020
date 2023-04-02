# 用 yup 验证 Node/Express.js REST API 调用

> 原文：<https://javascript.plainenglish.io/validate-your-node-express-js-rest-api-calls-with-yup-5c4080fdae87?source=collection_archive---------1----------------------->

![](img/ae2e93d7af29042ca32b25e812df6a3a.png)

Photo by [Gabriel Crismariu](https://unsplash.com/@momentsbygabriel?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 概观

当您的 Express API 收到 HTTP 请求时，如何检查请求是否有效？特别是对于 POST 和 PUT 请求，您希望请求体包含要处理的整个对象，那么您如何知道它是否有正确的字段和有效值呢？

在我早期的一些 API 中，在我的路径中编写我自己的验证代码来检查字段和类型，或者甚至在模型层中编写一些代码，在将对象放入数据库之前检查它的各种条件，这似乎是很自然的。这些解决方案总是以混乱/不可靠/不完整告终。本文向您展示了如何使用一个名为 yup **的很酷的验证库** [**创建定制的 Express 中间件来验证 HTTP 请求体。**我还将展示一个在 yup 模式中嵌套对象和条件验证的例子。](https://github.com/jquense/yup)

所有这些示例的代码都可以在这个存储库中找到:

[](https://github.com/neightjones/express-yup-validation) [## 邻居/快递-是-验证

### 该模板使用 babel 创建了一个基本的 Node.js / Express.js API。它还为 eslint 和…设置了很好的默认值

github.com](https://github.com/neightjones/express-yup-validation) 

*这个库是基于*[https://github.com/neightjones/node-babel-template](https://github.com/neightjones/node-babel-template)—*我在这两篇文章中写了建立这个 babel/eslint/prettle 简单样板项目的过程:*

*   [https://medium . com/JavaScript-in-plain-English/a-minimal-node-js-express-babel-setup-part-1-6 be 7 B3 F3 bb 55](https://medium.com/javascript-in-plain-english/a-minimal-node-js-express-babel-setup-part-1-6be7b3f3bb55)
*   [https://medium . com/JavaScript-in-plain-English/a-minimal-node-js-setup-part-2-eslint-beauty-vs-code-7963768 dbb 30](https://medium.com/javascript-in-plain-english/a-minimal-node-js-setup-part-2-eslint-prettier-vs-code-7963768dbb30)

# 1.我们的新“产品”模型

首先，让我们创建一个可以在整个示例中使用的新类型的对象。让我们假设我们正在创建一个电子商务网站，在那里我们有很多产品列表供人们购买。目前，我们的产品模型非常简单，包含以下几个方面:

*   `id`:这是每个产品的唯一标识符——这是**要求的**
*   `name`:表示产品名称的字符串— **必选**
*   `description`:描述产品的字符串——这是**可选**
*   这个产品多少钱？—这是**要求的**
*   `category`:尽管我们可能不会这样做，但这将是一个简单的字符串，用于指定产品的类型(`sporting goods`、`clothing`等)。)—这是**要求的**

这是一个有效产品的例子:

```
// Looks good!
const product = {
  id: 1,
  name: 'The Imitation Game',
  description: 'Movie about Alan Turing',
  price: 19.99,
  category: 'movie',
};
```

唯一可选的字段是`description`，所以我们不需要添加它来获得有效的产品。所有其他字段(使用正确的类型！)是必需的。

# 2.含 yup 的产品模式

考虑到我们新的`product`模型，让我们看看如何将这些信息编码到一个 yup 模式中。

[Yup 声称](https://github.com/jquense/yup)它是“一个用于值解析和验证的 JavaScript 模式构建器。”在本帖中，我们将重点关注验证用例。需要做两件事:

1.  我们定义一个模式来表示我们的`product`对象模型——这意味着编码它的字段、类型、什么是必需的或者不是必需的，等等。
2.  我们将看到如何使用 yup 模式来测试一个 JavaScript 对象是否有效

本节中的代码在关联回购中找不到——这只是通过一些例子来感受一下 yup 验证是如何工作的。

看看这个最初的例子:

*   第 1 行:从 yup 导入所有内容，这样我们就可以使用`yup.<x>`语法
*   第 3 行:这是我们的第一个 yup 模式……我们很快会对此进行扩展，但是现在我们说`productSchema`只是有一个名为`name`的字段，它的类型是 string
*   第 7、8 行:这是两个简化的产品示例
*   第 10、14 行:这两个块使用`productSchema`上的`isValid`方法来检查我们传入的对象是否有效(它返回一个承诺，所以我们使用`.then`来等待结果并注销它

如您所料，`product1`是有效的——它有`name`字段，这是到目前为止模式中提到的唯一字段。但是，`product2`是不是**也有效** …在我们的 schema 中，并没有说`name`字段是必需的，所以`product2`只有一个名为`title`的字段也没关系，多余的字段也可以。

幸运的是，我们可以在每个 yup 模式字段上链接不同的操作符，包括`.required()`调用。记住这一点，让我们用最初描述的字段创建完整的`productSchema`,并在需要的地方使用`.required()`:

*   第 3 行:我们的`productSchema`现在拥有了我们讨论过的所有字段。除了`description`之外的所有字段都链接了`required()`调用，因为`description`是唯一的非必填字段。此外，您可以看到数字字段有一些额外的验证(两个都是正数，一个是整数)……[您可以在这里找到所有这些选项，例如数字的选项](https://github.com/jquense/yup#number)。
*   第 11 行:`product1`包含所有必需的字段，并且每个字段都有适当的类型
*   第 19 行:`product2`缺少`description`，这很好，因为它不是必需的。然而，看看它的`price`字段……该模式需要一个数字，*而不是*一个字符串。
*   基于此，控制台日志将显示`product1`有效，但`product2`是**而不是**。当我们将实际代码添加到我们的项目中时，您还将看到如何捕获*对象无效时的验证错误*。

这是让我们继续前进的基本思路，所以我们将深入实际代码。稍后，我们将通过一个稍微复杂一点的例子来展示如何在一个 yup 模式中进行条件验证。

# 3.添加一个简单的产品路由器

正如我之前提到的，你可以在这个报告中看到所有的最终代码[，或者如果你想继续下去，你可以从我之前写的这个样板文件](https://github.com/neightjones/express-yup-validation)中学习。

首先，我们将为`products`添加一个新的 routes 文件。在`src/routes`下，我们将创建一个名为`products`的新文件夹(我们使用一个文件夹而不是一个简单的`products.js`文件，因为我们将有更多的代码与这个新的路由逻辑组合在一起)…在`src/routes/products`内，添加一个`index.js`文件，并将其添加到您的新文件中:

接下来，您需要通过添加 2 行代码将它与`src/app.js`连接起来。首先，在导入其他路由器的地方导入`productsRouter`，就像这样:

```
import indexRouter from '#routes/index'; // this was already here
import usersRouter from '#routes/users'; // this was already here
import productsRouter from '#routes/products'; // this is new!
```

第二，您将告诉`app`使用此路由器用于`/products`路由，方法是在其他路由器被使用后添加一行，如下所示:

```
app.use('/', indexRouter); // this was already here
app.use('/users', usersRouter); // this was already here
app.use('/products', productsRouter); // this is new!
```

现在我们已经准备好给我们的新`productsRouter`打几个电话。这里没有太令人兴奋的事情发生。这是一个简单的`POST`端点，它记录在请求体中发送的对象，然后将该对象回显给请求者……在继续之前，让我们测试一下它是否工作。假设您已经在端口 3000 上启动了 Express 服务器，从终端运行以下命令:

```
curl -v localhost:3000/products \
-X POST \
-H "Content-Type: application/json" \
-d '{ "hello": "world" }'
```

如果一切顺利，您将看到您的 Express 服务器打印出对象`{ "hello": "world" }`，并从您的终端看到您的请求收到了一个`200`响应，其中`{ "hello": "world" }` JSON 被回显给您。

现在我们准备添加一些验证。

# 4.验证 POST / PUT 的快速中间件

Express 中的中间件允许您在主 API 端点代码中处理请求之前，向请求/响应周期添加逻辑层。我们添加一些定制的 Express 中间件的目标是什么？我们想在主处理程序之前**检查 POST 请求是否包含有效的产品对象。这样，有两种结果:**

1.  如果产品是有效的，我们的路线/服务/模型代码的剩余部分可以干净地编写，因为我们将保证`product`有它需要的字段，或者
2.  如果产品无效，最好尽早失败，并向请求者返回一个`400: Bad Request`。如果我们这样做了，我们就成功地将验证逻辑隔离在一个地方，一旦它变得复杂，这就是一个巨大的胜利。

## 创建验证程序文件

在上面创建的新文件夹`src/routes/products`中创建一个新文件，命名为`validator.js`。该文件将简单地包含我们需要在该文件夹的相应 API 端点中验证的任何对象的 yup 模式。现在，我们只处理`products`，所以我们将使用我们之前创建的`productSchema`并使其成为默认导出，如下所示:

当然，这还没有做任何事情…我们将需要创建实际的 Express 中间件，并最终与中间件一起使用这个`productSchema`。

## 创建中间件

尽管我们现在只处理`products`路线，但是让我们在任何新路由器都可以使用它来验证其对象的地方创建中间件。在`src/routes`中创建一个名为`middleware`的新文件夹，并在该文件夹中创建一个名为`validateResource.js`的文件。添加以下代码:

让我们来看看这是如何工作的:

快速中间件的标准签名如下所示:

```
function myMiddleware(req, res, next) {
  // logic for my middleware here
}
```

这里有一个关于中间件的快速指南。重要的是，您可以访问请求和响应对象。这样，您可以看到请求体、方法等内容。，如果您愿意，也可以通过`res`参数返回一个响应。最后，`next`参数是您如何将中间件链接在一起——当您调用`next();`时，您是在告诉 Express 继续前进到链中的下一个中间件(或者，在我们的例子中，继续前进到主路由处理器，它本质上只是另一个中间件)。

我们上面的`validateResourceMW`中间件的一个有趣的属性是，它*返回* *一个函数*，该函数具有作为一个 Express 中间件的适当形式。我们这样做是为了能够参数化我们的中间件。因此，导出的函数将一个名为`resourceSchema`的参数作为它的单个参数，并返回一个在其逻辑中利用了`resourceSchema`的有效中间件函数。通过这种方式，不同的路由器可以将不同的对象模式传递给这个通用中间件，这使得每个路由器能够为其关心的对象类型获得正确的定制中间件。在我们的例子中，我们将传入我们刚刚在位于产品路由器旁边的`validator.js`文件中创建的`productSchema`。

让我们看看中间件功能中的逻辑:

*   第 7 行:我们试图验证的“资源”是请求体。在我们的`productsRouter`中，将是一个产品对象被发布
*   第 10 行:我们现在使用稍微不同的 yup 来验证资源。您可以看到对`resourceSchema.validate(resource);`的异步调用。这很方便，因为如果资源无效，它将抛出一个错误，这就是为什么我们将它包装在一个`try` `catch`块中。
*   第 11 行:如果我们到达这里，我们知道资源是有效的，所以我们调用`next()`，它将请求转发给我们的路由处理器
*   第 13–14 行:如果`validate`调用抛出一个错误，我们就进入`catch`块。为了捕捉错误，我们在这里使用`validate`而不是`isValid`(之前的)。您可以看到，我们返回了一个使用了`e.errors.join(', ')`的定制 JSON 响应的`400`错误——这些错误是关于验证期间出错的有用字符串。我们会看到一些例子。

## 更新`Product Schema`

在我们把所有的部分联系在一起之前，让我们回顾一下我们在`src/routes/products/validator.js`中创建的`productSchema`。当我们之前运行一些基本测试时，这是非常有意义的…具体来说，一个产品有一个`id`字段是有意义的，因为我们正在回答问题*“这是一个有效的产品吗？”*然而，在 POST 请求的情况下，不应该有与请求主体相关联的`id`——数据库将为我们生成一个新的、唯一的 id。因此，让我们删除 id 字段，留给我们一个更新后的`validator.js`文件，如下所示:

## 把它们绑在一起

最后，我们需要在我们的`products`路由器索引文件中使用中间件，这又需要我们在`validator.js`中定义的`productSchema`…现在，新的`products`路由器文件将看起来像这样:

我们将`validateObjectMW`(中间件)和`productSchema`(yup 模式)导入到路由索引文件中。然后，在第 7 行，您可以看到`post`的第二个参数现在是`validateObjectMW(productSchema)`，它告诉 Express 在运行我们的 POST 端点中的逻辑之前运行这个中间件。

重启你的 Express 服务器，让我们运行几个`curl`来测试一下。

初次尝试:

```
curl -v localhost:3000/products \
-X POST \
-H "Content-Type: application/json" \
-d '{ "name": "The Imitation Game", "price": 19.99, "category": "movie" }'
```

这成功地通过了，因为我们的请求体是一个有效的产品(我没有添加`description`，但这不是必需的)。既然成功了，您应该会看到日志“很好！我们通过了验证。”以及一个`200`的回应随着 JSON 的身体回响了回来。

接下来，让我们通过测试这个无效体来证明我们的验证是有效的:

```
curl -v localhost:3000/products \
-X POST \
-H "Content-Type: application/json" \
-d '{ "name": "The Imitation Game", "price": "$19.99", "category": "movie" }'
```

我收到一个带有以下 JSON 正文的`400`状态响应:

```
{"error":"price must be a `number` type, but the final value was: `NaN` (cast from the value `\"$19.99\"`)."}
```

完美！您是否希望向用户返回类似这样的内容是另一回事，但是现在我们知道我们可以成功地从我们的模式中捕获验证错误。还要注意的是，服务器上没有日志显示“太好了！我们通过了验证。”中间件在任何路由处理器代码运行之前返回`400`响应，这正是我们想要的结果。

`POST`和`PUT`请求的工作方式非常相似。与`PUT`的唯一区别是，您可能有一个路径变量来表示被更新资源的 id(例如`PUT /products/:id`)。在这种情况下，您仍然可以使用我们定义的相同的`productSchema`请求体。

如果请求命中我们的 API 来发布或放置无效对象，我们已经实现了尽早失败的目标。我喜欢这个解决方案，因为它很好地将验证对象和验证对象上的任何业务逻辑分开。简单的情况已经解决了，但是让我们看看几个更有趣的特性，我们可以在 yup 中使用它们来处理更复杂的模式。

# 5.yup 模式中的嵌套对象

到目前为止，我们的`product`对象只有简单的字段类型，比如数字和字符串。更复杂类型的模式看起来如何？让我们给我们的`product`模型添加另一个名为`locations`的字段。`locations`键将包含一个表示产品销售地点的对象数组。每个位置都有以下字段:

*   `city`:所在城市
*   `state`:位置的 2 个字母的州代码

幸运的是，我们可以嵌套 yup 对象，并通过分离子对象(`location`)信息来保持我们的模式整洁。这是我们更新的产品路线的`validator.js`文件:

我们现在有了一个名为`locationSchema`的新的 yup 模式，它反映了上面的描述。我在 yup 的`string` api 上使用了一个名为`matches`的特性——它使用了一个正则表达式，并确保字符串相对于正则表达式是有效的。对于两个字母的州代码来说，这不是一个完美的解决方案，但至少我们检查了我们有两个大写字母。

在第 16 行，我们将两个模式联系在一起。`productSchema`现在有了`locations`，这是一个必须遵循我们的`locationSchema`的对象数组。注意，值不是`.required()`，所以产品不需要有`locations`才有效。下面是一个将成功通过验证的测试:

```
curl -v localhost:3000/products \
-X POST \
-H "Content-Type: application/json" \
-d '{ "name": "The Imitation Game", "price": 19.99, "category": "movie", "locations": [{ "city": "New York", "state": "NY" }, { "city": "Denver", "state": "CO" }] }'
```

这里发生了什么？

```
curl -v localhost:3000/products \
-X POST \
-H "Content-Type: application/json" \
-d '{ "name": "The Imitation Game", "price": 19.99, "category": "movie", "locations": [{ "city": "New York", "state": "NY" }, { "city": "Denver", "state": "Colorado" }] }'
```

我们得到一个`400`和一个有用的错误消息:

```
{"error":"locations[1].state must match the following: \"/^[A-Z]{2}$/\""}
```

发送的是完整的州名“科罗拉多”，而不是它的两个字母的代码“CO”，这触发了一个验证错误。

# 6.yup 模式中的条件逻辑

让我们做最后一个例子。如果我们想在我们的`product`模型中添加一个`subCategory`字段，但是只有某些`category`值需要它，该怎么办？例如，如果`sporting goods`类别需要一个类似`basketball`或`football`的子类别，类似的`electronics`类别需要一个类似`laptops`或`tvs`的子类别，该怎么办？我们假设没有其他的`category`需要一个`subCategory`。

我们可以在 Yep 中轻松处理这个问题。这是我们模式的另一个更新版本:

我们利用了 yup 的`.when`条款。内容如下:*“子类别是一个字符串…当类别字段为‘体育用品’时，我们知道子类别是一个必需的字符串，如果类别为‘电子产品’，情况也是如此，但是，如果两个条件都不成立，则子类别不是必需的。”*

让我们测试几个`curl` s…

```
curl -v localhost:3000/products \
-X POST \
-H "Content-Type: application/json" \
-d '{ "name": "The Imitation Game", "price": 19.99, "category": "movie" }'
```

第一次调用是成功的，因为类别是`movie`，所以不需要添加`subCategory`字段。让我们合并一个具有新需求的类别:

```
curl -v localhost:3000/products \
-X POST \
-H "Content-Type: application/json" \
-d '{ "name": "The Imitation Game", "price": 19.99, "category": "sporting goods" }'
```

此示例未通过验证。`category`是“体育用品”，但是我们的新模式中没有指定包含`subCategory`。最后:

```
curl -v localhost:3000/products \
-X POST \
-H "Content-Type: application/json" \
-d '{ "name": "The Imitation Game", "price": 19.99, "category": "sporting goods", "subCategory": "basketball" }'
```

这是一个成功的调用，因为我们再次为`category`使用了“sporting goods ”,并且这次根据需要包含了一个`subCategory`字符串。这个想法对于我们模式中的另一个有趣的“电子”也是一样的。

Yep 中的`.when`选项提供了很大的灵活性。如果您需要更大的灵活性，甚至还有一个逃生舱，您可以在`when`子句中使用更通用的函数— [参见文档](https://github.com/jquense/yup#mixedwhenkeys-string--arraystring-builder-object--value-schema-schema-schema)中的这些示例。

感谢您的关注，如果您发现任何问题或有其他改进工作流程的建议，请与我联系。