# Angular HTTP 客户端—配置请求

> 原文：<https://javascript.plainenglish.io/angular-http-client-configuring-requests-9bf902671c7f?source=collection_archive---------5----------------------->

![](img/770d81b3092714eb3d838bb4f2bdf7a4.png)

Photo by [Bruce Jastrow](https://unsplash.com/@brucej6767?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

Angular 是 Google 制作的一个流行的前端框架。像其他流行的前端框架一样，它使用基于组件的架构来构建应用程序。

在本文中，我们将看看如何配置 Angular HTTP 客户端来发出我们想要的请求。

# URL 查询字符串

我们可以使用`HttpParams`类将 URL 查询字符串添加到我们的`HtttpRequest`中。

例如，我们可以编写以下代码来从 Unsplash API 获得响应:

`app.module.ts`:

```
import { BrowserModule } from "@angular/platform-browser";
import { NgModule } from "@angular/core";
import { HttpClientModule } from "@angular/common/http";
import { AppComponent } from "./app.component";@NgModule({
  declarations: [AppComponent],
  imports: [BrowserModule, HttpClientModule],
  providers: [],
  bootstrap: [AppComponent]
})
export class AppModule {}
```

`app.component.ts`:

```
import { Component } from "[@angular/core](http://twitter.com/angular/core)";
import { HttpClient, HttpParams } from "[@angular/common](http://twitter.com/angular/common)/http";[@Component](http://twitter.com/Component)({
  selector: "app-root",
  templateUrl: "./app.component.html",
  styleUrls: ["./app.component.css"]
})
export class AppComponent {
  constructor(private http: HttpClient) {} ngOnInit() {
    this.getPhotos();
  } getPhotos() {
    const options = {
      params: new HttpParams()
        .set("page", "1")
        .set(
          "client_id",
          "your_unsplash_api_key"
        )
        .set("query", "photo")
    }; this.http
      .get("https://api.unsplash.com/search/photos", options)
      .subscribe(res => console.log(res));
  }
}
```

在上面的代码中，我们创建了一个新的`HttpParams`对象并调用实例上的`set`方法来添加一个新的查询字符串参数。

然后我们用传递了`options`对象的`get`方法发出 GET 请求，设置查询字符串。

# 使用`fromString`创建 HttpParams

我们也可以从字符串中为`HttpParams`创建查询。

为此，我们可以编写以下代码:

`app.component.ts`:

```
import { Component } from "@angular/core";
import { HttpClient, HttpParams } from "@angular/common/http";@Component({
  selector: "app-root",
  templateUrl: "./app.component.html",
  styleUrls: ["./app.component.css"]
})
export class AppComponent {
  constructor(private http: HttpClient) {} ngOnInit() {
    this.getPhotos();
  } getPhotos() {
    const options = {
      params: new HttpParams({
        fromString:
          "page=1&client_id=your_unsplash_api_key&query=photo"
      })
    }; this.http
      .get("https://api.unsplash.com/search/photos", options)
      .subscribe(res => console.log(res));
  }
}
```

在上面的代码中，我们将整个查询字符串作为属性`fromString`的值传入。

# 去抖请求

我们可以使用 Rxjs `debouceTime`操作符将请求延迟我们指定的时间。

例如，我们可以写:

`app.component.ts`:

```
import { Component } from "@angular/core";
import { HttpClient, HttpParams } from "@angular/common/http";
import { debounceTime, distinctUntilChanged } from "rxjs/operators";@Component({
  selector: "app-root",
  templateUrl: "./app.component.html",
  styleUrls: ["./app.component.css"]
})
export class AppComponent {
  constructor(private http: HttpClient) {} ngOnInit() {
    this.getPhotos();
  } getPhotos() {
    const options = {
      params: new HttpParams({
        fromString:
          "page=1&client_id=your_unsplash_api_key&query=photo"
      })
    }; this.http
      .get("https://api.unsplash.com/search/photos", options)
      .pipe(
        debounceTime(500),
        distinctUntilChanged()
      )
      .subscribe(res => console.log(res));
  }
}
```

在上面的代码中，我们使用`pipe`方法应用`debounceTime`操作符将请求延迟 500 毫秒。

然后我们通过使用`distinctUntilChanged`操作符，从`get`可观察值中发出值。

# 监听进度事件

我们可以通过将`reportProgress`选项设置为`true`来监听请求的进度。

例如，我们可以监听如下进度事件:

`app.component.ts`:

```
import { Component } from "@angular/core";
import { HttpClient, HttpParams, HttpRequest } from "@angular/common/http";
import {
  debounceTime,
  distinctUntilChanged,
  last,
  tap,
  map
} from "rxjs/operators";@Component({
  selector: "app-root",
  templateUrl: "./app.component.html",
  styleUrls: ["./app.component.css"]
})
export class AppComponent {
  constructor(private http: HttpClient) {} ngOnInit() {
    this.getPhotos();
  } getPhotos() {
    const options = {
      params: new HttpParams({
        fromString:
          "page=1&client_id=your_unsplash_api_key&query=photo"
      }),
      reportProgress: true
    }; const req = new HttpRequest(
      "GET",
      "https://api.unsplash.com/search/photos",
      {},
      options
    ); this.http
      .request(req)
      .pipe(
        map(event => console.log(event)),
        tap(message => console.log(message)),
        last()
      )
      .subscribe(res => console.log(res));
  }
}
```

在上面的代码中，我们调用了`request`方法来发出请求。

该方法接受一个`HttpRequest`对象，该对象将 HTTP 请求方法作为第一个参数，第二个是 URL，第三个是有效负载，第四个是选项。

我们将`reportProgress`添加到选项对象中。

然后为了获得进度，我们使用`tap`操作符来记录进度。

我们会得到这样的结果:

```
{type: 3, loaded: 65536, total: 72147}
```

其中`loaded`是加载的字节数。`total`是加载的总字节数。

![](img/ea1556e1c1468bf68280adfa5dafa9d7.png)

Photo by [Noah Rosenfield](https://unsplash.com/@noah2199?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 安全性:XSRF 保护

跨站点请求伪造(XSRF)是一种攻击，攻击者欺骗经过身份验证的用户在不知情的情况下在网站上运行操作。

`HttpClient`有内置的 XSRF 保护机制来防止这些攻击。

它用 XSRF 令牌设置请求头中的`X-XSRF-TOKEN`。

服务器端应用程序可以验证 XSRF 令牌是否正确，这样攻击者就无法伪装成合法用户发出请求。

# 配置自定义 XSRF Cookie/标头名称

我们可以更改 XSRF cookie 或头名称。通过将`HttpClientXsrfModule.withOptions()`添加到我们应用程序的`NgModule`的`imports`数组中。

例如，我们可以写:

`app.module.ts`:

```
import { BrowserModule } from "@angular/platform-browser";
import { NgModule } from "@angular/core";
import { HttpClientModule, HttpClientXsrfModule } from "@angular/common/http";
import { AppComponent } from "./app.component";@NgModule({
  declarations: [AppComponent],
  imports: [
    BrowserModule,
    HttpClientModule,
    HttpClientXsrfModule.withOptions({
      cookieName: "My-Xsrf-Cookie",
      headerName: "My-Xsrf-Header"
    })
  ],
  providers: [],
  bootstrap: [AppComponent]
})
export class AppModule {}
```

在上面的代码中，我们将 XSRF cookie 的键改为`My-Xsrf-Cookie`，将 XSRF 头的键改为`My-Xsrf-Header`。

# 结论

我们可以使用`HttpParams`对象来设置请求的查询字符串。

为了消除请求的抖动，我们可以使用`debounceTime`操作符来延迟请求。

使用 Angular 的 HTTP 客户端，我们还可以通过将`reportProgress`选项设置为`true`来监听请求的进度。

Angular 的 HTTP 客户端内置了 XSRF 保护来保护请求。

## **用简单英语写的 JavaScript 笔记**

我们已经推出了三种新的出版物！请关注我们的新出版物:[**AI in Plain English**](https://medium.com/ai-in-plain-english)，[**UX in Plain English**](https://medium.com/ux-in-plain-english)，[**Python in Plain English**](https://medium.com/python-in-plain-english)**——谢谢，继续学习！**

**我们也一直有兴趣帮助推广高质量的内容。如果您有一篇文章想要提交给我们的任何出版物，请发送电子邮件至[**submissions @ plain English . io**](mailto:submissions@plainenglish.io)**，并附上您的 Medium 用户名，我们会将您添加为作者。另外，请让我们知道您想加入哪个/哪些出版物。****