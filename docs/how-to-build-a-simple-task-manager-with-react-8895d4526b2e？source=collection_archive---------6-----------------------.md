# 如何用 React 构建一个简单的任务管理器

> 原文：<https://javascript.plainenglish.io/how-to-build-a-simple-task-manager-with-react-8895d4526b2e?source=collection_archive---------6----------------------->

![](img/970326b474d50f508ef785e8426143d4.png)

对于“*你有什么反应？”这个问题，我厌倦了用“*不*来回答。所以我决定学习它，用这篇文章记录我构建的项目，并希望帮助其他面临同样挑战的人学习 React。*

当学习新东西时，我选择从最简单的例子开始，这次是一个*待办事项列表*，即*任务管理器。*

顺便说一句，在这个旅程中，我意识到了 React 为什么是一个令人惊叹的库！:)

# 设置

我们将使用运行 React 应用程序所需的 [node.js](https://nodejs.org/en/) 和 [npm](https://www.npmjs.com/get-npm) (它们在一起)。

此外，我们将使用一个公共 API 来发出一些获取和存储数据的请求。有问题的 API 是 [JSONPlaceholder](https://jsonplaceholder.typicode.com/) 。

我们将通过运行以下命令创建一个 React 应用程序:

```
***npx******create-react-app <name-of-your-app>***
```

> [没错，是](https://medium.com/javascript-in-plain-english/yes-its-npx-not-npm-the-difference-explained-58cbb202ec33) `[*npx*](https://medium.com/javascript-in-plain-english/yes-its-npx-not-npm-the-difference-explained-58cbb202ec33)` [，不是](https://medium.com/javascript-in-plain-english/yes-its-npx-not-npm-the-difference-explained-58cbb202ec33) `[*npm*](https://medium.com/javascript-in-plain-english/yes-its-npx-not-npm-the-difference-explained-58cbb202ec33)` [:)](https://medium.com/javascript-in-plain-english/yes-its-npx-not-npm-the-difference-explained-58cbb202ec33)
> 顺便说一下， [npx](https://www.npmjs.com/package/npx) 是从 npm5.2 版本开始捆绑 npm 来执行节点包的工具。

好了，现在我们已经初始化了 React 应用程序，我们可以开始创建所需的组件了。

# 实施

出于任务管理器的目的，我们将只创建**三个组件** ( ***任务列表*** 、 ***加载器*** 和主 ***TaskApp*** )只是为了简单明了。

## 任务列表组件

Task List Component

我们的 TaskList 组件实际上是一个无序列表`<ul>`。

它由一个*构造器*、*三个方法*和默认的*渲染*方法组成。
为了更清楚地了解每种方法的作用:

*   ***构造函数*** :调用 *super(props)* ，它引用父类构造函数及其属性，同样，这里我们将`this`实例绑定到使用 TaskList 组件状态的方法，这样我们就可以在这些方法中访问它；

> 如果我们在某些方法中不需要**状态**，那么**就不需要**将`***this***`实例绑定到它上面。

*   ***displayTask:*** 这个方法，顾名思义，就是显示一个特定的任务，实际上就是在任务列表中添加一个`<li>`项目。
    它接收一个*任务*作为参数，这个参数是我们从任务列表中得到的一个对象，它是作为对 API 请求的响应而接收的。这个对象是我们获取所有需要的数据的来源，这些数据应该显示在单个任务上，如 *id* 、*标题*、任务完成后的*信息、*等；
*   ***updateTaskStatus:***该方法接收一个*任务*作为参数，并通过向 API 发送一个请求，将其状态设置为*已完成*，其中*已完成*字段为*真，*并且在请求成功时，我们删除任务，表示现在任务已完成；
*   ***finishTask:*** 该方法接收一个*事件*作为参数，从中我们得到*当前选中的任务*并将其传递给***updateTaskStatus***方法；
*   ***渲染* :** 默认的渲染方法有定义组件渲染时发生什么的代码。在这种情况下，当呈现 TaskList 组件时，会发生以下情况:

```
**1.** We check if the number of items (*tasks*) is not equal to zero i.e. if the array of items is not empty;**2.** If true, we loop through all of them and we display each *task*   i.e. we call the ***displayTask*** method for each task;**3.** Else, we’re showing a notification to the user that they don’t have any tasks at the moment.
```

## 加载器组件

Loader Component

加载器组件是必需的，因此我们可以向用户显示在发送请求的同时正在做什么。

该组件由两种方法组成:

*   ***updateLoaderVisibility:***更新*加载器* ( `true -> Visible` & `false -> Hidden`)的*可见*属性的状态；
*   ***render:*** 定义加载器本身的 HTML 结构，并根据`visible`属性的当前状态设置加载器是否隐藏；

## 任务应用程序组件

Task App Component

TaskApp 组件在我们的例子中是父组件，它由多个*方法*、*构造器*和默认的*呈现*方法组成。
下面详细介绍了它们的功能:

*   ***构造器:*** 和往常一样，这里我们调用 *super(props)* 并将`this`实例绑定到 TaskApp 组件的方法上。
    同样，在这个构造函数中，我们设置了属性的默认*状态*，这些属性是 items ( *tasks* )、current *task* 和对 *Loader* 组件的*引用*以及对 *task input* 的*引用*。在构造函数中可以看到，这三个属性的默认状态是:`items=[]` ( *我们默认没有任务*)和`task=''` ( *我们默认没有要添加的任务*)。

> **React 引用**是一种访问和操作 DOM 节点或 React 组件的方式。

在我们的例子中，我们使用它们:

```
**1.** To change Loader’s *visibility* *state* i.e. to switch the Loader component from hidden to shown and vice versa;**2\.** To focus the task input when needed;
```

> 更多关于 React Refs [的信息请点击](https://reactjs.org/docs/refs-and-the-dom.html)。

*   ***render:*** 在这个组件的 render 方法中，我们包含了 *Loader* 组件，它定义了 *loaderInstance* 的 *Ref* ，以及 *TaskList* 组件，我们与它共享`items`的当前状态以及一个*表单*，我们将使用它向列表添加新任务。表单本身非常简单，仅由一个*文本输入*和一个*提交按钮*组成。
    在输入改变时，我们调用***On inputchange***方法，输入的值是`task`属性的当前状态。在提交表单时，我们调用 ***addTask*** 方法。
*   ***on input change:***该方法接收*事件*作为参数，从中获取输入的当前*值*，并将`task`的当前状态设置为等于该值；
*   ***addTask:*** 这个方法是在点击表单的提交按钮时执行的。顺便说一下，它还接收事件作为参数，这只是为了防止提交表单事件的默认行为。除此之外，该函数还会检查`task`的当前值是否为空，如果是，它会警告用户任务(任务输入)的值不能为空，否则，如果不为空，它会显示加载程序，然后调用 ***storeTask*** 方法，最后它会将`task`的当前*状态*设置为空字符串，这样就为下一个应该添加的任务做好了准备。
*   ***storeTask:*** 这个方法向 API 发出一个 POST 请求，我们用它来存储任务和所有需要的数据。
    所有*任务*默认保存为*未完成*，默认 *userId* (任务所属的用户)为 *1* 。
    在请求的*成功*时，我们得到响应，其中包含存储的任务，我们将它添加到`items`的数组中，即我们将`items`属性的当前状态更改为多一个任务。
*   ***fetchTasks:*** 该方法向 API 发出 GET 请求，通过该请求我们得到一个*任务*列表，这些任务是`*user*``*id=1*`*的任务，当*请求成功*时，我们将`items`的当前状态设置为等于我们从请求中得到的响应列表，最后，我们隐藏了被设置为在 ***组件中显示的加载程序****
*   ****componentDidMount:***这个方法是 TaskApp 组件渲染后执行的方法。在这里，我们只显示加载程序并调用 ***fetchTasks*** 方法。基本上，这是这个小应用程序的主页加载时发生的事情。*

# *CSS 组织*

*简单解释一下 CSS 文件的组织。*

*也就是说，为了遵循基于组件的开发原则，我们已经创建了*三个*单独的 css 文件(每个组件一个: [TaskList.css](https://github.com/siljanoskam/react-task-manager-app/blob/master/src/TaskList.css) 、 [Loader.css](https://github.com/siljanoskam/react-task-manager-app/blob/master/src/Loader.css) 和 [App.css](https://github.com/siljanoskam/react-task-manager-app/blob/master/src/App.css) )，并且我们正在为每个组件分别导入相应的所需 CSS 样式。*

# *结论*

*构建这个小小的应用程序很有趣，希望跟随它你也能学到一些东西。*

*像任何应用程序一样，有一些改进要做，请随意使用和编辑它。你可以在这个 [**Github repo**](https://github.com/siljanoskam/react-task-manager-app) 里找到代码。*

*任何被省略的步骤都可以在**报告**的**自述文件**中找到，或者你可以在评论中询问。*

*继续编码，做令人敬畏的事情，保持快乐！😊*