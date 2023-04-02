# Rxjs 过滤运算符—区分度

> 原文：<https://javascript.plainenglish.io/rxjs-filtering-operators-distinctness-fb4c78bc1dd?source=collection_archive---------2----------------------->

![](img/0badfea634ea72f85ad6a9f41e751a39.png)

Photo by [Kelly Arnold](https://unsplash.com/@kelarn?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

Rxjs 是一个用于进行反应式编程的库。创建操作符对于从各种数据源生成供观察者订阅的数据非常有用。

在本文中，我们将研究一些过滤操作符，包括`distinct`、`distinctUntilChanged`和`distinctUntilKeyChanged`操作符。

# 明显的

`distinct`操作符发出来自源可观察的项目，这些项目与来自源的先前项目不同。

它有两个可选参数。第一个是`keySelector`函数，它让我们选择想要检查哪个值是不同的。

第二个是可选的`flushes`,用于清除操作者的内部哈希表。

它返回一个新的可观察对象，该对象发出不同的值。

下面是一个简单的例子:

```
import { of } from "rxjs";
import { distinct } from "rxjs/operators";of(3, 3, 3, 3, 3, 3, 35, 5, 7, 8, 4, 6, 3, 5, 2, 4, 2)
  .pipe(distinct())
  .subscribe(x => console.log(x));
```

我们有以下具有重复值的可观察值:

```
of(3, 3, 3, 3, 3, 3, 35, 5, 7, 8, 4, 6, 3, 5, 2, 4, 2)
```

来自它的值被`pipe` d 传递给`distinct()`操作符以过滤掉重复的值。

然后我们得到:

```
3
35
5
7
8
4
6
2
```

从`console.log`开始。

我们还可以检查某个条目的某个键是否不同，如下所示:

```
import { of } from "rxjs";
import { distinct } from "rxjs/operators";const people = [
  { age: 4, name: "Joe" },
  { age: 7, name: "Jane" },
  { age: 5, name: "Jane" }
];of(...people)
  .pipe(distinct(p => p.name))
  .subscribe(x => console.log(x));
```

上面的代码将`people`数组作为`of`操作符的参数展开，该操作符发出`people`数组中的对象。然后，发出的值被`pipe` d 到`distinct`操作符中，该操作符选择`name`属性来检查清晰度。

则仅发射具有不同`name`值的对象。

最后，我们得到:

```
{age: 4, name: "Joe"}
{age: 7, name: "Jane"}
```

作为可观察对象。

# distinctUntilChanged

`distinctUntilChanged`发射从源发出的所有可观察到的项目，这些项目与来自源的前一个项目相比是不同的。

它有两个可选参数，这是一个`compare`函数，用于测试一个项目是否与前一个项目不同。第二个参数是`keySelector`函数，用于返回我们想要检查的键值。

一个简单的例子如下:

```
import { of } from "rxjs";
import { distinctUntilChanged } from "rxjs/operators";of(1, 1, 5, 5, 6, 7, 8, 8, 8, 8, 9)
  .pipe(distinctUntilChanged())
  .subscribe(x => console.log(x));
```

从`of(1, 1, 5, 5, 6, 7, 8, 8, 8, 8, 9)`到`distinctUntilChanged`操作器的值是`pipe` d，之前发出的值与当前发出的值进行核对，看它们是否相同。

然后我们得到:

```
1
5
6
7
8
9
```

因为发出的值不同于之前发出的值。

对于发射对象的可观察对象，我们可以通过向`distinctUntilChanged`操作符传递一个函数来检查属性值是否相同，如下所示:

```
import { of } from "rxjs";
import { distinctUntilChanged } from "rxjs/operators";
const people = [
  { age: 4, name: "Joe" },
  { age: 7, name: "Jane" },
  { age: 5, name: "Jane" }
];
of(...people)
  .pipe(distinctUntilChanged((p, q) => p.name === q.name))
  .subscribe(x => console.log(x));
```

第一部分和最后一部分的工作方式与前面的示例类似。不同的是，现在我们有了`(p, q) => p.name === q.name`函数来检查先前发射的对象的`name`值是否与当前发射的相同。

然后我们得到:

```
{age: 4, name: "Joe"}
{age: 7, name: "Jane"}
```

作为`console.log`的输出。

![](img/2512621d003bedb6725d35d83e55aa6d.png)

Photo by [Francisco Delgado](https://unsplash.com/@fdelgado?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# distinctUntilKeyChanged

`distinctUntilKeyChanged`返回一个发出源可观察值的可观察值，该值通过与先前从源发出的对象的键进行比较来区分。

这意味着，如果给定键的值不同于先前发出的对象键的值，将会发出一个项。

它最多需要两个参数。第一个是`key`，它是一个带有 object 属性的字符串，用于查找每一项。

第二个是可选的`compare`函数，调用它来测试一个项目是否不同于之前从源 Observable 发出的项目。

它返回一个可观察对象，该可观察对象从源可观察对象中发出一个项目，该项目具有与先前从源发出的项目不同的属性值。

例如，我们可以重写前面的例子:

```
import { of } from "rxjs";
import { distinctUntilChanged } from "rxjs/operators";
const people = [
  { age: 4, name: "Joe" },
  { age: 7, name: "Jane" },
  { age: 5, name: "Jane" }
];
of(...people)
  .pipe(distinctUntilChanged((p, q) => p.name === q.name))
  .subscribe(x => console.log(x));
```

变成:

```
import { of } from "rxjs";
import { distinctUntilKeyChanged } from "rxjs/operators";
const people = [
  { age: 4, name: "Joe" },
  { age: 7, name: "Jane" },
  { age: 5, name: "Jane" }
];
of(...people)
  .pipe(distinctUntilKeyChanged("name"))
  .subscribe(x => console.log(x));
```

那么我们应该得到和以前一样的结果。我们所做的只是改变:

```
distinctUntilChanged((p, q) => p.name === q.name)
```

收件人:

```
distinctUntilKeyChanged("name")
```

我们可以使用`distinct`操作符，通过比较对象的原始值或属性值，获得从可观察源发出的不同值。

`distinctUntilChanged`和`distinctUntilKeyChanged`让我们从一个源可观测物发出的项目不同于先前由源可观测物发出的项目。同样，它可以通过对象的原始值或属性值进行比较。