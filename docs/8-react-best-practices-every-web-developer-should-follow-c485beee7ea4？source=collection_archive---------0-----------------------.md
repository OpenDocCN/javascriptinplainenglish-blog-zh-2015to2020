# 每个 Web 开发人员都应该遵循的最佳实践

> 原文：<https://javascript.plainenglish.io/8-react-best-practices-every-web-developer-should-follow-c485beee7ea4?source=collection_archive---------0----------------------->

## 无论 React 如何发展，这些实践永远不会过时。

![](img/8f5f7e1d243b52b5b05d524224996da1.png)

Photo by [Artem Sapegin](https://unsplash.com/@sapegin?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

你是一个每天使用 React 来构建令人敬畏的用户界面的前端开发人员吗？

您是否希望无论项目如何发展，您的代码都保持良好的状态？

那么你应该遵循下面的 8 个反应最佳实践。

# 1.掌握 JavaScript 的重要概念

只要有一点基础的 JavaScript 知识就可以直接跳转反应。但是要达到流利的水平，您需要理解一些重要的 JavaScript 概念。

什么概念？看看下面的故事:

[](https://medium.com/javascript-in-plain-english/11-javascript-concepts-every-web-developer-should-know-to-take-their-skills-to-the-next-level-37ef6693111a) [## 每个 Web 开发人员都应该知道的 11 个 JavaScript 概念，让他们的技能更上一层楼

### 不了解这些概念，就无法掌握 JavaScript。

medium.com](https://medium.com/javascript-in-plain-english/11-javascript-concepts-every-web-developer-should-know-to-take-their-skills-to-the-next-level-37ef6693111a) 

# 2.只有在必要时才发表评论

你觉得下面这段代码怎么样:

```
console.log(error); // Handle error
```

我只看到一个可笑的荒谬的评论。规则是不要写不必要的评论。

你认为在你的代码中到处留下注释会表明你关心你正在做的事情，并且会帮助其他人容易理解你的逻辑。并没有。这不仅会使你的代码混乱，而且会增加冲突的几率。

所以，只有在真的有必要的情况下才评论。

# 3.保持你的组件短小精悍

就像一个函数，你可以写一个大的组件做很多事情。然而，它弊大于利。当出现问题时，您将投入大量时间进行调试。随着您的应用程序变得越来越大，事情可能会失去控制。

一个好的组件应该很小，并且完成一个或几个它想要完成的任务。如果你能做到这一点，当你的项目变大时，它将更容易维护和扩展。

我认为创建小组件的最大好处是可重用性。你不想一遍又一遍地写同样的代码，对吗？如果你倾向于创建大的组件，很可能你会写已经存在的组件。

不要给你的组件任何做大的机会，把它们分成小的。

# 4.有状态组件与无状态组件

不要无意识地使用有状态和无状态组件。仅仅因为您想为用户组件使用有状态组件并不意味着您应该这样做。你必须了解它背后的原因，才能充分利用它。

简单地说，有状态组件有状态，而其他组件没有。这意味着有状态组件存储关于组件状态的所有信息，而无状态组件没有内存，它们只是打印出您传递给它们的数据。

看看下面的例子:

```
// Stateful component
class User extends React.Component {
  constructor() {
    super()

    this.state = {
      users: []
    };
  } render() {
    return (
      <UserList users={this.state.users} />
    );
  }
}// Stateless component
const UserList = ({users}) => {
  render() {
    return (
      <ul>
        {users.map(user => {
          return <li>user</li>;
        })}
      </ul>
    );
  }
}
```

那么什么时候应该选择有状态组件还是无状态组件呢？

如果您只想呈现普通数据，请使用无状态组件。当然，您也可以使用有状态组件来完成任务。但是，不值得。

有状态组件是为更复杂的用途而设计的，比如处理组件的状态、逻辑和转换数据(无状态组件的用途)。

此时，您应该知道什么时候使用什么样的组件。

# 5.为组件指定特定的名称

在命名方面，在**用户**和**VIP 用户**之间，您会选择什么？

**用户**无论在哪里使用，对任何用户来说都很舒适。很方便。然而，从长远来看，它也会在某些时候让您困惑。

对于 **VipUser** ，您正在限制使用的上下文。它更容易控制。您只能将其用于 VIP 用户。

我会选择 **VipUser** ，因为我想知道这个组件的名字到底是用来做什么的。此外，它让您团队中的其他人清楚地知道他们应该将组件用于什么。

# 6.大写组件名称

当涉及到组件命名(和任何其他命名)时，您应该遵循一致的约定。

**VipUser** 应该优先，而不是 **vipuser** 、 **vip_user** 或 **vipUser** 。为什么？这是当今最常见的习俗。因此，不要基于你的爱好来重新发明轮子。

# 7.编写可测试代码并始终将其覆盖

现在，这是下一级编码。你不仅要让你的代码工作，还要让它可测试。这应该是你的经验法则。

到目前为止，我所知道的最好的单元测试框架之一是 [Jest](https://jestjs.io/) 。请查看并满足您的代码覆盖率要求。

对于组件测试，我使用[反应测试库](https://github.com/testing-library/react-testing-library)。反应完全是基于组件的方法。每个组件都应进行有效测试。

不要在没有测试的情况下编写代码。

# 8.将相关文件保存在一个文件夹中

相关的，我不是指文件扩展名，而是服务于特定目的的文件。例如，与组件相关的所有文件都应该在同一个地方，包括样式文件。

它很有用，因为当您需要检查与当前文件相关的内容时，您不必绕过层次结构。有时，如果你在一个巨大的项目中，你可能会迷失方向。

正确放置您的文件，以便以后更容易导航。

# 结论

这些天来，越来越多的新前端框架出现了。不过，反动派仍然有其受人尊敬的地位。而且，我认为从长远来看，它会增长得更多。所以，在此过程中请记住这些最佳实践。

我希望你能从这个故事中受益，如果你能在下面的评论中分享更多，我将不胜感激。

[](https://medium.com/javascript-in-plain-english/21-react-ui-component-libraries-you-should-start-using-from-today-6249758d188) [## 21 您应该从今天开始使用的反应用户界面组件库

### 不要浪费时间自己创造一切

medium.com](https://medium.com/javascript-in-plain-english/21-react-ui-component-libraries-you-should-start-using-from-today-6249758d188)