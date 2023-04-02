# 我在 React 和 Vue 中创建了完全相同的应用程序。以下是不同之处。[2019 版]

> 原文：<https://javascript.plainenglish.io/i-created-the-exact-same-app-in-react-and-vue-here-are-the-differences-2019-edition-42ba2cab9e56?source=collection_archive---------0----------------------->

## React vs Vue。最后是 Vue 和 React 的并列代码对比！🎉[针对 2019 年更新和重写:现在带有 React 挂钩]

在工作中使用过 Vue 之后，我对它有了相当扎实的理解。然而，我很好奇栅栏另一边的草是什么样的——这种情况下的草会有什么反应。

*注:这里可以找到这篇文章的新版本:*[***https://sunilsandhu . com/posts/I-created-the-exact-same-app-in-react-and-vue-2020-edition***](https://sunilsandhu.com/posts/i-created-the-exact-same-app-in-react-and-vue-2020-edition)

我读了 React 文档，看了一些教程视频，虽然它们都很棒，但我真正想知道的是 React 和 Vue 有什么不同。我说的“不同”并不是指他们是否都有虚拟 DOMS 或者他们如何渲染页面。我希望有人花时间来解释代码！我想找一篇花时间解释这一点的文章，以便刚接触 Vue 或 React(或整个 Web 开发)的人能够更好地理解这两者之间的区别。

不幸的是，我找不到任何解决这个问题的方法。所以我意识到，我必须自己动手建造它，才能看到相似之处和不同之处。在这样做的时候，我想我应该把整个过程记录下来，这样一篇关于这个的文章就会最终存在。

![](img/c85f1afbf95d7fc38b1b42e2234bfd0f.png)

Who wore it better?

我决定尝试构建一个相当标准的待办事项应用程序，允许用户在列表中添加和删除项目。这两个应用程序都是使用默认 cli 构建的(react 使用 create-react-app，vue 使用 vue-cli)。顺便说一下，CLI 代表命令行界面。🤓

*喜欢这篇文章吗？如果有，获取更多类似内容通过* [***订阅解码，我的 YouTube 频道***](https://www.youtube.com/channel/UCtipWUghju290NWcn8jhyAw) ***！***

# 无论如何，这个介绍已经比我预期的要长了。让我们先来快速了解一下这两款应用的外观:

![](img/be38aef034d973643aa0bafdc33a1d51.png)

React vs Vue. The Immovable Object meets the Irresistible Force!

两个应用程序的 CSS 代码完全相同，但是它们的位置不同。记住这一点，接下来让我们看看这两个应用程序的文件结构:

![](img/5af971db8a1b084b491a630c8f7225bb.png)

你会发现它们的结构也几乎相同。这里唯一的区别是 React 应用程序有两个 CSS 文件，而 Vue 应用程序没有。这样做的原因是，在 create-react-app 中，react 组件将有一个附带的文件来保存其样式，而 Vue CLI 采用一种无所不包的方法，其中样式在实际的组件文件中声明。

*更新:事后看来，拥有三个 CSS 文件对 React 来说更有意义，因此 App.js、ToDo.js 和 ToDoItem.js 有一个单独的 CSS 文件*

最终，它们都实现了同样的事情，没有什么可以说你不能继续在 React 或 Vue 中构造不同的 CSS。真的归结为个人喜好。您将会听到来自开发社区的关于 CSS 应该如何构建的大量讨论，特别是关于 React，因为有许多 CSS-in-JS 解决方案，如 styled-components 和 emotion。顺便说一下，CSS-in-JS 就是字面上的意思。虽然这些都很有用，但现在，我们将只遵循两个 CLI 中的结构。

但是在我们继续之前，让我们快速看一下典型的 Vue 和 React 组件是什么样子的:

![](img/b075d3d007f680999260db53dc04654b.png)

React on the left. Vue on the right.

现在，让我们进入本质的细节！

# 我们如何改变数据？

但是首先，我们所说的“变异数据”是什么意思？听起来有点专业，不是吗？它基本上只是意味着改变我们已经存储的数据。所以，如果我们想把一个人的名字从约翰改成马克，我们就要“改变数据”。所以这就是 React 和 Vue 的关键区别所在。Vue 本质上创建了一个数据对象，其中的数据可以自由更新，而 React 通过所谓的状态挂钩来处理这个问题。

让我们看看下图中两者的设置，然后我们将解释接下来会发生什么:

![](img/46023023a79660a8c2219f3bfc0fd953.png)![](img/6ccf262bebd9683d12d91edeb175b613.png)

React on the left. Vue on the right.

因此，您可以看到，我们将相同的数据传递给了两者，但结构略有不同。

在 Vue 中，通常会将组件的所有可变数据放在一个`data()`函数中，该函数返回一个包含数据的对象(如右图所示)。

使用 React——或者至少从 2019 年开始——我们通常会通过一系列挂钩来处理状态。如果你以前没有见过这种类型的概念，这些可能看起来有点奇怪。基本上，它的工作方式如下:

假设我们想要创建一个待办事项列表。我们可能需要创建一个名为`list`的变量，它可能需要一个字符串或者对象的数组(如果我们想给每个`todo`字符串一个 ID 或者一些其他的东西。我们将通过编写`const [list, setList] = useState([])`来设置它。这里我们使用了 React 称之为钩子的东西 useState。这基本上让我们在组件中保持本地状态。

另外，你可能已经注意到我们在`useState()`中传递了一个空数组`[]`。我们在里面放的是我们希望`list`最初被设置的值，在我们的例子中，我们希望它是一个空数组。然而，你会从上面的图片中看到，我们在数组内部传入了一些数据，这些数据最终成为了`list`的初始化数据。想知道`setList`是做什么的？稍后会有更多关于这个的内容！

## **那么我们如何在应用程序中引用可变数据呢？**

好吧，假设我们有一些名为`name`的数据，它被赋予了一个值`‘Sunil**’**`。

在 Vue 中，它将位于`data()`对象的内部，并被称为`name: ‘Sunil'`。在我们的应用程序中，我们将通过调用`this.name`来引用它。我们也可以通过调用`this.name = ‘John’`来更新它。这会把我的名字改成约翰。我不确定我被叫做约翰是什么感觉，但是嘿，事情发生了！😅

在 React 中，由于我们有用`useState()`创建的更小的状态片段，很可能我们会按照`const [name, setName] = useState('Sunil')`的思路创建一些东西。在我们的应用程序中，我们将通过简单地调用`name`来引用相同的数据。现在这里的关键区别是，我们不能简单地编写`name = ‘John’`，因为 React 有适当的限制来防止这种容易的、无忧无虑的变异。所以在 React 中，我们会写`setName('John')`。这就是`setName`位发挥作用的地方。基本上，在`const [name, setName] = useState('Sunil')`中，它创建了两个变量，一个变成了`const name = 'Sunil'`，而第二个`const setName`被分配了一个函数，使得`name`能够用一个新值重新创建。

Effectively React 和 Vue 在这里做着同样的事情，那就是创建可以更新的数据。默认情况下，每当有数据更新时，Vue 都会组合自己版本的`name`和`setName`。所以简而言之，React 要求你用里面的值调用`setName()`来更新状态，Vue 假设如果你曾经试图更新数据对象里面的值，你会想要这样做。那么，为什么 React 还要费心将值从函数中分离出来，为什么还需要`useState()`？让我们把这个交给 [Revanth Kumar](https://medium.com/@revanth0212) 来解释:

> “这是因为 React 希望在状态改变时重新运行某些生命周期挂钩。当您调用 useState 函数时，它会知道状态已经改变。如果您直接改变状态，React 将不得不做更多的工作来跟踪变化以及运行什么生命周期挂钩等等。所以为了简单起见，React 使用了 useState。

![](img/8af27666f806002b3a9c52fb46f4b152.png)

Bean knew best.

现在我们已经有了一些变化，让我们看看如何在我们的待办事项应用程序中添加新的项目，从而进入本质。

# 我们如何创建新的待办事项？

## 反应:

```
const createNewToDoItem = () => {
  const newId = Math.max(...list.map((t) => t.id)) + 1
  const newToDo = { id: newId, text: toDo }; setList([...list, newToDo]);
  setToDo("");
};
```

## React 是怎么做到的？

在 React 中，我们的输入字段有一个名为 **value 的属性。**每当这个值通过所谓的 **onChange 事件监听器**改变时，这个值就会自动更新。JSX(基本上是 HTML 的一种变体)如下所示:

```
<input type="text" 
       value={toDo} 
       onChange={handleInput}/>
```

所以每次值改变时，它更新状态。`handleInput`函数看起来像这样:

```
const handleInput = (e) => {
  setToDo(e.target.value);
};
```

现在，每当用户按下页面上的 **+** 按钮来添加新项目时，就会触发**createnewdoitem**功能。让我们再来看一下这个函数，以分解正在发生的事情:

```
const createNewToDoItem = () => {
  const newId = Math.max(...list.map((t) => t.id)) + 1
  const newToDo = { id: newId, text: toDo }; setList([...list, newToDo]);
  setToDo("");
};
```

本质上，`newId`函数基本上是创建一个新的 ID，我们将赋予新的`toDo`项目。`newToDo`变量是一个对象，它有一个`id`键，键的值来自`newId`。它还有一个`text`键，将来自`toDo`的值作为它的值。这与输入值改变时更新的`toDo`相同。

然后我们运行`setList`函数，并传入一个数组，该数组包含我们的整个`list`以及新创建的`newToDo`。

如果`...list`，位看起来很奇怪，那么开头的三个点就是所谓的 spread 运算符，它基本上传递来自`list`的所有值，但作为单独的项，而不是简单地将整个项数组作为数组传递。迷茫？如果是这样，我强烈推荐阅读 spread，因为它很棒！

总之，最后我们运行`setToDo()`，传入一个空字符串。这使得我们的输入值为空，准备好输入新的 toDos。

## Vue:

```
createNewToDoItem() {
  const newId = Math.max(...this.list.map(t => t.id)) + 1;

  this.list.push({ id: newId, text: this.todo });
  this.todo = "";
}
```

## Vue 是怎么做到的？

在 Vue 中，我们的**输入**字段上有一个名为 **v-model** 的句柄。这允许我们做一些被称为**双向绑定**的事情。让我们快速查看一下我们的输入字段，然后我们将解释这是怎么回事:

```
<input type="text" v-model="todo"/>
```

V-Model 将这个字段的输入与我们的数据对象 toDoItem 中的一个键联系起来。当页面加载时，我们必须将 toDoItem 设置为一个空字符串，如下: **todo: ''** 。如果这里已经有一些数据，比如 **todo:'在这里添加一些文本'**，我们的输入字段将加载已经在输入字段内的*在这里添加一些文本*。无论如何，回到空字符串，我们在输入字段中输入的任何文本都会绑定到 **todo** 的值。这实际上是双向绑定(输入字段可以更新数据对象，数据对象可以更新输入字段)。

所以回头看看前面的**createnewdoitem()**代码块，我们看到我们将 **todo** 的内容推入**列表**数组中，然后将 **todo** 更新为空字符串。

我们还使用了 React 示例中使用的相同的`newId()`函数。

# 我们如何从列表中删除？

## 反应:

```
const deleteItem = (item) => {
  setList(list.filter((todo) => todo !== item));
};
```

## React 是怎么做到的？

因此，虽然`deleteItem()`函数位于 **ToDo.js** 内部，但我可以很容易地在 **ToDoItem.js** 内部引用它，方法是首先将 **deleteItem()** 函数作为道具传递给 **< ToDoItem/ >** :

```
<ToDoItem deleteItem={deleteItem}/>
```

这首先将功能向下传递，使孩子可以访问它。然后，在 **ToDoItem** 组件中，我们执行以下操作:

```
<button className="ToDoItem-Delete" onClick={() => deleteItem(item)}> - </button>
```

要引用父组件中的函数，我只需引用 **props.deleteItem** 。现在你可能已经注意到，在代码示例中，我们只是写了`deleteItem`而不是`props.deleteItem`。这是因为我们使用了一种被称为**析构**的技术，它允许我们获取**道具**对象的一部分，并将它们分配给变量。因此，在我们的 **ToDoItem.js** 文件中，我们有以下内容:

```
const ToDoItem = (props) => {
  const { item, deleteItem } = props;
}
```

这为我们创建了两个变量，一个叫做`item`，它被赋予与`props.item`相同的值，另一个叫做`deleteItem`，它被赋予来自`props.deleteItem`的值。我们可以通过简单地使用`props.item`和`props.deleteItem`来避免整个析构过程，但是我认为这值得一提！

## Vue:

```
onDeleteItem(item){
  **this**.list = **this**.list.filter(todo => todo !== item);
}
```

## Vue 是怎么做到的？

在 Vue 中需要一种稍微不同的方法。我们必须做三件事:

首先，在我们想要调用函数的元素上:

```
<div class=”ToDoItem-Delete” @click=”deleteItem(item)”>-</div>
```

然后我们必须创建一个 emit 函数作为子组件内部的方法(在本例中是 **ToDoItem.vue** )，如下所示:

```
deleteItem(item) {
    **this**.$emit('delete', item)
}
```

与此同时，你会注意到当我们在 **ToDo.vue** 内添加 **ToDoItem.vue** 时，我们实际上引用了一个**函数**:

```
<ToDoItem v-for="todo in list" 
          :todo="todo" 
          **@delete="onDeleteItem" //** <-- this :)
          :key="todo.id" />
```

这就是所谓的自定义事件侦听器。它监听任何使用字符串“delete”触发发出的情况。如果它听到这个消息，就会触发一个名为 **onDeleteItem** 的函数。该函数位于 **ToDo.vue，**内，而不是 **ToDoItem.vue** 内。这个函数，如前所述，简单地过滤****数据对象**中的 **todo 数组**，以移除被点击的项目。**

**这里还值得注意的是，在 Vue 示例中，我可以简单地在 **@click** 侦听器中编写 **$emit** 部分，如下所示:**

```
<div class=”ToDoItem-Delete” @click=”$emit(‘delete’, item)”>-</div>
```

**这会将步骤数从 3 个减少到 2 个，这完全取决于个人偏好。**

**简而言之，React 中的子组件将通过 **props** 访问父函数(假设您正在向下传递 props，这是相当标准的做法，您将在其他 React 示例中多次遇到这种情况)，而在 Vue 中，您必须从子组件发出事件，这些事件通常将在父组件中收集。**

# **我们如何传递事件侦听器？**

## **反应:**

**诸如点击事件等简单事件的事件侦听器是直接的。以下是我们如何为创建新 ToDo 项目的按钮创建 click 事件的示例:**

```
<button className=”ToDo-Add” onClick={createNewToDoItem}>+</div>.
```

**这里非常简单，看起来很像我们用普通 JS 处理内嵌 onClick 的方式。正如在 Vue 一节中提到的，每当按下 enter 按钮时，设置一个事件监听器来处理它要花一点时间。这实际上要求输入标记处理 onKeyPress 事件，如下所示:**

```
<input type=”text” onKeyPress={handleKeyPress}/>.
```

**每当该函数识别出“enter”键被按下时，就会触发**createnewdoitem**函数，如下所示:**

```
handleKeyPress = (e) => {
  if (e.key === ‘Enter’) {
    createNewToDoItem();
  }
};
```

## **Vue:**

**在 Vue 中，这是非常直接的。我们简单地使用 **@** 符号，然后是我们想要做的事件监听器的类型。例如，要添加一个点击事件监听器，我们可以编写如下代码:**

```
<button class=”ToDo-Add” @click=”createNewToDoItem()”>+</div>
```

**注: **@click** 其实就是简写 **v-on:click** 。Vue 事件侦听器最酷的一点是，您还可以将许多东西链接到它们上面，比如。once 防止事件侦听器被触发多次。在编写处理击键的特定事件侦听器时，也有许多快捷方式。我发现，每当按下 enter 按钮时，在 React 中创建一个事件侦听器来创建新的 ToDo 项会花费相当长的时间。在 Vue 中，我可以简单地写下:**

```
<input type=”text” v-on:keyup.enter=”createNewToDoItem”/>
```

## **我们如何将数据传递给子组件？**

## **反应:**

**在 react 中，我们在创建子组件时将道具传递给子组件。比如:**

```
<ToDoItem key={key.id} item={todo} />
```

**这里我们看到两个道具被传递给了 **ToDoItem** 组件。从这一点开始，我们现在可以通过 this.props 在子组件中引用它们。**

## **Vue:**

**在 Vue 中，我们在创建子组件时将道具传递给子组件。比如:**

```
<ToDoItem v-for="item in list" 
  :item="item" 
  @delete="onDeleteItem" 
  :key="item.id" />
```

**完成后，我们将它们传递给子组件中的 props 数组，如下: **props: [ 'todo' ]** 。然后可以在孩子中通过它们的名字引用它们——所以在我们的例子中，是**‘todo**’。**

# **我们如何将数据发送回父组件？**

## **反应:**

**我们首先将函数传递给子组件，在调用子组件的地方将它作为一个道具进行引用。然后我们通过引用**props . whateverthefunction 被称为**——或者**whateverthefunction 被称为**(如果我们使用了析构的话),通过任何方式在子节点上添加函数调用，比如 **onClick** 。这将触发父组件中的函数。我们可以在“如何从列表中删除”一节中看到整个过程的示例。**

## **Vue:**

**在我们的子组件中，我们只需编写一个函数，将值发送回父函数。在我们的父组件中，我们编写了一个函数来监听何时发出该值，然后触发一个函数调用。我们可以在*“如何从列表中删除”一节中看到整个过程的示例。***

# **我们做到了！🎉**

**我们已经了解了如何添加、删除和更改数据，如何以 props 的形式将数据从父节点传递到子节点，以及如何以事件侦听器的形式将数据从子节点发送到父节点。当然，React 和 Vue 之间还有许多其他的小差异和怪癖，但是希望本文的内容有助于为理解这两个框架如何处理东西提供一点基础🤓。**如果你喜欢读这篇文章，一定要留下一些掌声来表达你的爱——提示，你可以留下多达 50 个！****

**如果您对本文中使用的样式感兴趣，并想制作您自己的等效作品，请随时这样做！👍**

## **为什么不用 Vue 合成 API？**

**实际上有这篇文章的更新版本使用它！[最新版**在此阅读**！但是在你点击链接之前，如果你喜欢这篇文章，请一定留下一些掌声，因为它们有助于支持我们正在做的工作。](https://sunilsandhu.com/posts/i-created-the-exact-same-app-in-react-and-vue-2020-edition)**

## **Github 链接到两个应用程序:**

**https://github.com/sunil-sandhu/vue-todo-2019**

**react ToDo:[https://github.com/sunil-sandhu/react-todo-2019](https://github.com/sunil-sandhu/react-todo-2019)**

## **我最近在伦敦网络表演上谈到了这篇文章**

**看这里的谈话！[https://www.youtube.com/watch?v=dnNF8szmxXg](https://www.youtube.com/watch?v=dnNF8szmxXg)**

## **本文的 2021 版**

**[](/i-created-the-exact-same-app-in-react-and-vue-here-are-the-differences-2021-edition-a7ebfc19a9d) [## 我在 React 和 Vue 中创建了完全相同的应用程序。以下是不同之处。[2021 版]

### React vs Vue。Vue 和 React 的并列代码对比！🎉

javascript.plainenglish.io](/i-created-the-exact-same-app-in-react-and-vue-here-are-the-differences-2021-edition-a7ebfc19a9d) 

## 本文的 2020 年版本

[](https://sunilsandhu.com/posts/i-created-the-exact-same-app-in-react-and-vue-2020-edition) [## 我在 React 和 Vue 中创建了完全相同的应用程序。以下是不同之处。[2020 版]

### 几年前，我决定尝试在 React 和 Vue 中构建一个相当标准的 Do App。这两个应用程序都是使用…

sunilsandhu.com](https://sunilsandhu.com/posts/i-created-the-exact-same-app-in-react-and-vue-2020-edition) 

## 本文的 2018 版

[](/i-created-the-exact-same-app-in-react-and-vue-here-are-the-differences-e9a1ae8077fd) [## 我在 React 和 Vue 中创建了完全相同的应用程序。以下是不同之处。

### React vs Vue。最后是 Vue 和 React 的并列代码对比！🎉

javascript.plainenglish.io](/i-created-the-exact-same-app-in-react-and-vue-here-are-the-differences-e9a1ae8077fd) 

## 好奇想知道同一个应用程序如何与 React 和 Redux 一起工作吗？点击下面的链接了解更多信息:

[](/i-created-the-exact-same-app-with-react-and-redux-here-are-the-differences-6d8d5fb98222) [## 我在 React 和 Redux 中创建了完全相同的应用程序。以下是不同之处。

### 使用 Redux 的初学者指南以及 React with Redux 和……内置的确切应用程序的并行代码比较

javascript.plainenglish.io](/i-created-the-exact-same-app-with-react-and-redux-here-are-the-differences-6d8d5fb98222) 

## 你听说过苗条吗？嗯，我们也创建了同样的应用程序在苗条了！

[](/i-created-the-exact-same-app-in-react-and-svelte-here-are-the-differences-c0bd2cc9b3f8) [## 我在 React 和 Svelte 中创建了完全相同的应用程序。以下是不同之处。

### 反应 vs 苗条。最后一个并列代码对比！因为你已经听过关于苗条的大惊小怪，现在你想要…

javascript.plainenglish.io](/i-created-the-exact-same-app-in-react-and-svelte-here-are-the-differences-c0bd2cc9b3f8) 

## 翻译

[日语](https://coliss.com/articles/build-websites/operation/javascript/same-app-in-react-and-vue-here-are-the-differences-2019-edition.html)

[韩语](https://www.vobour.com/%EB%A6%AC%EC%95%A1%ED%8A%B8%EC%99%80-%EB%B7%B0%EC%97%90%EC%84%9C-%EB%98%91%EA%B0%99%EC%9D%80-%EC%95%B1%EC%9D%84-%EB%A7%8C%EB%93%A4%EC%97%88%EC%8A%B5%EB%8B%88%EB%8B%A4-%EC%97%AC%EA%B8%B0-%EC%B0%A8%EC%9D%B4%EC%A0%90%EC%9D%B4-%EC%9E%88%EC%8A%B5%EB%8B%88%EB%8B%A4-2)

如果你想把这篇文章翻译成另一种语言，请继续这样做——当它完成时让我知道，以便我可以把它添加到上面的翻译列表中。

![](img/787be6c671be8d345dc786dad8729ce5.png)

[Click here to check out Decoded, our official YouTube channel!](https://www.youtube.com/channel/UCtipWUghju290NWcn8jhyAw)**