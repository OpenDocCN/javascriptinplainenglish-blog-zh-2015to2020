# 有角度的小抄

> 原文：<https://javascript.plainenglish.io/angular-cheat-sheet-cddd2f295b2c?source=collection_archive---------0----------------------->

![](img/efa87d88cf2c154820aadd41c465d7aa.png)

Angular 是一个基于打字稿的开源 web 应用框架，用于构建基于 web 和移动的应用。在本文中，我们将通过解释它的一些核心 API 来介绍一些 angular 特性。你可以按照这个角度备忘单来构建你的项目。我们试图在这里涵盖角度 CLI、角度生命周期挂钩、角度路由等等。

# 角度 CLI

Angular 让我们能够使用他们的 CLI 做很多事情。您可以只使用 CLI 配置整个应用程序。以下是一些命令:

*   这个命令将使用 npm 把 Angular CLI 安装到我们的本地机器上。
*   `ng new <application name>`:这将使用`ng new`命令设置一个新的角度应用。
*   `ng new <application name> --prefix best`:这将创建一个新项目，并将项目前缀设置为 new。
*   `ng new --help`:返回所有可用的角度命令列表。
*   `ng lint my-app`:这个命令检查我们的整个应用程序是否有林挺警告。
*   如果有任何形式的林挺错误，这个命令将修复它。
*   这格式化了我们的整个代码库。
*   `ng lint my-app --help`:该命令返回所有可用的林挺命令列表。
*   `ng add <package name>`:这个命令将使用你的包管理器来下载新的依赖项，并用配置变更来更新项目。
*   `ng generate component <name>`:这将在我们的应用程序上创建一个新的组件。我们也可以使用`ng g c <name>`简写来做到这一点。
*   `ng g d <directive name>`:该命令角度指令。
*   `ng g s <service name>`:创建新的基于 Javascript 类的服务。
*   `ng g p <pipe name>`:生成新管道
*   `ng g cl <destination>`:这将在指定目录下创建一个新类。
*   `ng build`:构建用于生产的应用程序，并将其存储在`dist`目录中。
*   `ng serve -o`:通过使用任何端口 4200 或任何可用端口在浏览器中打开应用程序来服务应用程序。
*   `ng serve -ssl`:使用 ssl 服务应用程序

# 有角度的生命周期挂钩

Angular 中的一个组件有一个生命周期，它经历了从出生到死亡的许多不同阶段。我们可以挂钩到这些不同的阶段，以获得对我们的应用程序的一些非常细粒度的控制。这里你可以看到一些有角度的生命周期钩子。

*   `ngOnChanges`:每当其中一个输入属性改变时调用。
*   `ngOnInit`:在`ngOnChanges`完成后立即调用，调用一次。
*   `ngOnDestroy`:angular 销毁指令或组件前调用
*   `ngDoCheck`:每当运行变更检测时，就会调用这个函数。
*   `ngAfterContentInit`:在 Angular 执行任何内容投影到组件视图后调用*。*
*   `ngAfterContentChecked`:每次 Angular 的变化检测机制检查给定组件的内容时，都会调用这个函数。
*   `ngAfterViewInit`当组件的视图被完全初始化时，这个函数被调用。
*   `ngAfterViewChecked`:角度变化检测机制每次检查给定组件的视图时调用。

# 如何使用角钩

永远记住钩子在一个组件或目录中工作，所以在我们的组件中使用它们，我们可以这样做:

