# 应该用 MobX 而不是 Redux 吗？

> 原文：<https://javascript.plainenglish.io/javascript-state-should-you-use-mobx-instead-of-redux-ccbbc1418b21?source=collection_archive---------13----------------------->

MobX 是流行的状态管理库之一，它使用非常流行的发布-订阅模式在整个前端应用程序中存储数据。其实 MobX 和前端代码无关，你可以用它来做任何 JavaScript 代码。

Redux library 因其冗长的启动时间而闻名，因为需要样板代码。它有一些基本的概念，比如:提供者、动作、商店和全能的`connect()`功能。然而，设置所有这些通常需要几个文件和对所有这些部分如何连接在一起的深入理解。

## 逐步演练

如果您更喜欢 MobX 的演示视频，您也可以观看这个视频，在这个视频中，我为我的 React 应用程序创建了一个“TodoStore ”:

Step by step walkthrough for Todo List

# MobX 有什么不同？

MobX 努力做到“神奇”。不完全是。它只是很好地使用了观察者-可观察(发布-订阅)模式，并为您抽象出细节。所以假装很神奇。例如，假设您需要一个跨整个应用程序的全局数组。您可以只使用这几行简单的代码:

```
**import** { observable } **from** "mobx"

**export const** todos = observable([])
```

稍后，如果您需要用新的 todo 项更新数组，您只需**改变**可观察对象:

```
todos.push({
  item: ’Read more today’,
  id: 0, 
  completed: false 
});
todos[0].completed = true;
```

是的，没错。您可以将一个新项目推送到现有的 todos 对象，整个应用程序(无论它在哪里被观察)都会得到通知。在非 react 代码中，这可能涉及到使用`autorun`函数，当`todos`更新时，无论何时您需要执行一个任务。注意“todos”还是同一个对象，每当 todos 更新时， *autorun* 函数负责运行这段代码。

```
autorun(() => {
  if (todos.length > 5) {
    // do something if todos is more than 5
  } 
});
```

## 数据突变

注意这里发生的事情很重要:我们实际上是在“改变”数据(todos 数组),而不是创建一个全新的对象。通常，在 redux 库中，不允许改变状态，因为库不会跟踪您对现有数据所做的更改。它能知道的唯一方法是旧对象是否被销毁，一个新对象是否被创建。

## 基于类的状态

在 MobX 中，您还可以使用 ES6 类来保存您的所有状态。例如，我们可以创建一个类来保存我们所有的待办事项，并定义我们的状态所需的所有动作。

如果使用 TypeScript，我们可以为我们的 Todos 创建一个接口，并用一个空数组初始化它。我们还将有另外两个动作——添加一个待办事项，并切换完成或未完成。

```
import {observable, action, computed} from ’mobx’;
interface TodoItem {
  title: string;
  id: number;
  completed: boolean;
}
class TodoStore { todos: TodoItem[] = []; constructor() {
    makeObservable(this, {
      todos: observable,
      addTodo: action,
      toggleCompleted: action,
    });
  }
```

`makeObservable`方法用 MobX 类型初始化我们的状态:动作是方法，可观察的是你需要在状态中保持的任何对象；在我们的例子中，它是我们的 todos 数组。动作可以定义为改变原始状态的简单方法:

```
 addTodo(title: string) {
    this.todos.push({
      title,
      id: Date.now(),
      completed: false
    })
  } toggleCompleted(index: number) {
    this.todos[index].completed = this.todos[index].completed;
  }
```

我们还可以通过向构造函数中的对象添加一个新属性来拥有一个“计算”类型:`countCompleted: computed`(这也是从 mobx 导入的)

```
 countCompleted() {
    return this.todos.filter(i => i.completed).length;
  }
}
```

我们需要做的最后一件事是在我们的应用程序中维护一个实例，它可以从文件中导出。我们将永远只有这个商店的一个实例。这可以简化你的应用状态，除非你真的需要多个商店实例。

```
export const TodoStoreInstance = new TodoStore();
```

# React MobX 集成

React 和 MobX 配合得很好——实际上有一个名为`mobx-react`的包可以帮助你将 React *组件*集成到你的 MobX 商店中。在本文中，我们将使用 MobX 商店创建一个“Todo”应用程序。

这个新包，有一个高阶函数叫做`observer`。这个高阶函数负责用 MobX 存储中的最新值更新组件。不管它只是一个观察者对象，还是一个类实例。

## 考虑视图层

```
import {observer} from ’mobx-react’;7
const TodoList: React.FC = observer(props => {
  const { todoStore } = props; // Use TodoStoreInstance here
  return <div>
    {todoStore.todos.map(item => {
      return <div>item.title</div>
    })
  </div>
});
```

如您所见，我们需要在 props 中获得 TodoStoreInstance。这与我们在上面创建的类文件中导出的 TodoStoreInstance 相同。将它作为一个属性传递并使用 observer 函数，将确保您的商店属性在视图层中得到更新。

```
<TodoList todoStore={TodoStoreInstance} />
```

![](img/8933e4fa3854e49902a096a7f2866351.png)

# 概括起来

如果您正在寻找一个轻量级的库来处理您的 React 状态(或任何其他框架)，MobX 可能是一个非常好的选择。减少了样板代码，真的很容易上手。您也可以从其他库慢慢过渡到 MobX，因为它独立于应用程序的任何其他部分来处理状态。

您可以阅读更多关于 MobX 的内容，并在 [MobX 文档中找到更多示例。](https://mobx.js.org/README.html)