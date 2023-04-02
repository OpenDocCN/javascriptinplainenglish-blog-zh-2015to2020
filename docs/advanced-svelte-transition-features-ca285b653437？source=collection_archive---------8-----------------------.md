# 高级纤薄过渡功能

> 原文：<https://javascript.plainenglish.io/advanced-svelte-transition-features-ca285b653437?source=collection_archive---------8----------------------->

![](img/a6adcd80e15555f099ed97700e6186dc.png)

Photo by [Ashley Rich](https://unsplash.com/@a5hleyrich?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

Svelte 是一个正在兴起的前端框架，用于开发前端 web 应用程序。

它使用简单，让我们快速创建结果。

在本文中，我们将看看如何使用高级的苗条转换特性，包括转换事件，本地和延迟转换。

# 过渡事件

我们可以为各种转换事件添加事件处理程序来完成日志记录之类的工作。

例如，我们可以编写以下代码，在 p 元素被打开和关闭时为其添加动画，并显示动画的状态:

`App.svelte`:

```
<script>
 import { fly } from "svelte/transition";
 let visible = true;
 let status = "";
</script><button on:click={() => visible = !visible}>Toggle</button>
<p>Animation status: {status}</p>
{#if visible}
<p
  transition:fly="{{ y: 200, duration: 2000 }}"
  on:introstart="{() => status = 'intro started'}"
  on:outrostart="{() => status = 'outro started'}"
  on:introend="{() => status = 'intro ended'}"
  on:outroend="{() => status = 'outro ended'}"
>
  foo
</p>
{/if}
```

在上面的代码中，我们为设置`status`的`introstart`、`outrostart`、`introend`和`outroend`事件添加了事件处理程序。

然后我们在 p 标签中显示`status`。当我们点击切换按钮时，我们将看到动画发生并且`status`被更新。

# 局部过渡

我们可以通过局部转换将转换应用于单个列表元素。例如，我们可以编写下面的代码来给列表中的每一项添加一个`slide`效果:

`App.svelte`:

```
<script>
  import { slide } from "svelte/transition";
  let items = [];
  const addItem = () => {
    items = [...items, Math.random()];
  };
</script><button on:click={addItem}>Add Item</button>
{#each items as item}
  <div transition:slide>
    {item}
  </div>
{/each}
```

在代码中，我们使用了`addItem`函数向数组中添加一个新数字。然后我们在标记中显示数组。

在`each`内部的 div 中，我们添加了`transition:slide`指令，以便在添加项目时应用幻灯片效果。

因此，当我们单击 Add Item 时，我们将看到幻灯片效果。

![](img/932adfea8c79a0309f038aeba357a97c.png)

Photo by [Alexander Schimmeck](https://unsplash.com/@alschim?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 延期过渡

有了 Svelte，我们可以推迟过渡，这样我们就可以在多个元素之间协调动画。

我们可以使用`crossfade`功能来实现这一点。它创建了一对名为`send`和`receive`的转换。

如果元素被发送，那么它会寻找接收到的对应元素，并生成一个转换，将该元素转换到其对应元素的位置并淡出。

当接收到一个元素时，就会发生相反的情况。如果没有对应项，则使用`fallback`转换。

例如，我们可以动画显示项目在两个列表之间的移动，如下所示:

`App.svelte`:

```
<script>
  import { cubicOut } from "svelte/easing";
  import { crossfade } from "svelte/transition"; const [send, receive] = crossfade({
    duration: d => Math.sqrt(d * 200), fallback(node, params) {
      const style = getComputedStyle(node);
      const transform = style.transform === "none" ? "" : style.transform; return {
        duration: 600,
        easing: cubicOut,
        css: t => `
          transform: ${transform} scale(${t});
          opacity: ${t}
        `
      };
    }
  }); let uid = 1; let todos = [
    { id: uid++, done: false, description: "write some docs" },
    { id: uid++, done: false, description: "eat" },
    { id: uid++, done: true, description: "buy milk" },
    { id: uid++, done: false, description: "drink" },
    { id: uid++, done: false, description: "sleep" },
    { id: uid++, done: false, description: "fix some bugs" }
  ]; const mark = (id, done) => {
    const index = todos.findIndex(t => t.id === id);
    todos[index].done = done;
  };
</script><style>
  #box {
    display: flex;
    flex-direction: row;
  }
</style><div id='box'>
  <div>
    Done:
    {#each todos.filter(t => t.done) as todo}
      <div 
        in:receive="{{key: todo.id}}"
        out:send="{{key: todo.id}}"
      >
        {todo.description}
        <button on:click={mark(todo.id, false)}>Not Done</button>
      </div>
    {/each}
  </div>
  <div>
    Not Done:
    {#each todos.filter(t => !t.done) as todo}
      <div
        in:receive="{{key: todo.id}}"
        out:send="{{key: todo.id}}"
      >
        {todo.description}
        <button on:click={mark(todo.id, true)}> Done</button>
      </div>
    {/each}
  </div>
</div>
```

在上面的代码中，我们用`crossfade`函数创建了`send`和`receive`转换函数。

当`send`没有对应的`receive`时，就会使用`fallback`动画，反之亦然，如果我们没有在两个元素之间移动一个元素，就会发生这种情况。

回退动画只是对正在制作动画的元素进行一些缩放和不透明度更改。

我们为已完成和未完成的项目呈现 2 个 todo 列表。当我们单击 todo 项旁边的 Done 或 Not Done 按钮时，我们会看到一个淡入淡出效果，因为该项在包含 div 之间移动。

这是通过`in`和`out`动画实现的，我们传入一个带有`key`属性的对象，其值设置为`id`，这样 Svelte 就知道我们正在移动哪个元素。

# 结论

当涉及多个元素时，我们可以使用`crossfade`来协调转换。

此外，我们使用局部过渡来动画显示正在呈现的列表项。

最后，我们可以通过监听在转换过程中发出的事件来跟踪转换效果。

## **简明英语团队的笔记**

你知道我们有四种出版物吗？给他们一个 follow 来表达爱意:[**JavaScript in Plain English**](https://medium.com/javascript-in-plain-english)[**AI in Plain English**](https://medium.com/ai-in-plain-english)[**UX in Plain English**](https://medium.com/ux-in-plain-english)[**Python in Plain English**](https://medium.com/python-in-plain-english)**—谢谢，继续学习！**

**此外，我们总是有兴趣帮助推广好的内容。如果您有一篇文章想要提交给我们的任何出版物，请发送电子邮件至[**submissions @ plain English . io**](mailto:submissions@plainenglish.io)**并附上您的媒体用户名和您感兴趣的内容，我们将会回复您！****