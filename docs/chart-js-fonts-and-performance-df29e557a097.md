# Chart.js 字体和性能

> 原文：<https://javascript.plainenglish.io/chart-js-fonts-and-performance-df29e557a097?source=collection_archive---------9----------------------->

![](img/0451786ce27082f45bcaf1fc38e9076e.png)

Photo by [Marc-Olivier Jodoin](https://unsplash.com/@marcojodoin?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

我们可以使用 Chart.js 使在网页上创建图表变得容易。

在本文中，我们将了解如何使用 Chart.js 创建图表。

# 字体

我们可以通过设置`options.legend.labels.fontColor`属性来改变字体设置。

例如，我们可以写:

```
Chart.defaults.global.defaultFontColor = 'red';var ctx = document.getElementById('myChart').getContext('2d');
var myChart = new Chart(ctx, {
  type: 'bar',
  data: {
    labels: ['Red', 'Blue', 'Yellow'],
    datasets: [{
      label: '# of Votes',
      data: [2, 19, 3],
      backgroundColor: [
        'rgba(255, 99, 132, 1)',
        'rgba(54, 162, 235, 1)',
        'rgba(255, 206, 86, 1)',
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
    scales: {
      yAxes: [{
        ticks: {
          beginAtZero: true
        }
      }]
    },
    legend: {
      labels: {
        fontColor: 'green'
      }
    }
  }
});
```

使用`Chart.defaults.global.defaultFontColor`属性全局改变颜色。

我们用`options.legend.labels.fontColor`属性改变图例文本的颜色。

# 旋转

我们可以将`minRotation`和`maxRotation`属性设置为相同的值，以避免图表必须自动确定要使用的值。

# 抽样

此外，我们可以设置`ticks.sampleSize`选项，通过查看标签的子集来确定标签的大小，从而更快地渲染轴。

# 禁用动画

我们可以用`animation`、`responsiveAnimationDuration`和`hover`选项禁用动画。

比如，我们可以写；

```
var ctx = document.getElementById('myChart').getContext('2d');
var myChart = new Chart(ctx, {
  type: 'bar',
  data: {
    labels: ['Red', 'Blue', 'Yellow'],
    datasets: [{
      label: '# of Votes',
      data: [2, 19, 3],
      backgroundColor: [
        'rgba(255, 99, 132, 1)',
        'rgba(54, 162, 235, 1)',
        'rgba(255, 206, 86, 1)',
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
    scales: {
      yAxes: [{
        ticks: {
          beginAtZero: true
        }
      }]
    },
    animation: {
      duration: 0
    },
    hover: {
      animationDuration: 0
    },
    responsiveAnimationDuration: 0
  }
});
```

使用`options`属性禁用所有动画。

# 禁用折线图中的贝塞尔曲线

我们可以在折线图中禁用贝塞尔曲线，因为画直线比用贝塞尔曲线更快。

我们可以通过以下方式做到这一点:

```
var ctx = document.getElementById('myChart').getContext('2d');
var myChart = new Chart(ctx, {
  type: 'line',
  options: {
    elements: {
      line: {
        tension: 0 
      }
    }
  },
  data: {
    labels: ['Red', 'Blue', 'Yellow'],
    datasets: [{
      label: '# of Votes',
      data: [2, 19, 3],
      borderColor: [
        'rgba(255, 99, 132, 1)',
        'rgba(54, 162, 235, 1)',
        'rgba(255, 206, 86, 1)',
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

属性禁止绘制贝塞尔曲线。

# 禁用线条绘制

我们也可以通过`datasets`或`options`属性中的`showLines`属性禁用留置权提取。

例如，我们可以写:

```
var ctx = document.getElementById('myChart').getContext('2d');
var myChart = new Chart(ctx, {
  type: 'line',
  data: {
    labels: ['Red', 'Blue', 'Yellow'],
    datasets: [{
      showLine: false,
      label: '# of Votes',
      data: [2, 19, 3],
      borderColor: [
        'rgba(255, 99, 132, 1)',
        'rgba(54, 162, 235, 1)',
        'rgba(255, 206, 86, 1)',
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

对数据集禁用它。

# 结论

我们可以用 Chart.js 改变字体。

此外，我们可以禁用各种动画和绘图来提高渲染性能。

喜欢这篇文章吗？如果有，通过 [**订阅我们的 YouTube 频道**](https://www.youtube.com/channel/UCtipWUghju290NWcn8jhyAw?sub_confirmation=true) **获取更多类似内容！**