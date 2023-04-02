# Chart.js 时间轴

> 原文：<https://javascript.plainenglish.io/chart-js-time-axis-1ca3af85c586?source=collection_archive---------3----------------------->

![](img/388e8214d4771b2e8dc91160a8fcdec6.png)

Photo by [Aron Visuals](https://unsplash.com/@aronvisuals?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

我们可以使用 Chart.js 使在网页上创建图表变得容易。

在本文中，我们将了解如何使用 Chart.js 创建图表。

# 步长

我们可以改变线性轴的步长。

例如，我们可以写:

```
var ctx = document.getElementById('myChart').getContext('2d');
var myChart = new Chart(ctx, {
  type: 'line',
  data: {
    datasets: [{
      label: 'First dataset',
      data: [0, 0.5, 1, 1.5, 2]
    }],
    labels: ['January', 'February', 'March', 'April']
  },
  options: {
    scales: {
      yAxes: [{
        ticks: {
          max: 5,
          min: 0,
          stepSize: 0.5
        }
      }]
    }
  }
});
```

我们将`stepSize`更改为 0.5，这样刻度的间隔为 0.5。

# 时间笛卡尔轴

时间笛卡尔轴用于显示时间和日期。

添加刻度后，它会根据刻度的大小自动计算最舒适的单位。

例如，我们可以写:

```
<script src="https://cdnjs.cloudflare.com/ajax/libs/moment.js/2.27.0/moment.min.js" integrity="sha512-rmZcZsyhe0/MAjquhTgiUcb4d9knaFc7b5xAfju483gbEXTkeJRUMIPk6s3ySZMYUHEcjKbjLjyddGWMrNEvZg==" crossorigin="anonymous"></script><script src='https://cdn.jsdelivr.net/npm/chart.js@2.9.3/dist/Chart.min.js'></script><canvas id="myChart" width="400" height="400"></canvas>
```

在 Chart.js 之前添加 moment.js。

这样，我们可以使用库来计算节拍间隔。

然后我们可以写:

```
var ctx = document.getElementById('myChart').getContext('2d');
var myChart = new Chart(ctx, {
  type: 'line',
  data: {
    datasets: [{
      label: 'First dataset',
      data: [{
        x: new Date(2020, 1, 1),
        y: 1
      }, {
        t: new Date(2020, 2, 1),
        y: 10
      }]
    }],
  },
  options: {
    scales: {
      xAxes: [{
        type: 'time',
        time: {
          unit: 'month'
        }
      }]
    }
  }
});
```

用`t`和`y`属性添加时间和数字数据。

我们将 x 轴的`type`设置为`'time'`，这样我们就可以显示时间。

而`time.unit`是`'month'`，所以我们显示月份。

# 显示格式

我们可以将刻度的显示格式更改为 moment.js 支持的任何格式。

例如，我们可以写:

```
var ctx = document.getElementById('myChart').getContext('2d');
var myChart = new Chart(ctx, {
  type: 'line',
  data: {
    datasets: [{
      label: 'First dataset',
      data: [{
        x: new Date(2020, 1, 1),
        y: 1
      }, {
        t: new Date(2020, 4, 1),
        y: 3
      }, {
        t: new Date(2020, 7, 1),
        y: 5
      }, {
        t: new Date(2020, 10, 1),
        y: 7
      }]
    }],
  },
  options: {
    scales: {
      xAxes: [{
        type: 'time',
        time: {
          displayFormats: {
            quarter: 'MMM YYYY'
          }
        }
      }]
    }
  }
});
```

将季度格式设置为月和年格式。

# 规模分布

我们可以改变数据分布的规模分布。

例如，我们可以写:

```
var ctx = document.getElementById('myChart').getContext('2d');
var myChart = new Chart(ctx, {
  type: 'line',
  data: {
    datasets: [{
      label: 'First dataset',
      data: [{
        x: new Date(2020, 1, 1),
        y: 1
      }, {
        t: new Date(2020, 4, 1),
        y: 3
      }, {
        t: new Date(2020, 7, 1),
        y: 5
      }, {
        t: new Date(2020, 10, 1),
        y: 7
      }]
    }],
  },
  options: {
    scales: {
      xAxes: [{
        type: 'time',
        distribution: 'series'
      }]
    }
  }
});
```

我们改变了属性来控制数据在标尺上的分布。

可以改成`'linear'`，按照他们的时间来分摊数据。

并且`'series'`以彼此相同的距离展开数据。

# 结论

我们可以用 Chart.js 对时间轴进行各种修改。

喜欢这篇文章吗？如果有，通过 [**订阅我们的 YouTube 频道**](https://www.youtube.com/channel/UCtipWUghju290NWcn8jhyAw?sub_confirmation=true) **获取更多类似内容！**