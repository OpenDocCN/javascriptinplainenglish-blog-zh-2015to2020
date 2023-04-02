# 引导程序 5 —关于卡的更多信息

> 原文：<https://javascript.plainenglish.io/bootstrap-5-more-about-cards-3d93e1330e9f?source=collection_archive---------8----------------------->

![](img/b6f2eb79ab9a7ec06b83db7217105749.png)

Photo by [Amanda Jones](https://unsplash.com/@amandagraphc?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

**撰写本文时，Bootstrap 5 处于 alpha 版本，可能会有变化。**

Bootstrap 是任何 JavaScript 应用程序的流行 UI 库。

在这篇文章中，我们将看看如何添加更多的 Bootstrap 5 卡。

# 卡片页脚和列表组

我们可以在卡片的列表组下面添加一个页脚。

为此，我们可以写:

```
<div class="card" style="width: 18rem;">
  <ul class="list-group list-group-flush">
    <li class="list-group-item">Lorem ipsum dolor sit amet,</li>
    <li class="list-group-item">Ut ac ante enim</li>
    <li class="list-group-item">Phasellus ullamcorper tellus vitae magna vulputate egestas.</li>
  </ul>
  <div class="card-footer">
    Featured
  </div>
</div>
```

我们刚刚用`.card-footer`类添加了一个 div 来添加页脚。

# 厨房水槽

我们可以在一张卡中添加图像、标题、卡片文本、列表组。

例如，我们可以写:

```
<div class="card" style="width: 18rem;">
  <img src="http://placekitten.com/200/200" class="card-img-top" alt="cat">
  <div class="card-body">
    <h5 class="card-title">Card title</h5>
    <p class="card-text">Some quick example text to build on the card title and make up the bulk of the card's content.</p>
  </div>
  <ul class="list-group list-group-flush">
    <li class="list-group-item">Lorem ipsum dolor sit amet,</li>
    <li class="list-group-item">Ut ac ante enim</li>
    <li class="list-group-item">Phasellus ullamcorper tellus vitae magna vulputate egestas.</li>
  </ul>
  <div class="card-footer">
    Featured
  </div>
</div>
```

把它们都放在一张卡里。

带有`.card-body`的 div 有主体。

`.list-group`有列表组。

`.card-footer`有页脚。

# 页眉和页脚

我们可以在卡片中添加页眉和页脚。

例如，我们可以写:

```
<div class="card">
  <div class="card-header">
    Featured
  </div>
  <div class="card-body">
    <h5 class="card-title">Special title</h5>
    <p class="card-text">Lorem ipsum dolor sit amet, consectetur adipiscing elit. In eget ultricies enim, vitae rutrum magna. In sagittis volutpat justo, nec luctus metus auctor non. Vestibulum dictum pharetra libero in malesuada. Curabitur lacinia mauris quis molestie auctor. Suspendisse finibus posuere neque, vestibulum imperdiet arcu elementum quis. Nam eget purus ac ante egestas viverra. Fusce a bibendum neque, eu ornare mi. Curabitur convallis volutpat dolor, sed cursus odio consequat a. Proin sit amet dui nisi. Etiam eget turpis in augue pharetra consequat.
    </p>
    <a href="#" class="btn btn-primary">Go somewhere</a>
  </div>
</div>
```

我们添加了一个带有`.card-header`类的头。

可以通过添加`.card-header`到`h*`元素来设计卡片标题。

例如，我们可以写:

```
<div class="card">
  <h5 class="card-header">Featured</h5>
  <div class="card-body">
    <h5 class="card-title">Special title</h5>
    <p class="card-text">Lorem ipsum dolor sit amet, consectetur adipiscing elit. In eget ultricies enim, vitae rutrum magna. In sagittis volutpat justo, nec luctus metus auctor non. Vestibulum dictum pharetra libero in malesuada. Curabitur lacinia mauris quis molestie auctor. Suspendisse finibus posuere neque, vestibulum imperdiet arcu elementum quis. Nam eget purus ac ante egestas viverra. Fusce a bibendum neque, eu ornare mi. Curabitur convallis volutpat dolor, sed cursus odio consequat a. Proin sit amet dui nisi. Etiam eget turpis in augue pharetra consequat.
    </p>
    <a href="#" class="btn btn-primary">Go somewhere</a>
  </div>
</div>
```

我们有一个 h5 元素，而不是一个 div 作为标题。

页眉将有一个灰色背景。

我们还可以在卡片中添加块引号:

```
<div class="card">
  <div class="card-header">
    Quote
  </div>
  <div class="card-body">
    <blockquote class="blockquote mb-0">
      <p>Lorem ipsum dolor sit amet, consectetur adipiscing elit. In eget ultricies enim, vitae rutrum magna. In sagittis volutpat justo, nec luctus metus auctor non. Vestibulum dictum pharetra libero in malesuada. Curabitur lacinia mauris quis molestie auctor. Suspendisse finibus posuere neque, vestibulum imperdiet arcu elementum quis. Nam eget purus ac ante egestas viverra. Fusce a bibendum neque, eu ornare mi. Curabitur convallis volutpat dolor, sed cursus odio consequat a. Proin sit amet dui nisi. Etiam eget turpis in augue pharetra consequat.</p>
      <footer class="blockquote-footer">Someone famous <cite title="Source Title">person</cite></footer>
    </blockquote>
  </div>
</div>
```

内容也可以居中:

```
<div class="card text-center">
  <div class="card-header">
    Featured
  </div>
  <div class="card-body">
    <h5 class="card-title">Special title</h5>
    <p class="card-text">Lorem ipsum dolor sit amet, consectetur adipiscing elit. In eget ultricies enim, vitae rutrum magna. In sagittis volutpat justo, nec luctus metus auctor non. Vestibulum dictum pharetra libero in malesuada. Curabitur lacinia mauris quis molestie auctor. Suspendisse finibus posuere neque, vestibulum imperdiet arcu elementum quis. Nam eget purus ac ante egestas viverra. Fusce a bibendum neque, eu ornare mi. Curabitur convallis volutpat dolor, sed cursus odio consequat a. Proin sit amet dui nisi. Etiam eget turpis in augue pharetra consequat.</p>
    <a href="#" class="btn btn-primary">Go somewhere</a>
  </div>
  <div class="card-footer text-muted">
    20 days ago
  </div>
</div>
```

我们使用`text-center`类来使一切居中。

![](img/40e589f36efcbf7b3b11e6a67968bc17.png)

Photo by [Erik Lucatero](https://unsplash.com/@erik_lucatero?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 结论

我们可以添加带有列表组和其他内容的类。

它们可以居中放置在不同的位置。