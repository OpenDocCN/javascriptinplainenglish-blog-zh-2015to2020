# 控制 NGRX 中效果的订阅

> 原文：<https://javascript.plainenglish.io/controlling-the-subscriptions-of-effects-in-ngrx-956822e1d0b1?source=collection_archive---------3----------------------->

![](img/21477c881b0ce47506d56c9e1111eed8.png)

Photo by [Joao Branco](https://unsplash.com/@jfobranco?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) on [Unsplash](https://unsplash.com/s/photos/stream?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)

效果用于 [NGRX](https://ngrx.io/) 中的副作用。它们监听动作，将这个动作转换成副作用，如网络请求、网络套接字请求、浏览器 API 事件等。并且可能会也可能不会从中分派新的动作。

默认情况下，一个效果类是单例的，尽管它是在根或特性级别创建的。它永远不会被销毁，所以它包含的所有动作监听器将永远监听存储动作。有些情况下，我们希望取消所有侦听器对操作的订阅，过一段时间后再重新订阅它们。

# 真实世界场景

假设我们有一个购物应用程序。有一个购物车页面，用户可以看到他的购物车物品。他可以使用登录弹出窗口授权，成功授权后，我们必须加载购物车物品，而无需刷新页面。

所以我们有`LOGIN_USER_SUCCESS`动作，通知用户已经被授权

还有，我们有`LOAD_CART`动作，授权后会调度

以及将这两者结合起来的`CartEffect`类本身

(为了简单起见，我们只调度一个动作，而不从后端 API 获取数据)

现在让我们想象一下，一个用户导航到购物车页面(我们的效果已初始化)，然后导航到其他页面并在那里获得授权。因为这个效果已经被初始化了，它仍然会监听`LOGIN_USER_SUCCESS`的动作，会调度`LOAD_CART`的动作，即使这不是我们的本意。这是一个非常简单和明显的错误。怎么才能修好？

我们必须做到以下几点

*   仅当用户在购物车页面上时，才监听`LOGIN_USER_SUCCESS`动作
*   每当用户离开页面时，取消侦听器的订阅

# 实现 ngrxOnRunEffects

我们可以实现一个`effect`类的`ngrxOnRunEffects`方法。该方法负责根据条件订阅和取消订阅所有效果解析器。

因此，我们需要两个额外的动作

*   购物车 _ 页面 _ 初始化
*   购物车 _ 页面 _ 已销毁

每当`CART_PAGE_INITIALIZED`动作将被调度时，我们必须订阅我们的效果解析器，并且每当`CART_PAGE_DESTROYED`将被调度时，我们必须取消订阅我们的效果解析器。

让我们添加这些操作:

现在将它们分派到`cart-page.component`里面，恰如其分:

现在我们剩下的就是，实现`ngrxOnRunEffects`方法:

无论何时`CART_PAGE_INITIALIZED`将被调度(使用`exhaustMap`操作符),我们都订阅我们的效果解析器，并且无论何时`CART_PAGE_DESTROYED`将被调度(使用`takeUntil`操作符),我们都取消订阅它们