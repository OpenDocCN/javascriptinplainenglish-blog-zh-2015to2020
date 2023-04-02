# 应用于角度分析时，可观察值和承诺值之间的比较

> 原文：<https://javascript.plainenglish.io/the-comparison-between-observables-and-promises-when-applied-to-angular-7f4a8f23fb2d?source=collection_archive---------9----------------------->

![](img/60429a3ab576eba32057bbfa041172fc.png)

Photo by [Zachary Spears](https://unsplash.com/@zachspears?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

Angular 是 Google 制作的一个流行的前端框架。像其他流行的前端框架一样，它使用基于组件的架构来构建应用程序。

在这篇文章中，我们将比较可观察到的和承诺之间的区别。

它们都是异步的，但是它们非常不同。

# 与承诺相比，可观察到的

可观察到的和承诺之间有很多不同。

可观测量是说明性的。计算直到订阅才开始。

承诺在创造时立即生效。这使得 observables 更适用于我们只需要在有需要时运行的情况。

可观察提供了许多价值。承诺提供一个。

可观察性区分了链接和订阅。承诺只能用`then`子句链接。

因此，可观察对象可用于为它们发出的数据创建复杂的转换。

Observables `subscribe`负责处理错误。承诺将错误推送到子承诺。这使得错误处理更加集中和可预测。

# 创建和订阅

我们订阅可观测量，而不是像承诺的那样立即运行代码。

例如，我们通过撰写以下内容来订阅 observables:

```
new Observable((observer) => { //... });

observable.subscribe(() => {
  //...
});
```

当订阅后发出值时，回调中的代码运行。

鉴于承诺立即运行:

```
const promise = new Promise((resolve, reject) => { resolve('foo') });

promise.then((value) => {
  console.log(value)
});
```

在上面的代码中，我们创建了一个承诺，然后通过对它调用`then`来运行它。

# 链接

可观测量与`pipe`和`map`等操作符链接如下:

```
observable.pipe(map((x) => 2*x));
```

对于承诺，我们在`then`回调中解析值如下:

```
promise.then((v) => 2*v);
```

来映射值。

# 取消

我们可以调用可观察对象上的`unsubscribe`来取消订阅，如下所示:

```
const sub = obs.subscribe(...); 
sub.unsubscribe();
```

承诺没有这个功能。

# 错误处理

错误是用可观察对象中的错误处理程序来处理的。订阅中的错误被发送到错误处理程序。

例如，如果我们有:

```
of(1, 2, 3).subscribe(
  () => {
    throw Error("error");
  }
);
```

然后我们抛出一个错误。`of`是发出有限数值的可观察物体。

我们在承诺的`then`回调中抛出错误:

```
promise.then(() => {
  throw Error('my error');
});
```

# 与事件 API 相比的可观察值

Observables 也可以处理 DOM 事件。

例如，我们可以通过编写以下命令来监听可观察到的按钮点击声:

```
import { fromEvent } from "rxjs";const buttonEl = document.querySelector("button");
const clicks$ = fromEvent(buttonEl, "click");let subscription = clicks$.subscribe(e => console.log("clicked", e));
```

用`fromEvent`创建一个可观察对象，然后用`subscribe`方法订阅它。

在`subscribe`回调中，我们记录从可观察对象发出的值。

我们通过写信取消订阅可观察到的事件:

```
subscription.unsubscribe();
```

为了使用本地事件 API 实现这一点，我们编写了以下代码:

```
const buttonEl = document.querySelector("button");
const handler = e => console.log("clicked", e);
buttonEl.addEventListener("click", handler);
```

然后我们可以调用`removeListener`来删除监听器，如下所示:

```
button.removeEventListener(‘click’, handler);
```

对于可观测量，我们可以将从`fromEvents`可观测量发出的值转换如下:

```
import { fromEvent } from "rxjs";
import { map } from "rxjs/operators";
const buttonEl = document.querySelector("button");
const clicks$ = fromEvent(buttonEl, "click").pipe(map(e => e.target));
let subscription = clicks$.subscribe(e => console.log("clicked", e));
```

这不能用本地 DOM 事件 API 来完成。

![](img/ca4162d18997fee36725aa4f5fd86f64.png)

Photo by [David Clode](https://unsplash.com/@davidclode?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 与数组相比的可观测量。

可观测量随着时间的推移发出数值。数组是一组静态的值。

然而，常见的阵列操作也有其可观察的类似物。

如果我们有以下可观察到的:

```
const obs = of(1, 2, 3);
```

和数组:

```
const arr = [1, 2, 3];
```

我们可以对每个进行以下操作:

## 串联

我们可以用 Rxjs `concat`函数连接观察值:

```
import { of, concat } from "rxjs";
const obs = of(1, 2, 3);
const obsB = of(4, 5, 6);
concat(obs, obsB);
```

对于数组，我们可以使用`concat`方法返回一个新的数组:

```
const arr = [1, 2, 3];
const arrB = [4, 5, 6];
const newArr = arr.concat(arrB);
```

## 过滤

我们可以如下使用 Rxjs `filter`运算符:

```
import { of } from "rxjs";
import { filter } from "rxjs/operators";
const obs = of(1, 2, 3);
obs.pipe(filter(v => v > 1));
```

对于数组，我们使用`filter`方法返回一个新数组，该数组的值满足返回的谓词:

```
arr.filter(v => v > 1);
```

## 发现

我们可以如下使用 Rxjs `find`操作符来发出满足条件的单个值:

```
import { of } from "rxjs";
import { find } from "rxjs/operators";
const obs = of(1, 2, 3);
obs.pipe(find(v => v > 1));
```

Arrays 还有一个`find`方法来返回满足谓词的第一个值:

```
arr.find(v => v > 1);
```

## 为每一个

Rxjs 有`tap`操作符来获取发出的值:

```
obs.pipe(tap((v) => {
  console.log(v);
}))
```

Array 有`forEach`方法来获取正在循环的值:

```
arr.forEach((v) => {
  console.log(v);
})
```

## 地图

我们可以使用`map`运算符来映射可观测量发出的值:

```
obs.pipe(map((v) => 2*v))
```

数组也有`map`方法:

```
arr.map((v) => 2*v)
```

## 组合值

Rxjs 有一个`reduce`操作符，将发出的值组合成一个值:

```
obs.pipe(reduce((s, v)=> s + v, 0))
```

数组也有一个`reduce`方法来做同样的事情:

```
arr.reduce((s,v) => s + v, 0)
```

# 结论

可观测量在角坐标的许多地方都有使用。它们在 JavaScript 中有类似的各种实体。

可观察值发出被观察者订阅的值。除非有人订阅，否则它们不会运行。

它们总是异步的，并且有异步代码不具备的额外功能，比如取消和重试。

## **用简单英语写的 JavaScript 笔记**

我们已经推出了三种新的出版物！请关注我们的新出版物，表达对它们的喜爱:[**AI in Plain English**](https://medium.com/ai-in-plain-english)，[**UX in Plain English**](https://medium.com/ux-in-plain-english)，[**Python in Plain English**](https://medium.com/python-in-plain-english)**——谢谢，继续学习！**

**我们也一直有兴趣帮助推广高质量的内容。如果您有一篇文章想要提交给我们的任何出版物，请发送电子邮件至[**submissions @ plain English . io**](mailto:submissions@plainenglish.io)**，使用您的 Medium 用户名，我们会将您添加为作者。另外，请让我们知道您想加入哪个/哪些出版物。****