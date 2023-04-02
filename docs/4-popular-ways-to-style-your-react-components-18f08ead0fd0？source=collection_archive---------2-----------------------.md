# 4 种流行的 React 组件样式化方法

> 原文：<https://javascript.plainenglish.io/4-popular-ways-to-style-your-react-components-18f08ead0fd0?source=collection_archive---------2----------------------->

让你的组件看起来非常漂亮

![](img/af838fec58f29d4da8762d36f441a59d.png)

Illustration by Icons 8 from [Icons8](https://icons8.com/)

您的 React 应用程序就是这样，一个混合在一起创建用户界面和功能的[组件](https://medium.com/the-andela-way/understanding-react-components-37f841c1f3bb#:~:text=Components%20are%20the%20building%20blocks,(User%20Interface)%20should%20appear.)的集合。当然，你不想让你的应用程序缺乏*的颜色*、*的动画*和酷酷的风格。

**这就是为什么您应该真诚地关心您计划如何定制和制作漂亮的 React 组件。**

下面，你会发现 4 种最常用的方法来使你的 React 应用程序看起来漂亮。但是请小心。因为每一种编码解决方案都会带来选择的负担。

因此，你必须非常小心地根据你的需求、**你的目标和你的应用**来选择。

## 内嵌样式

一个方便，快速使用的模式来设计你的反应组件。在这种情况下，与您在 HTML 中看到的[相反，您必须指定一个包含您的规则的*对象*，而不是一个简单的*字符串*。然后提供给一个叫做`style`的道具](https://www.w3schools.com/tags/att_style.asp)

请注意两点:

*   您通常用破折号定义的规则，在本例中为`margin-left`，需要使用[驼峰式符号](https://en.wikipedia.org/wiki/Camel_case)来定义。
*   各种样式的值表示为*字符串*

像这样定义所有的内联样式并不是强制性的。例如，您可以创建一个包含您的规则的对象，然后在以后使用它，甚至多次使用。

*   **优点:**快捷易用。尤其适用于像边距或填充这样的超小调整。
*   **缺点:**完全不适应大型生产环境。缺少对[媒体查询](https://developer.mozilla.org/en-US/docs/Web/CSS/Media_Queries/Using_media_queries)、[伪类](https://developer.mozilla.org/en-US/docs/Web/CSS/Pseudo-classes#:~:text=A%20CSS%20pseudo%2Dclass%20is,user's%20pointer%20hovers%20over%20it.)的支持，并且通常会混淆动态样式和大型组件。

## CSS 模块

这个概念背后的道理很简单:

> 每个 React 组件都获得一个 CSS 文件，该文件位于该组件的本地。在运行时，这些 CSS 文件中定义的名称被映射到自动生成的名称，并导出为您可以使用的 JS 对象文字。

注意到

*   我们简单地从 CSS 文件`./styles.css`中导入一个对象
*   我们使用对象`styles`来访问我们的规则
*   **优点:**没有名称空间冲突和名字冲突，易于使用和重构，允许你轻松地从你的项目中取消*死代码*。另外，它们支持你在 CSS 中需要的一切。
*   缺点:主要是在你的文件中使用它们时会有更冗长的体验。

[](https://medium.com/@pioul/modular-css-with-react-61638ae9ea3e) [## 带有 React 的模块化 CSS

### 在 Buffer，我们喜欢 React，并逐渐将大量前端代码转移到它上面。在它上面有通量，我们…

medium.com](https://medium.com/@pioul/modular-css-with-react-61638ae9ea3e) [](https://medium.com/nulogy/how-to-use-css-modules-with-create-react-app-9e44bec2b5c2) [## 如何在 Create React 应用程序中使用 CSS 模块

### 简而言之:不能。你得去⏏

medium.com](https://medium.com/nulogy/how-to-use-css-modules-with-create-react-app-9e44bec2b5c2) 

## CSS 样式表

这不是我推荐的方式。这里，您只是在组件中导入了一个 CSS 文件，错过了许多其他可能的功能，比如防止名称冲突:

## 样式组件

我最喜欢的用 React 写 CSS 的方式。使用[**styled-components**](https://styled-components.com/)，您将在组件级别安装编写样式，允许您定义适当的、定制的 React 组件，这些组件将按照您的意愿进行样式化。

*   **优点**:没有类名冲突，不那么庞大，更有意义的组件。CSS 的轻松删除和维护。支持重要的 CSS 特性和动态样式。
*   **缺点:**风格化组件肯定有一个学习曲线。另外，如果没有快捷方式，它们往往会提供更差的 ide 体验(由于一些 [vs 代码扩展](https://medium.com/javascript-in-plain-english/how-to-choose-the-perfect-extensions-for-your-vs-code-setup-66ecc7425d4b)，也有例外)。

[](https://medium.com/styled-components/styled-components-getting-started-c9818acbcbbd) [## 样式化组件入门

### 我们将使用样式化组件对基本的 create react 应用程序进行样式化，如下所示:

medium.com](https://medium.com/styled-components/styled-components-getting-started-c9818acbcbbd) [](https://medium.com/styled-components/how-to-build-a-great-component-library-a40d974a412d) [## 我们如何建立一个人们真正喜欢使用的组件库

### 我们的三人团队为一个 60 人的设计师和工程师团队构建了一个组件库，他们喜欢使用它。以下是如何…

medium.com](https://medium.com/styled-components/how-to-build-a-great-component-library-a40d974a412d) 

## 关键要点

*   仅在非常小的项目或对某些 UI 元素的外观进行非常简单的调整时使用内联样式
*   如果你喜欢使用 CSS 并且不想面对学习曲线，使用 CSS 模块并享受你的技能
*   如果你不介意模板文字和学习一种创建 CSS 的新方法，可以考虑使用样式化组件，因为它们非常适合构建定制组件库和可伸缩组件。

一如既往，感谢您的时间，并保持评论在下面的特别部分。

— *皮耶罗*

## 资源

*   [了解 React 组件](https://medium.com/the-andela-way/understanding-react-components-37f841c1f3bb#:~:text=Components%20are%20the%20building%20blocks,%28User%20Interface%29%20should%20appear.)
*   [HTML 样式属性](https://www.w3schools.com/tags/att_style.asp)
*   [骆驼格符号](https://en.wikipedia.org/wiki/Camel_case)
*   [样式-组件](https://styled-components.com/)
*   [CSS 媒体查询](https://developer.mozilla.org/en-US/docs/Web/CSS/Media_Queries/Using_media_queries)
*   [CSS 伪类](http://pseudo-classes)
*   [VS 代码扩展](https://medium.com/javascript-in-plain-english/how-to-choose-the-perfect-extensions-for-your-vs-code-setup-66ecc7425d4b)