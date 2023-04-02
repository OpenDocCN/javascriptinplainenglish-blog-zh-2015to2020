# 使用 Angular HTTP 客户端发出请求

> 原文：<https://javascript.plainenglish.io/making-requests-with-the-angular-http-client-f9030c925206?source=collection_archive---------8----------------------->

![](img/7884d31150b8f8ba23d65363a8d77079.png)

Photo by [Sneha](https://unsplash.com/@sne5ha?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

Angular 是 Google 制作的一个流行的前端框架。像其他流行的前端框架一样，它使用基于组件的架构来构建应用程序。

在本文中，我们将看看如何使用 Angular `HttpClient`来发出 HTTP 请求。

# 设置

要使用 Angular HTTP 客户端，我们必须导入`HttpClientModule`。

我们可以这样做:

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

一旦我们这样做了，我们必须注入`HttpClient`模块作为依赖项，如下所示:

`app.component.ts`:

```
import { Component } from "@angular/core";
import { HttpClient } from "@angular/common/http";@Component({
  selector: "app-root",
  templateUrl: "./app.component.html",
  styleUrls: ["./app.component.css"]
})
export class AppComponent {
  constructor(private http: HttpClient) {}
}
```

# 提出请求

为了发出请求，我们通过调用`http`模块中可用的方法来调用请求方法。

例如，如果我们想发出一个 GET 请求，我们可以写如下:

```
import { Component } from "@angular/core";
import { HttpClient } from "@angular/common/http";@Component({
  selector: "app-root",
  templateUrl: "./app.component.html",
  styleUrls: ["./app.component.css"]
})
export class AppComponent {
  constructor(private http: HttpClient) {} ngOnInit() {
    this.getPost();
  } getPost() {
    this.http
      .get("https://jsonplaceholder.typicode.com/posts/1")
      .subscribe(res => console.log(res));
  }
}
```

在上面的代码中，我们用我们可以向其发出请求的 URL 调用了`get`方法。然后我们订阅带有`subscribe`的响应。

`res`已响应的数据。

如果我们有更大的应用程序，我们应该编写服务来保存 HTTP 请求代码，这样我们就可以在多个地方使用这些方法。

# 请求键入的响应

我们可以在响应中添加类型注释，这样我们就可以自动完成响应。

为此，我们可以通过运行以下命令来创建一个接口:

```
ng g interface post
```

然后在`post.ts`中，我们添加:

```
export interface Post {
    userId: number,
    id: number,
    title: string,
    body: string,
}
```

最后在`app.component.ts`中，我们写道:

```
import { Component } from "@angular/core";
import { HttpClient } from "@angular/common/http";
import { Post } from './post';@Component({
  selector: "app-root",
  templateUrl: "./app.component.html",
  styleUrls: ["./app.component.css"]
})
export class AppComponent {
  post: Post;
  constructor(private http: HttpClient) { } ngOnInit() {
    this.getPost();
  } getPost() {
    this.http
      .get<Post>("https://jsonplaceholder.typicode.com/posts/1")
      .subscribe(post => {
        this.post = post;
      });
  }
}
```

然后，我们确保我们的响应是类型`Post`。我们将`post`设置为类型`Post`，它仍然可以编译和运行。

我们将 JSON 转换为我们在传递到括号中的类型名中指明的数据类型。

# 阅读完整回复

我们可以使用`observe`选项获得完整响应，如下所示:

`app.component.ts`:

```
import { Component } from "[@angular/core](http://twitter.com/angular/core)";
import { HttpClient } from "[@angular/common](http://twitter.com/angular/common)/http";
import { Post } from './post';[@Component](http://twitter.com/Component)({
  selector: "app-root",
  templateUrl: "./app.component.html",
  styleUrls: ["./app.component.css"]
})
export class AppComponent {
  post: Post;
  headers;
  constructor(private http: HttpClient) { } ngOnInit() {
    this.getPost();
  } getPost() {
    this.http
      .get<Post>("https://jsonplaceholder.typicode.com/posts/1", { observe: 'response' })
      .subscribe(res => {
        const keys = res.headers.keys();
        this.headers = keys.map(key =>
          `${key}: ${res.headers.get(key)}`);
        this.post = { ...res.body };
      });
  }
}
```

在上面的代码中，我们使用带有对象中的`observe`的`'response'`值的`get`方法，并将其传递给第二个参数以获得完整的响应。

然后在`subscribe`回调中，我们用`res.header.keys()`得到响应头键，用`res.body`得到响应体。

![](img/237aac00eaf24a561e99bf5ba420c048.png)

Photo by [Benoit Gauzere](https://unsplash.com/@bgauzere?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 发出 JSONP 请求

要发出 JSONP 请求，我们必须将`HttpClientJsonpModule`添加到应用程序的模块中。然后我们调用`http.jsonp`方法来发出请求。

为了做到这一切，我们写道:

`app.module.ts`:

```
import { BrowserModule } from "@angular/platform-browser";
import { NgModule } from "@angular/core";
import { HttpClientModule, HttpClientJsonpModule } from "@angular/common/http";
import { AppComponent } from "./app.component";@NgModule({
  declarations: [AppComponent],
  imports: [BrowserModule, HttpClientModule, HttpClientJsonpModule],
  providers: [],
  bootstrap: [AppComponent]
})
export class AppModule { }
```

`app.component.ts`:

```
import { Component } from "@angular/core";
import { HttpClient } from "@angular/common/http";
import { Post } from './post';@Component({
  selector: "app-root",
  templateUrl: "./app.component.html",
  styleUrls: ["./app.component.css"]
})
export class AppComponent {
  post: Post;
  headers;
  constructor(private http: HttpClient) { } ngOnInit() {
    this.getPost();
  } getPost() {
    this.http.jsonp<Post>("https://jsonplaceholder.typicode.com/posts/1", 'callback')
      .subscribe(post => {
        this.post = post;
      })
  }
}
```

在上面的代码中，我们将`HttpClientJsonpModule`放入`AppModule`。

然后我们调用`http.jsonp`来发出我们想要的请求，并在订阅回调中得到响应。

# 结论

Angular 附带了一个 HTTP 客户端来发出 HTTP 请求。

我们必须添加`HttpClientModule`来使用我们应用程序模块中的客户端。

为了发出请求，我们调用请求方法，然后调用`subscribe`并在回调中获取数据。

默认情况下，它只发出响应正文，但是也可以配置它来获得整个响应。

## **用简单英语写的 JavaScript 笔记**

我们已经推出了三种新的出版物！ [**AI 说白了**](https://medium.com/ai-in-plain-english)[**UX 说白了**](https://medium.com/ux-in-plain-english)[**Python 说白了**](https://medium.com/python-in-plain-english) **。跟随你感兴趣的人继续你的成长——谢谢，继续学习！**

我们也一直有兴趣帮助推广高质量的内容。如果您有一篇文章想要提交给我们的任何出版物，请发送电子邮件至[**submissions @ plain English . io**](mailto:submissions@plainenglish.io)**，并附上您的 Medium 用户名，我们会将您添加为作者。另外，请让我们知道您想加入哪个/哪些出版物。**