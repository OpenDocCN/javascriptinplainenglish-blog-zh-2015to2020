# 使用 JavaScript 历史 API

> 原文：<https://javascript.plainenglish.io/using-the-javascript-history-api-589948f05b18?source=collection_archive---------11----------------------->

## 让读者更顺畅地浏览 JavaScript 增强的网站

![](img/151167bd9fb4677e63ad9d925aa66a5b.png)

Photo by [Kyle Glenn](https://unsplash.com/@kylejglenn?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

## 历史简史

后退按钮是网络的“撤销”，实际上是仅次于超链接的杀手锏。后退按钮的可用性和简单性使得链接跟踪成为一种廉价的、非破坏性的操作。后退按钮鼓励读者多浏览。

在 JavaScript 出现之前，后退按钮的作用很简单:返回上一页，即 URL。这是字面上的意思，但是客户端技术的进步现在让我们质疑页面是什么，它与 URL 的对应程度如何。

例如，考虑一个包含选项卡式界面的页面，其中一组链接显示内容的不同部分。这些是独立的“页面”吗？浏览器历史，包括后退按钮，应该如何在它们之间导航？

## 重估历史

很明显，对于 JavaScript 增强的页面，我们需要一些额外的细微差别来处理历史。令人欣慰的是，[历史 Web API](https://developer.mozilla.org/en-US/docs/Web/API/History_API) 可以帮上忙。`[History](https://developer.mozilla.org/en-US/docs/Web/API/History)`界面不仅允许以编程方式浏览浏览器历史，它还包含修改历史的方法，包括在历史堆栈中每个条目旁边存储额外的自定义状态。

`replaceState()`方法允许更新历史中当前页面的条目，以响应客户端可能发生的其他事件:

```
history.replaceState(*stateObj*, *title*, [*url*])
```

# 短期变化

对于只涉及特定导航实例的完全瞬时状态的更改，使用历史的 *stateObj* 参数来保持一致性。在这个取自我自己的站点的例子中，我跟踪了一个表当前被排序的列:

```
table.addEventListener("click", function(ev) {
    var index; if (ev.target.parentNode.parentNode.parentNode.tagName
        == 'THEAD'
    ) {
        ev.preventDefault();
        index = siblingIndex(ev.target.parentNode);
        sortRows(table, index);
 **history.replaceState({ "sort": index }, "");**
    }
}
```

注意 *title* 参数是必需的，即使为空，但是 *url* 可以省略。

在页面加载时，状态被读取，并相应地采取行动:

```
window.addEventListener("load", function(ev) {
    if (history.state && history.state.hasOwnProperty("sort")) {
        sortRows(table, history.state.sort);
    }
});
```

我使用`if (history.state.hasOwnProperty("sort"))`而不是更简单的`if (history.state)`来避免在存储第一列的`0`索引时出现假阴性。

# 稍微长期一点

请注意，上面的示例没有对您在对表格进行排序时所查看的 URL 进行任何更改。这可能没问题，但是如果您希望新状态作为一个独立的页面存在，您可能还需要修改 URL。

这种方法的一个潜在用途是引入链接。如果您希望读者能够对您的表进行排序，然后将结果视图标记为书签，或者从其他地方链接到该特定视图，那么最好提供一个唯一的 URL 来表示该状态。这是我在自己的网站上使用的[方法；尝试对表格进行排序，导航到其中一个游戏标题，然后使用后退按钮返回。](https://bobbyjack.me/2020/03/16/all-switch-purchases)

这一次，我们可以使用 URL 参数将排序状态数据从历史的 state 对象移动到 URL 本身:

```
table.addEventListener("click", function(ev) {
    var index**, url**; if (ev.target.parentNode.parentNode.parentNode.tagName
        == 'THEAD'
    ) {
        ev.preventDefault();
        index = siblingIndex(ev.target.parentNode);
        sortRows(table, index);
 **url = new URL(window.location);
        url.searchParams.set("sort", index);
        history.replaceState(null, "", url);**
    }
}
```

创建一个`URL`对象是为了使搜索参数处理更加容易。该 url 然后作为可选的第三个参数发送给`replaceState()`。对此状态的操作类似于前面的示例:

```
window.addEventListener("load", function(ev) {
    var sort;
    var url = new URL(window.location);

    if (sort = url.searchParams.get("sort")) {
        sortRows(table, parseInt(sort));
    }
});
```

逻辑本质上是相同的(获取任何现有的状态，根据它对表进行排序)，但是这次在状态的存储方式上有一个重要的区别:URL 参数总是字符串，所以我们需要首先对列索引进行`parseInt()`。

这一次，当我们按不同的列对表进行排序时，URL 将发生变化(这是调用 replaceState 的副作用)。但是，请注意，它不会在历史堆栈中创建任何额外的条目:我们正在替换当前条目，而不是添加到历史中。在实践中，这意味着如果你已经点击了几列来依次排序，你不必一直点击“返回”来返回到上一页。历史记录中只存储最近的 url。

# 更长期和持久

如果您希望更持久地存储这类信息，或者在全局基础上存储，可以使用 History API 的替代方法。例如，您的站点可能包含许多表，每个表都共享一个相似的结构，如果读者按特定的列对一个表进行排序，您可能希望该选择适用于所有表。

长期的选择包括在静态站点上进行客户端处理的本地存储(甚至是 cookies ),甚至是后端服务器上的全面数据库存储。

无论您选择哪种方法，都要考虑当读者与页面上的某个元素进行交互，离开，然后返回到他们可能认为的“同一页面”时会发生什么。

喜欢这篇文章吗？如果有，通过 [**订阅解码获得更多类似内容，我们的 YouTube 频道**](https://www.youtube.com/channel/UCtipWUghju290NWcn8jhyAw) **！**