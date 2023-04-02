# jQuery 技巧——多个 Ajax 请求、滚动和属性的搜索部分

> 原文：<https://javascript.plainenglish.io/jquery-tips-multiple-ajax-requests-scrolling-and-search-parts-of-an-attribute-f911a9e945c?source=collection_archive---------4----------------------->

![](img/01d6e279a6f14fd1d7e742badf4ea9e2.png)

Photo by [Vivi Bzk](https://unsplash.com/@vivibzk?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

尽管年代久远，jQuery 仍然是操纵 DOM 的流行库。

在本文中，我们将了解一些使用 jQuery 的技巧。

# jQuery 选择器中的通配符

有几种方法可以将通配符添加到我们用来通过 jQuery 获取元素的选择器中。

要获得 ID 以给定字符串开头的元素，我们可以编写:

```
$("[id^=foo]")
```

`^`意味着我们寻找一个 ID 以`foo`开始的元素。

我们可以使用`$`符号来搜索 ID 以给定字符串结尾的元素。

例如，我们可以写:

```
$("[id$=foo]")
```

`$`意味着我们寻找一个 ID 以`foo`结尾的元素。

我们可以用任何其他属性代替`id`。

此外，我们可以使用星号在任何地方查找属性的一部分。

例如，我们可以写:

```
$("[id*=foo]")
```

`*`意味着我们在 ID 值的任何地方寻找`foo`。

# 检查用户是否已经滚动到底部

我们可以检查用户是否已经滚动到底部，我们可以用回调来调用`scroll`方法。

在回调中，我们可以调用`scrollTop`属性来获取窗口滚动条的当前垂直位置。

我们把它加到窗户的高度上。

然后我们把它加起来，然后和文档的高度进行比较。

例如，我们可以写:

```
$(window).scroll(() => {
   if ($(window).scrollTop() + $(window).height() === $(document).height()) {
     console.log("scroll to bottom");
   }
});
```

我们将滚动高度与文档高度进行比较，看用户是否滚动到底部。

# 等到所有 jQuery Ajax 请求都完成

为了等到所有的 jQuery ajax 请求都完成，我们可以使用 jQuery Ajax 调用作为参数来调用`when`方法。

通过这种方式，我们等待所有的请求被完成并得到所有的结果。

例如，我们可以写:

```
const ajax1 = () => {
  return $.ajax({
    url: "/foo",
    dataType: "json",
    data,            
    //...
  });
}//...$.when(ajax1(), ajax2(), ajax3())
  .done((a1, a2, a3) => {
    //...
});
```

我们有 3 个 Ajax 函数`ajax1`、`ajax2`和`ajax3`。

他们都通过呼唤`$.ajax`来回报我们得到的承诺。

我们对每个请求做出承诺，然后传递结果。

然后我们调用`done`来获得所有承诺的结果。

`a1`、`a2`和`a3`是每个承诺的结果。

由于`$.ajax`返回承诺，我们可以将它传递给`Promise.all`。

例如，我们可以写:

```
Promise.all([ajax1(), ajax2(), ajax3()])
  .then(([a1, a2, a3]) => {
    //...
  }).catch(() => {
    //...
  })
```

`Promise.all`让我们得到所有请求的结果，就像 jQuery 的`when`一样。

我们传入一组承诺，然后返回。

然后我们在承诺完成后调用`then`去做一些事情。

每个承诺的结果都在`then`回调的第一个参数的数组中。

用`async`和`await`语法，我们可以写:

```
const makeRequests = async () => {
  try {
    const results = await Promise.all([ajax1(), ajax2(), ajax3()]);
  } catch(ex) { 
    console.log(ex);
  }
}
```

我们只是用更短的语法重写了代码，但是代码做的和`then`一样。

`results`拥有所有承诺的结果数组。

# 选择 href 以某个字符串结尾的元素

我们可以用`$=`操作符得到一个以给定字符串结尾的元素。

例如，我们可以写:

```
$('a[href$="foo"]')
```

这将获得以`foo`结尾的 href 属性。

除了`$=`，还有很多操作符让我们检查字符串的其他部分。

`=`完全相等。

`!=`不相等。

`^=`表示始于。

`$=`是以结尾。

`*=`意为包含。

`~=`表示匹配包含一个单词。

`|=`表示以给定的前缀开始。前缀类似于`prefix-`。

![](img/b15c58bedec45afaa6fddb9c2b9592a6.png)

Photo by [Josh Howard](https://unsplash.com/@thejoshhoward?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 结论

有许多运算符可以让我们匹配属性值的各个部分。

此外，我们可以通过比较高度来检查用户是否滚动到底部。

jQuery 使用的 CSS 选择器中可以包含通配符。

在所有 jQuery Ajax 请求完成后，有许多方法可以运行代码。