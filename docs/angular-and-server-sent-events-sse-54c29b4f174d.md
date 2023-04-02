# 角度和服务器发送事件(SSE)

> 原文：<https://javascript.plainenglish.io/angular-and-server-sent-events-sse-54c29b4f174d?source=collection_archive---------2----------------------->

![](img/ab65f615e0d5bef21ff09abb94cf6d72.png)

在这篇文章中，我将向你展示如何在 [Angular](https://angular.io/) 应用中连接到[服务器发送的事件](https://developer.mozilla.org/en-US/docs/Web/API/Server-sent_events) (SSE)源。我们将创建一个小型原型，它将使用事件源 API 连接到服务器发送事件(SSE)端点，从而将事件打包到[可观察的](https://angular.io/guide/observables)中，并在[角度区域](https://angular.io/api/core/NgZone)内运行。

***服务器发送事件****(****SSE****)是一种* [*服务器推送*](https://en.wikipedia.org/wiki/Server_push) *技术，使客户端能够通过 HTTP 连接从服务器接收自动更新。服务器发送事件 EventSource API 由 W3C*[](https://en.wikipedia.org/wiki/World_Wide_Web_Consortium)**标准化为*[*html 5*](https://en.wikipedia.org/wiki/HTML5)*的一部分。——*[**维基百科**](https://en.wikipedia.org/wiki/Server-sent_events)*

*对于本教程，您将需要以下工具:*

*   *[Node.js](https://nodejs.org/en/) —我用的是 13.2.0 版本*
*   *角度 CLI —我使用 8.3.20 版本*

# *创建干净的角度项目*

*首先让我们创建一个干净的角度项目。从您的终端使用以下 Angular CLI 命令来执行此操作:*

```
*ng new angular-sse*
```

*此命令创建一个干净的项目并安装所有依赖项。幸运的是，这个项目不需要任何第三方 dep——Angular 提供了与服务器发送事件(SSE)交互所需的一切*

# *正在连接到服务器发送的事件(SSE)端点*

*接下来，进入项目目录(在我的例子中是 *angular-sse* ，并使用下面的终端命令创建一个新服务:*

```
*ng generate service sse*
```

*结果，`SseService`被创建并连接到角度项目中。现在，让我们写一些实际的代码。下面的代码片段是`SseService`类的完整代码:*

```
*import { Injectable, NgZone } from "@angular/core";
import { Observable } from "rxjs";@Injectable({
  providedIn: "root"
})
export class SseService {
  constructor(private _zone: NgZone) {} getServerSentEvent(url: string): Observable<any> {
    return Observable.create(observer => {
      const eventSource = this.getEventSource(url); eventSource.onmessage = event => {
        this._zone.run(() => {
          observer.next(event);
        });
      }; eventSource.onerror = error => {
        this._zone.run(() => {
          observer.error(error);
        });
      };
    });
  } private getEventSource(url: string): EventSource {
    return new EventSource(url);
  }
}*
```

*生成的服务创建了一个简洁易用的接口，用于与服务器发送的事件(SSE)进行交互。这里，我们统一了用于连接任何支持 SSE 的端点的逻辑。*

*原则上，该服务使用[事件源 API](https://developer.mozilla.org/en-US/docs/Web/API/EventSource) 连接到 SSE 端点，允许将其打包到`Observable`对象中。这个[可观测的](https://angular.io/guide/observables)然后在[角度区域](https://angular.io/api/core/NgZone)内运行。这允许 Angular 检测事件并正确执行底层逻辑。*

# *订阅可观察的*

*接下来，让我们使用`SseService` observable 创建一个订阅 SSE 端点的组件。与创建服务类似，为此使用 Angular CLI:*

```
*ng new component home*
```

*此外，我将使用这个新创建的`HomeComponent`来测试服务并连接到测试 SSE 端点。打开`home.component.ts`文件并插入以下内容:*

```
*import { Component, OnInit } from "@angular/core";
import { SseService } from "src/app/services/sse/sse.service";@Component({
  selector: "app-home",
  templateUrl: "./home.component.html",
  styleUrls: ["./home.component.scss"]
})
export class HomeComponent implements OnInit {
  constructor(private sseService: SseService) {} ngOnInit() {
    this.sseService
      .getServerSentEvent("http://localhost:8082/raw")
      .subscribe(data => console.log(data));
  }
}*
```

*上面的代码使用`SseService`连接到 SSE 端点(在我的例子中是*http://localhost:8082/raw*)。之后，事件被记录到控制台中，以便进行调试。*

# *摘要*

*总之，在 Angular 中使用服务器发送事件(SSE)非常简单，可以创建很酷的反应式应用程序。我希望这篇文章对你有用。如果有，不要犹豫，喜欢或分享这个帖子。此外，如果你愿意，你可以在我的社交媒体上关注我:)*