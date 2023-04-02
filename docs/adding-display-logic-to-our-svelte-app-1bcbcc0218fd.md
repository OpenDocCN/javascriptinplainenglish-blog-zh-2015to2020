# 向我们的苗条应用添加显示逻辑

> 原文：<https://javascript.plainenglish.io/adding-display-logic-to-our-svelte-app-1bcbcc0218fd?source=collection_archive---------8----------------------->

![](img/9d2094d987e28d120d808295391ed670.png)

Photo by [Melanie Hughes](https://unsplash.com/@nutsycoco?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

Svelte 是一个正在兴起的前端框架，用于开发前端 web 应用程序。

它使用简单，让我们快速创建结果。

在本文中，我们将看看如何在我们的瘦组件模板中添加条件逻辑和迭代逻辑。

# 条件显示

我们可以在模板中添加一个`{#if}`块来有条件地显示一些内容。

例如，我们可以编写以下代码:

`App.svelte`:

```
<script>
let show = false;
const toggle = () => (show = !show);
</script><main>
  <button on:click={toggle}>Toggle</button>
  {#if show}
    <p>hello</p>
  {/if}
</main>
```

在上面的代码中，我们使用了`toggle`函数来切换`show`变量。

然后在模板中，我们有我们的`{#if}`块，如果`show`是`true`，它显示`hello`。

如果`{#if}`块中的条件是`false`，我们还可以添加一个`{:else}`块来显示一些东西。

例如，我们可以写:

`App.svelte`:

```
<script>
let showFoo = false;
const toggle = () => (showFoo = !showFoo);
</script><main>
  <button on:click={toggle}>Toggle</button>
  {#if showFoo}
    <p>foo</p>
  {:else}
    <p>bar</p>
  {/if}
</main>
```

在上面的代码中，如果`showFoo`是`false`，我们有`{:else}`块来显示一些东西。

因此，当我们单击切换按钮时，我们将看到在我们单击切换按钮时显示的`foo`或`bar`。

`#`总是表示块开始标记。一个`/`字符总是表示一个块结束标签。一个`:`总是指示一个块延续标签。

我们也可以将`:else`与一个条件一起使用，并将多个条件链接在一起，如下所示:

`App.svelte`:

```
<script>
let count = 0;
const increment = () => count++;
</script><main>
  <button on:click={increment}>Increment</button>
  <p>{count}</p>
  {#if count < 5}
    <p>Count is less than 5</p>
  {:else if count >= 5 && count <= 10}
    <p>Count is between 5 and 10</p>
  {:else}
    <p>Count is big</p>
  {/if}
</main>
```

在上面的代码中，我们使用了`increment`函数来在点击 increment 按钮时增加`count`的值。

然后我们有`if`和`else`模块，它们有多个条件检查。

因此，如果我们单击“增量”按钮，我们可能会根据`count`的值得到这些消息中的任何一个。

# 环

我们可以使用`{#each}`块来呈现集合中的项目。

集合可以是任何可迭代的对象。

例如，我们可以编写以下代码来将数组呈现到屏幕上的名称列表中:

`App.svelte`:

```
<script>
  let persons = [{ name: "Jane" }, { name: "John" }, { name: "May" }];
</script><main>
  {#each persons as person}
    <p>{person.name}</p>
  {/each}
</main>
```

在上面的代码中，我们使用了`{#each}`块来遍历`persons`，并以`person`的形式检索每个条目。然后我们在 p 标签中显示`person.name`的值。

因此，我们得到:

```
JaneJohnMay
```

显示在屏幕上。

它也可以是类似数组的对象。例如，我们可以写:

`App.svelte`:

```
<script>
  let persons = new Set(["John", "Jane", "May"]);
</script><main>
  {#each [...persons] as person}
    <p>{person}</p>
  {/each}
</main>
```

通过将`Set`展开到一个数组中来迭代`Set`。

然后我们得到:

```
JohnJaneMay
```

显示在屏幕上。

通过编写下面的代码，我们可以获得正在迭代的项的索引:

`App.svelte`:

```
<script>
  let persons = [{ name: "Jane" }, { name: "John" }, { name: "May" }];
</script>
<main>
  {#each persons as person, index}
    <p>{index}: {person.name}</p>
  {/each}
</main>
```

我们在`as`后面添加了`index`来访问被迭代的元素的索引。

当我们修改一个`each`块的值时，它将在该块的末尾添加和删除项目，并更新任何已更改的值。

为了避免这种情况并只更新我们想要的值，我们应该向`each`块添加一个键，如下所示:

`App.svelte`:

```
<script>
  import Info from "./Info.svelte";
  let persons = [
    { id: 1, name: "Jane" },
    { id: 2, name: "John" },
    { id: 3, name: "May" }
  ];
  const handleClick = () => {
    persons = persons.slice(1);
  };
</script><main>
  <button on:click={handleClick}>
    Remove first person
  </button>{#each persons as person (person.id)}
   <Info current={person.name} />
  {/each}
</main>
```

`Info.svelte`:

```
<script>
  export let current;
  const initial = current;
</script><p>{initial} {current}</p>
```

如果没有`person.id`，传入`current`的最后一项将首先被删除。

这与我们想要的不同，我们想要的是总是首先删除第一个项目。

一旦我们像添加`App.svelte`一样添加了`(person.id)`，Svelte 就可以识别出我们真正想要删除的项目，并相应地进行删除。

![](img/07d81792567821bb26078b22e2c0e186.png)

Photo by [Roman Arkhipov](https://unsplash.com/@lomogee?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 等待块

我们可以添加一个`await`块来加载来自承诺的数据。

例如，我们可以将加载文本和实际数据添加到同一个块中显示，如下所示:

`App.svelte`:

```
<script>
  let promise = Promise.resolve(100);
</script>{#await promise}
  <p>...waiting</p>
{:then number}
  <p>The number is {number}</p>
{:catch error}
  <p>Error: {error}</p>
{/await}
```

上面的代码在有加载文本的`await`部分有一个`promise`承诺，解析后的值作为值`number`加载到`then`块中。`number`相当于`then`块的参数。

我们还可以通过传入的`error`对象来捕捉`:catch`块中的错误。

# 结论

我们可以使用块来为我们的应用程序添加显示逻辑。

`{#if}`块和`{:else}`一起让我们有条件地显示事物。

`{#each}`块将可迭代对象渲染成屏幕上的项目。

`{#await}`块将数据从 promises 加载到屏幕上。

## **用简单英语写的 JavaScript 笔记**

我们已经推出了三种新的出版物！请关注我们的新出版物: [**AI in Plain English**](https://medium.com/ai-in-plain-english) ，[**UX in Plain English**](https://medium.com/ux-in-plain-english)，[**Python in Plain English**](https://medium.com/python-in-plain-english)**——谢谢，继续学习！**

**我们也一直有兴趣帮助推广高质量的内容。如果您有一篇文章想要提交给我们的任何出版物，请发送电子邮件至[**submissions @ plain English . io**](mailto:submissions@plainenglish.io)**，使用您的 Medium 用户名，我们会将您添加为作者。另外，请让我们知道您想加入哪个/哪些出版物。****