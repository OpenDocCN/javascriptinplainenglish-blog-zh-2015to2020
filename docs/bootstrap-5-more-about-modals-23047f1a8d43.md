# bootstrap 5—关于情态动词的更多信息

> 原文：<https://javascript.plainenglish.io/bootstrap-5-more-about-modals-23047f1a8d43?source=collection_archive---------10----------------------->

![](img/f8b7ba234c06474fbbe5c9e14494bf1e.png)

Photo by [Mayte Marin](https://unsplash.com/@mayte_minze?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

**写这篇文章时，Bootstrap 5 处于 alpha 状态，可能会有变化。**

Bootstrap 是任何 JavaScript 应用程序的流行 UI 库。

在本文中，我们将看看如何在 Bootstrap 5 中用 JavaScript 添加模态。

# 可滚动模式

我们可以通过添加`.modal-dialog-scrollable`类来创建一个允许我们滚动模态体的模态:

```
<button type="button" class="btn btn-primary" data-toggle="modal" data-target="#modal">
  Launch modal
</button><div class="modal" tabindex="-1" id='modal' data-backdrop="static">
  <div class="modal-dialog modal-dialog-scrollable">
    <div class="modal-content">
      <div class="modal-header">
        <h5 class="modal-title">Modal title</h5>
        <button type="button" class="close" data-dismiss="modal" aria-label="Close">
          <span aria-hidden="true">&times;</span>
        </button>
      </div>
      <div class="modal-body">
        <p>Lorem ipsum dolor sit amet, consectetur adipiscing elit. Nam at neque orci. In vehicula metus eu euismod rhoncus. Pellentesque pharetra ligula sed efficitur ultricies. Quisque et congue metus, id rutrum purus. Maecenas finibus efficitur mauris, quis varius enim elementum a. Phasellus vel aliquet diam. Proin tincidunt eros pellentesque mauris laoreet, nec ullamcorper magna vulputate. Suspendisse potenti. Nullam lobortis ultricies nunc id vulputate. Curabitur pulvinar, lorem ac tempus lobortis, tortor tortor dignissim enim, quis tempor est lacus ut augue. Sed tincidunt lectus urna, ac aliquet velit suscipit a. Nullam tempus sem id nibh vulputate, nec ultricies lorem porttitor. Sed pharetra lacinia odio non sodales.
        </p>
        <p>
          Mauris ullamcorper ornare dui at bibendum. Duis sed quam ac purus tincidunt aliquet. Suspendisse potenti. Praesent vitae turpis dui. Fusce ut tincidunt est, ut sagittis mi. Sed euismod pharetra pulvinar. Morbi at facilisis quam. Vestibulum sagittis arcu sit amet scelerisque vestibulum. Nulla nisl libero, ornare et facilisis et, tempus sagittis massa. Sed tincidunt metus non ex ornare, eu elementum lorem sagittis. Nam a sapien eleifend, dignissim purus sit amet, ultrices neque. Suspendisse imperdiet purus eu posuere pellentesque. Nulla facilisi.
        </p>
      </div>
      <div class="modal-footer">
        <button type="button" class="btn btn-secondary" data-dismiss="modal">Close</button>
        <button type="button" class="btn btn-primary">Save changes</button>
      </div>
    </div>
  </div>
</div>
```

# 垂直居中

我们可以使用`modal-dialog-centered`类来垂直居中模态:

```
<button type="button" class="btn btn-primary" data-toggle="modal" data-target="#modal">
  Launch modal
</button><div class="modal" tabindex="-1" id='modal' data-backdrop="static">
  <div class="modal-dialog modal-dialog-centered">
    <div class="modal-content">
      <div class="modal-header">
        <h5 class="modal-title">Modal title</h5>
        <button type="button" class="close" data-dismiss="modal" aria-label="Close">
          <span aria-hidden="true">&times;</span>
        </button>
      </div>
      <div class="modal-body">
        <p>Lorem ipsum dolor sit amet, consectetur adipiscing elit.
        </p>
      </div>
      <div class="modal-footer">
        <button type="button" class="btn btn-secondary" data-dismiss="modal">Close</button>
        <button type="button" class="btn btn-primary">Save changes</button>
      </div>
    </div>
  </div>
</div>
```

# 使用网格

我们可以把网格放在模态体内。

这样，我们可以为我们的内容创建布局。

例如，我们可以写:

```
<button type="button" class="btn btn-primary" data-toggle="modal" data-target="#modal">
  Launch modal
</button><div class="modal" tabindex="-1" id='modal' data-backdrop="static">
  <div class="modal-dialog modal-dialog-centered">
    <div class="modal-content">
      <div class="modal-header">
        <h5 class="modal-title">Modal title</h5>
        <button type="button" class="close" data-dismiss="modal" aria-label="Close">
          <span aria-hidden="true">&times;</span>
        </button>
      </div>
      <div class="modal-body">
        <div class="modal-body">
          <div class="container-fluid">
            <div class="row">
              <div class="col-md-6">.col-md-6</div>
              <div class="col-md-6 ml-auto">.col-md-6 .ml-auto</div>
            </div>
            <div class="row">
              <div class="col-md-4 ml-auto">.col-md-4 .ml-auto</div>
              <div class="col-md-2 ml-auto">.col-md-2 .ml-auto</div>
            </div>
            <div class="row">
              <div class="col-md-6 ml-auto">.col-md-6 .ml-auto</div>
            </div>
            <div class="row">
              <div class="col-sm-9">
                Level 1: .col-sm-9
                <div class="row">
                  <div class="col-8 col-sm-6">
                    Level 2: .col-8 .col-sm-6
                  </div>
                  <div class="col-4 col-sm-6">
                    Level 2: .col-4 .col-sm-6
                  </div>
                </div>
              </div>
            </div>
          </div>
        </div>
      </div>
      <div class="modal-footer">
        <button type="button" class="btn btn-secondary" data-dismiss="modal">Close</button>
        <button type="button" class="btn btn-primary">Save changes</button>
      </div>
    </div>
  </div>
</div>
```

我们将网格放在带有`modal-body`类的 div 中。

内容将自动调整大小。

# 变化的模态内容

我们可以在模态体中放置一个形式。

比如说。我们可以写:

```
<button type="button" class="btn btn-primary" data-toggle="modal" data-target="#modal">
  Launch modal
</button><div class="modal" tabindex="-1" id='modal' data-backdrop="static">
  <div class="modal-dialog">
    <div class="modal-content">
      <div class="modal-header">
        <h5 class="modal-title" id="modal-message">New post</h5>
        <button type="button" class="close" data-dismiss="modal" aria-label="Close">
          <span aria-hidden="true">&times;</span>
        </button>
      </div>
      <div class="modal-body">
        <form>
          <div class="mb-3">
            <label for="recipient-name" class="col-form-label">Title:</label>
            <input type="text" class="form-control" id="recipient-name">
          </div>
          <div class="mb-3">
            <label for="message-text" class="col-form-label">content:</label>
            <textarea class="form-control" id="message-text"></textarea>
          </div>
        </form>
      </div>
      <div class="modal-footer">
        <button type="button" class="btn btn-secondary" data-dismiss="modal">Close</button>
        <button type="button" class="btn btn-primary">Save</button>
      </div>
    </div>
  </div>
</div>
```

我们将一个表单放在模态体中。

![](img/81f2560c9408a8cd41e163f5af2c3f5b.png)

Photo by [Mark DeYoung](https://unsplash.com/@tempestdesigner?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 结论

我们可以添加一个带有可滚动内容的模型。

此外，我们可以在模态体内放置一个网格或表单。

## 简单英语的 JavaScript

喜欢这篇文章吗？如果有，通过 [**订阅解码获得更多类似内容，我们的 YouTube 频道**](https://www.youtube.com/channel/UCtipWUghju290NWcn8jhyAw) **！**