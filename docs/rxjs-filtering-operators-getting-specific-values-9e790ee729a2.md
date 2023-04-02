# Rxjs 过滤运算符—获取特定值

> 原文：<https://javascript.plainenglish.io/rxjs-filtering-operators-getting-specific-values-9e790ee729a2?source=collection_archive---------0----------------------->

![](img/846eb3afae745cf2464f584170f9547b.png)

Photo by [joel herzog](https://unsplash.com/@joel_herzog?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

Rxjs 是一个用于进行反应式编程的库。创建操作符对于从各种数据源生成供观察者订阅的数据非常有用。

在本文中，我们将看看一些过滤操作符，包括`elementsAt`、`filter`、`first`、`ignoreElements`和`last`。

# 元素 At

`elementAt`运算符让我们在可观测源的一系列发射中的指定`index`处获得一个单一值。

它最多需要两个参数。第一个是`index`，它是从 0 开始的数字，是源可观察对象从订阅开始发出的。

第二个是`defaultValue`，它是一个可选参数，如果没有找到给定索引的值，则返回默认值。

它返回一个可观察对象，如果找到它，它会发出一个单独的项目。否则，将发出由可选的第二个参数提供的默认值。

如果提供的`index`小于 0，或者在发出给定`index`的发射之前，可观测值已经完成，将抛出`ArgumentOutOfRangeError`。

例如，我们可以如下使用它:

```
import { of } from "rxjs";
import { elementAt } from "rxjs/operators";const of$ = of(1, 2, 3);
const result = of$.pipe(elementAt(2));
result.subscribe(x => console.log(x));
```

然后我们从`console.log`得到 3。

# 过滤器

`filter`操作符通过从满足谓词函数返回的条件的源可观察对象中发出值，让我们过滤掉从源可观察对象中发出的值。

它最多需要两个参数。第一个是`predicate`函数，它在源可观察对象每次发出一个值时被评估。根据这个值进行检查，如果它符合标准，则由返回的 Observe 发出。

`predicate`函数有两个参数，分别是源可观测物体和`index`发射的物体。`index`从 0 开始，是订阅以来发出的第`index`个对象。

例如，我们可以如下使用它:

```
import { of } from "rxjs";
import { filter } from "rxjs/operators";const of$ = of(1, 2, 3, 4, 5, 6);
const result = of$.pipe(filter(x => x % 2 === 0));
result.subscribe(x => console.log(x));
```

然后我们得到:

```
2
4
6
```

# 第一

`first`操作符仅发出由源可观察对象发出的第一个值。

它有两个可选参数。第一个是`predicate`函数，每个条目都调用它来测试条件匹配。

第二个是默认值，可选。这是在从源中找不到有效值的情况下发出的默认值。

例如，我们可以如下使用它:

```
import { of } from "rxjs";
import { first } from "rxjs/operators";const of$ = of(1, 2, 3, 4, 5, 6);
const result = of$.pipe(first());
result.subscribe(x => console.log(x));
```

上面的代码从`of$`可观测值中获取值并发出第一个值，所以我们从`console.log`中得到 1。

我们还可以传入一个函数来检查来自源可观察对象的匹配。例如，我们可以写:

```
import { of } from "rxjs";
import { first } from "rxjs/operators";const of$ = of(1, 2, 3, 4, 5, 6);
const result = of$.pipe(first(x => x % 3 === 0));
result.subscribe(x => console.log(x));
```

因为我们将`x => x % 3 === 0`传递给了`first`操作符，`first`将检查第一个匹配我们条件的操作符，它是一个能被 3 整除的数。

然后我们从`console.log`得到值 3，因为这是第一个符合条件的`of$`数。

我们可以传入一个默认值作为`first`的第二个参数:

```
import { of } from "rxjs";
import { first } from "rxjs/operators";
const of$ = of(1, 2, 3, 4, 5, 6);
const result = of$.pipe(first(x => x % 3 === 10, "none"));
result.subscribe(x => console.log(x));
```

因为在`of$`中没有一个数在除以 3 后还有余数 10，所以`'none'`字符串将由返回的可观测值发出。

![](img/121904dbf830092252ae4a56f9279492.png)

Photo by [Pedro Lastra](https://unsplash.com/@peterlaster?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# ignoreElements

`ignoreElements`忽略源可观察对象发出的所有项目，只传递对`complete`或`error`的调用。

这不需要争论。

例如，我们可以如下使用它:

```
import { of } from "rxjs";
import { ignoreElements } from "rxjs/operators";
const of$ = of(1, 2, 3, 4, 5, 6);
const result = of$.pipe(ignoreElements());
result.subscribe(
  val => console.log(val),
  err => console.log(err),
  () => console.log("end")
);
```

因为在完成之前没有从`of$`可观察到的东西发出，所以我们只记录了`'end'`。

# 最后的

`last`操作符返回一个可观察对象，它只发出源可观察对象发出的最后一项。如果一个`predicate`函数被传入`last`，那么与`predicate`返回的条件相匹配的最后一个值将被发出。

它有两个可选参数。第一个是`predicate`，可选。`predicate`返回信号源发射值必须满足的条件。

第二个是`defaultValue`的可选参数，如果从源可观测对象发出的任何东西都不满足`predicate`函数返回的条件，那么将发出这个值。

例如，我们可以如下使用它:

```
import { of } from "rxjs";
import { last } from "rxjs/operators";
const of$ = of(1, 2, 3, 4, 5, 6);
const result = of$.pipe(last());
result.subscribe(val => console.log(val));
```

然后我们记录 6，因为它是由`of$`可观测值发出的最后一个值。

我们还可以指定如下条件:

```
import { of } from "rxjs";
import { last } from "rxjs/operators";
const of$ = of(1, 2, 3, 4, 5, 6);
const result = of$.pipe(last(x => x % 2 === 1));
result.subscribe(val => console.log(val));
```

然后我们记录 5，因为它是来自奇数的`of$`可观察值的最后一个值。

`elementAt`运算符让我们在指定的`index`从可观测源的一系列发射中获得单个值。

`filter`操作符只从满足谓词函数返回的条件的源可观察对象中发出值。

`first`运算符仅发出可观测源发出的第一个值。

`ignoreElements`并且只传递`complete`或`error`的呼叫，而忽略其他一切。

`last`返回一个可观察对象，该可观察对象只发出源可观察对象发出的最后一项，或者是满足谓词函数中条件的最后一项(如果指定的话)。