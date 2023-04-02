# 在苗条应用中处理组件和 DOM 事件

> 原文：<https://javascript.plainenglish.io/handling-component-and-dom-events-in-svelte-apps-6049dcd843b7?source=collection_archive---------5----------------------->

![](img/8731719a142a20a5f65a6ca79c5878ba.png)

Photo by [britt gaiser](https://unsplash.com/@brittanyg?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

Svelte 是一个正在兴起的前端框架，用于开发前端 web 应用程序。

它使用简单，让我们快速创建结果。

在本文中，我们将看看如何在苗条的组件中处理 DOM 事件。

# 事件

我们可以使用`on:`指令来处理事件。例如，当我们在屏幕上移动鼠标时，我们可以编写下面的代码来获取鼠标位置:

`App.svelte`:

```
<script>
  let pos = { x: 0, y: 0 }; const handleMousemove = event => {
    pos.x = event.clientX;
    pos.y = event.clientY;
  };
</script><style>
  div {
    width: 100vw;
    height: 100vh;
  }
</style><div on:mousemove={handleMousemove}>
  The mouse position is {pos.x} x {pos.y}
</div>
```

在上面的代码中，我们用`on:mousemove={handleMousemove}`将`handleMousemove`处理程序附加到我们的 div。

`handleMousemove`函数有`event`参数，也就是`MouseEvent`对象。

因此，我们可以使用鼠标 x 和 y 位置的`clientX`和`clientY`属性来访问鼠标位置。

最后，我们把它们显示在屏幕上。

事件处理程序也可以内联声明，如下所示:

`App.svelte`:

```
<script>
  let pos = { x: 0, y: 0 };
</script><style>
  div {
    width: 100vw;
    height: 100vh;
  }
</style><div on:mousemove={event => {
    pos.x = event.clientX;
    pos.y = event.clientY;
  }}>
  The mouse position is {pos.x} x {pos.y}
</div>
```

或者我们可以将它们放在引号中，如下所示:

`App.svelte`:

```
<script>
  let pos = { x: 0, y: 0 };
</script><style>
  div {
    width: 100vw;
    height: 100vh;
  }
</style><div on:mousemove="{event => {
    pos.x = event.clientX;
    pos.y = event.clientY;
  }}">
  The mouse position is {pos.x} x {pos.y}
</div>
```

引号在某些环境中提供了语法高亮，但是它们是可选的。

内联事件处理程序不会影响应用程序的性能，因为 Svelte 会通过不动态连接和分离来优化它。

# 修饰语

DOM 事件指令可以附加修饰符来改变它们的行为。

例如，我们可以将`once`修饰符添加到`on:click`指令中，只监听一次元素的鼠标点击:

`App.svelte`:

```
<script>
  const handleClick = () => {
    alert("alert");
  };
</script><button on:click|once={handleClick}>
  Click Me Once
</button>
```

上面代码中的`once`修饰符只运行`handleClick`处理程序一次，所以如果我们再次单击按钮，它将什么也不运行。

其他改性剂包括:

*   `preventDefault` —在运行处理程序之前调用`event.preventDefault`
*   `stopPropagation` —调用`event.stopPropagation`以防止事件到达下一个元素
*   `passive` —改善触摸或滚轮事件的滚动性能
*   `capture` —在捕获阶段而不是冒泡阶段触发处理程序
*   `self` —如果`event.target`是元素本身，则仅触发处理程序。

也可以像`on:click|once|self`一样用链子拴在一起。

![](img/2cd2df3f67eaf740a3548afa272bd303.png)

Photo by [Marvin Meyer](https://unsplash.com/@marvelous?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 组件事件

组件也可以调度事件。例如，我们可以编写以下代码来将事件从子进程调度到父进程:

`App.svelte`:

```
<script>
  import Button from "./Button.svelte";
  const handleGreet = event => {
    alert(event.detail.text);
  };
</script><Button on:greet={handleGreet} />
```

`Button.svelte`:

```
<script>
  import { createEventDispatcher } from "svelte"; const dispatch = createEventDispatcher(); const greet = () => {
    dispatch("greet", {
      text: "Hello Mary"
    });
  };
</script><button on:click={greet}>
  Send Greeting
</button>
```

在`Button`中，我们用`createEventDispatcher`函数创建了`dispatch`常量。`dispatch`是一个函数，我们用事件名称调用它，用事件发射作为参数发送有效负载。

然后在`App`中，我们通过将`handleGreet` 事件监听器附加到`on:greet`指令来监听从`Button`发出的`greet`事件。

当`handleGreet`在`greet`事件发出后运行时，那么`event`对象将数据与事件发出一起发送，我们用`event.detail.text`检索它。

因此，当我们单击“发送问候”按钮时，我们会看到一个警告框中显示“Hello Mary”。

# 事件转发

组件事件不像 DOM 事件那样冒泡。为了将它们转发给另一个组件，我们可以添加一个调用`dispatch`的`forward`函数。

然后，我们可以在事件被转发到的组件中监听事件。

例如，我们可以写:

`Button.svelte`:

```
<script>
  import { createEventDispatcher } from "svelte"; const dispatch = createEventDispatcher(); const greet = () => {
    dispatch("greet", {
      text: "Hello Mary"
    });
  };
</script><button on:click={greet}>
  Send Greeting
</button>
```

`Parent.svelte`:

```
<script>
  import Button from "./Button.svelte";
  import { createEventDispatcher } from "svelte"; const dispatch = createEventDispatcher(); const forward = () => {
    dispatch("greet", event.detail);
  };
</script><Button on:greet />
```

`App.svelte`:

```
<script>
  import Parent from "./Parent.svelte";
  const handleGreet = event => {
    alert(event.detail.text);
  };
</script><Parent on:greet={handleGreet} />
```

在上面的代码中，`Button.svelte`发出`greet`事件，然后`Parent.svelte`组件转发该事件，然后`App.svelte`监听`greet`事件。

在`Parent.svelte`中，

```
<Button on:greet />
```

与以下内容相同:

```
<Button on:greet={forward} />
```

# DOM 事件转发

我们可以像组件事件一样转发 DOM 事件。例如，我们可以写:

`Button.svelte`:

```
<button on:click>
  Send Greeting
</button>
```

`Parent.svelte`:

```
<script>
  import Button from "./Button.svelte";
</script><Button on:click />
```

`App.svelte`:

```
<script>
  import Parent from "./Parent.svelte";
  const handleClick = event => {
    alert("Hello");
  };
</script><Parent on:click={handleClick} />
```

上面的代码将点击事件从`Button`转发到`Parent`，从`Parent`转发到`App`。

因此，当我们单击“发送问候”按钮时，我们会看到屏幕上显示“Hello”警告。

# 结论

苗条的组件和 DOM 元素可以附加事件处理程序。

我们使用带修饰符的`on:`指令来监听事件。

事件可以用没有设置任何值的`on:`指令来转发。

## **用简单英语写的 JavaScript 笔记**

我们已经推出了三种新的出版物！请关注我们的新出版物: [**AI in Plain English**](https://medium.com/ai-in-plain-english) ，[**UX in Plain English**](https://medium.com/ux-in-plain-english)，[**Python in Plain English**](https://medium.com/python-in-plain-english)**——谢谢，继续学习！**

**我们也一直有兴趣帮助推广高质量的内容。如果您有一篇文章想要提交给我们的任何出版物，请发送电子邮件至[**submissions @ plain English . io**](mailto:submissions@plainenglish.io)**，使用您的 Medium 用户名，我们会将您添加为作者。另外，请让我们知道您想加入哪个/哪些出版物。****