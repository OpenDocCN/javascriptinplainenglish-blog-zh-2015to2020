# 更多 Rxjs 变换运算符—合并扫描和选取

> 原文：<https://javascript.plainenglish.io/more-rxjs-transformation-operators-mergescan-and-pluck-2f09510abef?source=collection_archive---------6----------------------->

![](img/726b1f1ecdd41f839ac9c101f23fbf52.png)

Photo by [Taylor](https://unsplash.com/@xoutcastx?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

Rxjs 是一个用于进行反应式编程的库。创建操作符对于从各种数据源生成供观察者订阅的数据非常有用。

在本文中，我们将看看一些转换操作符，如`mergeScan`、`pairwise`、`partition`和`pluck`操作符。

# 合并扫描

`mergeScan`运算符对源可观测值应用累加器函数，累加器函数本身返回一个可观测值，返回的每个中间可观测值被合并到输出可观测值中。

最多需要 3 个参数。第一个是对每个源值调用的累加器函数。

第二个参数是`seed`参数，取初始累加值。

最后一个参数是可选参数，它采用了要同时订阅的最大数量的`concurrent`输入观察值。

例如，我们可以如下使用它:

```
import { of, interval } from "rxjs";
import { mapTo, mergeScan } from "rxjs/operators";const interval$ = interval(5000);
const one$ = interval$.pipe(mapTo(1));
const seed = 0;
const count$ = one$.pipe(mergeScan((acc, one) => of(acc + one), seed));
count$.subscribe(x => console.log(x));
```

上面的代码有`interval$`可观察值，它通过`mapTo`操作符映射到值 1。然后我们将`seed`初始值设为 0。

然后，我们通过`mergeScan`操作符将`interval$`可观测值的发出值发送给累加器函数，当`interval$`的值发出时，累加器函数一直加 1。然后，当我们订阅`count$`时，我们从`$count`获取发出的号码。

# 成对地

`pairwise`将来自可观测源的成对连续发射组合在一起，然后将它们作为数组 2 值发射。

它需要争论。

例如，我们可以在下面的示例中使用它:

```
import { of } from "rxjs";
import { pairwise } from "rxjs/operators";const of$ = of(1, 2, 3, 4, 5, 6);
const pair$ = of$.pipe(pairwise());
pair$.subscribe(x => console.log(x));
```

上面的代码将把从`of$`可观察值发出的值分组成对，然后用`pairwise()`操作符发出这些值。然后我们从`console.log`得到以下输出:

```
[1, 2]
[2, 3]
[3, 4]
[4, 5]
[5, 6]
```

# 划分

`partition`操作符将源可观察对象分成两部分，其中一部分的值满足谓词，另一部分的值不满足谓词。

它最多需要两个参数。第一个是`predicate`，这是一个评估源可观测值发出的每个值的函数。然后如果函数返回`true`，那么在第一个返回上发出的值在发出的数组中可观察到。否则，它会在第二个可观察到的数组中发出。

第二个是`thisArg`，它让我们在谓词函数中设置`this`的值。

例如，我们可以如下使用它:

```
import { of } from "rxjs";
import { partition } from "rxjs/operators";const of$ = of(1, 2, 3, 4, 5, 6, 7, 8, 9, 10);
const parts = of$.pipe(partition(x => x % 2 === 0));
const [even, odd] = parts;
odd.subscribe(x => console.log(`odd numbers: ${x}`));
even.subscribe(x => console.log(`even numbers: ${x}`));
```

在上面的代码中，我们用一些数字观察到了`of$`。然后我们`pipe`将它放入`partition`操作符中，该操作符采用一个谓词函数来检查从`of$`可观察对象发出的数字是否为偶数。

`parts`将被设置为从`partition`操作器返回的 2 个可观测量。`even`可观察对象发射`even`号，而`odd`可观察对象发射奇数号。

然后，我们从`odd`可观察值中获得以下输出:

```
odd numbers: 1
odd numbers: 3
odd numbers: 5
odd numbers: 7
odd numbers: 9
```

然后，我们从`even`可观察值中得到以下输出:

```
even numbers: 2
even numbers: 4
even numbers: 6
even numbers: 8
even numbers: 10
```

![](img/22779d2eced90f8b4000a09655e50eed.png)

Photo by [Thomas Borowski](https://unsplash.com/@thomasborowski?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 勇气

`pluck`操作符将每个从源可观察对象发出的值映射到其指定的嵌套属性。

它接受一个参数，即`properties`对象，这是从每个源值中提取的嵌套属性。

例如，我们可以如下使用它:

```
import { of } from "rxjs";
import { pluck } from "rxjs/operators";const people = [
  { name: { firstName: "Mary" }, age: 10, gender: "female" },
  { name: { firstName: "Joe" }, age: 11, gender: "male" },
  { name: { firstName: "Amy" }, age: 10, gender: "female" }
];
const people$ = of(...people);
const plucked = people$.pipe(pluck("name", "firstName"));
plucked.subscribe(x => console.log(x));
```

上面的代码有一个`people`数组，其中每个条目都有一个`name`对象。我们用`of`操作符发出值，然后用`pipe`发出值，并使用`pluck`获得嵌套属性`firstName`，它在`name`内部。

那么我们应该得到:

```
Mary
Joe
Amy
```

作为最后一行的输出。

`mergeScan`对源可观察值应用累加器函数，累加器函数本身返回一个可观察值。然后将返回的每个中间可观测值合并到输出可观测值中，一起发出。

`pairwise`将来自可观测源的成对连续发射组合在一起，然后将它们作为数组 2 值发射。

`partition`操作符将源可观察对象分成两部分，其中一部分的值满足谓词函数返回的条件，另一部分的值不满足。

`pluck`操作符将每个从源可观察对象发出的值映射到指定嵌套属性的值。