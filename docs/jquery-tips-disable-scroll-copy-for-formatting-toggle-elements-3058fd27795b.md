# jQuery 提示-禁用滚动、复制格式、切换元素

> 原文：<https://javascript.plainenglish.io/jquery-tips-disable-scroll-copy-for-formatting-toggle-elements-3058fd27795b?source=collection_archive---------4----------------------->

![](img/fe78e37d0d093f969e99003732316c82.png)

Photo by [Grant Durr](https://unsplash.com/@blizzard88?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

尽管已经过时，jQuery 仍然是一个流行的操纵 DOM 的库。

在本文中，我们将了解一些使用 jQuery 的技巧。

# 暂时禁用滚动

我们可以通过隐藏正在滚动的 div 的溢出来暂时禁用滚动。

例如，我们可以创建`disableScroll`功能来禁用滚动。

我们可以通过写信来实现这一点:

```
function disableScroll() {
  document.getElementById('scrollbar').style.display = 'block';
  document.body.style.overflow = 'hidden';
}
```

我们得到 ID 为`scrollbar`的元素，并将其显示为块元素。

然后设置`overflow`为隐藏。

# 使用 jQuery 复制到带格式的剪贴板

要使用 jQuery 将带有格式的文本复制到剪贴板，我们可以使带有格式的元素可编辑。

然后，我们可以为焦点事件设置一个处理程序。

当元素被聚焦时，我们调用`document.execCommand`方法选择所有项目。

一旦选择了项目，我们就可以调用文档上的`copy`命令。

然后我们在元素上调用`removeChild`。

为此，我们可以写道:

```
const copyWithFormatting = (elementId) => {
  const temp = document.createElement("div");
  temp.setAttribute("contentEditable", true);
  temp.innerHTML = document.getElementById(elementId).innerHTML;
  temp.setAttribute("onfocus", "document.execCommand('selectAll', false, null)"); 
  document.body.appendChild(temp);
  temp.focus();
  document.execCommand("copy");
  document.body.removeChild(temp);
}
```

我们创建一个临时 div，然后通过设置`contentEditable`属性使其可编辑。

然后我们从带有`elementId`的元素中获取 HTML，并将其设置为`temp`元素的`innerHTML`的 HTML 内容。

然后我们在它上面添加一个焦点处理程序来选择元素中的所有项目:

```
temp.setAttribute("onfocus", "document.execCommand('selectAll', false, null)");
```

然后我们称之为`appendChild`将其附着在身体上。

然后我们专注于我们创建的临时元素。

一旦我们这样做了，我们就可以调用`'copy'`命令来复制临时元素的内容，因为我们已经选择了所有内容。

然后当我们完成复制时，我们调用`removeChild`来移除临时元素。

但是，我们必须知道`execCommand`被标记为过时。

当新的剪贴板 API 被广泛使用时，我们应该用新的 API 替换它。

# 跳到 jQuery .每个()中的下一个迭代

使用`$.each`跳转到下一个迭代，我们可以在回调中返回`true`跳转到下一个迭代。

例如，我们可以写:

```
const arr = ['foo', 'bar', 'baz', 'qux'];
$.each(arr, (i) => {
  if (arr[i] === 'bar') {
    return true;
  }
  console.log(arr[i]);
});
```

我们通过在回调中返回`true`跳到下一个迭代，正如我们在:

```
if (arr[i] === 'bar') {
  return true;
}
```

# 使用 jQuery 更改 CSS 显示无或块属性

jQuery 有专门的方法让我们改变元素来显示块或不显示。

例如，我们可以写:

```
$('#foo').hide();
```

隐藏标识为`foo`的元素。

同样，我们可以写道:

```
$('#foo').show();
```

显示我们的 ID 为`foo`的元素。

我们也可以用 CSS 方法来做同样的事情。

例如，我们可以写:

```
$("#foo").css("display", "none");
```

隐藏该元素并为其他元素释放空间。

我们可以写:

```
$("#foo").css("display", "block");
```

使 ID 为`foo` 的元素显示为块元素。

# 使用 jQuery 在“Enter”上提交表单

我们可以通过使用`keypress`方法监听按键事件，在 Enter 键被按下时提交一个表单。

例如，我们可以写:

```
$('.input').keypress((e) => {
  if (e.which === 13) {
    $('form#task').submit();
    return false;
  }
});
```

我们监听类为`input`的元素上的按键事件。

我们调用`keypress`来添加按键监听器。

在处理程序回调中，我们使用`which`属性来获取被按下的键的键码。

如果是 13，就按回车键。

然后我们调用 ID 为`task`的表单元素上的`submit` 来提交表单的内容。

我们必须在最后返回`false`来停止默认的提交行为，并停止按键事件的传播。

![](img/bb33008ecba279327504385095cafebd.png)

Photo by [Ibrahim Rifath](https://unsplash.com/@photoripey?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 结论

我们可以通过禁止从主体溢出来禁止滚动。

要从一个元素中复制内容，我们可以使用`document.execCommand`方法来完成。

尽管它已经过时了，但我们可以使用它，直到剪贴板 API 广泛可用。

当我们使用`each`时，我们可以返回`true`来跳过一次迭代。

## 简单英语的 JavaScript

喜欢这篇文章吗？如果有，通过 [**订阅解码，我们的 YouTube 频道**](https://www.youtube.com/channel/UCtipWUghju290NWcn8jhyAw) **获取更多类似内容！**