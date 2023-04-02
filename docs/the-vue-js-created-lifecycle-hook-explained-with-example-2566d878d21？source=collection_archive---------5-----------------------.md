# Vue.js 创建的生命周期钩子举例说明

> 原文：<https://javascript.plainenglish.io/the-vue-js-created-lifecycle-hook-explained-with-example-2566d878d21?source=collection_archive---------5----------------------->

## 通过示例了解创建的 Vue.js 生命周期挂钩

![](img/a392d644e746f7d0c1a1782bbe867397.png)

Photo by [heylagostechie](https://unsplash.com/@heylagostechie?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

## 什么是生命周期挂钩？

生命周期挂钩是纯函数，其上下文绑定到它们的实例。对于这种情况，Vue.js 生命周期钩子不应该被声明为箭头函数。

## **为什么没有箭头功能？**

生命周期挂钩应该是纯函数，因为箭头函数的上下文完全绑定到它们的父上下文。
在 arrow 函数中使用 ***这个*** 访问数据、方法和计算属性，会在 Vue.js 实例上抛出一个未定义的错误。

## **创建了钩子**

这个生命周期钩子在数据创建后被同步调用。这发生在$el 设置之前，但在数据观察、计算属性、观察器/事件回调和方法设置之后。

创建的挂钩允许您添加代码，如果创建了 Vue 实例，就会运行这些代码。

一个 [Vue 生命周期的步骤](https://vuejs.org/images/lifecycle.png)。是:创建前、已创建、装载前、已装载、更新前、已更新、销毁前、已销毁。

此时，$el 属性还不可访问，并且完全不可用。
所以，当你需要在数据实例创建后立即运行一个特定的函数时，我们可以使用这个钩子。

还要注意，创建的钩子只在数据、方法和计算属性设置好之后运行一次。

**示例**

```
async created() {*//Called synchronously after the instance is created**try* {const response = *await* fetch(`https://api.pexels.com/v1/search?query=cars&per_page=40`);const data = *await* response.json();console.log(data)} *catch* (error) {console.log(error);}}
```

在上面显示的例子中，我们向端点发出一个 fetch API 请求，并等待一个承诺。

如果承诺得到解决，我们只需将其记录到控制台，如果出现任何错误，我们也会将其记录到控制台。

我们还可以将 async 附加到创建的生命周期挂钩上，以进行异步调用。

***注意:*** *不要使用* [*箭头函数*](https://developer.mozilla.org/en/docs/Web/JavaScript/Reference/Functions/Arrow_functions) *对某个 options 属性或回调，如* `*created: () => console.log(this.a)*` *或* `*vm.$watch('a', newValue => this.myMethod())*` *。由于箭头函数没有* `*this*` *，* `*this*` *将被视为任何其他变量，并通过父作用域进行词汇查找，直到找到为止，这通常会导致错误，如* `*Uncaught TypeError: Cannot read property of undefined*` *或* `*Uncaught TypeError: this.myMethod is not a function*` *。vuejs.org*

## **结论**

简单回顾一下，我们已经看到了创建的生命周期挂钩是如何工作的，以及何时在我们的应用程序中使用它。

当我们想在 Vue.js 应用程序中运行一次函数时，也可以使用它。

感谢您通读这篇文章。如果你觉得这篇文章有帮助，请不要犹豫，分享出来。