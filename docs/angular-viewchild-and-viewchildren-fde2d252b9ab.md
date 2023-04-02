# 角度视图子视图和视图子视图

> 原文：<https://javascript.plainenglish.io/angular-viewchild-and-viewchildren-fde2d252b9ab?source=collection_archive---------0----------------------->

## 关于 ViewChild 和 ViewChildren 您需要知道的一切

![](img/172e99fca1fa8f9149b047ac95da1990.png)

Working with ViewChild and ViewChildren

TechnoFunnel 发表了另一篇文章，介绍如何理解角度组件中的 **@ViewChild 和@ViewChildren** 的用法。我们将看看如何有效地使用这些功能来实现您想要的结果。

组件可以获得对元素或指令的引用，因此我们可以直接访问它。该指令可以是 Angular 自己的，也可以是用户定义的自定义指令。我们有时可能需要直接访问元素或组件，并操纵组件或元素的属性。这在某种程度上等同于使用 JavaScript 获取 DOM 元素并更新 DOM 元素的属性和行为。

# 使用 Angular 的 ViewChild

**@ViewChild 可以用来获取角度组件内部渲染的 DOM 元素的引用。**我们可以使用 DOM 元素的引用来操纵元素属性。要获取组件，我们需要指定选择器。

Access this code [here](https://gist.github.com/Mayankgupta688/3b9c5cd65490de5d8bef0b93953583a7)

上面给出了使用普通 JavaScript 或 Angular 的 **@ViewChild** 访问 DOM 元素的代码。使用 JavaScript，我们可以使用一个选择器来提取组件。根据下面的语句，我们尝试使用元素 ID 来访问元素。

`<div id=”someElement”>Sample Code</div>`

下面给出了一个`div`元素，它的模板引用被标记为`someElement`。模板引用以#开头。在 Angular 的情况下，我们可以使用这些模板引用来访问元素。可以通过使用 **@ViewChild** 和为其指定的模板引用变量来检索元素引用。让我们来看看实际情况。

`<div #someElement>Sample Code</div>`

# 什么时候可以引用这个 ViewChild 变量？

一旦`View`被初始化，对 **@ViewChild** 变量的引用就被赋值。Angular 提供了一个名为`ngAfterViewInit`的生命周期钩子，一旦`View`被初始化，这个钩子就会被调用。一旦`View`被初始化和呈现， **@ViewChild** 就可以使用模板引用访问元素。它为我们提供了对元素/指令的访问。

让我们看看代码:

Access this code [here](https://gist.github.com/Mayankgupta688/703891c75a2d7ac5a569ddc1efe0ea76)

在上面的代码中，需要考虑以下几点:

1.  我们可以使用 **@ViewChild** 来访问具有模板引用变量`“someElement”`的输入元素。
2.  ViewChild 元素`domReference`只有在 DOM 元素被渲染后才能访问它。一旦呈现了组件，就会调用一个名为`ngAfterViewInit`的生命周期事件。因此，我们可以在这个生命周期事件或以后的生命周期事件中引用该元素。
3.  **@ViewChild** 可以让用户访问呈现的`View`的原生 DOM 元素。使用这个 DOM 引用，我们可以访问和修改 DOM 属性，比如操作样式、内部文本、值和其他与引用的元素相关的属性。
4.  我们直接访问 DOM，所以我们与浏览器紧密耦合。因此，我们可能无法使用服务器端渲染来使用这些引用，这也可能带来安全威胁。

# 使用角度方向访问元素

我们可以使用类似`NgModel` 的角度指令和 **@ViewChild** 。让我们寻找使用 **@ViewChild** 访问角度指令的需求和用例场景。

Access this code [here](https://gist.github.com/Mayankgupta688/db14a303c3f94ee2c85b89b1992ace9e)

我们可以访问 **@ViewChild** 中的`NgModel`指令，并订阅值的变化。上面给出的是代码，我们试图使用角度指令`NgModel`访问一个元素。ID 为`userName`的元素被添加了指令`NgModel`。使用 **@ViewChild** ，我们将跟踪输入元素中任何值更新的变化。

我们获取对输入元素的`NgModel`数据结构的引用，通过引用，我们可以访问它的状态信息，比如它是否被修改过，或者值是否有效。

可以在`ngAfterViewInit`生命周期内访问它。我们可以接触到所有的国家信息。它还提供有关属性的任何更新的信息。此引用是只读的。它给了我们访问可观察对象的权限，我们可以用这个可观察对象来订阅`valueChanges`可观察对象。

每当值被更新时，与之相关联的回调函数被触发，我们可以添加定制逻辑来响应更新。

# 使用 Angular 的视图子对象

使用 **@ViewChildren** 与 **@ViewChild** 类似，但两者的区别在于 **@ViewChildren** 提供了元素引用的列表，而不是返回单个引用。它用于引用多个元素。然后，我们可以迭代该变量引用的元素列表。

以下选择器可以与 **@ViewChildren** 一起使用:

## 1.我们可以使用带有角度指令的 ViewChildren，比如`NgModel`

我们可以在 ViewChild 中使用像`NgModel`这样的内置指令。它将给出所有附加了指令`NgModel`的元素的列表。

View this code [here](https://gist.github.com/Mayankgupta688/3c1f83f52957e46801f7e8981d701315)

上面给出的是使用角度指令`NgModel`提取元素的代码。可以检索包含指定角度方向的所有组件，并对其进行进一步评估。

## 2.使用子组件访问元素

类似于使用带有 **@ViewChild** 的指令，我们可以使用子组件名来访问使用 **@ViewChildren** 的元素。这就需要我们在主组件中有一些子组件(例如，`user-details`)。让我们用一个例子来看看下面的场景。

View this code [here](https://gist.github.com/Mayankgupta688/e75b17c94b7fa34486913a353a451cb9)

上面的代码获取了父组件中包含的所有子组件引用的列表。

然后，我们可以使用这些引用来完成定制逻辑。然后，开发人员可以使用该列表来完成进一步的任务。

## **3。使用模板参考变量**

元素中的多个组件可以包含相同的模板引用。如果我们在多个地方使用一个模板引用，我们会收到模板中同一个模板引用变量所引用的所有组件的引用列表。

View this code [here](https://gist.github.com/Mayankgupta688/88bfd250bffba72a1de745f94bb8cec0)

上面的代码包含多个具有相同模板引用变量的组件。 **@ViewChildren** 将使用户能够访问引用模板引用`applicationInfo`的所有组件。

## 4.**访问多个模板参考变量**

选择器可以是一组模板引用。我们可以指定多个模板引用。从组件中检索包含列表中指定的模板引用的所有组件。

View this code [here](https://gist.github.com/Mayankgupta688/86cc2ea1000605e7e04edb6d1b365969)

在上面的代码中，我们在 **@ViewChild** 中添加了模板引用变量列表。包含列表中包含的元素引用的所有组件都将被检索，并且可以使用变量名进行访问。

# 使用 ViewChild 和子组件

ViewChild 和 ViewChildren 可用于访问子组件的属性和方法。使用 **@ViewChildren** 和 **@ViewChild** ，我们可以获得子组件的引用，这进一步给出了对所有属性和方法的访问。这可以使父组件访问子组件，并在它们之间实现通信。

让我们看一下代码，以便更好地理解这个概念:

View this code [here](https://gist.github.com/Mayankgupta688/5385ef1d09e83214bdfbb8ae7802b674)

上面的代码包含一个简单的子组件，它有属性`userName`和函数`updateUserName`，规定用来更新组件的`userName`属性。

现在让我们添加一个新组件，它将作为上述`ChildComponent`的父组件。我们将研究使用`@ViewChild`从父组件访问子组件的属性和方法的代码。让我们看看下面的代码。

View this code [here](https://gist.github.com/Mayankgupta688/444608affbdb56530a0399130c8c2ca0)

上面的代码代表一个父组件。在为父组件指定的模板中，我们添加了一个子组件。

子组件用模板引用变量标记。我们可以使用这个模板引用来访问子组件的属性和变量。

`@ViewChild(“userInformation”) childComponentReference: any;`

**@ViewChild** 可用于访问具有模板引用`userInformation`的子组件，模板引用代表子组件。使用这个`childComponentReference`，我们可以进一步访问属性并调用子组件的功能，如上所述。

# 结论

我希望这篇文章对你有益。欢迎在下方留言评论。

*更多内容请看*[***plain English . io***](http://plainenglish.io)