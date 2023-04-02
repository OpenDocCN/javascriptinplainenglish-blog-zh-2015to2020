# 通过使用异步管道避免 Angular 中的内存泄漏

> 原文：<https://javascript.plainenglish.io/avoid-memory-leaks-in-angular-by-using-the-async-pipe-95455470701e?source=collection_archive---------12----------------------->

## 如何使用异步管道避免 Angular 中可观察到的内存泄漏

![](img/45fbfec134230e14db35902944374efa.png)

[Photo by Luis Quintero from Pexels](https://www.pexels.com/photo/photo-of-gray-faucet-2339722/)

# 介绍

在本指南中，我将向您展示如何利用异步管道来避免由 Angular 应用程序中未订阅的可观测量导致的内存泄漏。

async operator 在订阅 Observables 时有几个优点，包括。

*   这很容易实现
*   干净和
*   自动取消订阅可观测量

Rxjs 可观测量很受欢迎，广泛用于角度应用。因此，知道如何避免由可观测量引起的内存泄漏是至关重要的。

未订阅的可观测量导致的内存泄漏会降低应用程序的性能，甚至会导致浏览器崩溃。

# 为什么取消订阅一个可观察的

在这个阶段，你可能想知道:我真的需要退订一个可观察的吗？简单的回答是:是啊！

*   如果您有一个无限的可观察对象，那么该可观察对象的订阅者将持续监听数据更改，如果不取消订阅，这将导致内存泄漏。
*   当组件被垃圾收集器销毁时，并不是所有的订阅者都会在此过程中被销毁。这可能导致内存泄漏。
*   最后，在订阅完成之前，组件可能会从 DOM 中删除，从而导致内存泄漏。

# 异步管道的工作原理

Angular 框架提供了异步管道，用于从异步原语(如 Observables 或 Promises)中解包数据。顾名思义，异步管道的使用方式与其他角形管道非常相似。

当在一些可观察类型上使用时，异步管道将订阅可观察类型并返回它的最新值。每当数据发生变化时，将检查组件的变化。

异步管道最好的一点是，每当组件被销毁时，它会自动取消对可观察对象的订阅。你不必做任何额外的事情。

# 异步管道入门

假设您有一个简单的服务，它通过 HTTP 从服务器获取一些更改日志数据。此类功能的服务类似于下面的服务:

***change-log . service . ts***

```
@Injectable({
    providedIn: 'root'
})export class GetLogsService { constructor(private httpClient: HttpClient ) { }
    private urlEndPoint: string = 'http://localhost:3000/api/change-log'; public getLogs(): Observable<any>{
         return this.httpClient.get(this.urlEndPoint);
    }}
```

为了让 HTTP 调用返回的数据进入组件，我们可以简单地创建一个 Observable 类型的属性。然后，在组件初始化时，使用服务方法调用分配 Observable 属性，如下所示:

```
import { Component, **OnInit** } from '@angular/core';
**import { Observable } from 'rxjs';**
**import { GetLogsService } from './get-logs.service';**@Component({
    selector: 'app-root',
    templateUrl: './app.component.html',
    styleUrls: ['./app.component.scss']
})export class AppComponent **implements OnInit** {
    title = 'rxjs-observables'; ** constructor(private getLogsService: GetLogsService){}** **public logs$: Observable<any>;

    ngOnInit(): void {
        this.logs$ = this.getLogsService.getLogs();
    }**
}
```

## 如何使用异步管道

和所有其他角度管道一样，异步管道主要用在 HTML 模板中。

为了显示所有数据，我们可以将它与 ngFor 循环一起使用，如下所示。其中，用户、日期和描述是返回数据的属性。

```
<div *ngFor="let log of logs$ | async ">
    {{ log.user }}
    {{ log.date }}
    {{ log.description }}
</div>
```

或者，我们可以结合 json 管道使用异步管道将所有原始数据输出到模板，如下所示:

```
<div>
    {{ logs$ | async | json }
</div>
```

# 最后的想法

异步管道是订阅和获取可观察数据的一个很好的选择，而不用担心内存泄漏。

内存泄漏会导致应用程序运行缓慢、无响应，甚至浏览器崩溃。

能够优雅地取消订阅 Observables 对于拥有高性能和高质量的应用程序非常重要。