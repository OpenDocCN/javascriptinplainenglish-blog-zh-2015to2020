# 通过 3 个简单的步骤将 TypeScript 添加到 Vue 或 Nuxt 中

> 原文：<https://javascript.plainenglish.io/add-typescript-to-nuxt-or-vue-in-3-easy-steps-360a356460b4?source=collection_archive---------4----------------------->

![](img/73b2f63fcefa959189a1b79e7b8aef81.png)

Photo by [Photos Hobby](https://unsplash.com/@photoshobby?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

在 Nuxt 或 Vue 中使用 TypeScript 可能非常复杂。我只想做类型检查！仅此而已。我花了很长时间才发现如何在 Nuxt 中启用类型检查，所以我将分享我的发现(你也可以在 Vue 中应用这些发现)。

## 步骤 1:更改脚本语言

这应该很明显。在最初的网络时代，我们必须将脚本标签的语言明确设置为 JavaScript。JavaScript 在早期并不是默认语言。从那以后，浏览器现在假定脚本标签总是 JavaScript。

要在`.vue`页面中启用 TypeScript，请更改脚本语言。

```
<script lang="ts">
const myVar: string = 'hello'
</script>
```

我们现在可以使用 TypeScript。简单。

## 步骤 2:为数据对象创建一个接口

TypeScript 非常擅长捕捉错误。你弄乱了类型，你得到一个错误。因此，我们应该定义数据对象应该是什么样子。

```
<script lang="ts">
interface Data {
  var1: boolean;
}
export default {
  data(): Data {
    var1: true,
  }
}
</script>
```

现在，每当我们动态更新数据或更新代码时，我们都会进行类型检查。

## 步骤 3:为此创建一个接口

Vue 页面允许我们使用`this`变量来访问 Nuxt/Vue 属性和数据属性。我们应该定义一个接口来允许使用我们的函数的`this`。

```
<script lang="ts">
interface Data {
  var1: boolean;
}
interface This extends Data {
  [key: string]: any;
}
export default {
  data(): Data {
    var1: true,
  },
  created(this: This) {
    this.var1 = false;
  }
}
</script>
```

我们现在可以在 Vue 生命周期挂钩中使用`this`，比如`created`。由于`this`有如此多的属性，我们需要添加一个通用键定义来考虑所有其他属性。

## (可选)步骤 4:为方法对象创建一个接口

如果我们有方法，我们可以选择确保它们接受正确的输入和输出类型。

```
<script lang="ts">
interface Data {
  var1: boolean;
}
interface Methods {
  onCreated(): void
}
interface This extends Data, Methods {
  [key: string]: any;
}
export default {
  data(): Data {
    var1: true,
  },
  created(this: This) {
    this.var1 = false;
    console.log(this.onCreated(true))
  },
  methods: <Methods>{
    onCreated(this: This, val: boolean): string {
      return `my val = ${val}`
    }
  }
}
</script>
```

这些方法被添加到`this`中，这就是为什么我们扩展了`This`接口。

## 结论

我假设你已经有了`tsconfig.json`和所有其他必要的项目来添加适当的 TypeScript npm 包，以保持这篇文章简短。我描述的方法是非传统的，但是它在 Vue 页面中保持了简单的类型。此外，推荐的 Nuxt 和 Vue 方法没有向您展示如何对`data`和`methods`对象进行类型检查。

我希望这篇文章对你有用。

# 作者的笔记

加入我的邮件列表来接收关于我写作的更新。

访问[**miguelacallesmba.com/subscribe**](https://miguelacallesmba.com/subscribe)并报名。

保持安全，米格尔

## 关于作者

Miguel 是首席安全工程师，也是“T0”无服务器安全一书的作者。作为一名开发人员和安全工程师，他参与了多个无服务器项目，为开源无服务器项目做出了贡献，并以各种工程角色从事大型军事系统的工作。