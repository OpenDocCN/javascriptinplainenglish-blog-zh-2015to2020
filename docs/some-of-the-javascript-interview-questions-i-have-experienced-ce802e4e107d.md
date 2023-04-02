# 我经历过的一些 JavaScript 面试问题

> 原文：<https://javascript.plainenglish.io/some-of-the-javascript-interview-questions-i-have-experienced-ce802e4e107d?source=collection_archive---------7----------------------->

## 我的一些个人经历

![](img/f28495b85fa0fa9090c91030c0e5692f.png)

Photo by [Headway](https://unsplash.com/@headwayio?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

我最近参加了一些 JS 面试，在面试中我被要求实现一些常用 JS 函数的定制实现。我们在日常的 JS 任务中使用它们。所以我想和大家分享这些。

虽然你可能从来不需要提出自己的东西，但理解和思考它们是很好的。

如果你没有为面试做准备，只是想从总体上了解 JavaScript，我邀请你一起来通读，因为你可能会从这篇文章中有所收获。

# 平的

将嵌套数组展平到所需级别。

例如

```
const arr = [[1,2,3], 2,5,4,56, [4,5,[45,6,7]]];
const flatArr = arr.flat(1);
flatArr; // [1,2,3,2,5,4,56,4,5,[45,6,7]]
```

下面是自定义实现:

customFlat 接受三个参数:`array`、`level`、`result array`(可选)。

我们简单地遍历数组并检查当前项是否是一个数组；如果是，我们把它分布在结果数组中，否则我们直接把它推入数组。此后，我们递归调用平面函数，直到级别为 0。我们最终返回结果数组。

下面是用法:

```
const arr = [[1,2,3], 2,5,4,56, [4,5,[45,6,7]]];
const flatArr = customFlat(arr, 2);
flatArr; // [1,2,3,2,5,4,56,4,5,[45,6,7]]
```

# 去抖

去抖可以用在像搜索这样的场景中，你不想每次击键都触发请求；相反，您希望在发送请求之前有一个冷却期。

下面的示例记录了每次按钮点击:

Logs on every button click

让我们先看看去抖的实现，然后应用到例子中:

分解一下:

1.  我们的去抖函数有两个参数:`delay`和`function`来执行这个延迟。
2.  我们在去抖函数中定义了一个`timeoutID`，借助闭包的魔力，我们能够在从它返回的函数内部访问它。
3.  在内部函数中，我们清除 timeoutID(如果有的话),否则我们通过调用 setTimeout 来分配一个新的 ID。
4.  **清零** `**timeoutID**` **可确保 setTimeout 内的函数不会执行，直到延迟时间过去。**

Logs after 2s has passed

# 喉咙

虽然反跳确保传递给它的函数在一定的延迟后被调用，但 throttle 每次都至少在延迟过去后调用该函数。

以下是自定义节流的代码:

分解一下:

1.  这个函数还接受两个参数:`delay`和`function`来执行。
2.  它有一个`last`变量，该变量被设置为功能注册时的当前时间。
3.  内部功能检查`current`时间和`last`时间的差值是否小于`delay`。如果是，我们返回，因为我们不想调用该函数，否则我们执行该函数，并将最后一次作为当前时间。

logs after atleast 2s

# 约束

`bind()`方法返回一个新的函数，当被调用时，它的`this`被设置为一个特定的值。

如您所见，Person 函数显式绑定到传递给它的对象，即 p1。

让我们现在编写 bind 的自定义版本:

该函数的作用如下:

1.  我们正在`Function prototype`上定义一个功能`myBind`
2.  它接受的参数为`args`
3.  有`obj`设置为我们正在调用的功能`myBind`。在我们的案例中，它是人。
4.  有`rest`取除第一个参数以外的所有参数。`args[0]`是我们绑定函数的地方，其余的参数是我们称之为`myBind`函数的地方。在我们的例子中，`args[0]`是 p1 对象，其余的参数是`cricket`
5.  最后，我们返回一个函数，它接受自己要调用的参数。我们在传递相关参数时，简单地将从外部范围获得的`obj`称为`apply`。

在前面的示例中使用它:

```
const bindMe = Person.myBind(p1, 'cricket');
console.log(bindMe('Sachin'));// Ravi 65 [ 'cricket', 'Sachin' ]
console.log(bindMe('Sehwag'));// Ravi 65 [ 'cricket', 'Sehwag' ]
```

# 深度冻结对象

![](img/9556f123eadfb4b9ecf473c8b8519fcf.png)

Photo by [Barbara Kosulin](https://unsplash.com/@barkosulin?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

我们已经在 javascript 对象上使用了 freeze 方法，它可以防止新属性被添加到其中，现有属性被移除，防止现有属性的可枚举性、可配置性或可写性发生变化，并防止现有属性的值发生变化。

但它只在第一层起作用。因此，作为一个挑战，我被要求实现一个在嵌套层冻结对象的自定义实现。

这是我提出的递归实现:

这就是上述函数的作用:

1.  检查`typeof`对象的值本身是否是一个对象。如果没有，我们就回来。
2.  然后，我们循环对象值，用传入的值递归调用 deepFreeze。
3.  最后，我们只需调用我们的`obj`上的`Object.freeze`

测试以上内容:

```
const obj = {
    a:123,
    b:{
       c:4334534534
    }
}
deepFreeze(obj);obj.b.c = 234;console.log(obj); // {a:123, b:{c: 4334534534}}
```

# 结论

这些都是被问到的问题。很明显，我无法解决所有问题，但后来我明白了如何在本地解决。我希望这能对某人未来的努力有所帮助。

[](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/flat) [## Array.prototype.flat()

### flat()方法创建一个新数组，所有子数组元素递归地连接到该数组中，直到指定的…

developer.mozilla.org](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/flat) [](https://css-tricks.com/debouncing-throttling-explained-examples/) [## 通过示例解释去抖动和节流| CSS 技巧

### 以下是伦敦前端工程师 David Corbacho 的客座博文。我们以前讨论过这个话题，但是…

css-tricks.com](https://css-tricks.com/debouncing-throttling-explained-examples/) [](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_objects/Function/bind) [## 函数.原型.绑定()

### bind()方法创建了一个新函数，当调用该函数时，它的 this 关键字被设置为提供的值，并带有一个…

developer.mozilla.org](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_objects/Function/bind) [](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Object/freeze#:~:text=A%20frozen%20object%20can%20no,existing%20properties%20from%20being%20changed.) [## Object.freeze()

### 方法的作用是:冻结一个对象。冻结的对象不能再被改变；冻结对象会阻止新的…

developer.mozilla.org](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Object/freeze#:~:text=A%20frozen%20object%20can%20no,existing%20properties%20from%20being%20changed.) 

## **用简单英语写的 JavaScript**

你知道我们有四份出版物和一个 YouTube 频道吗？在 [**plainenglish.io**](https://plainenglish.io/) 和 [**找到它们订阅我们的 YouTube 频道**](https://www.youtube.com/channel/UCtipWUghju290NWcn8jhyAw) **！**