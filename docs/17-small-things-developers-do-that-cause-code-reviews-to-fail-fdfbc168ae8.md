# 开发人员做的导致代码评审失败的 17 件小事

> 原文：<https://javascript.plainenglish.io/17-small-things-developers-do-that-cause-code-reviews-to-fail-fdfbc168ae8?source=collection_archive---------4----------------------->

![](img/35b8b92b775139531f8f632455c36302.png)

Photo by [Pankaj Patel](https://unsplash.com/@pankajpatel?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

代码审查有时也称为同行审查，是检查同行开发人员的代码以查看是否有任何错误并符合业务标准的行为。

在我目前的角色中，我负责审查我团队的代码。每天我都会遇到一些导致评审失败的常见错误。下面是一些例子。我是一个全栈开发人员，所以有广泛的技术，希望你喜欢！

1.  **未使用 switch 语句**

就像用`switch`代替`else if`一样简单

*失败:*

```
if (clicks == 0) 
{
    newUser();
} 
else if (clicks == 1) 
{
   returningUser();
}
else if (clicks == 2) 
{ 
  almostFinished();
}
else if (clicks == 3) 
{
   finished();
} 
else if (clicks == 4) 
{
   restart();
}
```

*通过:*

```
switch (clicks)
  {
`     case 0:
          newUser();
          break;      
      case 1:
          returningUser();
          break;
      case 2:
          almostFinished();
          break;
      case 3:
          finished();
          break;      
      case 4:
          restart();
          break;
  }
```

**2。添加不必要的 else 语句**

一个小的但只是返回值。

*失败:*

```
if (id == 0) 
{
    return null;
} 
else 
{
  return success();
}
```

*通过:*

```
if (id == 0) 
{
    return null;
}return success();
```

**3。没有关于接口的注释**

我的意思是，你可以说有很多地方你应该添加注释，但是有了界面，就没有借口了！这也有将`/// <inheritdoc /`添加到将继承该接口的类中的好处。

*失败:*

```
interface IQueue
{
    bool Proccess(string message);
    bool Close();
}
```

*通过*:

```
/// <summary>
/// This interface is awesome
/// </summary>
interface IQueue
{
    /// <summary>
    /// This method is awesome
    /// </summary>
    /// <param name="message">The awesome parameter</param>
    /// <returns>A awesome bool</returns>
    bool Proccess(string message);

    /// <summary>
    /// This method is awesome as well
    /// </summary>
    /// <returns>A awesome bool</returns>
    bool Close();
}
```

*继承文件:*

```
public class Queue : IQueue
{
   /// <inheritdoc />
   public bool Proccess(string message) 
   {
```

**4。重命名 WCF 标记文件**

如果你曾经和 WCF 一起工作过，你会知道如果它和网络配置不匹配会导致问题。

这通常发生在开发人员复制现有的 WCF 服务文件(。svc)来创建一个新文件，但是他们只改变了服务名文件，而没有改变 WCF 服务标记。

标记文件通常如下所示:

