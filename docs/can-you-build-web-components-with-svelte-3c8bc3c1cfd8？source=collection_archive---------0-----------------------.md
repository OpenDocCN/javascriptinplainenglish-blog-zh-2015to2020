# 可以用 Svelte 构建 Web 组件吗？

> 原文：<https://javascript.plainenglish.io/can-you-build-web-components-with-svelte-3c8bc3c1cfd8?source=collection_archive---------0----------------------->

我最近看到了 Svelte，因为我听到了很多关于它的消息，很好奇它如何与 Vue 相抗衡，我几乎所有的前端工作都使用 Vue。特别是，我很好奇它是否更适合构建 web 组件，因为与 Vue 相比，它似乎有一些主要优势。我决定写这篇文章，因为我认为 Svelte 可能是构建 web 组件的完美框架，但目前有一些事情阻碍了它的发展，如果您正在考虑使用它，在您决定使用哪个框架之前，您应该了解它们以及可能的解决方法。

理论上，你需要做的就是把你现有的苗条应用程序转换成一个网络组件，在你的。svelte 文件，并启用编译器中的`customElement: true`选项。就是这样！编译完成后，您需要做的就是包含构建的包，然后您就可以开始使用全新的 web 组件了。

这看起来太容易了，那么问题从哪里开始呢？和往常一样,“Hello World”按预期工作，但是一旦你尝试做其他事情，你就开始遇到问题了。我会在这里列出我到目前为止遇到的所有问题，包括任何解决方法，我会尽可能地链接到相关的 Github 问题。

# 您必须标记您使用的每个组件！

