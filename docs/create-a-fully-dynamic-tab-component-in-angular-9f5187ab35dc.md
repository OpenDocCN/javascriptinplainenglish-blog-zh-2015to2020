# 在 Angular 中创建完全动态的选项卡组件

> 原文：<https://javascript.plainenglish.io/create-a-fully-dynamic-tab-component-in-angular-9f5187ab35dc?source=collection_archive---------0----------------------->

![](img/3b97f97161ce52075a1010457e2894a2.png)

选项卡组件是每个 web 应用程序的重要组成部分。尽管如此，提供它的开源库还是太少了。

[Angular Material](https://material.angular.io/) 提供了一个非常灵活和强大的选项卡组件，但是它会随着材质主题、额外的依赖项和材质指南而变化。

创建选项卡组件就像抛物线。起初，它看起来很复杂，在创建了一些基本功能后，它看起来简单多了，但在尝试添加动态功能后，它又变得复杂了。

但是作为一个整体，不需要太多的努力就可以创建一个完全动态和灵活的选项卡组件。

我们自己创造吧。

# 组件结构

组件结构将以如下方式

*   它将是子选项卡的组组件
*   `app-tab-item-component`它将是一个单独的选项卡组件，将包含在 app-tabs-component 中
*   `app-tab-label-component`它将用于单个标签组件的标题目的
*   `app-tab-body-component`它将用于选项卡组件的内容目的

下面是我们的组件的样子

[](https://stackblitz.com/edit/angular-tabs-component-tony?embed=1&file=src/app/app.component.ts) [## 角形标签-组件-Tony-stack blitz

### 导出到 Angular CLI 的 Angular 应用程序的启动项目

stackblitz.com](https://stackblitz.com/edit/angular-tabs-component-tony?embed=1&file=src/app/app.component.ts) 

现在让我们一个接一个地编写这些组件

# TabLabelComponent

`TabLabelComponent`负责，在标签页组件中呈现标签页标题。无论在`LabelComponent`中传递什么，都应该呈现在 Tabs 头中。所以如果我们写

```
<app-tab-label>

   <h1> Hello I am tab header </h1></app-tab-label>
```

我们必须在标签头中插入`<h1> Hello I am tab header </h1>`。我们如何做到这一点？使用`ng-template` *和* `ng-content` 手法

这是我们写的

无论通过什么，在`tab-label-component`*里面，我们都把它包装起来，在`ng-template` 里面，并持有它的一个引用。我们为什么需要它？因为我们将使用这个模板引用来呈现选项卡标题中的内容。现在，这可能有点混乱，但是，跟着我，你会发现一切。我们的下一个组件是`TabBodyComponent`*

# *TabBodyComponent*

*`TabBodyComponent` 具有与`TabLabelComponent` *完全相同的签名。唯一不同的是，`TabBodyComponent` 负责呈现正文内容，而不是标题。没有任何进一步的解释，让我们添加一些代码**

*除了，命名部分没有区别。*

*现在我们可以探索`tab-item-component`*

# *TabItemComponent*

*`tab-item-component` 只是将标签和主体组件组合在一起，我们在`tab-item-component`中通过`ng-content`并持有它们的组件引用*

*此外，我们只有一些额外的字段，如`label` 和`isActive`。顾名思义，如果我们希望在`tabs-header`而不是复杂的 HTML 中有一些字符串类型的值，就使用标签字段，并且`isActive` 字段是一个检查器，这个选项卡是活动的还是不活动的。*

*现在我们到了最重要的部分，让我们来探究一下这些组件的主:`tabs-component`*

# *选项卡组件*

*我们需要在`tabs-component`中做的是，我们必须使用`@ContentChildren`获取所有的`TabItems` ，并在`tabs-header`区域中呈现它们的`LabelComponent` ，我们还必须监听 tab-header 单击事件，以相应地激活 tab。*

*也许，乍一看，它看起来很可怕，但不要担心，它很快就会变得足够清晰。*

## *呈现选项卡标题*

*为了呈现选项卡标题，我们使用`@ContentChildren`抓取传递的选项卡*

```
*@ContentChildren(TabItemComponent)  tabs: QueryList<TabItemComponent>;ngAfterContentInit(): void {  
  this.tabItems$ = this.tabs.changes      
       .pipe(startWith(""))      
       .pipe(delay(0))      
        .pipe(map(() => this.tabs.toArray()));}*
```

*(我们必须在第一次用`startWith` 触发一些东西，因为在`QueryList`第一次初始化时，changes 事件是无声的)*

*当我们按住所有的`TabItems`时，我们就可以平滑地将它呈现在标签页标题中:*

*我们正在遍历所有的标签项，检查是否存在`TabLabelComponent` (所以用户想要呈现一些动态 HTML)，如果存在，我们就使用`*ngTemplateOutler`技术来呈现它的 HTML。例如，如果我们在`app-component`中写下这样的内容*

*我们将有以下关系*

*`*TabsComponent => TabItemComponent => TabLabelComponent => LabelContnet => First Tab*`*

*我们所做的就是在标签页的标题区域中渲染它。*

*但无论如何，我们可以在标签输入中传递一些值，它会被打印出来，而不是动态 HTML。*

*现在让我们来看看如何呈现内容*

## *呈现内容*

*呈现内容要简单得多，我们只需监听 tab header click 事件，激活适当的选项卡项，并呈现其主体组件的内容。*

# *摘要*

*编写自己的选项卡组件有很多好处，比如可以添加任意多的特性，按照自己的意愿设计样式，还可以了解 Angular 内部的动态组件呈现。*

# ***简明英语笔记***

*你知道我们推出了一个 YouTube 频道吗？我们制作的每个视频都旨在教给你一些新的东西。点击 [**点击**](https://www.youtube.com/channel/UCtipWUghju290NWcn8jhyAw) 查看我们，并确保订阅该频道😎*