```
<%@ ServiceHost Language="C#" Debug="true" Service="WebSrv.MyService" Factory="Tools.Services.ServiceHostFactory`1[WebSrv.MyService]"  CodeBehind="MyService.svc.cs" %>
```

**5。缺少信息应用/网络配置**

如果你曾经和。这在发布到 live 时会导致问题。这可能是缺少服务引用，或者**没有**将实时信息添加到发布转换配置中。

6。弄乱项目文件

这种情况在. net 项目中经常发生，通常是在没有定期获取/获取最新版本的代码时，连锁效应会导致混乱的合并。

**7。最小测试断言**

根据我的经验，在创建测试时，你应该检查代码的很多方面，以帮助你的应用程序成长。

关于资产的想法

*   数据库数据是所期望的
*   无效支票
*   如果返回了响应，检查它是否与请求相关

**8。不检查空值**

空值会导致很多错误！重要的是要有正确的检查来阻止那些对象引用错误！

*失败:*

```
var account = _accountRepository.GetAccountByID(accountID);if (validateEmailAddress(account.emailAddress))
{
```

*通过:*

```
var account = _accountRepository.GetAccountByID(accountID);if (account == null) 
{
    return Error("Account not found");
}if (validateEmailAddress(account.emailAddress))
{
```

**9。不使用枚举**

枚举有助于提高可读性，并且有助于将来更有效地进行更改。

*失败:*

```
var contracts = GetContractsByStatus(2);
```

*通过:*

```
var contracts = GetContractsByStatus(ContractStatusEnum.Active);
```

**10。为可选参数**传入 NULL 时不使用命名参数

与 enum 示例一样，下面的代码确实有助于提高可读性。

*失败:*

```
var contracts = SearchContracts(null, lastWeek, null);
```

*通过:*

```
var contracts = SearchContracts(status: null, startDate: lastWeek, maxNumber: null);
```

**11。不明确的变量命名**

这可以适用于任何情况。方法，类。等等..

*失败:*

```
var x = false;
```

*通过:*

```
var isValid = false;
```

![](img/7af7c8bc71bd6e4736261c8fbf4601e3.png)

Photo by [Franki Chamaki](https://unsplash.com/@franki?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

**12。选择***

如果你曾经使用过数据库，你就会明白我的意思！尤其是当表格有很多列时。

*失败:*

```
SELECT*
FROM  acc.Account a
WHERE a.AccountID = [@AccountID](http://twitter.com/AccountID)
```

*通过:*

```
SELECT a.EmailAddress,
       a.StatusFK
FROM  acc.Account a
WHERE AccountID = [@AccountID](http://twitter.com/AccountID)
```

13。错误的 SQL 格式

SQL 不是最容易阅读的语言，随着查询变得更大更复杂，格式化对于可读性变得极其重要。每个人都有自己的风格，所以以我的经验来看，这可能会造成意见分歧。

*失败:*

```
SELECT id, FirstName, LASTNAME,c.nAme FROM people p left JOIN cities AS c on c.id=p.cityid;
```

*及格(可能)——更好:*

```
SELECT p.PersonId,
       p.FirstName,
       p.LastName,
       c.Name
FROM [dbo].[person]  p 
     LEFT JOIN [dbo].[city] c 
        ON p.CityId = c.CityId;
```

14。没有向源代码管理添加 SQL 脚本！

没有什么比当您创建一个发布计划，然后意识到 SQL 脚本在源代码控制中丢失更糟糕的了！在评论中抓住这些是至关重要的。

![](img/79ffbc0044188ad17c74a9776bee4993.png)

Photo by [Valery Sysoev](https://unsplash.com/@valerysysoev?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

15。内嵌 CSS

CSS 需要从 HTML 中分离出来，否则会很快变得混乱。

*失败:*

```
<div style="font-size:18px;color:#C8C8C8;padding:20px;margin-top:5px;">
```

*通过:*

*CSS:*

```
.header-wrapper {
  font-size: 18px;
  color:#C8C8C8;
  padding:20px;
  margin-top:5px;
}
```

*HTML:*

```
<div class="header-wrapper">
```

16 岁。Javascript 中没有使用= = =

如果你用 Javascript 编码过，你就会明白我的意思了！

*失败:*

```
if (isValid == true) {
```

*过关:*

```
if (isValid === false) {
```

**17。离开控制台日志**

通常，这些只是留在那里，因为开发人员需要它们进行调试！小心这些，因为它可能会导致向客户展示商业信息。

```
console.log(123);
console.log(error);
console.log("hello world");
```

**18。在类型脚本**中使用“任何”

您可以更改`tsconfig.json`文件，这样使用`any`类型会导致构建失败。你可以通过设置`"noImplictAny": true`来做到这一点

*失败:*

```
createNewUser(userDetails: any) {
```

*通过:*

```
createNewUser(userDetails: user) {
```

**19。利用！在 CSS 中覆盖样式很重要**

这很快就会变得令人困惑！当我看到太多`!imporant`时，通常意味着 HTML 或 CSS 有问题。

```
.active {
   font-size: 25px !important;
   color: green !important;
}
```

# 结论

失败的代码审查几乎是不可能停止的，这很好！为了创建一个可扩展的、易于理解的应用程序，关注代码中较小的细节是很重要的。

减少失败的代码审查的方法:

*   编码标准:对于您使用的每个技术堆栈
*   设计模式
*   沟通:让开发人员知道这一点很重要，这样他们就可以自己解决问题。
*   良好的源代码控制:我发现在像 TFS 这样的地方使用 GIT 更容易执行编码标准和审查变更。

## **用简单英语写的 JavaScript 的注释**

我们已经推出了三种新的出版物！请关注我们的新出版物:[**AI in Plain English**](https://medium.com/ai-in-plain-english)[**UX in Plain English**](https://medium.com/ux-in-plain-english)[**Python in Plain English**](https://medium.com/python-in-plain-english)**——谢谢，继续学习！**

**我们也一直有兴趣帮助推广高质量的内容。如果您有一篇文章想要提交给我们的任何出版物，请发送电子邮件至[**submissions @ plain English . io**](mailto:submissions@plainenglish.io)**，并附上您的 Medium 用户名，我们会将您添加为作者。另外，请让我们知道您想加入哪个/哪些出版物。****