[https://github.com/sveltejs/svelte/issues/3594](https://github.com/sveltejs/svelte/issues/3594)

因此，您已经完成了“Hello World”web 组件，并想开始做一些更高级的东西，您已经创建了另一个苗条的组件。你不想把它作为一个 web 组件导出，你只想在你的主组件内部使用它，但是那样不行。

```
// App.svelte
<svelte:options tag="my-awesome-component" /><script>
  import OtherComponent from './OtherComponent.svelte';
</script><OtherComponent />
```

在一个单独的文件中，我们有:

```
// OtherComponent.svelte<p>Hello World!</p>
```

构建良好，但当您尝试使用它时:

```
TypeError: new.target does not define a custom element
```

解决这个问题的方法是在你使用的每个组件中添加`<svelte:options tag="some-name">`。这迫使你从代码中使用的每一个纤细的元素中创建一个 web 组件。

![](img/68b2277799068db9bff5214a94627fb0.png)

好吧，看起来我们可以接受…我们只是添加了一个我们从未使用过的标签名。虽然这对于我们自己的组件来说很好，但是尽量使用任何苗条的组件库。它们没有为每个单独的组件指定标签名，所以你不能使用它。羞耻:(

**更新**:这里提到了一个变通方法:[https://github . com/svelte-society/recipes-MVP/issues/41 # issue-638005462](https://github.com/svelte-society/recipes-mvp/issues/41#issue-638005462)。它的工作原理是使用一个命名约定来区分我们想要作为 web 组件使用的组件，以及我们只想在内部使用的普通瘦组件。在本例中，`.wc.svelte`中的任何组件 eding 都将在打开`customElement`选项的情况下编译，而其余组件将关闭该选项。

```
// rollup.config.js
svelte({ customElement: true, include: /\.wc\.svelte$/ }),
svelte({ customElement: false, exclude: /\.wc\.svelte$/ }),
```

# 嵌套组件中没有 CSS，也没有全局 CSS

没错，这是一个大的。你看到的所有使用 svelet 的 web 组件的例子要么只有一个 svelet 组件，要么根本不使用 css。看看下面，我们的主组件的 css 将工作，但不会被限定范围，这意味着它将泄漏到所有子组件。

```
// App.svelte
<svelte:options tag="my-awesome-component" /><script>
  import OtherComponent from './OtherComponent.svelte';
</script><style>
  p {
    background-color: red; // Yes, I work... but everywhere!
  }
</style><p>I'm red!</p><OtherComponent />
```

在一个单独的文件中，我们有:

```
// OtherComponent.svelte<style>
  p {
    background-color: blue; // useless
  }
</style><p>I'm red too!</p>
```

所以，如果作用域不起作用，我们可以只使用`:global(p)`来设计所有的`p`标签吗？不，全局修饰符在 web 组件中不起作用。[https://github.com/sveltejs/svelte/issues/2969](https://github.com/sveltejs/svelte/issues/2969)我们不能把所有的 css 都放在里面吗？不完全是。如果我们从`App.svelte`中移除`p`标签，我们也会失去子组件的样式。任何“未使用的”css 都将在编译时被删除，因为编译器正确地看到我们在`App.svelte`中没有`p`标签，所以它抛出了那部分 css。

那么解决办法是什么？我能想到的最好的办法是找到一种方法来禁用对未使用的 css 的修剪，并依靠作用域不起作用这一事实。不太好，但这是我们最好的了。我仍在试图找出如何实际实现这一点，所以如果你知道如何，留下评论，我会更新文章。

# 道具不会立即被填充

[https://github.com/sveltejs/svelte/issues/2227](https://github.com/sveltejs/svelte/issues/2227)

当处理纤细的组件时，我们习惯于在组件渲染时填充道具。如果一个道具有价值，它就已经在那里，随时可供我们使用。如果没有设置，我们可以选择指定一个缺省值或者不定义它。让我们看看这是如何与定制元素一起工作的。

```
// App.svelte
<svelte:options tag="my-awesome-component" /><script>
  export let myProp = "some default value";
</script>My prop is: {myProp}!
```

在我们开始之前，我们需要做一个改变，因为 camel casing props 由于一个 bug 不能与 web 组件一起工作。我们需要将我们的道具重命名为`myprop`，这样我们就可以从一个属性中设置它。
[https://github.com/sveltejs/svelte/issues/3852](https://github.com/sveltejs/svelte/issues/3852)

```
// App.svelte
<svelte:options tag="my-awesome-component" /><script>
  export let myprop = "some default value";

  $: console.log(myprop); // console.log() every time myprop changes
</script>My prop is: {myProp}!
```

然后我们可以在一个 HTML 页面中尝试一下。如果我们不指定`myprop`属性，它会像预期的那样工作，并采用默认值。但是如果我们为属性指定如下内容:

```
// foo.html
...
<my-awesome-component myprop="Foo" />
...
```

让我们看看我们的控制台输出:

```
#: some default value                (App.svelte:4)
#: Foo                               (App.svelte:4)
```

即使我们已经为我们的 prop 指定了一个值，它在呈现元素时使用缺省值，并且只是在稍后才将其更改为属性值，就好像它只是在稍后才被更改一样。这可能会导致各种各样的问题，但幸运的是，我们有一个方法可以绕过它，尽管它并不漂亮。

```
// App.svelte
<svelte:options tag="my-awesome-component" /><script>
  import { onMount, tick } from 'svelte'; export let myprop = "some default value"; onMount(async () => {
    console.log(myprop); // "some default value"
    await tick();
    console.log(myprop); // "Foo"
  });
</script>My prop is: {myProp}!
```

在等待`tick()`之后，我们可以确定所有的属性都有正确的值，并且在这一点上我们可以设置一个变量`ready`，我们可以用它来可选地呈现页面，或者做任何其他的逻辑。这是一个额外的不需要的逻辑层，但它解决了问题，直到有一个适当的修复。

# 老虎机和$ $老虎机

[https://github.com/sveltejs/svelte/issues/5594](https://github.com/sveltejs/svelte/issues/5594)

*更新:这个问题现在已经在 3.29.5 中修复了，所以* `*$$slots*` *应该又可以正常工作了。*

插槽是一种允许定制 web 组件的有用方式。我们可以使用它们来允许在我们定义的组件的某些部分嵌入 HTML 内容。不过，有一个小问题。在 [svelte 3.25.0](https://github.com/sveltejs/svelte/blob/master/CHANGELOG.md#3250) 中添加的`$$slots`在 web 组件的顶层不起作用。因此，尽管插槽被正确地呈现，我们不能检查哪些插槽实际上被使用了。如果一个槽没有被使用，这在我们不想呈现组件的某个部分的情况下会很有用。看看下面的例子，它可以很好地工作，但是当用于 web 组件时`$$slots`总是`{}`。

```
// App.svelte
<svelte:options tag="my-awesome-component" /><p>I am always rendered</p>{#if $$slots.default}
  <p>
    I only want to be displayed if the slot is being used. <br />
    <slot></slot>
  </p>
{/if}
```

我们能找到解决办法吗？让我们利用 [HTMLSlotElement](https://developer.mozilla.org/en-US/docs/Web/API/HTMLSlotElement) 来制作我们自己版本的`$$slots`。如果我们得到 web 组件中所有插槽的列表，我们可以使用 [assignedNodes](https://developer.mozilla.org/en-US/docs/Web/API/HTMLSlotElement/assignedNodes) 来检查是否有任何节点被分配给我们的插槽。我们的默认内容不会显示在这里，只有来自外部的内容。

> `[HTMLSlotElement](https://developer.mozilla.org/en-US/docs/Web/API/HTMLSlotElement)`接口的`**assignedNodes()**`属性返回分配给该槽的节点序列，如果`flatten`选项设置为`true`，则返回作为该槽后代的任何其他槽的分配节点。如果没有找到分配的节点，它将返回插槽的回退内容。

然后，我们简单地计算节点数，看看插槽是否被使用。我们可以用这个来准备一个模拟`$$slots`的布尔映射。现在剩下的就是跟上 DOM 中的任何变化。我们可以监听插槽的变化，以响应任何正在使用或未使用的插槽。

> 当槽中包含的节点改变时，在一个`[HTMLSlotElement](https://developer.mozilla.org/en-US/docs/Web/API/HTMLSlotElement)`实例(`[<slot>](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/slot)`元素)上触发`**slotchange**`事件。

由于此事件会因任何更改而触发，因此我们添加了一个条件来检查节点数量是否已更改，并且仅在需要时更新。注意我们如何使用[扩展语法](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Spread_syntax)来触发[反应](https://svelte.dev/tutorial/updating-arrays-and-objects)。

```
// App.svelte
<svelte:options tag="my-awesome-component" /><script>
import { onMount } from 'svelte';
import { get_current_component } from "svelte/internal";const thisComponent = get_current_component();
let slots = {}; // we can use this instead of $$slots

onMount(async () => {
  // get a list of all slots in our webcomponent
  const elements = [
   ...thisComponent.shadowRoot.querySelectorAll('slot')
  ]; // create our $$slots replacement
  slots = Object.fromEntries(
    elements.map(e => ([e.name, !!e.assignedNodes().length]))
  ); // keep up to date with changes
  elements.forEach(slot => slot.addEventListener(
   'slotchange', () => {
    // only update if we need to
    if (slots[slot.name] !== !!slot.assignedNodes().length) {
     slots = {...slots, [slot.name]: !!slot.assignedNodes().length};
    }
   }
  ));
});</script>{#if slots.mySlot}
  Only display me, if slot is used <br />
{/if}
<slot name="mySlot">Some default content</slot>
```

有用吗？是啊！完事了吗？我希望:(边缘案例来了…让我们看看这个例子。这里，我们将插槽放在 if 块内部。

```
{#if slots.mySlot}
  Only display me, if slot is used <br />
  <slot name="mySlot">Some default content</slot>
{/if}
```

虽然这可以和`$$slots`一起工作，但是我们的版本不行。所发生的是我们的槽没有得到渲染，因为条件最初是假的。因此，当我们调用`.shadowRoot.querySelectorAll('slot')`时，它会给我们一个空的节点列表。因此，我们要么用 css 而不是 if 块隐藏内容，要么找到其他方法来列出组件中的所有道具。不幸的是，我不知道这样的方式，所以现在它的 css。

# 事件:如果你做对了，就去工作

[https://github.com/sveltejs/svelte/issues/3119](https://github.com/sveltejs/svelte/issues/3119)

能够从您的 web 组件发送事件无疑是能够与“外部世界”通信的关键部分。用户是否点击了按钮？我们可以发送一个事件吗？当然，让我们看看如何做。

基本上，我们需要做的是创建一个 [CustomEvent](https://developer.mozilla.org/en-US/docs/Web/API/CustomEvent) 并从我们的顶级元素调度它。查看文档，我们可以看到我们需要设置`composed`来跨越影子 DOM 边界。

> `[Event](https://developer.mozilla.org/en-US/docs/Web/API/Event)`接口的只读`**composed**`属性返回一个`[Boolean](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Boolean)`，指示事件是否会跨越影子 DOM 边界传播到标准 DOM 中。

让我们看看这是如何在我们苗条的组成部分。

```
// App.svelte
<svelte:options tag="my-awesome-component" /><script>
  import { get_current_component } from "svelte/internal";

  const thisComponent = get_current_component(); // example function for dispatching events
  const dispatchWcEvent = (name, detail) => {
    thisComponent.dispatchEvent(new CustomEvent(name, {
      detail,
      composed: true, // propagate across the shadow DOM
    }));
  };</script><button on:click={() => dispatchWcEvent("foo", "bar")}>
  Click me to dispatch a "foo" event!
</button>
```

然后在我们的 HTML 中，我们可以这样做:

```
// foo.html
...
<script>
  window.addEventListener('DOMContentLoaded', () => {
    document.getElementById("foobar").addEventListener("foo", e => {
      alert(`Got foo event with detail: ${e.detail}`);
    });
  });
</script>
...
<my-awesome-component id="foobar"/>
...
```

# 让我们看一些例子

[](https://itnext.io/svelte-web-component-5-4kb-4afe46590d99) [## 纤薄的 Web 组件— 5.4KB

### 让我们来看看如何在现实生活中制作苗条的 Web 组件，并将其与用…创建的类似 Web 组件进行比较

itnext.io](https://itnext.io/svelte-web-component-5-4kb-4afe46590d99) [](https://dev.to/silvio/how-to-create-a-web-components-in-svelte-2g4j) [## 如何在 Svelte 中创建 Web 组件

### 在这篇文章中，我们将看到如何使用 Svelte 框架创建一个 web 组件。在我们开始写之前…

开发到](https://dev.to/silvio/how-to-create-a-web-components-in-svelte-2g4j)