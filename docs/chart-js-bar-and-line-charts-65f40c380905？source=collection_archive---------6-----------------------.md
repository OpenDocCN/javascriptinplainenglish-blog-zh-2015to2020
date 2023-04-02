# Chart.js 条形图和折线图

> 原文：<https://javascript.plainenglish.io/chart-js-bar-and-line-charts-65f40c380905?source=collection_archive---------6----------------------->

![](img/524f9ce222869f133adf8230bf184697.png)

Photo by [Patrick Tomasso](https://unsplash.com/@impatrickt?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

我们可以使用 Chart.js 使在网页上创建图表变得容易。

在本文中，我们将了解如何使用 Chart.js 创建图表。

# 弧形结构

我们可以更改极区图、圆环图和饼图的弧形选项。

属性都在`Chart.defaults.global.elements.arc`对象中。

例如，我们可以写:

```
Chart.defaults.global.elements.arc.backgroundColor = 'green';var ctx = document.getElementById('myChart').getContext('2d');
var myChart = new Chart(ctx, {
  type: 'polarArea',
  data: {
    datasets: [{
      data: [10, 20, 30]
    }],
    labels: [
      'Red',
      'Yellow',
      'Blue'
    ]
  }
});
```

我们将`Chart.defaults.global.elements.arc.backgroundColor`属性设置为条形的颜色。

极区图现在所有的切片都是绿色的。

其他选项包括描边对齐、边框颜色和边框宽度。

# 折线图

我们可以通过设置几个选项来创建折线图。

要添加折线图，我们通过编写以下内容来创建画布:

```
<canvas id="myChart"></canvas>
```

在 HTML 中。

然后在 JavaScript 代码中，我们编写:

```
var ctx = document.getElementById('myChart').getContext('2d');
var myChart = new Chart(ctx, {
  type: 'line',
  data: {
    labels: ['Monday', 'Tuesday', 'Wednesday'],
    datasets: [{
      label: '# of Votes',
      data: [12, 19, 3],
      borderWidth: 1
    }]
  },
  options: {
    scales: {
      yAxes: [{
        ticks: {
          beginAtZero: true
        }
      }]
    }
  }
});
```

我们可以在`datasets`条目中设置很多属性。

其中一些包括:

*   `backgrounrColor` —折线图的背景色
*   `borderColor` —线条颜色
*   `borderDash` —破折号的长度和间距
*   `borderWidth` —线宽
*   `label` —数据集标签
*   `order` —绘图顺序
*   `pointStyle` —点样式。

还有很多。

还可以使用各种选项来更改点和线的样式。

# 堆积面积图

我们可以将多个折线图堆叠在一起。

为了补充说明，我们写道:

```
var ctx = document.getElementById('myChart').getContext('2d');
var myChart = new Chart(ctx, {
  type: 'line',
  data: {
    labels: ['Monday', 'Tuesday', 'Wednesday'],
    datasets: [{
      label: '# of Apples',
      data: [12, 19, 3],
      borderWidth: 1,
      borderColor: 'red'
    }, {
      label: '# of Oranges',
      data: [20, 13, 5],
      borderWidth: 1,
      borderColor: 'blue'
    }]
  },
  options: {
    scales: {
      yAxes: [{
        stacked: true,
        ticks: {
          beginAtZero: true
        }
      }]
    }
  }
});
```

我们将`stacked`属性设置为`true`,使两条线相互堆叠。

设置`borderColor`以区分它们。

# 条形图

我们可以创建条形图，将值显示为竖条。

例如，我们可以写:

```
var ctx = document.getElementById('myChart').getContext('2d');
var myChart = new Chart(ctx, {
  type: 'bar',
  data: {
    labels: ['Red', 'Blue', 'Yellow'],
    datasets: [{
      label: '# of Votes',
      data: [12, 19, 3],
      backgroundColor: [
        'rgba(255, 99, 132, 0.2)',
        'rgba(54, 162, 235, 0.2)',
        'rgba(255, 206, 86, 0.2)',
      ],
      borderColor: [
        'rgba(255, 99, 132, 1)',
        'rgba(54, 162, 235, 1)',
        'rgba(255, 206, 86, 1)'
      ],
      borderWidth: 1
    }]
  },
  options: {
    scales: {
      yAxes: [{
        ticks: {
          beginAtZero: true
        }
      }]
    }
  }
});
```

我们可以用`backgroundColor`来设置条形的背景颜色。

`borderColor`设置条形的边框颜色。

`borderWidth`设置边框宽度。

`labels`有 x 轴标签。

`label`有图例标签。

`data`有 y 坐标值。

其他选项包括条形厚度、填充类别的百分比和最大条形厚度:

```
var ctx = document.getElementById('myChart').getContext('2d');
var myChart = new Chart(ctx, {
  type: 'bar',
  data: {
    labels: ['Red', 'Blue', 'Yellow'],
    datasets: [{
      barPercentage: 0.5,
      barThickness: 6,
      maxBarThickness: 8,
      label: '# of Votes',
      data: [12, 19, 3],
      backgroundColor: [
        'rgba(255, 99, 132, 0.2)',
        'rgba(54, 162, 235, 0.2)',
        'rgba(255, 206, 86, 0.2)',
      ],
      borderColor: [
        'rgba(255, 99, 132, 1)',
        'rgba(54, 162, 235, 1)',
        'rgba(255, 206, 86, 1)'
      ],
      borderWidth: 1
    }]
  },
  options: {
    scales: {
      yAxes: [{
        ticks: {
          beginAtZero: true
        }
      }]
    }
  }
});
```

`barPercentage` 和`categoryPercentage`出现如下:

```
// categoryPercentage: 1.0
// barPercentage: 1.0
Bar:        | 1.0 | 1.0 |
Category:   |    1.0    |
Sample:     |===========|

// categoryPercentage: 1.0
// barPercentage: 0.5
Bar:          |.5|  |.5|
Category:  |      1.0     |
Sample:    |==============|

// categoryPercentage: 0.5
// barPercentage: 1.0
Bar:            |1.||1.|
Category:       |  .5  |
Sample:     |==============|
```

# 结论

我们可以改变一些图的弧结构。

同样，我们可以用 Chart.js 添加条形图和折线图。

喜欢这篇文章吗？如果是这样，通过 [**订阅我们的 YouTube 频道**](https://www.youtube.com/channel/UCtipWUghju290NWcn8jhyAw?sub_confirmation=true) **获取更多类似内容！**