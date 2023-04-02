# 本周打字提示:泛型

> 原文：<https://javascript.plainenglish.io/typescript-tip-of-the-week-generics-57110db5b3cb?source=collection_archive---------5----------------------->

![](img/0aea803169bd72a03e05d4508b367f24.png)

周一快乐，感谢您阅读本周第一篇打字提示。我开始了这一系列有用的 TypeScript 见解和技巧，作为让更多人对使用 TypeScript 感兴趣的一种方式。

本周我想谈谈类型脚本泛型。有了支持 TypeScript 的令人难以置信的类型系统，泛型使您能够轻松地重用逻辑，并使您能够动态定义您期望从该逻辑返回的数据类型。让我们看一个例子。

```
function foo<T>(arg: T): T {
 return arg;
}const bar = foo<string>(‘baz’);
```

这是一个超级基础的例子，但是如果我们想对它进行运算会发生什么呢？

```
function foo<T>(arg: T): T {
 console.log(arg.length);
 return arg;
}
```

有许多类型都有一个`.length`属性，但是如果我们试图使用这个新代码，我们会得到一个错误，因为有些类型没有这个属性。为了解决这个问题，我们可以扩展这些类型。

```
function foo<T extends string>(arg: T): T {
 console.log(arg.length);
 return arg;
}
```

这是可行的，但是，这有点违背了初衷，因为`T`可能是一个数组或一个带有长度属性的对象。使用自定义接口，我们可以专注于需要用泛型类型扩展的属性。

```
interface GenericLengthType {
 length: number;
}function foo<T extends GenericLengthType>(arg: T): T {
 console.log(arg.length);
 return arg;
}const a = foo([‘foo’, ‘bar’, ‘baz’]);
const b = foo(‘bar’);
const c = foo({length: 10});
const d = foo(10); // error number doesn’t have property length
```

这很好，但是让我们看一个更实际的例子，一个我在编写类型脚本代码时经常使用的例子。

```
function TryWrapper<T extends Function>(fn: T): T {
 return <any>function() {
   try {
     return fn(...arguments);
   } catch (e) {
     // handle error gracefully
   }
 };
}const test = TryWrapper((arg: string) => {
 if (arg.length < 5) {
   throw new Error(“Argument length must be longer than 5”);
 }
 return arg.split(“”);
});
```

我们在返回的函数上使用类型`any`可能看起来很奇怪，但是，如果我们不在那里放置任何东西，我们会得到这个错误

```
Type ‘(args: any) => any’ is not assignable to type ‘T’.
‘(args: any) => any’ is assignable to the constraint of type ‘T’,
but ‘T’ could be instantiated with a different subtype of constraint ‘Function’.
```

当我们把那个`any`去掉时，它基本上是想把那个类型赋给 T，它告诉我们这是无效的。我们还使用了`arguments`关键字来获取传递给函数的所有参数。然后使用 spread 操作符确保参数被传递到包装的函数中。我们这样做的原因是，一个包装的函数可能有几个参数传递给它，我们希望确保保留它们。

```
const add = TryWrapper((a: number, b: number) => a + b);
```

这里有一个关于 stackblitz 的实例

感谢阅读本周第一个打字技巧！在评论里说说你的看法。

如果你喜欢这篇文章，我希望你[加入我的邮件列表](https://email.sam-redmond.com/)，在那里我会发送更多的提示&技巧！

如果你觉得这篇文章有帮助，有趣，或娱乐性[请给我买杯咖啡](https://email.sam-redmond.com/products/throw-me-a-bit)来帮助我继续发布内容！