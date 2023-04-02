# 使用 Angular HTTP 客户端发出更复杂的请求

> 原文：<https://javascript.plainenglish.io/making-more-complex-requests-with-the-angular-http-client-2ff621b5c61?source=collection_archive---------4----------------------->

![](img/92278069eb258d5d2caa7b3d95999944.png)

Photo by [Andrew Alexander](https://unsplash.com/@aa_photography?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

Angular 是 Google 制作的一个流行的前端框架。像其他流行的前端框架一样，它使用基于组件的架构来构建应用程序。

在本文中，我们将看看如何使用 Angular 的 HTTP 客户端发出更复杂的 HTTP 请求。

# 请求非 JSON 数据

我们可以用 Angular 的 HTTP 客户端请求非 JSON 数据。

为此，我们将`responseType`选项设置为`'text'`。我们可以这样做:

```
import { Component } from "@angular/core";
import { HttpClient } from "@angular/common/http";
import { tap } from "rxjs/operators";@Component({
  selector: "app-root",
  templateUrl: "./app.component.html",
  styleUrls: ["./app.component.css"]
})
export class AppComponent {
  constructor(private http: HttpClient) { } ngOnInit() {
    this.getText();
  } getText() {
    this.http
      .get("https://www.w3.org/TR/PNG/iso_8859-1.txt", { responseType: "text" })
      .pipe(tap(data => console.log(data), error => console.log(error)))
      .subscribe(res => console.log(res));
  }
}
```

在上面的代码中，我们传入了一个将`responseType`设置为`'text'`的对象。

然后我们使用`tap`操作符来查看得到的响应。

然后我们调用`subscribe`来获得响应并再次记录。

# 错误处理

如果有任何问题，我们可以处理由 HTTP 请求引起的错误。

为此，我们将一个错误处理程序作为第二个参数传递给`subscribe`，如下所示:

`app.component.html`:

```
import { Component } from "@angular/core";
import { HttpClient } from "@angular/common/http";
@Component({
  selector: "app-root",
  templateUrl: "./app.component.html",
  styleUrls: ["./app.component.css"]
})
export class AppComponent {
  constructor(private http: HttpClient) { }
  ngOnInit() {
    this.getPost();
  } getPost() {
    this.http
      .get("https://jsonplaceholder.typicode.com/posts/1")
      .subscribe(res => console.log(res), err => console.log(err));
  }
}
```

# 获取错误详细信息

我们可以通过创建一个错误处理程序来获得错误的细节，然后将它传递给`catchError`操作符，如下所示:

`app.component.ts`:

```
import { Component } from "@angular/core";
import { HttpClient, HttpErrorResponse } from "@angular/common/http";
import { catchError } from "rxjs/operators";
import { throwError } from 'rxjs';@Component({
  selector: "app-root",
  templateUrl: "./app.component.html",
  styleUrls: ["./app.component.css"]
})
export class AppComponent {
  constructor(private http: HttpClient) { }
  ngOnInit() {
    this.getPost();
  }
  getPost() {
    this.http
      .get("https://jsonplaceholder.typicode.com/posts/1")
      .pipe(
        catchError(this.handleError)
      );
  } private handleError(error: HttpErrorResponse) {
    if (error.error instanceof ErrorEvent) {
      console.error('An error occurred:', error.error.message);
    }
    else {
      console.error(
        `${error.status} ${error.error}`);
    }
    return throwError('error');
  }
}
```

在上面的代码中，我们有一个`handleError`方法，它接受一个`HttpErrorResponse`对象。

从中我们可以检查`error`的`error`属性是否是`ErrorEvent`的实例。

如果是，那么我们记录消息。

否则，我们记录状态代码和错误。在每种情况下，我们用`throwError`再次抛出错误。

# 再试

使用可观测量的好处之一是我们可以重试它们。

为此，我们可以使用 Rxjs `retry`运算符，如下所示:

`app.component.ts`:

```
import { Component } from "@angular/core";
import { HttpClient, HttpErrorResponse } from "@angular/common/http";
import { catchError, retry } from "rxjs/operators";
import { throwError } from 'rxjs';@Component({
  selector: "app-root",
  templateUrl: "./app.component.html",
  styleUrls: ["./app.component.css"]
})
export class AppComponent {
  constructor(private http: HttpClient) { }
  ngOnInit() {
    this.getPost();
  }
  getPost() {
    this.http
      .get("https://jsonplaceholder.typicode.com/posts/1")
      .pipe(
        retry(3),
        catchError(this.handleError)
      );
  } private handleError(error: HttpErrorResponse) {
    if (error.error instanceof ErrorEvent) {
      console.error('An error occurred:', error.error.message);
    }
    else {
      console.error(
        `${error.status} ${error.error}`);
    }
    return throwError('error');
  }
}
```

在上面的代码中，我们添加了`retry`操作符，并将重试次数作为参数传递给函数。

因此，当请求失败时，它最多会重试 3 次。

# HTTP 标题

我们可以用`HttpHeaders`构造函数发送消息头。

要使用它，我们只需传入一个带有标题键和值的对象。我们可以如下使用它:

`app.component.ts`:

```
import { Component } from "@angular/core";
import { HttpClient, HttpHeaders } from "@angular/common/http";@Component({
  selector: "app-root",
  templateUrl: "./app.component.html",
  styleUrls: ["./app.component.css"]
})
export class AppComponent {
  constructor(private http: HttpClient) { } ngOnInit() {
    this.getPost();
  } getPost() {
    const httpOptions = {
      headers: new HttpHeaders({
        "Content-Type": "application/json"
      })
    }; this.http
      .get("https://jsonplaceholder.typicode.com/posts/1", httpOptions)
      .subscribe(res => console.log(res));
  }
}
```

在上面的代码中，我们通过传递一个带有我们想要发送的请求头的对象来创建`HttpHeaders`对象。然后我们用 object 作为值设置`headers`属性。

然后我们将`httpOptions`作为`get`方法的第二个参数传入。

![](img/9a28425ee4f9186f74b11314104b00eb.png)

Photo by [Kristina Tripkovic](https://unsplash.com/@tinamosquito?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 提出发布请求

我们可以用与 GET 请求相似的方式发出 POST 请求。`post`方法将 URL 作为第一个参数，将请求体作为第二个参数，将其他请求选项作为第三个参数。

例如，我们可以编写以下代码来发出请求:

`app.component.ts`:

```
import { Component } from "@angular/core";
import { HttpClient, HttpHeaders } from "@angular/common/http";
import { Post } from "./post";@Component({
  selector: "app-root",
  templateUrl: "./app.component.html",
  styleUrls: ["./app.component.css"]
})
export class AppComponent {
  constructor(private http: HttpClient) {}
  ngOnInit() {
    this.addPost();
  } addPost() {
    const body = {
      userId: 1,
      title: "title",
      body: "body"
    }; const httpOptions = {
      headers: new HttpHeaders({
        "Content-Type": "application/json"
      })
    }; this.http
      .post<Post>(
        "https://jsonplaceholder.typicode.com/posts/",
        body,
        httpOptions
      )
      .subscribe(res => console.log(res));
  }
}
```

在上面的代码中，我们有`body`对象，它作为第二个参数传入，是 POST 请求的请求体。

那么第三个参数是带有`headers`的`httpOptions`。

# 结论

我们可以通过将`responseType`选项设置为`true`来获取非 JSON 数据。

为了处理错误，我们可以向`subscribe`方法传递一个错误处理程序。

为了获得错误的细节，我们可以使用带有回调函数的`catchError`操作符，该回调函数接受`error`对象，然后我们可以对它做任何我们想做的事情。

我们可以通过创建一个`HttpHeaders`对象并将它作为一个选项传入来为请求添加请求头。

发出 POST 请求类似于 GET 请求。唯一的区别是我们将一个 body 对象作为第二个参数传入。请求选项作为第三个参数传递给`post`方法。

## **用简单英语写的 JavaScript 笔记**

我们已经推出了三种新的出版物！请关注我们的新出版物:[**AI in Plain English**](https://medium.com/ai-in-plain-english)，[**UX in Plain English**](https://medium.com/ux-in-plain-english)，[**Python in Plain English**](https://medium.com/python-in-plain-english)**——谢谢，继续学习！**

**我们也一直有兴趣帮助推广高质量的内容。如果您有一篇文章想要提交给我们的任何出版物，请发送电子邮件至[**submissions @ plain English . io**](mailto:submissions@plainenglish.io)**，并附上您的 Medium 用户名，我们会将您添加为作者。另外，请让我们知道您想加入哪个/哪些出版物。****