# 角度安全最佳实践

> 原文：<https://javascript.plainenglish.io/angular-security-best-practices-54fadf8974a1?source=collection_archive---------3----------------------->

![](img/06132901ba901cf28a3780d48bc023eb.png)

Photo by [Bernard Hermant](https://unsplash.com/@bernardhermant?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

Angular 是谷歌制作的一个流行的前端框架。像其他流行的前端框架一样，它使用基于组件的架构来构建应用程序。

在本文中，我们将研究编写 Angular 应用程序时要遵循的一些安全最佳实践。

# 防止跨站点脚本编写(XSS)

跨站点脚本使攻击者能够将恶意代码注入网页。

为了防止这种情况，角度会清理插入到模板中的变量。

角度模板是可执行代码，所以任何潜在的恶意代码片段都必须被清除。

插值已清除，但`innerHtml`未清除。例如:

```
<p>{{htmlSnippet}}</p>
```

已消毒，但:

```
<p [innerHTML]="htmlSnippet"></p>
```

不是。

因此，使用`innerHTML`时要小心，防止带有`script`标记的字符串执行。

# 直接使用 DOM APIs 和显式清理调用

内置的 DOM APIs 不能自动保护我们免受安全漏洞的攻击。

`document`、`ElementRef`都有不安全的方法。如果我们使用它们，我们应该运行`DomSanitizer.sanitize`方法和适当的`SecurityContext`。

# 使用离线模板编译器

我们可以在生产环境中使用离线模板编译器来防止模板注射。

# 信任安全价值观

我们可以通过使用`DomSanitizer`来信任安全值，并调用以下方法之一:

*   `bypassSecurityTrustHtml`
*   `bypassSecurityTrustScript`
*   `bypassSecurityTrustStyle`
*   `bypassSecurityTrustUrl`
*   `bypassSecurityTrustResourceUrl`

信任动态设置的代码。

例如，我们可以编写以下代码，让我们在`href`属性中运行 JavaScript 代码:

`app.component.ts`:

```
import { Component } from "@angular/core";
import { DomSanitizer } from "@angular/platform-browser";@Component({
  selector: "app-root",
  templateUrl: "./app.component.html",
  styleUrls: ["./app.component.css"]
})
export class AppComponent {
  dangerousUrl;
  trustedUrl;
  constructor(private sanitizer: DomSanitizer) {
    this.dangerousUrl = 'javascript:alert("Hi")';
    this.trustedUrl = sanitizer.bypassSecurityTrustUrl(this.dangerousUrl);
  }
}
```

`app.component.html`:

```
<p><a class="e2e-trusted-url" [href]="trustedUrl">Click me</a></p>
```

当将`this.dangerousUrl`传递到`sanitizer.bypassSecurityTrustUrl`时，我们相信代码是安全的。

因此，当我们单击“单击我”时，我们会看到弹出的警告框，因为 JavaScript 代码没有经过杀毒。

# HTTP 级漏洞

Angular 内置了支持功能，有助于防止 2 个常见的 HTTP 漏洞。跨站点请求伪造(CSRF 或 XSRF)和跨站点脚本编写(XSS)。

跨站点请求伪造是一种攻击者假装是合法用户并代表该用户发出请求的攻击。

常见的反 CSRF 技术是在 cookie 中发送随机生成的身份验证令牌。客户端读取 cookie，并在所有后续请求中添加带有令牌的自定义请求头。

服务器将接收到的 cookie 值与请求标头值进行比较，如果值缺失或不匹配，则拒绝请求。

所有浏览器都执行同源策略。只有来自设置了 cookie 的网站的代码才能读取 cookie 并在网站上设置自定义标题。

Angular 的 HTTP 客户端可以从服务器接收 cookie，并在所有后续请求中添加自定义请求头。

![](img/10fb5ba1140e13a472489267392409f4.png)

Photo by [Rayner Simpson](https://unsplash.com/@rayner?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 跨站点脚本包含(XSSI)

这是旧浏览器上的一个漏洞，通过覆盖本机 JavaScript 对象构造函数并使用`script`标签包含 API URL。

只有当 JSON 有可执行的 JavaScript 代码时，它才是成功的。

Angular 的`HttpClient`自动从 JSON 中串出括号和圆括号来防止这种攻击。

# 结论

跨站点脚本攻击是指恶意脚本被注入到应用程序的代码中并运行。

这是通过消毒字符串来防止的。Angular 还提供了绕过净化的选项。

DOM 方法并不安全，所以我们在直接使用它们时应该意识到这一点。

交叉请求伪造是指攻击者通过伪装成合法用户来发出请求。

Angular 的`HttpClient`可以从服务器读取定制的 cookies，并发送一个带有唯一标识的请求头，该标识由服务器验证，以防止这种攻击。

我们可以通过净化 JSON 来防止可执行 JSON 字符串的执行，从而防止跨站点脚本包含攻击。

## **用简单英语写的 JavaScript 笔记**

我们已经推出了三种新的出版物！请关注我们的新出版物: [**AI in Plain English**](https://medium.com/ai-in-plain-english) ，[**UX in Plain English**](https://medium.com/ux-in-plain-english)，[**Python in Plain English**](https://medium.com/python-in-plain-english)**——谢谢，继续学习！**

**我们也一直有兴趣帮助推广高质量的内容。如果您有一篇文章想要提交给我们的任何出版物，请发送电子邮件至[**submissions @ plain English . io**](mailto:submissions@plainenglish.io)**，使用您的 Medium 用户名，我们会将您添加为作者。另外，请让我们知道您想加入哪个/哪些出版物。****