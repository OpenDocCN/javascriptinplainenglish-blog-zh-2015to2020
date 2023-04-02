# Vue.js 入门:组件

> 原文：<https://javascript.plainenglish.io/getting-started-with-vue-js-components-cdfb763f78c9?source=collection_archive---------13----------------------->

在我最近的几篇博文中，我谈到了一些使用 Vue.js 跟踪网页上动态数据的方法，以及 Vue 如何给你一些指令来改变数据在页面上的显示方式。但是，正如我上次提到的，你需要一种方法来大规模地使用这些东西来制作一个好的网站。实现这一点的方法是使用 Vue 组件。在这篇博文中，我将给出这些组件能做的一些事情以及如何使用它们的一个非常基本的概述。

组件本质上是给定名称的可重用 Vue 实例。因为它们是实例，所以它们接受与`new Vue`相同的选项，比如`data`和`computed`，以及一个名称。基本格式如下所示:

```
Vue.component('*component-name*', {
  data: function () {
    return {
      // *some data*
    }},
  template: `<div>
    <!-- *some html*-->
  </div>`
})
```

分解一下，第一个参数`'*component-name*'`是您在 HTML 中使用组件时为其指定的名称。你必须给它一个名字，作为第二个变量传递的对象应该有一个带有 HTML 的`template`，这样当你使用它的时候你的组件就会出现在页面上。使用 kebab-case 作为名称也是最佳实践，全部使用小写字母和连字符，因为这符合自定义 HTML 标记名称的规则。

`data`部分是组件所需的数据，它应该始终是一个返回带有数据的对象的函数。这是为了使组件的每个实例都有自己的数据副本。如果它只是返回一个没有函数的`data`对象，那么组件的每个实例都会受到其中任何一个`data`变化的影响。组件的`template`部分是您希望组件拥有的 HTML。请注意，模板是用反引号括起来的，而不是单引号。这是因为`template`的字符串在几行中，所以要么需要用反引号括起来，要么在每行的末尾加反斜杠。

下面是一个组件的例子，它有一个增加计数器的按钮，展示了如何在您的页面中使用它:

```
// *in index.js*
Vue.component('counter-component', {
  data: function () {
    return {
      count: 0
    }
  },
  template: `<div>
    <button v-on:click="count++">Click me!</button>
    <p>You've clicked the button {{ count }} times</p>
  </div>`
})new Vue({
  el: '#component-example'
})// *in index.html* <div id='component-example'>
  **<counter-component></counter-component>**
</div>
```

从这个例子中你可以看到，你可以在你的网页中使用一个组件，就像它是一个 HTML 标签一样。您可以任意多次使用每个组件。这里还有一些其他需要注意的地方。首先，您需要在想要使用组件的地方使用`el`将一个 Vue 实例附加到 HTML。否则，在这种情况下，div 将为空，因为它不知道名为`counter-component`的元素。第二件事是，您需要将您的`template`包装在一个 HTML 元素中。如果我把示例中的`template`中的 *div* 去掉，就会导致错误并且无法工作。

Vue 组件主要用于当你写一段你想重用的 HTML 的时候。例如，一个网站的每个用户的个人资料应该有相似的格式，所以我们希望重用一些相同的 HTML，除了不同用户的数据不同，如姓名或电子邮件。但是要做到这一点，我们需要一种方法为组件的每次使用提供一组不同的信息。这是通过使用叫做*道具*的东西来实现的。道具是你可以给 Vue 组件的选项之一，就像这样:

```
Vue.component('profile', {
  props: ['name'],
  template: '<h3>{{ name }}</h3>'
})
```

然后，当我们使用它时，要传递它名称 prop，我们可以这样做:

```
<profile name="Jane Doe"></profile>
<profile name="John Smith"></profile>
<profile name="Jesse Baker"></profile>
```

但是如果我们希望我们的页面是动态的，那么在中硬编码名字并不是特别有用，所以你通常会希望循环遍历你的实例数据中的一个列表。幸运的是，Vue 的`v-for`与组件一起工作，所以你可以像这样使用这个`profile`组件:

```
// *in index.js* new Vue({
  el: '#people-list',
  data: {
    people: [
      { id: 1, name: 'Jane Do'},
      { id: 2, name: 'John Smith'},
      { id: 3, name: 'Jesse Baker'}
    ]
  }
})// *in index.html* <div id="people-list">
  <profile
    v-for="person in people"
    v-bind:key="person.id"
    v-bind:name="person.name"
  ></profile>
</div>
```

正如你在这个例子中看到的，你可以使用`v-bind`来传递动态的属性，所以这个例子在网页上看起来和我手动传递`name`的例子完全一样。另一件要考虑的事情是，随着我的`profile`组件的增长，它可能需要更多的道具。但是，对大量的道具反复使用`v-bind`会变得单调乏味，所以编写这个`profile`组件的另一种方式应该是这样的:

```
// *in index.js* Vue.component('profile', {
  props: ['person'],
  template: `<div>
    <h3>{{ person.name }}</h3>
    <img v-bind:src='person.img_url'></p>
    <p>{{ person.email }}</p>
  </div>`
})// *in index.html* <div id="people-list">
  <profile
    v-for="person in people"
    v-bind:key="person.id"
    v-bind:person="person"
  ></profile>
</div>
```

关于 Vue 组件，我要说的最后一件事是*插槽*。您可能已经注意到，在我使用的例子中，HTML 中组件的开始和结束标记之间没有任何东西。这是因为，如果你试图在一个组件的开始和结束标签之间放置任何东西，它将不会在页面上显示出来。但有时你可能想在它们之间放些东西。这就是*插槽*的用武之地。它们看起来像这样:

```
// *in index.js*
Vue.component('navigation-link', {
  props: ['url'],
  template: `
    <a v-bind:href="url">
      **<slot></slot>**
    </a>
  `
})// *in index.html*
<div id="profile-nav">
  <navigation-link url="/profile">
    Your Profile
  </navigation-link>
</div>
```

从粗体的部分，你可以看到我添加了`slot`元素，这是 Vue 给你的另一个东西。现在你在标签`navigation-link`之间写的任何东西都会显示在组件中你放置`slot`元素的地方。

至此，我想我已经对 Vue 组件做了一个很好的基本概述。我已经展示了它们的一些使用方法和它们能做的一些事情。但是你可以用组件做更多的事情，如果你感兴趣，我鼓励你去看看在 Vue 的[文档](https://vuejs.org/v2/guide/components.html)中关于组件的所有使用方法。通过这篇文章和我写的所有其他关于 Vue.js 的文章，我已经基本上涵盖了你开始使用 Vue.js 所需要的所有基础知识。