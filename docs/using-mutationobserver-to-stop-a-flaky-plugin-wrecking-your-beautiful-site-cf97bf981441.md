# 使用 MutationObserver 阻止一个片状插件破坏你美丽的网站

> 原文：<https://javascript.plainenglish.io/using-mutationobserver-to-stop-a-flaky-plugin-wrecking-your-beautiful-site-cf97bf981441?source=collection_archive---------10----------------------->

![](img/2be32d183652456ee69a87375e298955.png)

On the lookout for mutations in Lake DOM

当客户要求你“把这个脚本贴在网站上”时，你难道不喜欢吗？很少有什么“只是”，因为这些事情总是有连锁反应，最近当一个客户在他们的网站上安装了一个实时聊天插件时，我就遇到了这种情况。它看起来很糟糕，我对此无能为力，但它也重叠了移动设备上的一些关键功能，使它们无法使用。

我可以通过在 body 中添加一个类并相应地更新各种样式来解决这个问题，但是这只有在实时聊天就绪的情况下才有效，这是无法保证的。

该插件有时被启用，有时被禁用。有时会加载，有时不会。当它加载时，需要 15-20 秒。最糟糕的是，没有发出“嘿，我成功加载了”事件。所以我需要一些方法来判断实时聊天怪物是否存在。

我开始查看某种轮询脚本，每隔一段时间检查一下 DOM，看看它是否已经加载了(我甚至讨厌自己考虑这个问题)，但是我突然想到——我们现在不是有一个很好的 API 来处理这类事情吗？

事实证明，是的，我们有！它被称为 MutationObserver，从 2012 年开始在 Chrome 和 Firefox 中得到支持，甚至可以在 IE11 中工作。非常漂亮。您告诉浏览器注意 DOM 中的某些特定变化，它会在发现变化时通知您。就像你在尝试联系客服的时候，轮到你的时候可以按 1 得到回拨而不是坐等(旁白:为什么不是所有公司都这么做？！).

观察有三个主要部分:

1.  目标元素——您将关注 DOM 的哪一部分？
2.  配置—您希望什么样的变化？
3.  回调——当变化发生时，你会怎么做？

实时聊天窗口被附加到主体上，所以我的目标元素必须是主体。这听起来很昂贵(从计算角度来说)，但是在配置中，我们将确保我们没有看到整个 DOM。

```
const targetElement = document.body;
```

我们正在寻找的是新的子元素。有一个选项，叫做孩子列表。其他主要选项包括:

*   属性，它监视元素本身属性的变化
*   subtree，它监视从目标元素开始的整个 DOM 树

您可以混合和匹配这些选项，但是您只需要指定您想要启用的选项，因为默认情况下其他选项都是假的。所以我们的配置非常简单:

```
const config = { childList: true }
```

最后，回调，这是比较棘手的部分。我们传递了两个对象——一个突变列表和实际的观察者。

我们将循环遍历这些突变，对于那些类型为 childList 的突变，我们将查看是否有任何添加的节点。您还可以查看从目标中删除的节点，以及目标本身。对于每个节点，您可以访问完整的元素及其所有属性——就像使用 getElementById 或 querySelector 一样。一旦我们找到了我们正在寻找的节点，我们就可以将类添加到主体中，并断开与观察者的连接，这样我们就不会耗尽宝贵的 CPU 周期。

我的回调看起来是这样的:

```
const callback = function (mutationsList, observer) {
  for (const mutation of mutationsList) {
    if (mutation.type === 'childList') {
      for (const addedNode of mutation.addedNodes) {
        if (addedNode.id === 'live-chat-window') {
          document.documentElement.classList.add('live-chat-loaded');
          observer.disconnect();
        }
      }
    }
  }
}
```

现在剩下的就是激发观察者:

```
const observer = new MutationObserver(callback);
observer.observe(target, config);
```

极少量的代码，而且看不到 setTimeout！

这是一个简单的例子。现在我们可以做一些改进。

**健壮性:**为了防止竞态条件，我们应该在设置任何观察器之前检查节点是否存在。

**性能:**遍历突变[可能不是性能最好的](https://stackoverflow.com/questions/31659567/performance-of-mutationobserver-to-detect-nodes-in-entire-dom#39332340)。执行 getElementById 可能会快得多(尽管在我的实际问题中，我没有可预测的 Id 来检查)。

**兼容性:**有些浏览器不支持 MutationObserver，所以在尝试使用它之前检查它是否存在是个好习惯(`if (window.MutationObserver)`)。Polyfills 是存在的，但我不知道它们是否有用。如果你想要 IE 支持，你还需要把 for…of 循环改成简单的 for(或者让你的 transpiler 替你做)。

如果你想了解更多关于变异观察者的信息，请查看 MDN 页面和粉碎杂志指南。

喜欢这篇文章吗？如果有，通过 [**订阅解码，我们的 YouTube 频道**](https://www.youtube.com/channel/UCtipWUghju290NWcn8jhyAw) **获取更多类似内容！**