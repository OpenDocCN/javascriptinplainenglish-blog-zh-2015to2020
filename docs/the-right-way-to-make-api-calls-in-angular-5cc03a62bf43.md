# 在 Angular 中进行 API 调用的正确方法

> 原文：<https://javascript.plainenglish.io/the-right-way-to-make-api-calls-in-angular-5cc03a62bf43?source=collection_archive---------0----------------------->

![](img/0a1fb78774653fa9af9a52bb8fcd246d.png)

## 如何使用 **HTTPClient** 库做到这一点？

现在，几乎所有的前端应用程序都使用 HTTP 协议与后端服务进行通信。现代浏览器可以通过两种方式发出 HTTP 请求— ***XMLHttpRequest*** 接口和 ***fetch()*** API。

**Angular** 提供了 HttpClient 模块，允许开发者发送 HTTP **请求**并对远程 HTTP 服务器进行 **API 调用**。

> HttpClient 是一个可注入的类，带有执行 HTTP 请求的方法。每个请求方法都有多个签名，返回类型根据被调用的签名而变化(主要是 observe 和 responseType 的值)。”—棱角分明。超正析象管(Image Orthicon)

它在***@ angular/common/HTTP***模块中提供，提供了基于浏览器公开的***XMLHttpRequest***接口的简化客户端 **HTTP API** 。

*使用这个 api 有很多好处，值得一提的有—*

*   *简单测试*
*   *键入请求&响应对象*
*   *请求和响应拦截*
*   *API 可观察支持*
*   *简单的错误处理*

# 使用

在 app.module.ts 文件中:

```
import { HttpClientModule } from '[@angular/common](http://twitter.com/angular/common)/http';@NgModule({
  declarations: [
    ////....
  ],
  imports: [
    // Other Modules...,
    HttpClientModule    
  ],
  providers: [],
  bootstrap: [AppComponent]
})export class AppModule { }
```

一旦***http client module***被导入到 **AppModule** 中，我们就可以很容易地将 **HTTPClient** 注入到 data.service.ts 文件中如下所示的应用程序类中。

```
import { Injectable } from '@angular/core';
import { HttpClient } from '@angular/common/http';@Injectable()
export class DataService {
  constructor(private httpClient: HttpClient) { }getPosts() {
    return this.httpClient
    .get('https://jsonplaceholder.typicode.com/posts')
  }
}
```

# 键入的响应

我们还可以配置 **HTTPClient** 请求来期望类型化的响应对象。这将使用户更容易、更明显地消费输出。

```
getEntities(): Observable<Entity[]> {
    return this.httpClient.get<Entity[]>(this.apiUrl)     
}
```

## 不支持 CORS 时进行服务器调用:

当服务器不支持 **CORS** 时，我们也可以使用 HttpClient 模块进行 Http 调用。

在 Angular 中， ***JSONP*** 请求返回一个可观察值。这里，我们可以首先订阅 observables，然后使用 **RxJS** map 操作符来转换响应。一旦完成，我们就可以使用异步管道来管理响应结果。

要使用 ***JSONP*** ，我们必须先将***HttpClientJsonpModule***包含在 NgModule 导入中。

```
getPosts() : Observable {
    return this.httpClient.jsonp('apiURL', 'callback').pipe(
      catchError(this.handleError('searchHeroes', [])) 
      // then handle the errorResponse
   );
}
```

# HttpClient 中的错误处理

![](img/c9e51c73bd2898c4d5c3e42093ba8b35.png)

在任何应用程序中，我们都需要处理两种类型的错误——服务器端和客户端。

**服务器后端**可能会拒绝这个请求，返回一个带有状态代码的 HTTP 响应，比如 404 或 500。

**客户端**错误可以像网络错误一样导致请求未成功完成，或者在 ***RxJS*** 操作符中抛出任何异常。这些异常或错误的结果将是 JavaScript***error event***对象。

HttpClient api 的一个好处是，它可以捕获其***HttpErrorResponse***中的两种错误，我们可以使用响应来找出问题的真正原因。

*   **创建错误处理程序**

```
/**
 * Handle Http operation that failed.
 * Let the app continue.
 * @param operation - name of the operation that failed
 * @param result - optional value to return as the observable result
 */**private handleError(errorResponse: HttpErrorResponse)** {
    if (errorResponse.error instanceof ErrorEvent) {
      // A client-side or network error occurred. Handle it   accordingly.
      console.error('An error occurred:',   errorResponse.error.message);
    } 
   else {
      // The backend returned an unsuccessful response code.
      // The response body may contain clues as to what went wrong,
      console.error(
        'Backend returned code ${errorResponse.status}, '+
        'body was: ${errorResponse.error}');
    } // return an observable with a user-facing error message
    return throwError(
      'Error Occurred; please try again later.');
  };
```

在上面的例子中，我们返回了***RxJS******error observable***，这样服务消费者就可以很容易地处理它。

```
getEntities(): Observable<Entity[]> {
    return this.httpClient.get<Entity[]>(this.apiUrl)
     .pipe(
      catchError(this.**handleError**)
     ); 
  }
```

*   **更通用的方法**

```
/**
 * Handle Http operation that failed.
 * Let the app continue.
 * @param operation - name of the operation that failed
 * @param result - optional value to return as the observable result
 */
  **private handleError<T>(operation = 'operation', result?: T)** {
    return (error: any): Observable<T> => {// TODO: send the error to remote logging infrastructure
      console.error(error); // log to console instead// TODO: better job of transforming error for user consumption
      this.log(`${operation} failed: ${error.message}`);// Let the app keep running by returning an empty result.
      return of(result as T);
    };
  }
```

现在使用上面的方法如下-

```
getEntities(): Observable<Entity[]> {
    return this.httpClient.get<Entity[]>(this.apiUrl)
     .pipe(      
   catchError(this.**handleError**<Entity>('getEntities'))
     ); 
  }
```

*   **我们也可以使用**[***http interceptor***](https://angular.io/api/common/http/HttpInterceptor#description)**来处理错误。**使用这个，我们还可以检查并转换从应用程序到服务器的 HTTP 请求。

```
import { HttpInterceptor, HttpRequest, HttpHandler, HttpEvent, HttpErrorResponse } from '@angular/common/http';
```

拦截器还可以在一个例程中为每个 HTTP 请求/响应执行多个*隐式*任务，从认证到日志。在这篇文章中，我不会详细讨论它的实现，我会试着写另一篇文章。

# 重试请求

有些情况下，重试 **HttpRequest** 会导致成功完成，就像大多数网络错误一样。

RxJS 库提供了几个我们可以使用的重试操作符。最简单的是 ***retry()*** 我们可以用它来自动重新订阅一个失败的可观察对象，指定次数。

一旦我们重新订阅了一个 **HttpClient** 方法的结果，它将自动重新发出 HTTP 请求。

```
getEntities(): Observable<Entity[]> {
    return this.httpClient.get<Entity[]>(this.apiUrl)
     .pipe(
 **     retry(2), // retry a failed request up to 2 times**
      catchError(this.handleError<Entity>('getEntities'))
     ); 
  }
```