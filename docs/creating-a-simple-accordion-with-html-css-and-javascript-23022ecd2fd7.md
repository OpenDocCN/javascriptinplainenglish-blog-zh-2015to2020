# 用 HTML，CSS 和 JavaScript 创建一个简单的手风琴

> 原文：<https://javascript.plainenglish.io/creating-a-simple-accordion-with-html-css-and-javascript-23022ecd2fd7?source=collection_archive---------0----------------------->

![](img/25b829c846963b63e1db31d5fbaba2bb.png)

Photo by [Domenico Loia](https://unsplash.com/@domenicoloia?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

对于我目前正在做的一个客户项目，我想使用一个 accordion 来保存某些信息，这样用户可以更轻松地查看所有内容。我寻找不同的博客文章，试图找到一个如何创建一个手风琴的好解释，但我找到的都是没有解释的代码。作为一名开发人员，我认为向人们详细解释东西是很重要的，因为有些人不只是希望复制代码并继续前进，他们希望从经验中学习。因为我肯定是这些人中的一员，**我做了一个如何制作手风琴的简单指南，这样其他人就可以过来学习了**。

**仅供参考，我从另一篇博文中获得了这篇文章中的 html 和 css 代码:**[https://webdevtrick . com/html-CSS-JavaScript-accordion-source-code](https://webdevtrick.com/html-css-javascript-accordion-source-code)/

我只是给出一个解释，这个人没有给出努力建立在他们已经提供给我们的基础上。我还应该提到，我对这篇文章最初提供的 JavaScript 代码做了一些修改。

是的，我知道我们可以使用 boostrap 将一个手风琴插入到我们的页面中，或者我们可以查找我们需要的代码并将其粘贴到我们需要的地方，但我认为实际学习如何使用基本的 html、css 和 javascript 创建其中一个会很酷。

**这是最终结果的样子**

3 simple accordion buttons

在上面的代码笔中，你会看到我们将要制作的 3 个“简单手风琴”。所以在读完这篇博文后，你可以使用下面的代码来创建你所需要的任何数量的手风琴。

# 超文本标记语言

```
<div class="container"> <h1>Simple Accordion</h1> <button class="accordion">First Accordion</button> <div class="accordion-content"> <p>Lorem ipsum dolor sit amet, consectetur adipisicing elit. Quas deleniti molestias necessitatibus quaerat quos incidunt! Quas officiis repellat dolore omnis nihil quo, ratione cupiditate! Sed, deleniti, recusandae! Animi, sapiente, nostrum?</p> <p>Lorem ipsum dolor sit amet, consectetur adipisicing elit. Quas deleniti molestias necessitatibus quaerat quos incidunt! Quas officiis repellat dolore omnis nihil quo, ratione cupiditate! Sed, deleniti, recusandae! Animi, sapiente, nostrum?</p> <p>Lorem ipsum dolor sit amet, consectetur adipisicing elit. Quas deleniti molestias necessitatibus quaerat quos incidunt! Quas officiis repellat dolore omnis nihil quo, ratione cupiditate! Sed, deleniti, recusandae! Animi, sapiente, nostrum?</p> </div> <button class="accordion">Second Accordion</button> <div class="accordion-content"> <p>Lorem ipsum dolor sit amet, consectetur adipisicing elit. Quas deleniti molestias necessitatibus quaerat quos incidunt! Quas officiis repellat dolore omnis nihil quo, ratione cupiditate! Sed, deleniti, recusandae! Animi, sapiente, nostrum?</p> </div> <button class="accordion">Third Accordion</button> <div class="accordion-content"> <p>Lorem ipsum dolor sit amet, consectetur adipisicing elit. Quas deleniti molestias necessitatibus quaerat quos incidunt! Quas officiis repellat dolore omnis nihil quo, ratione cupiditate! Sed, deleniti, recusandae! Animi, sapiente, nostrum?</p> </div></div>
```

所以我们从 html 开始

这里我们有一个带有类**容器**的**div**；这个 div 将在一个地方保存我们所有的 html 标记。容器中还有以下三个 html 标记:

1.  手风琴类的按钮
2.  具有 accordion 内容类的 div
3.  p 标记，用于保存将出现在折叠面板中的实际文本

您可能已经注意到，上面列出的这三个 html 元素在带有类容器的 div 中重复了三次

当我们进入制作这个手风琴所需的 css 和 javascript 代码时， **accordion** 和 **accordion-content** 类将会非常有用。

# 半铸钢ˌ钢性铸铁(Cast Semi-Steel)

```
.container {
  width: 80%;
  max-width: 600px;
  margin:50px auto;
}button.accordion {
  width: 100%;
  background-color: whitesmoke;
  border: none;
  outline: none;
  text-align: left;
  padding: 15px 20px;
  font-size: 18px;
  color: #333;
  cursor: pointer;
  transition: background-color 0.2s linear;
}button.accordion:after {
  font-family: FontAwesome;
  content: '\f150';
  font-family: "fontawesome";
  font-size: 14px;
  float: right;
}button.accordion.is-open:after {
  content: '\f056';
}button.accordion:hover, button.accordion.is-open {
  background-color: #ddd;
}.accordion-content {
  background-color: white;
  border-left: 1px solid whitesmoke;
  border-right: 1px solid whitesmoke;
  padding: 0 20px;
  max-height: 0;
  overflow: hidden;
  transition: max-height 0.2s ease-in-out;
}
```

好了，这里肯定有很多东西要解开，所以我将一段一段地粘贴代码，这样你就可以清楚地看到发生了什么

```
.container {
  width: 80%;
  max-width: 600px;
  margin:50px auto;
}
```

css 的这一部分用于设置 accordion 将在其父元素中占用多少空间，在本例中，父元素是具有 container 类的 div。我们将最大宽度设置为 600 像素，这意味着这将是手风琴父元素的最大尺寸。

max-width 是 accordion 的父元素在页面中可以获得的最大宽度。它最终的实际大小将取决于使用什么设备来查看它，但是无论如何，它都不会超过我们设置的最大宽度。

请注意，我们还将容器的宽度设置为 **80%** ，这告诉我们该容器将占用其父容器(在本例中是主体)中 80%的空间。同样，这实际上看起来像什么将取决于用于呈现容器的设备的视口，并且记住我们不能超过设置的最大宽度。

最后，**边距**将容器顶部和底部的边距设置为 50px，并在屏幕的左右两边自动调整。边距只是用来在容器外部创造一些空间。在这个特定的示例中，在容器顶部和底部的外侧添加了 50 个像素的空间，并在左侧和右侧使用 auto 将容器水平居中放置在 html 文档的正文中。

**请记住，用于容器的 css 代码与实际制作手风琴没有任何关系，它只是设置容纳手风琴的容器的宽度和间距**

```
button.accordion {
  width: 100%;
  background-color: whitesmoke;
  border: none;
  outline: none;
  text-align: left;
  padding: 15px 20px;
  font-size: 18px;
  color: #333;
  cursor: pointer;
  transition: background-color 0.2s linear;
}
```

这个选择器用来改变手风琴类的按钮

这里有很多，所以我将使用要点来解释 css 的每一行是做什么的

*   宽度——在我们的 html 中，我们有 3 个大按钮，每个代表一个手风琴。我们在这里使用了 width 属性来设置每个按钮的大小。所以手风琴基本上就是一个很大的按钮，里面装着内容。这里使用的宽度使得每个按钮都占据其父元素的 100%,在本例中，父元素是带有类容器的 div。您可以根据需要调整折叠按钮或父容器的宽度。
*   背景色—设置每个折叠按钮的背景色
*   边框和轮廓—确保每个按钮上没有边框或轮廓，但如果需要，您可以添加一个🙂
*   文本对齐——嗯……它将文本向左对齐；如果 css 中的一切都是这种不言自明的，那不是很好吗
*   填充——这里我们设置每个按钮内部的填充，在顶部和底部有额外的 15 个像素的空间，在左右两边有 20 个像素的空间。您可以使用这里的值来进一步调整手风琴按钮的大小。**但是请记住，这里宽度设置为 100%的事实也会影响按钮的大小**
*   字体大小—设置折叠按钮内文本的字体大小
*   颜色—设置折叠按钮内文本的颜色
*   cursor——这个 css 属性允许用户将鼠标悬停在折叠按钮上时，鼠标变成指针。这很有用，因为它可以让用户知道他们必须单击折叠按钮
*   transition——这用于设置一个 css 转换，当用户将鼠标放在折叠按钮上时就会发生这个转换。`transition`作为一个简写，把很多和 transition 有关的 css 属性的值写在一个地方。我们要做的第一件事是说 css 转换将对`background-color`进行更改。这里我们使用 `transition`来设置`transition-property`的值。`transition-propery`允许我们指定过渡将应用于 css 的哪个方面。接下来，我们使用`transition`来设置`transition-duration`，这是过渡完成之前需要的时间。最后，我们使用 transition 来设置`transition-timing-function`，它将控制转换在其持续时间内移动的速度。我们将过渡时间函数的值设置为线性，这意味着过渡将在其持续时间内以一致的速度发生。

如果这些听起来真的令人困惑，请查看关于如何使用转换属性的 w3schools 文档

```
button.accordion:after {
  font-family: FontAwesome;
  content: “\f150”;
  font-family: “fontawesome”;
  font-size: 18px;
  float: right;
}
```

现在我们开始有趣的事情了。我们现在使用一个叫做**的 css 伪选择器:在**之后。

根据 w3schools，css 伪选择器用于定义元素的特殊状态。在这种情况下，我们使用的是 **:after** 伪选择器，它允许在指定的 html 元素后插入内容。

我们利用这个伪选择器在每个折叠按钮制作完成后插入一个字体很棒的图标。以防你不知道字体真棒，这是一个网站，你可以得到很酷的图标用于你的前端项目。我正在使用**:在**之后的伪选择器在我的手风琴按钮的右侧插入[这个](https://fontawesome.com/v4.7.0/icon/caret-square-o-down)字体很棒的图标。

为了使这个工作正常，我们必须在伪选择器的花括号中使用 **FontAwesome** 字体系列。我们必须这样做，以便我们的浏览器能够正确识别我们放在 css 下一行的内容值。 **content** 属性用于指定我们希望插入到带有 **:after** 伪选择器的可折叠按钮中的内容。 **"\f150"** 是一个内容值，允许我们使用字体 awesome 中的图标。如果您以前使用过字体 awesome，使用这个内容值就相当于在我们的 html 中直接使用`<i class=”fa fa-caret-square-o-down” aria-hidden=”true”></i>`。

至于 css 的最后两行，这里的 font-size 将指定字体 awesome 图标的字体大小，而 float 将允许图标放在手风琴按钮的最右边。

有关如何使用字体超棒图标以及如何使用内容值的更多信息，请查看以下资源:

*   [字体牛逼主网站](https://fontawesome.com/v4.7.0/)
*   [字体 awesome 图标的内容值列表](https://astronautweb.co/snippet/font-awesome/)

```
button.accordion.is-open:after {
  content: “\f056”;
}button.accordion:hover,button.accordion.is-open {
  background-color: #ddd;
}.accordion-content {
  background-color: white;
  border-left: 1px solid whitesmoke;
  border-right: 1px solid whitesmoke;
  padding: 0 20px;
  max-height: 0;
  overflow: hidden;
  transition: max-height 0.2s ease-in-out;
}
```

css 的这一部分与 javascript 结合使用，赋予每个手风琴打开和关闭的权力。注意我们有一个名为。`is-open`，这是我们将要用来控制手风琴按钮的状态；无论它当前是否开放。我们在`.is-open`上使用了一个伪选择器，在手风琴按钮打开时将另一个字体很棒的图标插入其中。所以每当你点击折叠按钮时，使用的图标实际上会来回切换。

接下来，我们一次为两个不同的 css 选择器设置值。我们将`button.accordion:hover`和`button.accordion.is-open` 的值设置为交互时有不同的背景色。`button.accordion:hover` 末尾的`:hover`是另一个伪选择器，它允许我们设置一些 css 代码，以便在用户将鼠标悬停在手风琴按钮上时触发。

还记得我们之前设置为`button.accordion`的转换属性吗？既然我们设置了这个，那么每当用户悬停在按钮上时，我们将会看到 css 转换的效果。过渡效果将允许背景色在悬停时改变。除此之外，每当按钮被点击并打开时，折叠按钮的背景颜色将是不同的。

这里的最后一段 css 代码将样式化手风琴按钮内部的内容。当按钮打开时，您将看到内容。内容的背景将是白色的，将有一个内容的左边和右边的边界。在边框内内容的左右两边也会有一些额外的空间；大约 20 个额外像素的空间。

我们还设置了内容的`max-height`，并将其`overflow`设置为隐藏。`max-heigh`和溢出用于隐藏每个有`<div class = “accordion-content”></div>`的 html 元素内部的内容。

最后，内部设置了一个 css 转换，以允许折叠按钮的内容在打开后轻松进入折叠按钮提供的空间。

# Java Script 语言

这是我们代码中的一部分，在这一部分中，我们通过使用浏览器内置的 DOM API，为我们的 accordion 按钮提供了打开和关闭的功能。DOM API 是浏览器的一部分，它允许您使用 javascript 对当前正在处理的 html 文档进行更改。

```
const accordionBtns = document.querySelectorAll(“.accordion”);accordionBtns.forEach((accordion) => {
  accordion.onclick = function () {

    this.classList.toggle(“is-open”);
    let content = this.nextElementSibling; if (content.style.maxHeight) {
      //this is if the accordion is open
      content.style.maxHeight = null; } else {
      //if the accordion is currently closed
      content.style.maxHeight = content.scrollHeight + “px”;
    } };});
```

我认为解释上面的 JavaScript 代码最好的方法是查看我在写代码时为自己写的伪代码。

```
//pseudocode/*1.Grab the accordion buttons from the DOM and store them into a variable2\. go through each accordion button one by one3\. Use the classlist dom method in combination with the toggle method provided by the DOM to add or remove the “is-open” class.4\. get the div that has the content of the accordion button you are currently looking at5\. set the max-height based on whether the current value of the max-height css property is a truthy value or not6\. If the accordion is closed we set the max-height of the currently hidden text inside the accordion from 0 to the scroll height of the content inside the accordion.*/
```

**现在让我们按照上面的伪代码一步一步的分解这段代码**

# 第一步:

所以我们在这里做的第一件事是使用 DOM API 提供给我们的文档对象的方法；我们使用文档对象来调用`.querySelectorAll`方法。这个方法将允许我们进入我们的 html 文档，并提取每个具有类名 accordion 的 html 元素。请记住，为了引用一个类，我们必须加上一个句点(。)在手风琴这个词之前，像这样:`.accordion`。我们也可以使用`.getElementsByClassName`方法，所以你可以选择你更喜欢的方法。无论您选择使用哪种方法，都将返回包含类“accordion”的所有 html 元素的集合，并存储在`accordionsBtn`变量中。

# 第二步:

在这一步中，我们使用`.forEach(callback)`数组方法遍历在第一步中存储到变量中的每个折叠按钮。这个方法使用回调函数作为参数，所以我使用了一个箭头函数来表示在`.forEach`方法中每个单独的折叠按钮会发生什么。为了结束这一步，我们将把`onclick`方法添加到我们正在使用的 accordion 按钮中，用按钮注册一个点击事件。由于我们使用了`.forEach()`，这个 onclick 事件监听器将被放置在步骤 1 中存储在我们的变量中的所有折叠按钮上。

**这只是我们可以向 html 元素添加事件监听器的方法之一，但是就像所有其他方法一样，我们需要给事件一个回调函数，以便在事件被触发时执行。**

# 第三步:

我们一起使用`.classList`方法和`.toggle`方法将`.is-open`类添加到手风琴按钮中。结合使用`.classList`和`.toggle`方法，我们可以在每次点击折叠按钮时添加或移除类，这对我们来说很重要，因为这是我们赋予每个折叠按钮打开或关闭能力的方式。每当手风琴按钮“打开”时，我们希望它显示内容，当它“关闭”时，我们希望它隐藏内容。

我还想提一下，当我们打开和关闭这个类时，在我们按钮开关的最右边会出现一个很棒的字体图标。如果你回头看看这篇文章的 CSS 解释，我确实提到过这种情况会发生。

此时，该按钮能够切换图标，但不会显示任何内容，因为我们尚未添加该功能。目前我们的 css 隐藏了选择器`accordion-content` 中具有以下属性的内容:`max-height:0` 和`overflow:hidden`。样式表中的这两个 css 属性隐藏了内容，我们希望使用 JavaScript 来更改 max-height 的值，这样我们就可以看到隐藏的内容。

# 第四步:

在这一步中，我们希望获得当前手风琴的内容，该内容在`.forEach`方法中被处理。我们可以使用`.nextElementSibling`方法，它允许我们查看当前 html 元素之后的 html 元素。因为我们目前正在查看带有类`.accordion`的按钮，所以下一个元素是带有类`.accordion-content`的 div。这正是我们想要的，因为它允许我们使用包含我们想要显示的内容的 div。此外，请注意，我们可以通过另一种方式到达这个 div，但这是到达我们答案的“最短路径”。

# 第五步:

在这一步中，我们需要在包含我们想要显示的内容的 div 上使用一些条件逻辑。此时，根据您的手风琴当前的状态，手风琴按钮的`max-height`可以有 3 个不同的值

1.  值为 0，因为它尚未打开
2.  值为空，这意味着它以前已经打开和关闭过一次，并且当前是关闭的
3.  一个整数值，表示它当前是打开的

请记住，值为 0 或 null 是**false sy**，值为整数是**true**。

这个事实对我们的 if 语句非常有用。

现在我们使用一个 if 语句来查看`max-height`的值是否为真，如果是，这意味着手风琴是打开的，我们可以将它设置为 null 来关闭它。如果不是，那么这意味着它是关闭的，我们需要将它设置为一个整数，以便正确地显示内容。

我们可以通过使用`scroll-height` css 属性来设置`max-height`的值。`scroll-height` 是指 html 元素的高度，以像素为单位。对于这个具体的例子，我们讨论的是带有类`accordion-content`及其所有嵌套的 p 元素的 div 的高度。

我们在这里使用 scroll height 将最大高度设置为一个足以清晰显示整个 div 及其所有嵌套的 p 元素的值。请随意再看一下这篇文章的 HTML 部分，这样您就可以看到嵌套了 p 元素的 div。

咻…我们终于完成了

现在，如果你跟着我，你应该能够创建自己的手风琴。希望你喜欢和我一起编码，并且对 bootstrap 或其他前端框架为你提供的抽象层次有更深的理解。

虽然我们总是可以使用一个框架来快速创建一个 UI 组件，但偶尔回到基础总是好的。