```
`class ComponentName {
    @Input('data') data: Data;
    constructor() {
        console.log(`new - data is ${this.data}`);
    }
    ngOnChanges() {
        console.log(`ngOnChanges - data is ${this.data}`);
    }
    ngOnInit() {
        console.log(`ngOnInit  - data is ${this.data}`);
    }
    ngDoCheck() {
        console.log("ngDoCheck")
    }
    ngAfterContentInit() {
        console.log("ngAfterContentInit");
    }
    ngAfterContentChecked() {
        console.log("ngAfterContentChecked");
    }
    ngAfterViewInit() {
        console.log("ngAfterViewInit");
    }
    ngAfterViewChecked() {
        console.log("ngAfterViewChecked");
    }
    ngOnDestroy() {
        console.log("ngOnDestroy");
    }
}
```

# 组件 DOM

Angular 自带 DOM 特性，可以从数据绑定和动态样式定义方面做很多事情。让我们来看看一些特性:
在我们深入研究这些特性之前，一个简单的 component.ts 文件是这样的:

```
import { Component } from '@angular/core';
@Component({
    // component attributes
    selector: 'app-root',
    templateUrl: './app.component.html',
    styleUrls: ['./app.component.less']
})
export class AppComponent {
    name: 'Sunil';
}
```

# 让我们来看看一些模板语法:

*   `Interpolation`:使用`{{data to be displayed}}`将显示 ts 文件的动态内容。
*   `<button (click)="callMethod()" ... />`:向按钮添加点击事件，调用 ts 文件中定义的方法
*   `<button *ngIf="loading" ... />`:向 to 元素添加条件。条件句必须听真值或假值。
*   `*ngFor="let item of items"`:遍历已定义的项目列表。把这想象成一个 for 循环。
*   `<div [ngClass]="{green: isTrue(), bold: itTrue()}"/>`:基于条件增加动态类。
*   `<div [ngStyle]="{'color': isTrue() ? '#bbb' : '#ccc'}"/>`:根据条件向模板添加动态样式

# 组件通信

从一个组件向另一个组件传递数据在 Angular。您可以在子组件和父组件之间、父组件和父组件之间传递数据:

*   `input()`:此方法有助于将值传递给子组件。

```
export class SampleComponent {
@Input() value: 'Some Data should go in here';
}
```

子组件在父组件中注册，如下所示:

```
<child-component [value]="data"></child-component>
```

*   `output()`:该方法向父组件发出事件。一组数据可以传递到发出的事件中，这使它成为从子节点向父节点传递数据的媒介:

要从子组件发出事件:

```
@Output() myEvent: EventEmitter < MyModel > = new EventEmitter();
calledEvt(item: MyModel) {
    this.myEvent.emit(item);
}
```

然后父组件监听该事件:

```
<parent-component 
(myEvent)="callMethod()"></parent-component>
```

# 角度路由

路由是 Angular 的另一个很酷的功能，通过 Angular 路由系统，我们可以浏览页面，甚至添加路由保护。

*   组件路由:通过定义路径和要呈现的组件，我们可以在应用程序中定义路由:

```
const routes: Routes = [ 
  { path: 'home', component:HomeComponent }, 
  { path: 'blog/:id', component: BlogPostCompoent }, 
  { path: '**', component: PageNotFoundComponent } 
];
```

为了使路由工作，将这个文件添加到您的`app.module.ts`文件中:

```
RouterModule.forRoot(routes)
```

在某些情况下，您希望跟踪路径中发生的情况，您可以添加此功能以在角度项目中启用跟踪:

```
RouterModule.forRoot(routes,{enableTracing:true})
```

要在 Angular 中浏览页面，我们可以使用`routerLink`属性，该属性接受我们要路由到的组件的名称:

```
<a routerLink="/home" routerLinkActive="active"> Crisis Center</a>
```

`routerLinkActive="active"`将在激活时向链接添加一个激活类。

# 书写路线保护

我们可以为路由认证定义保护。我们可以使用`CanActivate`类来做到这一点:

```
class AlwaysAuthGuard implements CanActivate {        
        canActivate() {
                return true;
        }
}
```

为了在我们的路线中使用这种机械防护，我们可以在这里定义它:

```
const routes: Routes = [
  { path: 'home', component:HomeComponent },
  { path: 'blog/:id', component: BlogPostCompoent,canActivate: [AlwaysAuthGuard],  },
    { path: '**', component: PageNotFoundComponent }
];
```

# 角度服务

当您可以处理 http 请求和在应用程序上播种数据时，Angular services 就派上了用场。他们专注于呈现数据，并将数据访问委托给服务。

```
@Injectable()
export class MyService {
    public users: Users[];
    constructor() { }
    getAllUsers() {
        // some implementation
    }
}
```

要在组件中使用此服务，请使用 import 语句导入它，然后在构造函数中注册它

```
import MyService from '<path>'
constructor(private UserService: MyService)
```

为了使事情变得简单，我们可以使用这个命令在 Angular 中生成一个服务

```
ng g s <service name>
```

# Http 服务

Angular 自带 http 服务，用于发出 http 请求。要使用它，您必须首先将它导入到您的根模块中:

```
import { HttpClientModule} from "@angular/common/http";
```

导入之后，我们现在可以在我们的服务中使用它来发出 http 请求:

```
import { Injectable } from '@angular/core';
import { HttpClient } from '@angular/common/http';
@Injectable({
    providedIn: 'root'
})
export class UserService {
    constructor(private http: HttpClient) { }
    getAllUsers() {
        return this.http.get(`${baseURL}admin/list-users`);
    }
}
```

# Http 拦截器

一个**拦截器**是一段代码，它被你的应用程序收到的每一个 **HTTP** 请求激活。把拦截器想象成 nodejs 中的中间件，http 请求通过这段代码传递。

要定义一个拦截器，在 src 目录中创建一个`http-interceptor.ts`文件，并添加如下内容:

```
import { Injectable } from '@angular/core';
import {
    HttpEvent,
    HttpInterceptor,
    HttpHandler,
    HttpRequest,
    HttpErrorResponse,
    HttpResponse
} from '@angular/common/http';
import { Observable } from 'rxjs';
import { tap } from 'rxjs/operators';
@Injectable({
    providedIn: 'root'
})
export class HttpConfigInterceptor implements HttpInterceptor {
    constructor() { }
    intercept(req: HttpRequest<any>, next: HttpHandler) {
        // Get the auth token from  localstorage.
        const authToken = localStorage.getItem('token');
        // Clone the request and replace the original headers with
        // cloned headers, updated with the authorization.
        const authReq = req.clone({
            headers: req.headers.set('Authorization', authToken)
        });
        // send cloned request with header to the next handler.
        return next.handle(authReq);
    }
}
```

这是一个简单的拦截器，它检查用户的设备 localstorage 中是否有令牌。如果用户这样做，它将在所有 http 头中传递令牌。

# 管道

Angular 中的管道让我们能够将数据转换成任何特定的格式。例如，您可以编写一个简单的管道，将整数格式化为货币格式，或者将日期格式化为任何格式。
Angular 附带了一些内置管道，比如日期和货币管道。

我们也可以这样定义我们自己的定制管道:

```
import { Pipe, PipeTransform } from '@angular/core';@Pipe({ name: 'exponentialStrength' })
export class ExponentialStrengthPipe implements PipeTransform {
    transform(value: number, exponent?: number): number {
        return Math.pow(value, isNaN(exponent) ? 1 : exponent);
    }
}
```

要在组件中使用管道，我们可以这样做:

```
{{power | exponentialStrength: factor}}
```

# WrapPixel 的自由角度模板

好吧，我希望你喜欢我们的 Angularjs 备忘单，并了解了 Angular Lifecycle 钩子、路由等等。这将有助于开发您的项目。同样，角度模板也可以帮助你的项目。因为它们有令人惊叹的设计界面和现成的组件，可以节省您的时间和金钱。你也可以通过 WraPixel 找到最好的免费角度模板。你可以零投资下载并在你的个人和商业项目中使用它。