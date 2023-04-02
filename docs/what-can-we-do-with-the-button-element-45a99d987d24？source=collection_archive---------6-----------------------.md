# HTML 中的 Button 元素有什么特别之处？

> 原文：<https://javascript.plainenglish.io/what-can-we-do-with-the-button-element-45a99d987d24?source=collection_archive---------6----------------------->

## 有许多特性我们可能不知道

![](img/e75555084b23bd48697c3a5b649b3a1d.png)

Photo by [Nirzar Pangarkar](https://unsplash.com/@nirzar?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

当我们点击这个按钮时，我们可以在网页上做一些事情。

在本文中，我们将查看 HTML 按钮中我们可能不知道它们存在的部分。

# 属性

HTML 按钮具有各种属性。具体如下:

*   `disabled` —如果是`true`，则防止按钮被点击
*   `form` —与按钮关联的`form`元素的 ID。这可以让我们把一个按钮放在表单之外，但仍然可以让我们在它被点击的时候对它做些事情
*   `formaction` —处理按钮提交的信息的 URL。如果指定，它将覆盖按钮表单所有者的`action`属性
*   `formenctype` —用于向服务器提交表单的数据编码类型。可能的值为`application/x-www-form-urlencoded`，这是默认值；对于具有文件输入的表单，则为`multipart/form-data`，而`text/plain`仅用于调试。这将覆盖按钮表单所有者的`enctype`属性
*   `formmethod` —如果按钮是提交按钮，则该属性指定浏览器用于提交表单的 HTTP 方法。可能的值为`get`和`post`。该值将覆盖按钮表单所有者的`method`属性
*   `formnovalidate` —一个布尔属性，用于指定提交表单时不进行验证。它会覆盖按钮表单所有者的`novalidate`属性
*   `formtarget` —覆盖按钮表单所有者的`target`属性。可以是`_self`、`_blank`、`_parent`或`_top`。`_self`将响应加载到与当前响应相同的浏览上下文中。这是默认的`_blank`加载到一个新的浏览上下文中，像一个新的标签或窗口。`_parent`将响应加载到当前一个的父浏览上下文中`_top`将响应加载到顶层浏览上下文中
*   `name` —作为表单数据的一部分与按钮的`value`成对提交的按钮的名称
*   `type` —按钮的默认行为。可能的值是向服务器提交数据的`submit`，将所有控件重置为初始值的`rset`，或者没有默认行为的`button`
*   `value` —按钮的初始值。当提交表单数据时，它定义与按钮的`name`相关的值

# 例子

我们可以将`button`元素与`form`元素一起使用，如下所示:

```
<form id="form">
  <input type="text" value="foo" />
</form>
<button type="reset" form="form">Reset</button>
```

我们设置了`form`的 ID，然后我们可以在表单外添加按钮来控制表单，就像我们对 reset 按钮所做的那样。

我们可以添加一个提交按钮如下:

`index.html`:

```
<form id="form">
  <input type="text" value="foo" name="name" />
</form>
<button type="submit" form="form">Submit</button>
<button type="reset" form="form">Reset</button>
```

`index.js`:

```
const form = document.getElementById("form");
form.onsubmit = e => {
  e.preventDefault();
  alert(
    [...form.elements].find(e => e.tagName.toLowerCase() === "input").value
  );
};
```

在上面的代码中，我们获取了`form`的元素，并将其扩展到一个数组中，这样我们就可以使用`find`方法。

我们调用`preventDefault()`来阻止默认提交行为。

然后我们通过检查`input`标签找到我们的`input`元素。

最后，我们可以获得它的值，并在一个警告框中显示该值。

正如我们所看到的，如果我们将`form`属性设置为我们想要控制的表单的 ID，我们可以将按钮元素放在任何地方，并且仍然可以控制表单。

![](img/0c1d477b42c57673f518fda781fe306c.png)

Photo by [Diomari Madulara](https://unsplash.com/@diomari?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 式样

按钮元素比输入元素更容易设计。我们可以在里面添加样式，比如`em`、`strong`或者`img`。

此外，我们可以使用`::after`和`::before`伪元素来添加更复杂的样式。

如果我们不用它来提交数据，我们必须将按钮的类型设置为`button`。

# 结论

我们不必在表单中放置一个按钮来控制表单。这是因为我们可以用表单 ID 的值指定`form`属性。

同样，我们可以指定`formmethod`、`formenctype`、`formtarget`等。重写在表单上设置的相应属性。

这使得按钮更加灵活。

它们的样式也比输入元素灵活得多。