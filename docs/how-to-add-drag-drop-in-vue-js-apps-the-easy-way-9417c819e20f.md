# 如何简单地在 Vue.js 应用程序中添加拖放功能

> 原文：<https://javascript.plainenglish.io/how-to-add-drag-drop-in-vue-js-apps-the-easy-way-9417c819e20f?source=collection_archive---------5----------------------->

![](img/9b71ac8be47a28f89d6afbcc714b06be.png)

拖放功能在手机和网络上的各种应用中都可以找到。虽然我们认为拖放在任何应用程序中都是一个好的 UI 模式，但是自己实现它可能比预期的更具挑战性。除了官方的 [HTML5-API](https://www.html5rocks.com/en/tutorials/dnd/basics/) ，还有几个 JavaScript 库提供了在你的 web 应用中实现拖放的定制解决方案。

拖放通常用于排序和重新排序列表。然而，除了简单的列表之外，拖放还可以用于各种上下文中:例如，您可以拖放文件来上传它，或者您可以自由地移动元素。对于这些和更高级的用例，有更强大的解决方案，如[可拖动](https://github.com/Shopify/draggable)可用。

尽管 Draggable 可以在不考虑您可能使用的任何 JS 框架的情况下实现，但它可以很容易地集成到流行的 [Vue.js](https://vuejs.org/) 框架中。我将使用 TypeScript 作为代码示例，但这也可以用普通的 JavaScript 来完成。

![](img/c14c884229c2da9f104c6849649e3126.png)

## 初始设置

首先，你需要创建一个 Vue.js 项目(比如用 [Vue CLI](https://cli.vuejs.org/guide/creating-a-project.html) )并通过 NPM 安装可拖动库。

```
npm install @shopify/draggable
```

当然，因为 Vue.js 可以在没有 NPM 的情况下使用，所以你也可以通过 CDN 获得这个可拖动的库。

```
<!-- Entire bundle; alternative when not using NPM -->
<script src="https://cdn.jsdelivr.net/npm/@shopify/draggable@1.0.0-beta.8/lib/draggable.bundle.js"></script>
```

## 指令

[指令](https://vuejs.org/v2/guide/syntax.html#Directives)是带有`v-`前缀的特殊属性。指令的工作是当它的表达式的值改变时，对 DOM 反应性地应用副作用。在本例中，我们创建了一个可排序的指令，允许我们对列表中的元素进行重新排序。

```
*import* {*DirectiveOptions*} *from* 'vue';
*import* {Vue} *from* "vue-property-decorator";
import { Sortable } from ‘@shopify/draggable’;*const* sortableDirective: *DirectiveOptions* = {
   inserted(el: *HTMLElement*, node, vNode) {
      *new* Sortable(el, node.value);
   }
};

Vue.directive('sortable', sortableDirective);

*export default* sortableDirective;
```

现在，我们可以在 HTML 模板中使用 v-sortable 指令:

```
<template>
  <ul *v-sortable=*"defaultOptions">
     <li><a *href=*"https://vuejs.org" *target=*"_blank" *rel=*"noopener">Core Docs</a></li>
     <li><a *href=*"https://forum.vuejs.org" *target=*"_blank" *rel=*"noopener">Forum</a></li>
     <li><a *href=*"https://chat.vuejs.org" *target=*"_blank" *rel=*"noopener">Community Chat</a></li>
     <li><a *href=*"https://twitter.com/vuejs" *target=*"_blank" *rel=*"noopener">Twitter</a></li>
     <li><a *href=*"https://news.vuejs.org" *target=*"_blank" *rel=*"noopener">News</a></li>
  </ul>
</template>
<script *lang=*"ts">
   *import* {Component, Vue} *from* "vue-property-decorator";
   *import* sortableDirective *from* "@/directives/sortable.directive";

   @Component({
      directives: {sortableDirective} // include the directive
   })
   *export default class* SortableList *extends* Vue {
      defaultOptions = {
         draggable: 'li',
         mirror: {
            constrainDimensions: *true*,
         }
      };
</script>
```

## 服务

有时，您希望拖放是动态可用的(例如，对于通过 jQuery 等第三方库插入的动态内容)。或者您希望对创建的实例有更多的控制，以便对拖放行为有更多的控制。这是一个基本类，它提供了为任何 HTML 元素启用拖放功能的函数。

```
*import* {Draggable, Droppable, Sortable, Swappable} *from* '@shopify/draggable';

*export class* DragDropService {

   *private readonly* defaultSortableOptions = {
      mirror: {
         constrainDimensions: *true*,
      }
   };

   createDraggable(containerElement: *HTMLElement*, options: *object* = *this*.defaultSortableOptions) {
      *return new* Draggable(containerElement, options);
   }

   createDroppable(containerElement: *HTMLElement*, options?: *any*) {
      *return new* Droppable(containerElement, options);
   }

   createSortable(containerElement: *HTMLElement*, options: *object* = *this*.defaultSortableOptions) {
      *return new* Sortable(containerElement, options);
   }

   createSwappable(containerElement: *HTMLElement*, options: *object* = *this*.defaultSortableOptions) {
      *return new* Swappable(containerElement, options);
   }

}
```

## 结论

感谢阅读这篇文章！正如你所看到的，让拖放在 Vue.js 中工作是相当容易和快速的，这要感谢像 Draggable 这样的库很容易与 Vue.js 集成。如果它对你有帮助，请在评论中告诉我。