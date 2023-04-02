# Chart.js 标题和图例

> 原文：<https://javascript.plainenglish.io/chart-js-titles-and-legends-4a53a892384b?source=collection_archive---------8----------------------->

![](img/f4106fc8f6fe5df91ac320ea2a9946ce.png)

Photo by [Victor Garcia](https://unsplash.com/@victor_g?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

我们可以使用 Chart.js 使在网页上创建图表变得容易。

在本文中，我们将了解如何使用 Chart.js 创建图表。

# 图例项选项

我们可以更改许多选项来配置图例。

`text`有标签文本。

`fillStyle`具有图例框的填充样式。

`lineCap`是一个带有边框的字符串 CSS。

`lineDash`是画布框边框的数字数组。

`lineDashOffset`有画布框边界偏移。

`lineJoin`拥有画布上下文`lineJoin`属性值。

`lineWidth`具有框边框的宽度。

`strokeStyle`具有图例笔画的颜色。

`pointStyle`具有图例框的点样式。

`rotation`有以度为单位的点的旋转。

例如，我们可以通过编写以下内容来链接两个数据集的显示:

```
var defaultLegendClickHandler = Chart.defaults.global.legend.onClick;
var newLegendClickHandler = function(e, legendItem) {
  var index = legendItem.datasetIndex;
  if (index > 1) {
    defaultLegendClickHandler(e, legendItem);
  } else {
    let ci = this.chart;
    [
      ci.getDatasetMeta(0),
      ci.getDatasetMeta(1)
    ].forEach(function(meta) {
      meta.hidden = meta.hidden === null ? !ci.data.datasets[index].hidden : null;
    });
    ci.update();
  }
};var ctx = document.getElementById('myChart').getContext('2d');
var myChart = new Chart(ctx, {
  type: 'bar',
  data: {
    labels: ['Red', 'Blue', 'Yellow'],
    datasets: [{
      label: '# of Votes',
      data: [12, 19, 3],
      borderWidth: 1
    }, {
      label: '# of Votes',
      data: [10, 19, 3],
      borderWidth: 1
    }]
  },
  options: {
    legend: {
      onClick: newLegendClickHandler
    },
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

`else`块在两个数据集上调用`update`。

如果一个没有被隐藏，如

```
meta.hidden === null
```

那么其他条也将显示为:

```
!ci.data.datasets[index].hidden
```

我们调用`ci.update`来更新图表。

然后我们使用这个函数作为`options.legend.onClick`属性的值，如果我们单击图例，就可以打开和关闭这两个条。

# HTML 图例

我们还可以添加`legendCallback`属性来呈现一个 HTML 图例。

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
        'rgba(255, 206, 86, 1)',
      ],
      borderWidth: 1
    }]
  },
  options: {
    legendCallback(chart) {
      //...
    },
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

# 标题

我们可以改变图表标题在图表顶部的呈现方式。

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
        'rgba(255, 206, 86, 1)',
      ],
      borderWidth: 1
    }]
  },
  options: {
    title: {
      display: true,
      text: 'Chart Title'
    },
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

设置`options.title.text`属性来设置选项文本。

# 结论

我们可以用一些属性来改变标题和图例选项。

喜欢这篇文章吗？如果有，通过 [**订阅我们的 YouTube 频道**](https://www.youtube.com/channel/UCtipWUghju290NWcn8jhyAw?sub_confirmation=true) **获取更多类似内容！**