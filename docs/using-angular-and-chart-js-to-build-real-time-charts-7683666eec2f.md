# 使用 Angular 和 Chart.js 构建实时图表

> 原文：<https://javascript.plainenglish.io/using-angular-and-chart-js-to-build-real-time-charts-7683666eec2f?source=collection_archive---------4----------------------->

![](img/1c59432e8b0732c71667e70eebbea730.png)

[Angular](https://angular.io/) 和 [Chart.js](https://www.chartjs.org/) 是创建任何数据可视化应用程序时的流行组合。第一个可以处理非常大的数据吞吐量，而后者能够实时渲染图形，这要归功于 Canvas API。在这篇文章中，我将指导你使用 Angular 和 Chart.js 创建一个实时图表

# 先决条件

在开始编写任何代码之前，请确保您具备以下条件:

*   [Node.js](https://nodejs.org/en/) —我用的是 13.2.0 版本
*   [角度 CLI](https://cli.angular.io/) —我用的是 8.3.20 版本
*   10 分钟的自由时间

# 创建新的角度项目

所需的第一步是创建一个新的角度项目。正如先决条件中提到的，我正在使用 Angular CLI 这样做，我强烈建议您也这样做。打开一个终端窗口，导航到所需的目录并执行命令:

```
ng new angular-charts --routing=true --styling=scss
```

该命令在同名目录下创建一个名为 *angular-charts* 的新角度项目。此外，我添加了两个可选标志— `routing`将路由器模块添加到应用程序中，`styling`设置所使用的样式表的扩展。

创建好项目后，在您选择的 IDE 中打开它——我将为此使用 Visual Studio 代码。

# 添加服务层*

本教程的下一步是添加服务图层。我用星号标记了这个步骤，因为它是可选的。如果您已经有一个，或者您不需要，那么可以跳过这一部分。

让我们从生成一个使用`Observable`访问实时数据源的服务开始这一部分。要生成服务，请使用以下命令:

```
ng generate service sse
```

执行该命令后，就会创建`SseService`，这就是服务层代码将被放置的位置。对于本教程，我使用 SSE 或服务器发送的事件数据源，你可以在这里找到[教程](https://bartoszgajda.com/2019/12/22/angular-and-server-sent-events-sse/)。如果你需要更多的解释，不要犹豫去读那个教程。为了避免在这篇文章中重复，我将只粘贴以下内容:

```
import { Injectable, NgZone } from "@angular/core";
import { Observable } from "rxjs";
@Injectable({
  providedIn: "root"
})
export class SseService {
  constructor(private _zone: NgZone) {}
  getServerSentEvent(url: string): Observable<any> {
    return Observable.create(observer => {
      const eventSource = this.getEventSource(url);
      eventSource.onmessage = event => {
        this._zone.run(() => {
          observer.next(event);
        });
      };
      eventSource.onerror = error => {
        this._zone.run(() => {
          observer.error(error);
        });
      };
    });
  }
  private getEventSource(url: string): EventSource {
    return new EventSource(url);
  }
}
```

# 挂钩图表. js

下一步是将 Chart.js 库挂接到我们的 Angular 项目中。有几种方法可以做到这一点，但是我将使用一个专用的包，名为 [**Ng2-Charts**](https://valor-software.com/ng2-charts/) 。这个包公开了一个更好的 API，同时保留了所有需要的功能。在我的例子中，我将以下依赖项添加到我的`package.json`文件中:

```
"chart.js": "^2.9.3",
"ng2-charts": "^2.3.0",
```

修改完`package.json`文件后，不要忘记根据您的包管理器运行`npm install`或`yarn`。

# 添加 HTML 模板

更进一步，我们必须添加一个 HTML 模板来呈现图表。在本教程中，您可以将它放在您喜欢的任何地方——代码是带有自定义属性的单个 HTML 标记，我们将在下一步中探讨。我将它放在一个名为`count-events.component.html`的组件 HTML 模板中。HTML 模板应该包括以下内容:

```
<canvas
    width="600"
    height="400"
    [datasets]="countEventsData"
    [chartType]="countEventsChartType"
    [labels]="countEventsLabels"
    [colors]="countEventsColors"
    [options]="countEventsOptions"
></canvas>
```

我已经把我的图表放在 count-events 文件夹中，因此所有的变量都加上了这些。在`canvas`标签中，我们指定了高度、宽度和变量配置，它们将被放在相应的`.ts`文件中。

# 配置 Chart.js

正如在上面的章节中提到的，我们将添加一些自定义配置到 Chart.js 图中。这个配置将被放在组件的 TypeScript 文件中，在我的例子中它被称为`count-events.component.ts`。

首先要设置的是`datasets`属性。这是一个容器，它将保存显示在绘图本身上的数据。这方面的代码应该如下所示:

```
countEventsData: ChartDataSets[] = [
  { data: [], label: "Number of Events", fill: false }
];
```

这个变量是一个数组，意味着在一个图上可以显示许多数据集。每个元素内部都有三个核心部分:

*   `data` -保存要在图表上显示的单个值的数组
*   `label` -数据集的标签
*   `fill` -配置选项设置数据集在图表上的外观

下一个配置是`chartType`属性。这是一个字符串，标记应该使用的图表类型。有各种各样的选项可用，包括线条、条形图、图表或饼图，但对于本教程，我们将坚持使用最简单的一行:

```
countEventsChartType = "line";
```

更进一步，必须设置`labels`属性。该元素设置 **X** 轴接收什么标签。然而，在我们的例子中，我们不想将它们设置为常量。我们希望能够根据输入的数据实时更新标签。因此，该属性被设置为空数组:

```
countEventsLabels: Label[] = [];
```

下一个属性是`colors`。名称本身可能是不言自明的，所以我将直接跳到代码:

```
countEventsColors: Color[] = [
    {
      borderColor: "#039BE5",
      pointBackgroundColor: "#039BE5"
    }
];
```

最后一点配置叫做`options`。这是所有可以设置的主要标志的中心配置点。可用选项的数量非常广泛，因此请参考 [Chart.js 文档](https://www.chartjs.org/docs/latest/general/options.html)获取完整的文档。在我们的例子中，我们只对删除动画感兴趣——这将优化图表，使它运行得更快。为此，请将以下内容粘贴到您的代码中:

```
countEventsOptions: ChartOptions = {
    animation: {
      duration: 0
    }
 };
```

# 连接服务和 Chart.js

本教程的最后一章将向您展示如何将服务和 Chart.js 粘合在一起。为了实现这一点，我们将在`count-events.component.ts`文件中实现几个函数。

我们从订阅数据源开始，在我们的例子中是一个`SseService`。这是在`ngOnInit`钩子中完成的，因此每当我们的组件被加载到应用程序中时，我们就连接到数据源。在这里，我们为端点创建一个`Subscription`并调用`pushEventToChartData`函数。

```
private countEventsSubscription$: Subscription;
ngOnInit() {
    this.countEventsSubscription$ = this.sseService
      .getServerSentEvent("http://localhost:8082/count-events")
      .subscribe(event => {
        let data = JSON.parse(event.data);
        this.pushEventToChartData(data);
      });
  }
```

前面提到的函数有一个简单的目的——它检查`datasets`是否达到了一个任意的限制(在本例中为 20 ),如果是，在将新的元素推入集合之前删除最后一个元素。记住一件事——如果添加或删除元素，必须对`datasets`集合和标签`collections`都这样做。它们都必须一直保持同步。

```
private pushEventToChartData(event: CountEvents): void {
    if (this.isChartDataFull(this.countEventsData, 20)) {
      this.removeLastElementFromChartDataAndLabel();
    }
    this.countEventsData[0].data.push(event.count);
    this.countEventsLabels.push(
      this.getLabel(event)
    );
  }
```

最后几段代码包括对 helper 函数的调用，可以在上面的代码片段中找到。第一个函数可以用来实现一些更漂亮的标签。第二个从`datasets`和`labels`集合中删除最后一个元素。第三个检查 a 集合是否达到了它的限制，在我的例子中我设置为 20。这些内容的片段如下:

```
private getLabel(event: CountEvents): string {
    return `${event.window}`;
  } private removeLastElementFromChartDataAndLabel(): void {
    this.countEventsData[0].data = this.countEventsData[0].data.slice(1);
    this.countEventsLabels = this.countEventsLabels.slice(1);
  } private isChartDataFull(chartData: ChartDataSets[], limit: number): boolean {
    return chartData[0].data.length >= limit;
  }
```

综上所述，`count-events.component.ts`文件的完整代码如下所示:

```
export class CountEventsComponent implements OnInit, OnDestroy {
  private countEventsSubscription$: Subscription;
  private eventsOnChartLimit = 20;
  countEventsChartType = "line";
  countEventsData: ChartDataSets[] = [
    { data: [], label: "Number of Events", fill: false }
  ];
  countEventsLabels: Label[] = [];
  countEventsColors: Color[] = [
    {
      borderColor: "#039BE5",
      pointBackgroundColor: "#039BE5"
    }
  ];
  countEventsOptions: ChartOptions = {
    animation: {
      duration: 0
    }
  }; constructor(private sseService: SseService) {} ngOnInit() {
    this.countEventsSubscription$ = this.sseService
      .getServerSentEvent("http://localhost:8082/count-events")
      .subscribe(event => {
        let data = JSON.parse(event.data);
        this.pushEventToChartData(data);
      });
  } private pushEventToChartData(event: CountEvents): void {
    if (this.isChartDataFull(this.countEventsData, 20)) {
      this.removeLastElementFromChartDataAndLabel();
    }
    this.countEventsData[0].data.push(event.count);
    this.countEventsLabels.push(
      this.getLabel(event)
    );
  } private getLabel(event: CountEvents): string {
    return `${event.window}`;
  } private removeLastElementFromChartDataAndLabel(): void {
    this.countEventsData[0].data = this.countEventsData[0].data.slice(1);
    this.countEventsLabels = this.countEventsLabels.slice(1);
  } private isChartDataFull(chartData: ChartDataSets[], limit: number): boolean {
    return chartData[0].data.length >= limit;
  } ngOnDestroy() {
    this.countEventsSubscription$.unsubscribe();
  }
}
```

本教程到此结束。使用 Angular 和 Chart.js 并不是火箭科学，拥有实时图表的好处是巨大的。

# 摘要

我希望这篇文章对你有用。如果有，不要犹豫，喜欢或分享这个帖子。此外，如果你愿意，你可以在我的社交媒体上关注我:)