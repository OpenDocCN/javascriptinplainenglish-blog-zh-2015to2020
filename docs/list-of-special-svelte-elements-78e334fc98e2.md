# 特殊苗条元素列表

> 原文：<https://javascript.plainenglish.io/list-of-special-svelte-elements-78e334fc98e2?source=collection_archive---------5----------------------->

![](img/8ddcfca19a68bce632accd0f6f5b77a6.png)

Photo by [Leighton Robinson](https://unsplash.com/@ljr?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

Svelte 是一个正在兴起的前端框架，用于开发前端 web 应用程序。

它使用简单，让我们快速创建结果。

在这篇文章中，我们将看看特殊的苗条元素，以及如何使用它们。

# 苗条:组件

我们可以使用`svelte:component`元素动态加载组件。它需要一个接受组件对象的`this`道具来加载。

例如，我们可以如下使用它:

```
<script>
  import Foo from "./Foo.svelte";
  import Bar from "./Bar.svelte";let component = Foo;
  const toggle = () => {
    component = component === Foo ? Bar : Foo;
  };
</script><button on:click={toggle}>Toggle</button><svelte:component this={component}/>
```

在上面的代码中，我们用`toggle`将`component`设置为我们想要加载的组件，方法是在`Foo`和`Bar`之间切换，作为`component`的值。

然后我们将`component`传递给`this` prop 来加载组件。

# 苗条:窗口

我们可以使用`svelte:window`组件向`window`对象添加监听器。

例如，我们可以如下使用它:

```
<script>
  let key;
  const handleKeydown = e => {
    key = e.keyCode;
  };
</script><p>{key}</p><svelte:window on:keydown={handleKeydown}/>
```

上面的代码将`handleKeydown`附加到`window`对象上。`handleKeydown`从事件对象`e`获取`keyCode`，然后将值设置为`key`。

`key`然后显示在一个 p 元素中。然后，当我们按下按键时，我们会看到被按下按键的键码。

我们也可以像其他 DOM 元素一样添加事件修饰符，如`preventDefault`到`svelte:window`。

此外，我们可以绑定到`window`的某些属性，如下所示:

*   `innerWidth`
*   `innerHeight`
*   `outerWidth`
*   `outerHeight`
*   `scrollX`
*   `scrollY`
*   `online`—`window.navigator.onLine`的别名

除了`scrollX`和`scrollY`之外的所有属性都是只读的。例如，我们可以编写以下代码在屏幕上显示这些值:

```
<script>
  let innerWidth;
  let innerHeight;
  let outerWidth;
  let outerHeight;
  let scrollX;
  let scrollY;
  let online;
</script><p>{innerWidth}</p>
<p>{innerHeight}</p>
<p>{outerWidth}</p>
<p>{outerHeight}</p>
<p>{scrollX}</p>
<p>{scrollY}</p>
<p>{online}</p><svelte:window 
  bind:innerWidth 
  bind:innerHeight 
  bind:outerWidth  
  bind:outerHeight 
  bind:online  
  bind:scrollX  
  bind:scrollY  
/>
```

因为我们使用了`bind`将值绑定到变量，所以值会自动更新。

# 苗条:身材

`svelte:body`元素允许我们监听在`document.body`触发的事件。

例如，我们可以使用它来监听鼠标在 body 元素上的移动，如下所示:

```
<script>
  let clientX;
  let clientY;
  const handleMousemove = e => {
    clientX = e.clientX;
    clientY = e.clientY;
  };
</script><p>{clientX}</p>
<p>{clientY}</p>
<svelte:body on:mousemove={handleMousemove} />
```

上面的代码观察鼠标在主体中的移动，将`on:mousemove`指令添加到`svelte:body`，并将`handleMousemove`处理函数设置为值。

然后我们通过从事件对象`e`中获取`clientX`和`clientY`并将其设置为变量来获得鼠标位置。

最后，我们通过在 p 元素中插入这些变量来显示这些值。

因此，当我们在体内移动鼠标时，我们会看到鼠标位置坐标的显示。

![](img/336998ff492a0abe257d90347c9496a4.png)

Photo by [Nikolay Hristov](https://unsplash.com/@nikolayh?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 苗条:头

`svelte:head`元素允许我们在文档的`head`中插入元素。例如，我们可以使用它来引用外部 CSS 文件，方法是编写:

```
<svelte:head>
  <link rel="stylesheet" href="https://stackpath.bootstrapcdn.com/bootstrap/4.3.1/css/bootstrap.min.css" integrity="sha384-ggOyR0iXCbMQv3Xipma34MD+dH/1fQ784/j6cY/iJTQUOhcWr7x9JvoRxT2MZw1T"
   crossorigin="anonymous">
</svelte:head><h1 class="text-primary">foo</h1>
```

在上面的代码中，我们只是将引导 CSS 的链接标签放在了`svelte:head`标签之间。然后我们可以看到蓝色的单词 foo。

在服务器端呈现模式中，`svelte:head`的内容与 HTML 的其余部分分开返回。

# 苗条:选项

`svelte:options`让我们指定编译器选项。我们可以通过将以下选项作为道具传递来更改它们:

*   `immutable={true}` —让编译器进行简单的引用检查，以确定值是否已更改
*   `immutable={false}` —默认选择。Svelte 对于可变对象是否改变会更加保守
*   `accessors={true}` —为组件的属性添加 getters 和 setters
*   `accessors={false}` —默认选择。
*   `namespace="..."` —将使用该组件的名称空间
*   `tag="..."` —将组件编译为自定义元素时使用的名称。

例如，我们可以编写以下代码将`immutable`选项设置为`true`:

```
<svelte:options immutable={true} /><h1>foo</h1>
```

# 结论

Svelte 的特殊组件让我们可以毫不费力地访问 DOM 的各个部分，比如`window`和`document.body`。

`svelte:component`让我们动态加载组件。

此外，我们可以引用`svelte:options`组件来设置各种应用程序范围的选项。

## **用简单英语写的 JavaScript 笔记**

我们已经推出了三种新的出版物！请关注我们的新出版物:[**AI in Plain English**](https://medium.com/ai-in-plain-english)[**UX in Plain English**](https://medium.com/ux-in-plain-english)[**Python in Plain English**](https://medium.com/python-in-plain-english)**——谢谢，继续学习！**

**我们也一直有兴趣帮助推广高质量的内容。如果您有一篇文章想要提交给我们的任何出版物，请发送电子邮件至[**submissions @ plain English . io**](mailto:submissions@plainenglish.io)**，并附上您的 Medium 用户名，我们会将您添加为作者。另外，请让我们知道您想加入哪个/哪些出版物。****