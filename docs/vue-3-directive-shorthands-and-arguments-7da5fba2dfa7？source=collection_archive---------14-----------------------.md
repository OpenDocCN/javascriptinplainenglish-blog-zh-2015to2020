# Vue 3 —指令缩写和参数

> 原文：<https://javascript.plainenglish.io/vue-3-directive-shorthands-and-arguments-7da5fba2dfa7?source=collection_archive---------14----------------------->

![](img/bd99cc40f48bef2c2a28f4aa1e380fa1.png)

Photo by [Sebastian Herrmann](https://unsplash.com/@officestock?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

Vue 3 处于测试阶段，可能会有变化。

Vue 3 是 Vue 前端框架的最新版本。

它建立在 Vue 2 的普及性和易用性之上。

在本文中，我们将研究指令的动态参数。

# 动态参数

Vue 3 指令可以接受动态参数。

例如，我们可以写:

```
<a v-bind:[attributeName]="url"> ... </a>
```

或者:

```
<a v-on:[eventName]="doSomething"> ... </a>
```

我们可以将一个参数传递给一个指令。

这适用于任何指令，包括我们创建的指令。

# 修饰语

我们也可以在指令中传递修饰符。

例如，`v-on`指令可以为某个事件接受一个修饰符。

我们可以写:

```
<form v-on:submit.prevent="onSubmit">...</form>
```

让我们调用`onSubmit`处理程序中的`event.preventDefault`。

这让我们能够阻止默认行为的发生。

例如，我们可以写:

```
<!DOCTYPE html>
<html lang="en">
  <head>
    <title>App</title>
    <script src="[https://unpkg.com/vue@next](https://unpkg.com/vue@next)"></script>
  </head>
  <body>
    <div id="app">
      <form v-on:submit.prevent="onSubmit">
        <input v-model="name" />
      </form>
    </div>
    <script>
      const vm = Vue.createApp({
        data() {
          return {
            name: ""
          };
        },
        methods: {
          onSubmit() {
            console.log(this.name);
          }
        }
      }).mount("#app");
    </script>
  </body>
</html>
```

使用`v-on:submit:prevent`指令创建一个表单，在一个指令中使用`event.preventDefault`方法运行`onSubmit`方法。

# 人手不足

我们可以把`v-bind`缩短为`:`。

例如，我们可以写:

```
<a :href="url"> ... </a>
```

而不是:

```
<a v-bind:href="url"> ... </a>
```

我们也可以用速记来传递一个论点:

```
<a :[key]="url"> ... </a>
```

我们可以通过书写来使用它:

```
<!DOCTYPE html>
<html lang="en">
  <head>
    <title>App</title>
    <script src="https://unpkg.com/vue@next"></script>
  </head>
  <body>
    <div id="app">
      <a :href="url">link</a>
    </div>
    <script>
      const vm = Vue.createApp({
        data() {
          return {
            url: "http://example.com"
          };
        }
      }).mount("#app");
    </script>
  </body>
</html>
```

`v-on`也有自己的速记。

而不是写:

```
<a v-on:click="doSomething"> ... </a>
```

我们可以写:

```
<a @click="doSomething"> ... </a>
```

简称。

如果我们想要一个动态事件，我们可以写:

```
<a @[event]="doSomething"> ... </a>
```

所以我们可以写:

```
<!DOCTYPE html>
<html lang="en">
  <head>
    <title>App</title>
    <script src="https://unpkg.com/vue@next"></script>
  </head>
  <body>
    <div id="app">
      <button @click="toggle">toggle</button>
      <p>{{on}}</p>
    </div>
    <script>
      const vm = Vue.createApp({
        data() {
          return {
            on: true
          };
        },
        methods: {
          toggle() {
            this.on = !this.on;
          }
        }
      }).mount("#app");
    </script>
  </body>
</html>
```

向我们的按钮添加一个点击监听器。

# 警告

动态值有一些限制。

方括号内的内容必须返回一个字符串或`null`。

`null`可用于解除捆绑。

任何其他值都将触发警告。

某些字符在 HTML 中是无效的，所以我们不能将它们作为动态参数的值。

所以我们不能有带引号的名字。

类似于:

```
<a v-bind:['foo ' + bar]="value"> ... </a>
```

行不通。

我们也应该避免用大写字母命名键，因为浏览器会将它们转换成小写字母。

# JavaScript 表达式

模板表达式是沙箱，所以我们不能在模板表达式中使用全局变量。

它只能从白名单中访问全局变量列表。

白名单位于[https://github . com/vue js/vue-next/blob/master/packages/shared/src/globalswhitelist . ts # L3](https://github.com/vuejs/vue-next/blob/master/packages/shared/src/globalsWhitelist.ts#L3)。

![](img/432d1fdc00c8d908ca5bf07d719114f2.png)

Photo by [Emiliano Vittoriosi](https://unsplash.com/@emilianovittoriosi?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 结论

Vue 指令可以接受动态参数。

此外，我们可以使用各种简写来代替一些指令。

喜欢这篇文章吗？如果有，通过 [**订阅获取更多类似内容解码，我们的 YouTube 频道**](https://www.youtube.com/channel/UCtipWUghju290NWcn8jhyAw) **！**