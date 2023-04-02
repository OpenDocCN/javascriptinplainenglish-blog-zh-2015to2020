# 外部点击:为无缝交互编写 Vue 指令

> 原文：<https://javascript.plainenglish.io/v-outside-click-writing-vue-directives-for-seamless-interactions-99abdb362813?source=collection_archive---------0----------------------->

## Vue 的流行的自定义指令新实施，并介绍了这一关键的 Vue 掌握。[已更新]

![](img/b58e327764faa2d2626a55bb81224911.png)

Directing: Photo by [Iewek Gnos](https://unsplash.com/@imkirk?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) on [Unsplash](https://unsplash.com/s/photos/direct?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)

## TL；博士:

+自定义指令允许**可重用的交互**与**元素**
+注册指令与**vue . directive**(' directive-name '，directive object)
+使用指令与**v-directive-name**:arg . modifier . modifier 2 = " value "
+参数**、修饰符、** & **值**都提供了传递指令数据的方法
+指令由 b…g . value&b…g . modifiers
+用 **el** &访问绑定元素，用 **vnode**
访问 Vue 实例+利用**事件监听器** &其他技巧充分利用指令
—
+**V-外部点击**如果在
之外点击元素，则触发事件+这也很重要 使用**虚拟类名**来表示被排除的元素
+参见本文底部**的完整**源代码****

# 介绍

{Vue Masters: Nuts & bolts 开始于 [**实现外部点击检测**](#77f1) 源代码在最末尾}
和我的大多数文章一样，这篇文章开始于我花费额外的时间试图实现一些我在互联网上找不到好文档的东西。然而，这一次我实际上有了一个不错的基础，但它不足以满足我的需求。因为我相信这个新的实现比我最初发现的要好得多，所以我认为有必要分享一下我对常用的“外部点击”指令的实现，同时介绍一下自定义指令。这个例子需要对 [Vue.js](https://vuejs.org/v2/guide/) 有适度的理解。

# 关于自定义指令

## 什么是自定义指令:

Vue 实现了[自定义指令](https://vuejs.org/v2/guide/custom-directive.html),作为开发人员在元素级别构建重用的一种方式。这使得开发人员可以为默认的元素交互创建自定义的反应，比如 v-click。任何 DOM 交互都可以很容易地写入自定义指令，并附加到元素上，而不管组件是什么。

## 注册指令:

指令可以全局注册，也可以本地注册(我在这里显示的是全局)。就像组件或插件一样，有一个函数可以将指令附加到根 Vue 实例。因此，新的应用程序实现可能如下所示:

```
import Vue from 'vue'
import App from './views/App'
import **OutsideClick** from '**./directives/OutsideClick**'
...Vue.component(...)Vue.use(...)**Vue.directive('outside-click', OutsideClick)**const app = new Vue({
...App
})export { app }
```

该指令附有一个名称“outside-click”{将被称为 v-outside-click}和一个定义该指令的对象 OutsideClick {从单独的文件导入}。

## 使用您的自定义指令:

显然，您希望实现一个自定义指令，以便可以使用它。幸运的是，这个最重要的部分是最容易的。一旦指令附加到父实例或组件，就可以像元素中的任何其他 Vue 指令一样引用它:

```
<div **v-custom-directive**:arg.modifier.modifier2="value">
...
</div>
```

指令捕获它所附加的元素，并将不同的参数传递给指令的绑定参数。第一个参数是“arg”，它可以是一个原始值，也可以是动态的(访问组件属性)，方法是将其包装在[]中，如:v-directive:[arg]…接下来是修饰符，它们充当真标志。可以有多个修饰符，如果存在，它将被添加到一个对象中，如{modifierName:true}。最后是价值论证。这是动态的，可以返回计算、方法、对象或原始字符串的组合。我的建议是将值做成一个对象，这样就可以清楚地知道传递的是什么参数:{handler:handlerMethod，excludeList: ["list "，" of "，" things"]}。自定义指令不一定需要向其传递任何参数，但是这些参数允许您将特定于组件的值和逻辑绑定到响应指令中。在这个具体的例子中，我同时传入了值和方法。该指令能够提取排除列表，并且只对不在列表中的元素做出反应。它还能够调用 handler 方法，允许元素基于 handler 方法中的特定执行逻辑来影响组件中的更改。通过这种方式，我可以让一个元素在外部注册单击时收缩，但让另一个元素使用完全相同的指令隐藏该元素。

现在我们只需要知道如何构造指令，使其真正有用。

## 自定义指令的剖析:

最简单地说，指令定义只是一个对象(就像 Vue 中的其他东西一样),附加到指令名上。属性表示元素生命周期中与“挂钩”相关的功能。

```
//a directive definition object with bind hook
const CoolDirective = {
   **bind**: function(){
      //do stuff here
   }
}export default CoolDirective
```

因此，一个指令定义可能看起来像这样，你可以导出它，然后导入到你的应用程序中。您也可以将整个对象包含在您的指令包含中，而不是在一个单独的文件中定义它，但是这样会更难看。

**生命周期挂钩**包括**绑定、插入、更新、组件更新**和**解除绑定**。通过向对象添加具有这些名称之一的属性，将在生命周期触发时调用该函数。例如，bind 属性上的函数只有在 DOM 将指令附加到元素时才会被触发。而 inserted 将在子元素插入父元素时触发。如果将对父节点的引用传递到指令中，这种差异可能很重要。当这些生命周期挂钩被触发时，函数被调用并被传递参数。

```
{
   bind: function(**el, binding, vnode**){
      //you can now access the hook arguments here
   }
}
```

**这些指令钩子参数**包括 el、binding、vnode 和 oldVnode。它们取决于该函数附加到哪个钩子上。例如，unbind 只能访问 el 参数。el 参数就是指令的威力所在:el 允许您修改绑定所附加到的元素。因此，钩子函数有一个非常干净简洁的元素访问，以及从组件内定义接收逻辑和变量的方法。这是通过绑定(.值，。arg，还有。修饰语)前面提到过。所有这些都可以在“绑定”钩子参数中找到。

```
//instantiation:
<div v-custom-directive**:ARG.MODIFIER**="**VALUE**"></div>//directive definition
{
   bind: function(**el, binding, vnode**){
      value = binding.value //equals the component value of VALUE
      modifierVal=binding.modifiers["**MODIFIER**"] //true if exists
      arg= binding.arg //will be literal "ARG" unless made dynamic
   }
}
```

vnodes 提供了对 Vue 实例的访问。整个绑定参数和 vnodes 都是只读的。

那么，我们如何利用所有这些论点呢？就像 [mixins](https://vuejs.org/v2/guide/mixins.html) 指令允许我们制作可重用的函数来与组件交互。但是现在我们也可以访问特定的元素。
**函数**允许我们执行由这些元素生命周期钩子触发的逻辑。因为您可以访问元素，所以可以更改可见性或颜色等值。因此，您可以在插入元素时将元素的颜色设置为给定值，如下所示:

```
{
   inserted: function(el, binding, vnode){
       el.style.color=binding.value
   }
}
//directive instantiation => <div v-custom-color="#ffffff"></div>
```

当然，这可以通过使用 Vue [风格绑定](https://vuejs.org/v2/guide/class-and-style.html#Binding-Inline-Styles)来实现。那么，你能用给定的工具做什么更有用的事情呢？vnode 让我们可以访问我们正在处理的 Vue 实例，通过使用上下文，我们可以访问拥有该元素的组件。考虑到这一点，我们可以从这个指令中访问组件中的方法和数据。因此，也许我们有一个特殊的方法，我们想运行基于元素生命周期。例如，我们可以在每次更新组件时对元素运行更新方法(在 binding 参数中命名):

```
{
     componentUpdated: function(el, binding, vnode){
       //get the component method with the given name
       vnode.context[binding.value](el)//send the element for edit
     }
}
//there is a simpler way of passing the method through binding.value
//instead of just the name, vnode usage is for example sake
```

这样，我们可以在组件中定义自定义方法，但在特定元素上执行它，而不会出现难看的文档查询或代码重复。但是这仍然在元素生命周期的有限范围内。我们如何打破这种局面。
在我看来，指令最强大的用途是初始化一个事件监听器，并赋予它在特定事件上运行的特殊方法。这提供了一个复杂的关系，当一个元素被绑定时，一个事件监听器被创建来调用组件方法，该组件方法被传递要被修改的元素。它看起来像这样:

```
{
   bind: function(el, binding, vnode){
    handleEvent = e => {
        e.stopPropagation()
        if(el.contains(e.target)){
            binding.value(el)// value is the method we execute
        }
    }
      document.addEventListener('click', handleEvent)
   }
}
```

这是我的外部点击指令和其他指令(如滚动指令)的基础。请注意，这个示例并不完整，因为当元素未绑定时，我们没有正确地解除事件侦听器的绑定。您可以在下面的完整示例中看到这一点。

这是自定义指令的基础，你可以在这里找到完整的文档[。但是现在，让我们更深入地看看具体的外部单击实现，以获得这些简单指令如何如此强大的第一手资料。](https://vuejs.org/v2/guide/custom-directive.html)

# V-外部单击指令

这个特定的定制指令似乎很受欢迎，是现代 UX 设计的关键部分。你会发现它有很多不同的名字，比如 v-closed，v-click-outside，v-click way 等等。但是它们都试图完成相同的目标:当用户在附加指令的元素之外单击时，执行一个动作。

## 外部点击检测背后:

外部点击检测允许当你点击它的外部时关闭一个下拉菜单或弹出菜单，当你点击离开它时调整一个元素的大小或改变它的内容。要捕获点击，您必须注册 click 的事件侦听器(可能还会触发事件，您将会看到)。在事件处理程序中，您需要检查事件侦听器元素(被单击的元素)是否与指令绑定元素匹配。允许忽略被点击的元素也是有利的，即使它们不是指令元素。

当一些其他元素打开您的指令元素时，这是特别必要的。否则，该指令会将另一个元素注册为外部单击，并触发您的处理程序(这可能是关闭，导致该指令元素无用)。我在这里遇到了现有实现的问题。所有建议的用于定义排除元素的方法都使用了组件引用，但是这有一个限制，即只能排除一个组件中的元素。它看起来会像下面这样:

```
let clickedOnExcludedEl = false
        exclude.forEach(refName => {
          // We only run this code if we haven't detected
          // any excluded element yet
          if (!clickedOnExcludedEl) {
            // Get the element using the reference name
            let excludedEl = vnode.context.$refs[refName]
            // Get the actual element if it is a Vue component
            excludedEl = (excludedEl instanceof Vue)
              ? excludedEl.$el
              : excludedEl
            if (excludedEl) {
              // See if this excluded element
              // is the same element the user just clicked on
              clickedOnExcludedEl = excludedEl.contains(e.target)
            }
          }
        })
```

这看起来非常复杂，最终只允许一个级别的元素被排除在触发关闭之外。为什么？因为它只检查元素所在的组件上下文中的引用。然而，我使用深度组件树，通常需要两个元素(即一个在组件中，一个在几层之上的标题中)来打开组件，而不是触发立即关闭。因此，经过大量思考后，我开发了自己的解决方案，其关键要素是公开目标元素类。

## 实施外部点击检测:

使用一个定义良好的对象通过指令绑定值传递我的参数，我从发送一个 exclude 元素列表和 handler 方法开始。我还在元素上定义了一个 id，它应该是惟一的，以后会用到。

```
<cardElement 
  **:id="someUniqueId"** v-if="showCard" 
  **v-outside-click="{
    exclude: ['outside-click-exclude'],
    handler: exitCard
  }"** >
</cardElement>
```

我还为打开卡片的按钮添加了一个虚拟类“外部点击排除”。

```
<button class="**outside-click-exclude**' @click="openCard">...
```

您还希望在组件中有一个处理程序方法，当正确的点击被触发时执行该方法。大概是这样的:

```
...
methods:{
...
  **exitCard**(){
    this.showCard = false
  },
  openCard(){
    this.showCard = true
  }
...
}
...
```

现在我们可以看实际的指令定义了。我们将使用两个钩子，bind & unbind，并且我们需要定义一个共享变量，该变量将包含要传递给事件侦听器的处理程序。

```
// create variable for our handler to be shared between the bind & unbind hooks
var **handleOutsideClick** = {} //an object to hold named functionsconst OutsideClick = {
  // this directive is run on the bind and unbind hooks
  **bind** (el, binding) {
   ...
  },
  **unbind** (el) {
   ...
}  
export default OutsideClick
```

当然，要让我们的处理程序操作点击动作，我们需要将它附加到点击监听器。在绑定时，我们希望将处理程序附加到侦听器上，而在解除绑定时，我们希望移除侦听器。请注意，这是事情变得有点棘手的地方。[事件监听器](https://www.w3schools.com/js/js_htmldom_eventlistener.asp)接受任何函数，但是要正确移除处理函数，它必须是一个命名函数。**如果同一个文档中的多个元素使用这个指令**，它们必须有不同命名的函数，这样移除才能正常工作。为此，我使用了存储在对象中的函数，以便可以根据元素 id 动态设置名称，如下所示:

```
...
bind (el, binding) {
   ...
   // Register our outsideClick handler on the click/touchstart 
       listeners
   **document.addEventListener('click', handleOutsideClick[el.id])**
   **document.addEventListener('touchstart',handleOutsideClick[el.id])**
},
unbind (el) {
    // If the element that has v-outside-click is removed, unbind 
      it from listeners
    **document.removeEventListener('click', handleOutsideClick[el.id]**)
    **document.removeEventListener(
      'touchstart',
      handleOutsideClick[el.id]** )
}
...
```

现在我们还需要定义这个处理程序在点击事件上要做什么。这将在 bind 钩子内部完成，在那里我们可以访问 el 和 binding:

```
...
bind(el, binding){
      **handleOutsideClick[el.id]**= e => {
      e.stopPropagation()
      // extract the handler and exclude from the binding value
      const { handler, exclude } = **binding**.value
      // set variable to keep track of if the clicked element is in 
         the exclude list
      let clickedOnExcludedEl = false
      // if the target element has no classes, it won't be in the 
         exclude list: skip the check
      if (e.target._prevClass !== undefined) {
        // for each exclude name check if it matches any of the 
           target element's classes
        for (const className of exclude) {
          clickedOnExcludedEl =
            e.target._prevClass.includes(className)
          if (clickedOnExcludedEl) {
            break // once we have found one match, stop looking
          }
        }
      }
      // don't call the handler if our directive element contains 
         the target element
      // or if the element was in the exclude list
      if (!(clickedOnExcludedEl || **el**.contains(e.target))) {
        handler()
      }
    }
    ...
}
```

我们在这里和这个负责人做什么？
因为我们不只是想将我们的原始组件方法直接传递给点击监听器(否则任何点击都会尝试调用您的方法),所以我们首先需要过滤掉我们不想触发它的任何点击。
这从遍历由排除列表给出的所有类名开始，并检查目标元素(由监听器传递)是否有这些类中的任何一个。使用`e.target._prevClass`访问元素类。一旦我们找到一个，我们继续前进。
**注意**:要认识到，既然我们使用的是类名，那么你整个 app 中的任何元素都可以被过滤掉。这是我需要让外部点击为我自己工作的技巧，我相信这是一个更干净的实现。

在获得元素是否被排除的真值后，我们接着查看指令元素(我们想要触发外部点击的元素)是否包含目标元素。如果是，这意味着我们在指令元素内部单击，并且不想调用处理程序。如果这两种情况都不成立，则调用绑定传入的处理程序方法。我的示例方法将元素的条件值设置为 false，将其从页面中取出。

这就是实现外部单击指令所需的全部内容。如果你觉得我错过了什么，请告诉我。否则，掌声是受欢迎的。如果你有其他你想了解的话题，请告诉我，我会考虑如何解决。指令定义的完整源代码如下。谢谢，继续编码；马库斯

# 完整源代码: