# NestJS —访问和刷新令牌 JWT 认证

> 原文：<https://javascript.plainenglish.io/nestjs-implementing-access-refresh-token-jwt-authentication-97a39e448007?source=collection_archive---------0----------------------->

![](img/04dbe6546854d6f02ddcb9e3a058df28.png)

Photo by [Lance Anderson](https://unsplash.com/@lanceanderson?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

如果您正在 Node.js 中构建应用程序，那么您可能会熟悉 [NestJS](https://nestjs.com/) ，这是一个自我描述的用于构建服务器端应用程序的功能打包框架。可以把它想象成 Laravel、Ruby on Rails 或 Flask for Node。NestJS 允许我们快速构建服务，其中包含路由、验证和数据库访问等基本功能。

令人欣慰的是，身份验证也获得了第一方的支持，通过 passport 和多重警卫支持各种提供商，包括今天的主题 JWT。Nest 为 JWT 的实现提供了一个基本的[指南](https://docs.nestjs.com/techniques/authentication#jwt-functionality)，但是它不一定包括你的典型应用可能需要的所有特性。

让我们来看看一个定制的、功能完整的实现，您可以在自己的应用程序中使用它。为了简单起见，我们假设您有一个基于 Nest 安装指南的现有项目，并且对 Nest 提供的各种组件有基本的了解。

## 入门指南

让我们从安装 Nest 的第一方认证包开始，它将为我们的实现提供 90%的逻辑，以及一些额外支持的外部模块。

```
$ npm install --save @nestjs/passport @nestjs/jwt passport passport-local passport-jwt bcrypt class-validator$ npm install --save-dev @types/passport-local @types/passport-jwt @types/bcrypt @types/jsonwebtoken
```

酷！ [@nestjs/passport](https://www.npmjs.com/package/@nestjs/passport) 和 [@nestjs/jwt](https://www.npmjs.com/package/@nestjs/jwt) 是 Nest 的第一方包，提供基本的功能，而 [passport](https://www.npmjs.com/package/passport) 、 [passport-local](https://www.npmjs.com/package/passport-local) 和 [passport-jwt](https://www.npmjs.com/package/passport-jwt) 是节点中的“标准”包认证。我们还将包括用于散列支持的 [bcrypt](https://www.npmjs.com/package/bcrypt) 和用于请求验证的 [class-validator](https://github.com/typestack/class-validator) 。当然，我们还将包括这些包的类型(假设您使用的是 TypeScript，这是应该的！)，以及 [jsonwebtoken](https://www.npmjs.com/package/jsonwebtoken) 的类型，它已经作为@nestjs/jwt 的依赖项包含在内。

安装完依赖项后，让我们讨论一下应用程序的基本结构。在实现身份验证时，我们将使用 Nest 的[模块](https://docs.nestjs.com/modules)系统将两个模块连接在一起，我们称之为`UsersModule`和`AuthenticationModule`。虽然我们不会特别关注这个实现的数据库部分，但是我们将讨论一个能够很好地工作并存储最基本数据的通用结构。用户数据将由我们的`UsersService`和`UsersRepository`处理，刷新令牌由`RefreshTokensRepository`处理，并由`TokensService`逻辑绑定在一起。

而数据库实现则不那么重要，它可以与您选择的任何提供者一起工作——sequel ize、TypeORM、MongoDB 等。我们仍然需要创建两个模型/实体来支持我们的认证和用户模块。同样，为了简单起见，我们将使用 Nest 的[第一方序列包](https://docs.nestjs.com/techniques/database#sequelize-integration)，但是将它转换成您选择的包应该相当简单。

让我们以树形图的形式将所有这些放在一起，以了解您最终会得到什么。

```
app/
|-- application.module.ts
|-- requests.ts
|-- modules/
    |-- users/
        |-- users.module.ts
         -- users.service.ts
    |-- authentication
        |-- authentication.module.ts
         -- authentication.controller.ts
         -- refresh-tokens.repository.ts
         -- tokens.service.ts
         -- jwt.guard.ts
         -- jwt.strategy.ts
|-- models/
    |-- user.model.ts
    |-- refresh-token.model.ts
```

有几个文件我们还没有讨论，比如`requests.ts`和`jwt.guard.ts`，但是它们的用法在后面会更明显。

当然，一切都可以定制，以适应您的应用程序的结构，您甚至可以选择为`tokens`创建一个单独的模块，或者创建一个或多个附加文件夹，以进一步分离您的存储库和服务。

## 用户和令牌

现在我们已经设计好了应用程序的结构，我们可以进入重要的部分——逻辑。首先，我们可能应该谈谈用户，因为他们是身份验证的核心。

我们将使用最简单形式的`User`模型，它包含两个属性——一个`username`和一个`password`,但是可以很容易地进行修改以添加您可能需要的任何附加信息。

模型定义相当简单，但是你可以在这里阅读更多关于 Sequelize 的类型脚本实现。

在这里，我们还将创建一个`RefreshToken`模型，它将允许我们为每个用户存储可重用的刷新令牌，同时还支持设置到期日期和可撤销性。

`user_id`列当然指的是拥有权的用户，`is_revoked`提供了立即撤销令牌的能力，`expires`提供了自动撤销的时间戳。*从技术上来说，*我们不一定需要包含一个`expires`字段，因为我们将在刷新令牌中嵌入到期日期，但是将它存储在数据库中允许我们在将来有选择地清除到期的令牌。

模型已经完成，但是我们需要一种方法来存储和检索它们的数据。让我们创建两个存储库类来提供可重用的功能。

如果我们提前考虑，我们将需要一些方法——一个根据用户 ID 查找用户以生成刷新令牌，一个根据用户登录时的用户名查找用户，最后一个方法创建新用户进行注册。

如前所述，我们将这个类称为`UsersRepository`，其实现类似于下面的内容。

如果这是你第一次在 NestJS 中看到使用序列模型的类，一定要在这里查看它们的参考指南，特别是关于构造函数注入的参考。

`findForId(id)`和`create(username, password)`都是不言自明的，而`findByUsername(username)`可能会稍微复杂一点，因为添加了支持不区分大小写的用户名登录的序列函数。幸运的是，这些内置函数中的每一个都在[序列文档](https://sequelize.org/master/manual/model-querying-basics.html)中进行了描述，以便快速参考。

在我们将服务和控制器中的一切联系在一起之前，让我们为我们的刷新令牌添加最后一个存储库。我们将需要创建令牌的能力，以及通过其 ID 找到一个令牌。

同样，相当简单。我们将创建一个新的`RefreshToken`，将它与一个`User`相关联，并基于`ttl`参数设置到期日期，指定令牌到期前的秒数。

顺便提一下——在第一个存储库中，我们使用了 Sequelize 的[存储库模式](https://www.npmjs.com/package/sequelize-typescript#repository-mode-1)，但是使用了刷新令牌的静态访问方法。后者允许从模型类中直接访问像`find`和`findOne`这样的方法，而存储库模式允许更好的关注点分离——但是这两种方法都是正确的。

## 服务层

数据库逻辑已经完成。我们已经创建了创建和检索新用户及其刷新令牌的方法，但是我们需要在我们的存储库和 API 控制器之间的最后一层——我们的服务，它将提供连接两者的业务逻辑。

首先，我们想要创建可重用的逻辑来生成访问和刷新令牌，这就是我们的`TokensService`发挥作用的地方。

让我们更深入地看看我们正在实施什么。

首先，我们将通过注入之前创建的`RefreshTokensRepository`类来设置我们的类。我们还将注入 Nest 的内置`[JwtService](https://github.com/nestjs/jwt/blob/master/lib/jwt.service.ts)`类，该类为 jwt 的签名和解码提供了围绕`[jsonwebtoken](https://www.npmjs.com/package/jsonwebtoken)`的包装函数。我们还将设置一个常量变量，声明将在我们生成的所有令牌之间共享的声明——类似于刷新和访问。虽然它们不是绝对必要的，但是它们可以为您的令牌提供额外的验证。所有潜在索赔都记录在 [JWT RFC](https://tools.ietf.org/html/rfc7519#section-4.1.1) 中。

方法上。我们的第一种方法`generateAccessToken`是两种方法中比较简单的一种。给定一个`User`，我们可以要求我们的注入`JwtService`用我们的`BASE_OPTIONS`声明签署一个访问令牌，以及一个附加的`subject`声明(简称为`sub`，它将标识为其生成令牌的用户。这将被嵌入到 JWT 中，允许我们以后解码它，并根据他们的 ID 从数据库中快速检索用户。

您可能想知道这是否安全。谢天谢地，JWT 的有效载荷在设计上是不可变的。由于有效载荷是通过其内容的散列来验证的，因此被潜在黑客或不法分子修改的有效载荷将被服务器视为无效，我们稍后将会看到这一点。

我们将把签名的 JWT 作为一个字符串返回，稍后在 API 控制器中返回给客户机。

至于剩下的方法——`generateRefreshToken`——我们将使用非常相似的逻辑来签署我们的刷新令牌。假设该方法接受一个`expiresIn`参数，我们将再次把它传递给签名选项，允许我们指定刷新令牌的到期日期。此外，我们需要包含`jwtid`声明(简称为`jit`，顾名思义，它的功能是嵌入令牌的 ID。就像我们可以通过`subject`声明来查找`User`一样，我们稍后也可以通过从声明中解码的 ID 来轻松地提取`RefreshToken`。

假设这些方法已经就位，我们可以进一步扩展这个类。最初生成访问和刷新令牌的逻辑已经准备好了，我们还缺少一些从刷新令牌生成访问令牌的方法。

让我们给`TokensService`类添加一些额外的方法。

五种新方法，但它们各自实现了更大难题的一小部分，使它们更容易破译。

让我们先来看看`decodeRefreshToken`，它被传递了一个字符串——也称为编码的 JWT。我们将再次使用内置的`JwtService`，但这次使用的是`verifyAsync`，它将解码 JWT 令牌并以对象形式返回其有效载荷。

说到——我们应该定义我们期望的有效载荷是什么样的。我们将使用一个 TypeScript 接口，用两个属性将其命名为`RefreshTokenPayload`:`jti`和`sub`，它们是我们之前传递给`signAsync`的完整声明的正确版本，分别是`jwtid`和`subject`。这两个属性将返回与令牌签名时嵌入的*完全相同的*值。

您可能还会注意到，我们在解码令牌时有意从`jsonwebtoken`中捕获内置的`TokenExpiredError`。正如我们前面提到的，我们不一定需要在数据库中存储到期日期，因为这个逻辑将确保 JWT 中嵌入的到期日期是有效的。我们还将捕捉任何额外的错误，并以控制器能够理解的格式返回它们。

另外两个方法`getUserFromRefreshTokenPayload`和`getStoredTokenFromRefreshTokenPayload`相当简单明了，几乎完全按照它们的名字来做。给定我们之前定义的解码后的`RefreshTokenPayload`，这些方法将简单地从有效载荷中提取必要的字段，并分别找到和返回`User`或`RefreshToken`。和以前一样，如果字段不在解码后的有效载荷中，我们将在这里抛出异常。

有了解码刷新令牌并从数据库中检索其相关令牌和用户记录的能力，我们可以将这一功能结合起来创建我们的`resolveRefreshToken`方法，该方法将解码并从数据库中返回`RefreshToken`和`User`模型，假设令牌是有效的，并且通过了我们已经实施的所有附加检查。

给定字符串形式的刷新令牌，我们将解码有效负载并从数据库中获取`RefreshToken`。假设令牌存在，并且它的`is_revoked`字段没有被切换，我们将能够在返回用户和令牌之前从数据库中检索用户。

在我们的最后一个方法`createAccessTokenFromRefreshToken`中，我们可以简单地使用我们刚刚实现的`resolveRefreshToken`方法，根据`sub`从刷新令牌中检索`User`，然后为该用户生成一个新的访问令牌。

代币？完成了。用户？差不多了。

现在，让我们创建一个超级简单的`UsersService`来为我们之前创建的`UsersRepository`提供额外的功能。

就个人而言，我喜欢使用服务类来处理业务逻辑，使用存储库来与数据库交互，但是这完全取决于您自己的偏好。在这种情况下，我将使用服务层来处理请求本身和数据库之间的交互。在我们创建这个服务之前，更重要的是在我们创建我们的控制器之前，我们需要定义我们的 HTTP 请求体，为此我们将利用[类验证器](https://github.com/typestack/class-validator)，与许多其他特性一样，Nest 提供了[一流的支持](https://docs.nestjs.com/techniques/validation#auto-validation)。

我们将在我们的`requests.ts`文件中设置我们的请求，但是您可以选择以您喜欢的任何结构将这些请求分成几个文件。

所有三个请求类都是不言自明的，一个提供登录功能，另一个提供注册，最后一个需要一个`refresh_token`,这将允许我们生成一个新的访问令牌。

请求已经定义好了，所以让我们创建我们的`UsersService`类。

每种方法本质上都是现有逻辑的包装器，但是实现起来不那么冗长，易于重用。`validateCredentials`方法使用 bcrypt 的`[compare](https://www.npmjs.com/package/bcrypt#to-check-a-password)`函数将用户存储在数据库中的散列密码与他们试图用来登录的密码进行比较。最后一个 *new* 方法`createUserFromRequest`从前面定义的请求中获取一个`RegisterRequest`对象，验证用户名的唯一性，然后将它和密码传递给`UsersRepository`以创建一个新用户。最后两个方法只是将调用代理到我们的`UsersRepository`中，这避免了我们以后必须注入存储库*和*服务。

## API 控制器路由

业务逻辑**完成。那太累人了。是时候将每个组件组合在一起，创建最重要的部分—路由。在这种情况下，我们将只使用一个控制器——我们的`AuthenticationController`提供注册、登录和刷新我们的用户及其令牌的方法。**

我们可能希望从我们的`register`端点开始，因为尝试登录或刷新甚至不存在的用户是没有意义的！

让我们通过注入前面的`UsersService`和`TokensService`以及实现我们的`register`方法来设置我们的`AuthenticationController`。我们还想添加 Nest 的`@Controller()`装饰器，在`/api/auth`设置控制器的基本路径，使我们的注册端点在`/api/auth/register`作为 POST 请求可用。

与其他逻辑一样，我们保持事情简单。我们通过将请求体作为包含我们的`username`和`password`属性的`RegisterRequest`传递给我们的`UsersService`来创建用户，它将负责创建我们的新用户。然后，我们将获取新创建的用户并生成一个访问令牌，以及一个将在 30 天后到期的刷新令牌。

我们还向控制器添加了一个名为`buildResponsePayload`的私有帮助器方法，接受用户、访问令牌和可选的刷新令牌作为参数。我们将多次重用这个函数来构建一个格式化的有效负载，作为来自每个身份验证端点的响应返回。在这样做的时候，我们还定义了我们的`AuthenticationPayload`接口，表示我们期望每次调用这个方法时接收到的确切结构。

现在我们可以注册用户了，让我们添加一个让他们登录的方法，也可以通过 POST 请求在`/api/auth/login`获得。该逻辑与注册端点几乎相同，但是它不是创建用户，而是从用户名和密码中解析。

我们只需要回顾一下`login`方法的前几行。由于我们需要在检索用户后验证密码，我们将首先尝试使用之前创建的`UsersService`方法，通过用户名找到`User`记录。

假设用户存在，我们将尝试将输入的密码与数据库中的散列密码进行匹配，再次使用我们的`UsersService`类中的`validateCredentials`方法。

如果用户不存在或者密码不匹配，我们将抛出一个`UnauthorizedException`来通知用户用户名或密码无效。否则，我们将继续使用与我们用来为新注册用户生成访问和刷新令牌的*完全相同的*逻辑。

快好了！让我们创建最后一个端点来刷新用户— `/api/auth/refresh`也可以作为 POST 请求使用。这个端点将在主体中接受一个`refresh_token`,我们将使用它来识别用户并生成新的访问令牌。

多亏了我们的`TokensService`，这个方法中的逻辑非常简单。我们需要从请求体中提取出`refresh_token`，并将其传递给我们的`createAccessTokenFromRefreshToken`方法，该方法将返回`User`，以及字符串形式的新的和编码的访问令牌。同样，我们可以构建响应负载，但是这次没有额外的`refreshToken`参数。您也可以选择每次都生成一个新的刷新令牌，但是出于本文的目的，我们暂时坚持重用刷新令牌。

## 模块注册

控制器搞定！此时，我们可以对它进行测试，但是我们需要首先在我们的模块中注册所有的服务、存储库和控制器。

因为我们选择使用两个模块，我们的`AuthenticationModule`和`UsersModule`，我们只需要在开始应用程序之前配置和注册这两个模块。

让我们先设置我们的`UsersModule`，因为前者将依赖于它。

如果你以前在 NestJS 中注册过一个[模块](https://docs.nestjs.com/modules)，这个语法应该很熟悉。`UsersService`和`UsersRepository`都被设置为提供者，然后被导出供其他模块使用。`User`模型也已经通过 Sequelize 导入，因此它可以被那些类解析。

注册了它的依赖项后，我们还可以设置我们的`AuthenticationModule`。

与之前类似，我们将设置我们的`TokensService`和`RefreshTokensRepository`为提供者，并注册我们之前的`AuthenticationController`。`RefreshToken`也将像`User`模型一样被导入，以及在此之前注册的`UsersModule`依赖项。

由于我们的`TokensService`依赖于 Nest 的内置`JwtService`,`JwtModule`也需要在模块中注册使用。我们可以查看文档中的选项，但是我们将特别需要指定一个`secret`以及`signOptions.expiresIn`，它指的是*访问*令牌的生命周期(不是刷新令牌！).该参数接受一个以秒为单位指定生命周期的整数，或者一个时间跨度，如分别代表五分钟或六十秒的`5m`或`60s`。当然,`secret`应该是私有的，永远不要暴露在 GitHub 仓库或最终用户面前。计划使用 Nest 建议的[配置设置](https://docs.nestjs.com/techniques/configuration)，或者您自己的实现来私有存储密钥。

最后，在测试之前，您需要注册这两个模块以及您的`ApplicationModule`中的模型。

它们可以简单地添加到类内的`imports`数组中。

```
// application.module.ts
...imports: [
  UsersModule,
  AuthenticationModule,
],...
```

请参见[嵌套文档](https://docs.nestjs.com/techniques/database#sequelize-integration)注册 Sequelize 模块，包括主机、密码等。还有你的模特班。

模块？注册了。

## 准备好了吗

假设应用程序按照 Nest 文档中的建议正确地设置了一个`main.ts`文件，它应该能够成功地启动并注册我们已经创建的所有类。

```
$ nest start --watch
```

让我们测试一下我们的注册端点。我们还假设我们的服务器运行在本地主机端口 3000 上。

```
curl http://127.0.0.1:3000/api/auth/register \
  -H "Content-Type: application/json" \
  --data '{"username": "jsmith", "password": "test123"}'
```

假设它工作正常，我们应该看到一个包含用户数据、访问令牌及其相应的刷新令牌的响应。

```
{
   "status": "success",
   "data": {
      "user": {
         "username": "jsmith"
      },
      "payload": {
         "type": "bearer",
         "token": "eyJ......HNA",
         "refresh_token": "eyJ......UEk"
      }
   }
}
```

看起来不错！让我们测试一下登录端点。

```
curl http://127.0.0.1:3000/api/auth/login \
  -H "Content-Type: application/json" \
  --data '{"username": "jsmith", "password": "test123"}'
```

这是完全相同的响应，但是当然使用了不同的访问和刷新令牌。在我们结束之前，让我们尝试一下刷新端点。

```
curl http://127.0.0.1:3000/api/auth/refresh \
  -H "Content-Type: application/json" \
  --data '{"refresh_token": "eyJ......UEk"}'
```

对此的响应是类似的，但是缺少新的刷新令牌，因为我们选择了*而不是*来生成新的刷新令牌。当然，您可以选择每次都生成一个新的刷新令牌。

```
{
   "status": "success",
   "data": {
      "user": {
         "username": "jsmith"
      },
      "payload": {
         "type": "bearer",
         "token": "eyJ......HNA",
      }
   }
}
```

## 认证路由

现在怎么办？我们已经生成了访问令牌和刷新令牌，甚至使用刷新令牌来生成访问令牌。

我们可以在这里结束，但是如何进行认证请求呢？

实施身份验证的全部目的是识别用户或访问受保护的资源，如用户的个人资料、订单历史或任何其他不能公开的特征。

让我们扩展@nestjs/jwt 构建的`JwtGuard`和`JwtStrategy`实现，而不是为每个方法重新实现访问令牌检查。

我们将在认证模块中创建`JwtStrategy`作为`jwt.strategy.ts`。

在类构造函数中，我们指定了与之前第一次配置`JwtModule`时相似的选项。应该指定相同的秘密，以及相同的令牌生命周期。我们还添加了`jwtFromRequest`选项来指定在哪里可以访问访问令牌，在这种情况下使用`Authorization`头，通过内置于`passport-jwt`文档[中的`ExtractJwt.fromAuthHeaderAsBearerToken`以及其他可能的提取选项。](http://www.passportjs.org/packages/passport-jwt/)

我们还定义了我们的`AccessTokenPayload`，它提供了与前面定义的`RefreshTokenPayload`相同的功能，用于访问刷新令牌的解码有效负载。访问令牌的有效负载将被传递到`validate`函数中，该函数是`passport-jwt`的包装器，用于检索`User`记录。

我们将从有效负载中提取`sub`属性，再次简称为`subject`，它标识了访问令牌所代表的用户 ID。假设 ID 是有效的，我们可以从我们的`UsersService`获取用户并返回它。现在可以通过控制器中的`@Req()`装饰器在 HTTP 请求中访问用户。

在添加认证路由之前，我们还需要添加一个`JwtGuard`，它将为路由提供实际的认证保护。

这只是对 [Nest 的认证守卫](https://docs.nestjs.com/techniques/authentication#extending-guards)的一个扩展，如果`err`或`info`被传递给`handleRequest`就会抛出一个错误，或者如果用户由于某种原因为空，可能会抛出自己的`UnauthorizedException`。

**重要提示:**`JwtGuard`需要添加到您的`AuthenticationModule`中的提供者列表中，以便注册和使用。

我们的警卫已经注册，并准备验证用户。让我们在我们的`AuthenticationController`中添加一个认证路由，以返回登录用户的配置文件。

使用`@Req()`装饰器，我们将能够访问请求，由于附加在方法上的额外的`@UseGuards(JWTGuard)`，请求现在被注入了登录的`User`。我们的防护将使用访问令牌自动验证和认证用户，然后在请求中使其可访问。然后，我们可以使用用户的 ID 来查找他们的数据库记录，并将其返回给客户端。

让我们试试看。

```
curl http://127.0.0.1:3000/api/auth/me \
  -H "Content-Type: application/json" \
  -H "Authorization: Bearer eyJ......UEk"
```

成功！该响应非常类似于我们的身份验证有效负载，但是只返回用户的信息，没有任何额外的令牌。

```
{
   "status": "success",
   "data": {
      "username": "jsmith"
   }
}
```

现在，您可以轻松保护任何资源。

## 包扎

身份验证不是一个简单的话题——因此这篇文章相当冗长，但是涵盖了在 Nest 中实现这样一个系统最有问题的部分，并试图尽可能地简化它。

肯定会有我错过的东西，或者可以进一步澄清的观点，或者甚至可以用完全不同(或者更好)的方式实现的逻辑。)way but Nest、Passport、Sequelize 和这里使用的所有其他包提供了容易访问且非常有用的文档，非常有用。如果所有的代码样本都不是清晰的小片段，它们可以在[这里](https://gist.github.com/jengel3/6a49a25b2fc2eb56fcf8b38f5004ea2c)或者[样本库](https://github.com/jengel3/nestjs-auth-example)一起查看。

如果你想在 Vue 中使用这个实现，我在这里[写了一整篇文章，记住了这个确切的设置。](https://medium.com/swlh/jwt-authentication-in-vue-nuxt-the-right-way-486e333b1d71)