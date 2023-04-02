# Chart.js 混合图表类型和轴选项

> 原文：<https://javascript.plainenglish.io/chart-js-mixed-chart-types-and-axes-options-87f03053e846?source=collection_archive---------4----------------------->

![](img/5849bd5ec3f01f064872674d922a8197.png)

Photo by [Markus Spiske](https://unsplash.com/@markusspiske?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

我们可以使用 Chart.js 使在网页上创建图表变得容易。

在本文中，我们将了解如何使用 Chart.js 创建图表。

# 混合图表类型

使用 Chart.js，我们可以在一个图表中有多种图表类型。

例如，我们可以写:

```
var ctx = document.getElementById('myChart').getContext('2d');
var myChart = new Chart(ctx, {
  type: 'bar',
  data: {
    datasets: [{
      label: 'Bar Dataset',
      data: [10, 20, 30, 40],
      backgroundColor: 'green'
    }, {
      label: 'Line Dataset',
      data: [50, 50, 50, 50],
      type: 'line',
      borderColor: 'blue'
    }],
    labels: ['January', 'February', 'March', 'April']
  },
});
```

我们将`type`设置为`'line'`,以便在同一个图表中有一个折线图和一个条形图。

# 绘图顺序

它们的绘制顺序也可以改变。

绘制数据集时，第一个数据集在最上面。

我们可以通过设置`order`属性来改变这一点:

```
var ctx = document.getElementById('myChart').getContext('2d');
var myChart = new Chart(ctx, {
  type: 'bar',
  data: {
    datasets: [{
      label: 'Bar Dataset',
      data: [10, 20, 30, 40],
      backgroundColor: 'green',
      order: 2
    }, {
      label: 'Line Dataset',
      data: [20, 20, 20, 20],
      type: 'line',
      borderColor: 'blue',
      order: 1
    }],
    labels: ['January', 'February', 'March', 'April']
  },
});
```

# 笛卡尔轴

笛卡尔轴用于折线图、条形图和气泡图。

默认情况下，Chart.js 中包含 4 个笛卡尔轴。

它们是线性、对数、类别和时间。

# 轴 ID

我们可以设置轴 ID 来设置轴的 ID。

例如，我们可以写:

```
var ctx = document.getElementById('myChart').getContext('2d');
var myChart = new Chart(ctx, {
  type: 'line',
  data: {
    datasets: [{
      label: 'Line 1',
      data: [10, 20, 30, 40],
      backgroundColor: 'green',
      yAxisID: 'first-y-axis'
    }, {
      label: 'Line 2',
      data: [20, 20, 20, 20],
      backgroundColor: 'blue',
      yAxisID: 'second-y-axis'
    }],
    labels: ['January', 'February', 'March', 'April']
  },
  options: {
    scales: {
      yAxes: [{
        id: 'first-y-axis',
        type: 'linear'
      }, {
        id: 'second-y-axis',
        type: 'linear'
      }]
    }
  }
});
```

我们在`options`对象中设置轴的`id`来设置要显示的轴。

# 范畴笛卡尔轴

我们可以改变笛卡尔坐标轴的范畴。

我们可以更改要显示的最小和最大项目。

例如，我们可以写:

```
var ctx = document.getElementById('myChart').getContext('2d');
var myChart = new Chart(ctx, {
  type: 'line',
  data: {
    datasets: [{
      data: [10, 20, 30, 40, 50, 60]
    }],
    labels: ['January', 'February', 'March', 'April', 'May', 'June']
  },
  options: {
    scales: {
      xAxes: [{
        ticks: {
          min: 'March'
        }
      }]
    }
  }
});
```

那么最左边的项目是`'March'`，不显示`'January'`和`'February'`。

# 线性笛卡尔轴

我们可以更改线性笛卡尔轴中的设置，笛卡尔轴是显示数字数据的轴。

例如，我们可以写:

```
var ctx = document.getElementById('myChart').getContext('2d');
var myChart = new Chart(ctx, {
  type: 'line',
  data: {
    datasets: [{
      label: 'First dataset',
      data: [0, 20, 40, 50]
    }],
    labels: ['January', 'February', 'March', 'April']
  },
  options: {
    scales: {
      yAxes: [{
        ticks: {
          suggestedMin: 50,
          suggestedMax: 100
        }
      }]
    }
  }
});
```

我们设置`suggestedMin`和`suggestedMax`属性来设置最小值和最大值，并调整以适应这些值。

如果数据集中的值低于 50 或高于 100，它们仍会显示。

# 结论

我们可以用 Chart.js 为轴设置各种选项。

喜欢这篇文章吗？如果有，通过 [**订阅我们的 YouTube 频道**](https://www.youtube.com/channel/UCtipWUghju290NWcn8jhyAw?sub_confirmation=true) **获取更多类似内容！**