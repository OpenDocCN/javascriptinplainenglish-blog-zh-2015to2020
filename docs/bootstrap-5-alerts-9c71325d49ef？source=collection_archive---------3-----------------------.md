# 引导程序 5 —警报

> 原文：<https://javascript.plainenglish.io/bootstrap-5-alerts-9c71325d49ef?source=collection_archive---------3----------------------->

![](img/6c45b8f44236497595ba5b7952f43bb6.png)

Photo by [Gábor Szűts](https://unsplash.com/@szutsi?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

**写这篇文章时，Bootstrap 5 处于 alpha 状态，可能会更改。**

Bootstrap 是任何 JavaScript 应用程序的流行 UI 库。

在本文中，我们将了解如何使用 Bootstrap 5 添加警报。

# 警报

警报向用户提供用户操作的上下文反馈消息。

它有各种各样的风格。

例如，我们可以写:

```
<div class="alert alert-primary">
  primary alert
</div>
<div class="alert alert-secondary">
  secondary alert
</div>
<div class="alert alert-success">
  success alert
</div>
<div class="alert alert-danger">
  danger alert
</div>
<div class="alert alert-warning">
  warning alert
</div>
<div class="alert alert-info">
  info alert
</div>
<div class="alert alert-light">
  light alert
</div>
<div class="alert alert-dark">
  dark alert
</div>
```

我们用给定的类添加警报的类。

# 链接颜色

`.alert-link`类让我们将彩色链接与任何警报相匹配。

例如，我们可以写:

```
<div class="alert alert-primary">
  primary alert <a href="#" class="alert-link">link</a>
</div>
<div class="alert alert-secondary" role="alert">
  secondary alert <a href="#" class="alert-link">link</a>
</div>
<div class="alert alert-success" role="alert">
  success alert <a href="#" class="alert-link">link</a>
</div>
<div class="alert alert-danger" role="alert">
  danger alert <a href="#" class="alert-link">link</a>
</div>
<div class="alert alert-warning" role="alert">
  warning alert <a href="#" class="alert-link">link</a>
</div>
<div class="alert alert-info" role="alert">
  info alert <a href="#" class="alert-link">link</a>
</div>
<div class="alert alert-light" role="alert">
  light alert <a href="#" class="alert-link">link</a>
</div>
<div class="alert alert-dark" role="alert">
  dark alert <a href="#" class="alert-link">link</a>
</div>
```

我们使用了`.alert-link`类来添加与警报风格相匹配的链接。

# 附加内容

我们可以在提醒中添加额外的内容。

可以用`alert-heading`类添加一个标题。

例如，我们可以写:

```
<div class="alert alert-success" role="alert">
  <h4 class="alert-heading">Heading</h4>
  <p>Lorem ipsum dolor sit amet, consectetur adipiscing elit. Duis aliquam hendrerit nisl. Nunc efficitur ex id orci lacinia, eget bibendum risus aliquam. Phasellus et ornare nisi, vel lacinia purus. Vivamus mollis cursus magna, quis consectetur justo bibendum vitae. Donec mattis sem non nisi elementum, non sollicitudin massa pretium. Donec ante enim, efficitur vitae pellentesque ac, varius eget purus. Aliquam ac ex sed dui tristique rutrum.</p>
  <hr>
  <p class="mb-0">In dictum elit vitae risus mattis sollicitudin non eu massa. Integer pretium pharetra nisi sed maximus. Sed sodales, sapien tristique posuere rhoncus, arcu quam cursus libero, in feugiat diam ante sed eros. Curabitur convallis auctor metus, eget lacinia tortor venenatis et. Praesent ligula metus, consequat sodales mauris fermentum, rhoncus placerat tortor. Cras nec laoreet purus, nec dictum erat. Quisque ac accumsan odio, non finibus urna.  </p>
</div>
```

将`alert-heading`类添加到标题中。

然后它下面的段落有主要内容。

# 解除警报

我们可以添加`.alert-dismissible`类来给警告添加一个关闭按钮。

在关闭按钮上，我们必须添加`data-dismiss="alert"`属性来关闭它。

`.fade`和`.show`类让我们在消除它们时添加过渡效果。

例如，我们可以写:

```
<div class="alert alert-warning alert-dismissible fade show">
  <strong>alert</strong>
  <button type="button" class="close" data-dismiss="alert">
    <span>&times;</span>
  </button>
</div>
```

我们用`close`类添加按钮来添加关闭按钮。

我们有`data-dismiss`属性让我们用它来关闭警报。

此外，我们有`alert-dismissible`类来使警报被忽略。

`fade show`显示警报时，给我们一个渐变过渡效果。

要做到这一点，需要引导 JavaScript 文件。

# JavaScript 行为

我们可以通过编写以下代码用 JavaScript 触发警报的解除:

```
const alertList = document.querySelectorAll('.alert')
alertList.forEach((alert) => {
  new bootstrap.Alert(alert)
})
```

然后，警报 HTML 可以写成:

```
<div class="alert alert-warning alert-dismissible fade show" >
  <strong>alert</strong>
  <button type="button" class="close" data-dismiss="alert" >
    <span>&times;</span>
  </button>
</div>
```

然后，我们可以获取一个现有的警报，并以编程方式关闭它:

```
setTimeout(() => {
  const alertNode = document.querySelector('.alert');  
  const alert = new bootstrap.Alert(alertNode)
  alert.close()
}, 3000)
```

我们用`bootstrap.Alert`构造函数创建了 alert 对象，这样我们就可以对它调用`close`来关闭它。

# 事件

警报也会发出事件，我们可以监听`close.bs.alert`和`closed.bs.alert`事件并做我们想做的事情。

![](img/74aa46c73a7caa407c5704c3843fee87.png)

Photo by [Rachel Hisko](https://unsplash.com/@rachelhisko?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 结论

我们可以使用 Bootstrap 5 添加警报并对其进行样式化。

它们可以通过编程方式添加和关闭。

# 简单英语的 JavaScript

喜欢这篇文章吗？如果有，通过 [**订阅获取更多类似内容解码，我们的 YouTube 频道**](https://www.youtube.com/channel/UCtipWUghju290NWcn8jhyAw) **！**