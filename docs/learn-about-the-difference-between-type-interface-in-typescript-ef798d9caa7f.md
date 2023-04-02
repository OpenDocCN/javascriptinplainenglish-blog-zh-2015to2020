# 了解 TypeScript 中类型和接口的区别

> 原文：<https://javascript.plainenglish.io/learn-about-the-difference-between-type-interface-in-typescript-ef798d9caa7f?source=collection_archive---------17----------------------->

![](img/795251cdfa36096b159d745170c818c9.png)

Photo by [Lagos Techie](https://unsplash.com/@heylagostechie?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

如果你使用 TypeScript，你可能会使用 interface 和 type，但是如果我问你它们之间的区别，你能回答吗？

在这篇文章的最后，你将能够在任何讨论或采访中回答它！

# 类型

基本的，它允许我们创造一个新的类型！

# 连接

与`type`相反，`interface`被限制为一个对象类型。

从新闻发布来看，`type`和`interface`相似，但也有一些不同。

# 相似

# 对象类型

您可以用两者来定义对象的形状，但是语法不一样

*带接口:*

```
interface A {
  a: number
}const a: A = { a: 5 }
```

*同类型:*

```
type A = {
  a: number
}const a: A = { a: 5 }
```

# 扩展

两者都可以扩展，区别是…是的，语法又来了！

*带接口:*

```
interface A {
 a: number
}interface AB extends A {
 b: number
}const ab: AB = { a: 5, b: 6 }
```

*同类型:*

```
type A = {
 a: number
}type AB = A & { b: number }const a: AB = { a: 5, b: 6 }
```

# 差别

# 类型能做什么，接口不能做什么

与`interface`不同，`type`可用于创建具有 union、tuples 的新类型，或者可用于定义原始类型！

```
type A = string | number // uniontype Primitive = string | boolean | number | null | interface | symbol // Create a new type from primitives typetype DataTuple = [number, string] // tuple typing
```

# 什么接口能做，什么类型不能做

一个**类**可以**实现**一个**接口**

*编辑:从 TS 2.7 开始，type 就可以在类上实现了，感谢* [*@faiwer*](https://dev.to/faiwer)

```
 interface A {
   a: number
}class Toto implements A {
   a = 55
}
```

如果定义了多次，接口可以合并在一个`interface`中

```
interface Toto {
   a: number
}interface Toto {
   b: number
}
const toto: Toto = {
   a: 55,
   b: 66,
}
```

# 结论

如你所见`type` & `interface`非常相似，但各有特色！

我个人在需要对对象结构进行类型化的时候使用接口，在需要从基元类型创建类型或者想把其他类型组合在一个类型中的时候使用 type！

希望你喜欢这篇读物！

🎁你可以免费获得我的新书《javascript 中被低估的技能》,《改变现状》( make the difference )( T30 ),前提是你在 Twitter 上关注我(T31 ),并给我打电话😁

或者在这里[得到它](https://codeoz.gumroad.com/l/RXLYp)

🎁[我的简讯](https://www.getrevue.co/profile/code__oz)

☕️你可以[支持我的作品](https://www.buymeacoffee.com/CodeoZ)🙏

🏃‍♂️，你可以跟着我👇

🕊 [推特](https://twitter.com/code__oz)

👨‍💻 [Github](https://github.com/Code-Oz)

你可以标记🔖这篇文章！

*更多内容看*[***plain English . io***](http://plainenglish.io/)