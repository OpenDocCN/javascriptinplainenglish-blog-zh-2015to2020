# 介绍 JavaScript 窗口对象—表单、标题、隐藏和图像

> 原文：<https://javascript.plainenglish.io/introducing-the-javascript-window-object-forms-head-hidden-and-images-40d272f6b47?source=collection_archive---------6----------------------->

![](img/67f1d93f5c2a587f945105442bb1fcd1.png)

Photo by [J-S Romeo](https://unsplash.com/@jromeo?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

`window`对象是一个全局对象，具有与当前 DOM 文档相关的属性，即浏览器选项卡中的内容。

`window`对象的`document`属性拥有 DOM 文档以及相关的节点和方法，我们可以用它们来操作 DOM 节点并监听每个节点的事件。

在本文中，我们将查看属性，包括`forms`、`head`、`hidden`和`images`属性。

# 窗口.文档.表单

`forms`属性是一个只读属性，它返回一个 HTMLCollection，其中包含对象中的`form`元素列表。HTMLCollection 对象是一个类似数组的对象，它包含我们视频的`form`元素。HTMLCollection 对象有一个`item`方法，让我们通过索引获取 HTMLCollection 对象中的 DOM 元素。此外，因为它是一个类似数组的对象，所以我们可以在`for...of`循环中使用它。

例如，如果我们有以下代码:

```
<form action="save.php" name="nameForm" method="post">
  <label for="firstName">First Name: </label>
  <input type="text" name="firstName" id="firstName" /><br />
  <label for="lastName">Last Name: </label>
  <input type="text" name="lastName" id="lastName" /><br />
  <input type="submit" value="Submit" />
</form>
```

并补充:

```
for (const form of document.forms) {
  const {
    action,
    name,
    method
  } = form;
  console.log(action, name, method);
}
```

然后我们有这样的东西:

```
[https://fiddle.jshell.net/_display/save.php](https://fiddle.jshell.net/_display/save.php) nameForm post
```

记录在`console.log`中。这些属性分别来自`form`元素的`action`、`name`和`method`属性。我们还可以使用 HTMLCollection 对象的`item`方法通过索引获取`form`元素。例如，我们可以用常规的 For 循环替换`for...of`循环，如以下代码所示:

```
for (let i = 0; i < document.forms.length; i++) {
  const {
    action,
    name,
    method
  } = document.forms.item(i);
  console.log(action, name, method);
}
```

如果我们运行代码，我们应该得到与上面相同的输出。我们也可以通过`form`元素的`name`属性得到一个`form`元素。例如，如果我们想得到我们之前得到的`nameForm`，我们可以写:

```
const {
  action,
  name,
  method
} = document.forms.nameForm;
console.log(action, name, method);
```

我们还可以使用括号符号来做与下面代码中相同的事情:

```
const {
  action,
  name,
  method
} = document.forms['nameForm'];
console.log(action, name, method);
```

上面的两段代码应该会得到和以前一样的输出。如果`form`元素的`name`属性的值不是一个有效的变量名，括号符号就很方便。我们可以用`elements`属性得到`form`元素的子元素。`elements`属性返回一个`HTMLFormControlsCollection`，这是另一个我们可以迭代的类似数组的对象。因此，我们可以使用`for...of`循环像任何其他类似数组的对象一样遍历这些项目。例如，我们可以写:

```
const nameForm = document.forms.nameForm;
const {
  action,
  name,
  method
} = nameForm;
for (const el of nameForm.elements) {
  const {
    tagName,
    value,
    type,
    id,
    name
  } = el;
  console.log(tagName, value || 'none', type, id, name);
}
```

在上面的代码中，我们用`for...of`循环遍历了`nameForm`中的元素。在循环内部，我们记录了标签名、属性值、`value`、`type`、`id`和`name`。我们应该得到的输出是:

```
INPUT none text firstName firstName
INPUT none text lastName lastName
INPUT Submit submit
```

注意只有`input`元素在`elements`属性的值中。我们也可以使用`elements`属性通过名称获取输入元素。例如，如果我们想要获取名为`firstName`的`input`元素，或者不使用`elements`属性直接访问表单元素，并直接传递`name`属性的值，我们可以编写以下代码:

```
const nameForm = document.forms.nameForm;
console.log(document.forms.nameForm.firstName.tagName);
console.log(document.forms.nameForm['firstName'].tagName);
console.log(document.forms.nameForm.elements.firstName.tagName);
console.log(document.forms.nameForm.elements['firstName'].tagName);
```

上面的所有四个`console.log`语句都将记录`INPUT`作为它们的输出，这证实了我们正在记录一个`input`元素。

# windows . document . head

`head`属性是返回当前文档的`head`元素的`document`对象的只读属性。例如，我们可以通过运行以下命令来记录`head`元素的内容:

```
console.log(document.head);
```

上面代码中的`console.log`应该会让我们看到这样的内容:

```
<head>
  <meta http-equiv="content-type" content="text/html; charset=UTF-8">
  <title></title>
  <meta http-equiv="content-type" content="text/html; charset=UTF-8">
  <meta name="robots" content="noindex, nofollow">
  <meta name="googlebot" content="noindex, nofollow">
  <meta name="viewport" content="width=device-width, initial-scale=1"><script type="text/javascript" src="/js/lib/dummy.js"></script><link rel="stylesheet" type="text/css" href="/css/result-light.css"><style id="compiled-css" type="text/css">

  </style><!-- TODO: Missing CoffeeScript 2 --><script type="text/javascript">//<![CDATA[window.onload=function(){

console.log(document.head)}//]]></script></head>
```

要获取`head`元素中的元素，我们可以使用`children`属性，如下所示:

```
for (const el of document.head.children) {
  console.log(el);
}
```

我们可以使用`for...of`循环，因为`document.head.children`是一个类似数组的对象，所以它可以和`for...of`循环一起使用。

# windows . document . hidden

`hidden`属性是一个只读属性，它返回一个布尔值，该值指示页面是否隐藏。例如，我们可以在下面的代码中使用它:

```
console.log(document.hidden);
document.addEventListener("visibilitychange", function() {
  console.log(document.hidden);
});
```

![](img/15605fee7aaa6bf46f1d0f1b245322b4.png)

Photo by [Jenny Hill](https://unsplash.com/@jennyhill?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 窗口.文档.图像

属性是一个只读属性，它以 HTMLCollection 的形式获取文档中所有的`img`元素。像所有 HTMLCollection 对象一样，它是一个类似数组的对象，这意味着我们可以像使用`for...of`循环遍历数组一样遍历它。例如，如果页面中有一些图像，如下面的代码所示:

```
<img src='[https://images.unsplash.com/photo-1572315831029-5d6f20e0035d?ixlib=rb-1.2.1&auto=format&fit=crop&w=1500&q=80'](https://images.unsplash.com/photo-1572315831029-5d6f20e0035d?ixlib=rb-1.2.1&auto=format&fit=crop&w=1500&q=80')><img src='[https://images.unsplash.com/photo-1570598339819-b07db356695e?ixlib=rb-1.2.1&ixid=eyJhcHBfaWQiOjEyMDd9&auto=format&fit=crop&w=334&q=80'](https://images.unsplash.com/photo-1570598339819-b07db356695e?ixlib=rb-1.2.1&ixid=eyJhcHBfaWQiOjEyMDd9&auto=format&fit=crop&w=334&q=80')><img src='[https://images.unsplash.com/photo-1569388037243-dfa034ecdbca?ixlib=rb-1.2.1&ixid=eyJhcHBfaWQiOjEyMDd9&auto=format&fit=crop&w=750&q=80'](https://images.unsplash.com/photo-1569388037243-dfa034ecdbca?ixlib=rb-1.2.1&ixid=eyJhcHBfaWQiOjEyMDd9&auto=format&fit=crop&w=750&q=80')><img src='[https://images.unsplash.com/photo-1569271836752-ed9351b75521?ixlib=rb-1.2.1&ixid=eyJhcHBfaWQiOjEyMDd9&auto=format&fit=crop&w=750&q=80'](https://images.unsplash.com/photo-1569271836752-ed9351b75521?ixlib=rb-1.2.1&ixid=eyJhcHBfaWQiOjEyMDd9&auto=format&fit=crop&w=750&q=80')><img src='[https://images.unsplash.com/photo-1568059151110-949642101084?ixlib=rb-1.2.1&ixid=eyJhcHBfaWQiOjEyMDd9&auto=format&fit=crop&w=750&q=80'](https://images.unsplash.com/photo-1568059151110-949642101084?ixlib=rb-1.2.1&ixid=eyJhcHBfaWQiOjEyMDd9&auto=format&fit=crop&w=750&q=80')>
```

然后我们可以用下面的循环遍历`img`元素:

```
for (const img of document.images) {
  console.log(img);
}
```

我们还可以使用`item`方法通过索引获得一个`img`元素。例如，我们可以写:

```
for (let i = 0; i < document.images.length; i++) {
  console.log(document.images.item(i));
}
```

对于这两个循环，我们应该得到相同的输出。我们还可以获得循环中`img`元素的属性。例如，如果我们有以下`img`元素:

```
<img src='[https://images.unsplash.com/photo-1572315831029-5d6f20e0035d?ixlib=rb-1.2.1&auto=format&fit=crop&w=1500&q=80'](https://images.unsplash.com/photo-1572315831029-5d6f20e0035d?ixlib=rb-1.2.1&auto=format&fit=crop&w=1500&q=80') width='100' height='100' alt='image'><img src='[https://images.unsplash.com/photo-1570598339819-b07db356695e?ixlib=rb-1.2.1&ixid=eyJhcHBfaWQiOjEyMDd9&auto=format&fit=crop&w=334&q=80'](https://images.unsplash.com/photo-1570598339819-b07db356695e?ixlib=rb-1.2.1&ixid=eyJhcHBfaWQiOjEyMDd9&auto=format&fit=crop&w=334&q=80') width='100' height='100' alt='image'><img src='[https://images.unsplash.com/photo-1569388037243-dfa034ecdbca?ixlib=rb-1.2.1&ixid=eyJhcHBfaWQiOjEyMDd9&auto=format&fit=crop&w=750&q=80'](https://images.unsplash.com/photo-1569388037243-dfa034ecdbca?ixlib=rb-1.2.1&ixid=eyJhcHBfaWQiOjEyMDd9&auto=format&fit=crop&w=750&q=80') width='100' height='100' alt='image'><img src='[https://images.unsplash.com/photo-1569271836752-ed9351b75521?ixlib=rb-1.2.1&ixid=eyJhcHBfaWQiOjEyMDd9&auto=format&fit=crop&w=750&q=80'](https://images.unsplash.com/photo-1569271836752-ed9351b75521?ixlib=rb-1.2.1&ixid=eyJhcHBfaWQiOjEyMDd9&auto=format&fit=crop&w=750&q=80') width='100' height='100' alt='image'><img src='[https://images.unsplash.com/photo-1568059151110-949642101084?ixlib=rb-1.2.1&ixid=eyJhcHBfaWQiOjEyMDd9&auto=format&fit=crop&w=750&q=80'](https://images.unsplash.com/photo-1568059151110-949642101084?ixlib=rb-1.2.1&ixid=eyJhcHBfaWQiOjEyMDd9&auto=format&fit=crop&w=750&q=80') width='100' height='100' alt='image'>
```

然后，我们可以通过使用`document.images`循环遍历`img`元素来获取属性值，如以下代码所示:

```
for (const img of document.images) {
  const {
    src,
    width,
    height,
    alt
  } = img;
  console.log(src,
    width,
    height,
    alt);
}
```

那么我们应该得到类似如下的`console.log`输出:

```
[https://images.unsplash.com/photo-1572315831029-5d6f20e0035d?ixlib=rb-1.2.1&auto=format&fit=crop&w=1500&q=80](https://images.unsplash.com/photo-1572315831029-5d6f20e0035d?ixlib=rb-1.2.1&auto=format&fit=crop&w=1500&q=80) 100 100 image[https://images.unsplash.com/photo-1570598339819-b07db356695e?ixlib=rb-1.2.1&ixid=eyJhcHBfaWQiOjEyMDd9&auto=format&fit=crop&w=334&q=80](https://images.unsplash.com/photo-1570598339819-b07db356695e?ixlib=rb-1.2.1&ixid=eyJhcHBfaWQiOjEyMDd9&auto=format&fit=crop&w=334&q=80) 100 100 image[https://images.unsplash.com/photo-1569388037243-dfa034ecdbca?ixlib=rb-1.2.1&ixid=eyJhcHBfaWQiOjEyMDd9&auto=format&fit=crop&w=750&q=80](https://images.unsplash.com/photo-1569388037243-dfa034ecdbca?ixlib=rb-1.2.1&ixid=eyJhcHBfaWQiOjEyMDd9&auto=format&fit=crop&w=750&q=80) 100 100 image[https://images.unsplash.com/photo-1569271836752-ed9351b75521?ixlib=rb-1.2.1&ixid=eyJhcHBfaWQiOjEyMDd9&auto=format&fit=crop&w=750&q=80](https://images.unsplash.com/photo-1569271836752-ed9351b75521?ixlib=rb-1.2.1&ixid=eyJhcHBfaWQiOjEyMDd9&auto=format&fit=crop&w=750&q=80) 100 100 image[https://images.unsplash.com/photo-1568059151110-949642101084?ixlib=rb-1.2.1&ixid=eyJhcHBfaWQiOjEyMDd9&auto=format&fit=crop&w=750&q=80](https://images.unsplash.com/photo-1568059151110-949642101084?ixlib=rb-1.2.1&ixid=eyJhcHBfaWQiOjEyMDd9&auto=format&fit=crop&w=750&q=80) 100 100 image
```

对于`document`对象，我们有一些方便的属性，让我们通过使用它的方便属性来获取文档中的元素。要从文档中获取所有的`form`元素，我们可以使用`forms`属性。为了获得`head`元素，我们可以使用`head`属性。

属性`hidden`让我们知道文档是否隐藏，属性`images`让我们得到文档的`img`元素。`forms`和`images`属性为我们获取 HTMLCollection 对象，它们是类似数组的对象，我们可以用`for...of`循环遍历它们。

此外，HTMLCollection 对象附带了一个`item`方法，我们可以通过索引获得想要的元素。

此外，我们可以通过使用属性名称作为元素的属性来获取代码中的属性值，从而获取 HTMLCollection 元素的属性。