# JavaScript 最佳实践—假设和优化

> 原文：<https://javascript.plainenglish.io/javascript-best-practices-assumptions-and-optimizations-9d78929779eb?source=collection_archive---------7----------------------->

![](img/e815c96299f478ffd2c072d8103833b1.png)

Photo by [Jametlene Reskp](https://unsplash.com/@reskp?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

像任何类型的应用程序一样，JavaScript 应用程序也必须写得很好。

否则，我们以后会遇到各种各样的问题。

在本文中，我们将研究一些在编写 JavaScript 代码时应该遵循的最佳实践。

# 使用模块模式来封装

我们可以使用模块模式来封装我们的代码。

这让我们可以在任何 JavaScript 代码中保存私有变量。

例如，我们可以写:

```
(function(){
  let x = 123;
  console.log(x);
})(); /
```

我们有只在函数内部可用的变量`x`。

当类或构造函数没有意义时，它也很有用。

# 名称空间我们的 JavaScript

如果我们需要引用我们的 JavaScript 代码，我们应该给它们命名空间，以便容易找到它们。

例如，我们可以写:

```
let MyNamespace = MyNamespace || {};

MyNamespace.MyModule = () => {
    // ...
}
```

我们的名称空间有属性，包括我们作为模块使用的对象。

# 匿名作用域 JavaScript

如果我们不在任何地方调用 JavaScript，我们应该限定它的范围。

例如，我们可以写:

```
(function(){
  let x = 123;
  console.log(x);
})(); 
```

将 JavaScript 的范围限定在函数内部。

这使得我们的变量只能在函数内部使用。

我们不能不小心修改它们。

# 优化时关注大事

大的性能改进可以来自几个地方。

DOM 操作开销很大，所以我们应该尽量少做。

任何迫使页面重新呈现的方法都不是最佳的。

像调整大小和滚动这样一直被触发的事件也可以被谴责或抑制。

如果我们有很多 HTTP 请求，我们也可以减少它们。

我们可以解决这些问题来提高响应能力。

# 延迟加载不立即需要的资产

如果我们有没有立即显示给用户的资产，那么我们应该延迟加载它们。

这样，它们只在需要显示的时候才被加载。

# `unbind()`绑定前的所有事件处理程序

我们应该`unbind`所有的事件处理程序，这样我们就不会有多个事件处理程序绑定到元素上。

例如，我们可以写:

```
$("a.some-link").unbind(handleClick).click(handleClick);
```

解除现有事件监听器的绑定，然后将一个新的点击监听器附加到`a`链接。

这确保了它只被绑定一次。

因为我们在任何地方都需要它，所以我们可以创建一个助手函数来帮助我们在任何地方都这样做。

# 不要把 JavaScript 当成经典的 OOP 语言

我们不应该把 JavaScript 当成经典的 OOP 语言。

尽管类语法存在，但它只是其原型继承模型之上的语法糖。

这一点从未改变。

所以即使 JavaScript 有类，它也不是经典的 OOP 语言。

# 不要滥用函数和对象文字的内联

深嵌套肯定不好。

它们使得代码难以阅读。

嵌套越深，越难跟踪。

例如，没有人想读这样的东西:

```
var myFunction = function() {
  $('form#my-form').submit(
    function(event) {
      event.preventDefault();
      $.ajax(
        '/some_service', {
          type: "POST",
          data: {
            name: name,
            name: company
          },
          success: function(data) {
            onSuccess({
              response1: data.value1,
              response2: data.value2
            })
          },
          error: function(data) {
            onError({
              response1: data.value1,
              response2: data.value2
            })
          }
        }
      );
    }
  );
};
```

我们应该通过减少嵌套来使其易于阅读。

内联一切也使得函数和变量无法重用。

![](img/ea88f2e7974deaf67aa00c58eb95185b.png)

Photo by [Hendo Wang](https://unsplash.com/@hendoo?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 结论

我们可以创建模块来分隔值。

此外，我们在附加事件处理程序之前，先解除它们与 jQuery 的绑定。

并且不要滥用内联，把 JavaScript 当成经典的 OOP 语言。

## 简单英语的 JavaScript

喜欢这篇文章吗？如果是这样，通过 [**订阅解码获得更多类似内容，我们的 YouTube 频道**](https://www.youtube.com/channel/UCtipWUghju290NWcn8jhyAw) **！**