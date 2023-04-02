# 使用 TypeScript 编写 Vue.js 应用程序

> 原文：<https://javascript.plainenglish.io/writing-vue-js-apps-with-typescript-ccb1df79aa68?source=collection_archive---------5----------------------->

![](img/1ba6036818073cdb7520c616c001b310.png)

Photo by [Sawyer Bengtson](https://unsplash.com/@sawyerbengtson?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

Vue.js 是一个易于使用的 web 应用框架，我们可以用它来开发交互式前端应用。

在本文中，我们将了解如何使用 TypeScript 编写 Vue.js 应用程序。

# 支持类型脚本

Vue.js 内置了对 TypeScript 的支持。它为我们提供了一个大部分是静态类型的系统，减少了代码中出现运行时错误的机会。

Vue 附带了用于 TypeScript 的正式类型声明。它包含在 Vue、Vue 路由器和 Vuex 中。

TypeScript 可以解析 NPM 包中的类型声明，所以我们不必担心自己解析它们。

# 使用 Vue CLI

我们可以使用 Vue CLI 创建一个 Vue TypeScript 项目。为此，我们必须通过运行以下命令来运行 Vue CLI:

```
npx vue create .
```

在我们的项目目录中。

然后我们选择“手动选择功能”，然后键入脚本。其余的可以保留默认选项。

然后我们运行`npm run serve`来运行项目热重装。

如果需要，我们还可以在`tsconfig.json`中设置以下推荐选项:

```
{
  "compilerOptions": {
    "target": "es5",
    "strict": true,
    "module": "es2015",
    "moduleResolution": "node"
  }
}
```

`target`是建造目标。将其设置为`es5`可以最大化对浏览器的支持。

`strict`启用或禁用严格类型检查。将其设置为`true`将启用它。

`“module”: “es2015”`利用 Webpack 2+或 Rollup 的树摇动功能来减小包的大小并加快加载速度。

# 定义和使用组件

我们可以通过使用类语法来定义组件。例如，使用我们用 Vue CLI 创建的 TypeScript 项目，我们可以这样做:

`src/components/Message.vue`:

```
<template>
  <div class="hello">
    <h1>{{ msg }}</h1>
  </div>
</template><script lang="ts">
import { Component, Prop, Vue } from 'vue-property-decorator';[@Component](http://twitter.com/Component)
export default class Message extends Vue {
  [@Prop](http://twitter.com/Prop)() private msg!: string;
}
</script>
```

在上面的代码中，我们用`@Prop`装饰器来定义`message`道具。`msg`后的感叹号使`msg`成为必填项。在冒号的右边，我们有道具的类型，它是一个字符串。

`src/App.vue`:

```
<template>
  <div id="app">
    <input v-model="message">
    <Message :msg="message"/>
    <button [@click](http://twitter.com/click)="showMessage">Show Message</button>
  </div>
</template><script lang="ts">
import { Component, Vue } from 'vue-property-decorator';
import Message from './components/Message.vue';[@Component](http://twitter.com/Component)({
  components: {
    Message,
  },
})
export default class App extends Vue {
  message: string = ''; showMessage(): void{
    alert(this.message)
  }
}
</script><style>
#app {
  font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif
}
</style>
```

在上面的代码中，我们定义了`message`字段，它被绑定到带有`v-model`的模板。

我们还有一个`showMessage`方法，当点击显示消息按钮时运行。

然后我们可以看到一个输入框，我们可以在里面输入东西。我们输入的内容将显示在`Message`组件的输入框下方，当我们单击显示消息时，我们会看到我们输入的内容显示在警告框中。

![](img/c9218b204cc4886e7f23c7d8295a88db.png)

Photo by [Hieu Vu Minh](https://unsplash.com/@spoony?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 使用 Vue.extend 创建组件

我们也可以使用`Vue.extend`来创建组件。

例如，我们可以用以下内容替换`src/components/Message.ts`

```
import Vue, { VNode } from 'vue'const Component = Vue.extend({
  props: ['msg'],
  computed: {
    greeting(): string {
      return `Hi. ${this.msg}`;
    }
  },
  render(createElement): VNode {
    return createElement('div', [
      createElement('p', this.msg),
      createElement('p', this.greeting),
    ])
  }
})export default Component;
```

在上面的代码中，我们使用`msg`属性和`greeting`计算属性呈现了一个`div`。

注意，我们为`render`准备了`VNode`返回类型。这将确保我们返回正确类型的数据。TypeScript 在很多情况下无法推断类型。

由于我们使用了`Vue.extend`，我们将得到类型推断和自动完成。

我们必须以书面形式导出:

```
export default Component;
```

以便其他组件可以使用它。

然后我们可以像往常一样在`src/App.vue`中导入它:

```
<template>
  <div id="app">
    <input v-model="message">
    <Message :msg="message"/>
    <button [@click](http://twitter.com/click)="showMessage">Show Message</button>
  </div>
</template><script lang="ts">
import { Component, Vue } from 'vue-property-decorator';
import Message from './components/Message';[@Component](http://twitter.com/Component)({
  components: {
    Message,
  },
})
export default class App extends Vue {
  message: string = ''; showMessage(): void{
    alert(this.message)
  }
}
</script><style>
#app {
  font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif
}
</style>
```

然后我们输入我们可以输入的东西，它会显示有和没有`Hi`添加在它前面。

我们保持显示消息按钮以前的行为方式。

# 扩充用于插件的类型

我们可以通过编写以下代码向现有类型添加我们自己的属性:

```
import Vue from 'vue'declare module 'vue/types/vue' {
  interface Vue {
    $myProperty: string
  }
}
```

上面的代码导入 Vue，然后添加`$myProperty`字符串属性作为有效属性。

这应该使 TypeScript 编译器知道代码，它应该能够成功编译。

# 结论

我们可以通过使用 Vue CLI 用 TypeScript 创建一个新项目。为此，我们选择手动选择功能，然后键入脚本。我们可以保留其余的默认选项，或者我们可以选择我们想要的，如果我们知道我们想要的。

为了获得类型推断和自动完成，我们可以用`Vue.extend`方法定义组件，或者将其定义为一个类。

我们可以通过使用`declare module`语法并在其中添加我们想要的东西来扩充现有的 Vue 类型。

TypeScript 在很多情况下无法推断函数的返回类型。因此，我们应该注释返回类型，以便进行自动完成和类型检查。

## **用简单英语写的 JavaScript 笔记**

我们总是有兴趣帮助推广高质量的内容。如果你有一篇文章想用简单的英语提交给 JavaScript，用你的中级用户名发邮件到[**submissions@javascriptinplainenglish.com**](mailto:submissions@javascriptinplainenglish.com)给我们，我们会把你添加为作者。

我们还推出了三种新的出版物！请关注我们的新出版物:[**AI in Plain English**](https://medium.com/ai-in-plain-english)，[**UX in Plain English**](https://medium.com/ux-in-plain-english)，[**Python in Plain English**](https://medium.com/python-in-plain-english)**—谢谢，继续学习！**