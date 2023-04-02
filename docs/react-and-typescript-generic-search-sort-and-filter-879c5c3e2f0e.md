# React 和 TypeScript:一般搜索、排序和过滤

> 原文：<https://javascript.plainenglish.io/react-and-typescript-generic-search-sort-and-filter-879c5c3e2f0e?source=collection_archive---------0----------------------->

## 利用 TypeScript 泛型的强大功能实现可重用的搜索、排序和过滤的分步指南。

![](img/93a85712faa4caa5164f5dd1320272b9.png)

[*本帖镜像在我的个人博客****chrisfrew . in****上，这里有语法高亮和代码片段复制！*](https://chrisfrewin.com/blog/react-typescript-generic-search-sort-and-filters/)

*通过我的课程中的完整视频课程，学习这篇文章中详细介绍的所有内容，以及更多内容，Skillshare 和 Udemy 上的* ***【高级打字稿:通用搜索、排序和过滤】****:*

[](https://skl.sh/3oGQMbr) [## 高级类型脚本:一般搜索、排序和筛选| Chris Frewin | Skillshare

### 这门课将会是关于在 TypeScript 中使用泛型。在本课程中，我们将首先回顾…

skl.sh](https://skl.sh/3oGQMbr) [](https://www.udemy.com/course/advanced-typescript-generic-search-sorting-and-filtering/?referralCode=22441D8B6B06045473D2) [## 高级类型脚本:一般搜索、排序和筛选

### Chris Frewin 是一名专业的全栈软件工程师，从事编程已经超过七年…

www.udemy.com](https://www.udemy.com/course/advanced-typescript-generic-search-sorting-and-filtering/?referralCode=22441D8B6B06045473D2) 

# 示例存储库

示例存储库在这里。

# 动机

在最近的一个项目中，我的任务是实现前端过滤和搜索功能。然而，该任务还要求排序和过滤功能可以很容易地应用于任何类型。幸运的是，我一直在大量使用[泛型](https://www.typescriptlang.org/docs/handbook/generics.html)(并且慢慢地越来越好！)，但是我给自己(和我的同事)留下了特别深刻的印象，我构建了一个轻量级的、完全可重用的解决方案。

我想我会和大家分享我的解决方案。尽情享受吧！

# 第一:通用搜索！

让我们先来看看搜索函数，因为它是我们将要构建的三个函数中最简单的一个。

假设我们有一个 API 端点，它返回一个类型为`**T**`的数组。对于搜索，通常，我们希望能够查询(潜在的)多个`**T**`属性，并让搜索返回至少有一个属性匹配的元素。因此，构建这样一个函数，我们需要对象本身、我们想要搜索的属性和查询。

这是 JavaScript 的 `[**Array.prototype.filter()**](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/filter)` [函数](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/filter)的一个完美用例，它从调用它的数组中接受一个对象并返回 true 或 false。

我们还需要函数中的查询值本身，以匹配对象的实际值。

至此，我们已经可以构建函数的签名了:

```
**export function genericSearch<T>(object: T, properties: Array<keyof T>, query: string): boolean {****}**
```

注意这里漂亮的`**keyof T**`输入。那是最重要的！😂

TypeScript 只允许我们传递表示对象`**T**`中键名的字符串值。记住，我们说过我们希望允许*有多个`**T**`类型的*属性，所以这些属性是`**keyof T**`的一个`**Array**`，即`**Array<keyof T>**`。所以我们需要做的第一件事就是遍历这些属性。`**map**`应该做的:

```
**properties.map(property => {****})**
```

在这个`**map**`中，我们现在可以像这样访问值:

```
**object[property]**
```

TypeScript 不会抱怨这个语法，因为它知道`**property**`实际上是一个`**keyof T**` - *但是*，我们不能直接访问属性值*，例如。`**object[property].toString()**`因为 TypeScript 会声明`**keyof T**`不属于`**string**`类型，所以我们需要在一个变量中存储该值的一个副本(这无论如何都会使代码更干净):*

```
***const value = object[property];***
```

*对于属性值和查询之间的实际字符串搜索，我决定使用漂亮的`[**String.prototype.includes()**](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/String/includes)` [函数](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/String/includes)。然而，如果我们试图在我们的`**value**` `**const**`上直接使用`**includes()**`，TypeScript 将*仍然*抱怨，这是理所当然的，因为我们的`**value**`不一定是类型`**string**`。我们可以做一些类型检查来让 TypeScript 满意(即使你决定使用正则表达式而不是`**includes()**`，你也需要同样的类型检查——因为正则表达式只能作用于`**string**`类型。)*

*通过这种类型检查，到目前为止，我们的函数如下所示:*

```
***export function genericSearch<T>(object: T, properties: Array<keyof T>, query: string): boolean {
    properties.map(property => {
        const value = object[property];
        if (typeof value === "string" || typeof value === "number") {
            return value.toString().includes(query)
        }
        return false;
    })
}***
```

*对于我正在构建的搜索的具体情况，我认为如果它是不区分大小写的，那么它是最用户友好的，所以我在对象和查询值上都使用了`**toLowerCase()**`。然而，这可能是一个额外的标志或选项，您可以在其中指定在最后的`**if**`块中做什么。所以，加上这两个`**toLowerCase()**`调用，我们有:*

```
***export function genericSearch<T>(object: T, properties: Array<keyof T>, query: string): boolean {
    properties.map(property => {
        const value = object[property];
        if (typeof value === "string" || typeof value === "number") {
            return value.toString().toLowerCase().includes(query.toLowerCase())
        }
        return false;
    })
}***
```

*现在让我们实际上为我们的`**map**`返回的内容分配一个`**const**`，称之为`**expressions**`:*

```
***export function genericSearch<T>(object: T, properties: Array<keyof T>, query: string): boolean {
    const expressions = properties.map(property => {
        const value = object[property];
        if (typeof value === "string" || typeof value === "number") {
            return value.toString().toLowerCase().includes(query.toLowerCase())
        }
        return false;
    })
}***
```

*现在，`**expressions**`是一个由`**boolean**`值组成的数组。如果*至少*一个如果`**true**`，我们想要返回`**true**`(因为如果查询在我们正在搜索的对象类型的属性的*任何*中，我们想要匹配)。我们可以编写自己的 for 循环或 map，并自己显式地进行这种检查，但这是 2020 年——数组函数再次拯救了我们！看看`[**Array.prototype.some()**](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/some)`！这正是我们想要的，基于一个测试函数。我们的值已经是 true / false，所以我们的测试函数只是返回布尔值本身。*

*所以我们总共有:*

```
***export function genericSearch<T>(object: T, properties: Array<keyof T>, query: string): boolean {
    const expressions = properties.map(property => {
        const value = object[property];
        if (typeof value === "string" || typeof value === "number") {
            return value.toString().toLowerCase().includes(query.toLowerCase())
        }
        return false;
    })
    return expressions.some(expression => expression);
}***
```

*但是等等，我们可以把*甚至*做得更好！由于我们的`**if**`语句和类型检查*是我们的*测试函数，我们可以稍微重构一下，删除`**map**`和`**expressions**`常量，直接在属性上调用`**.some()**`，并返回`**some()**`函数的结果:*

```
***// case insensitive search of n-number properties of type T
// returns true if at least one of the property values includes the query value
export function genericSearch<T>(
    object: T,
    properties: Array<keyof T>,
    query: string
): boolean {** **if (query === "") {
        return true;
    }** **return properties.some((property) => {
        const value = object[property];
        if (typeof value === "string" || typeof value === "number") {
            return value.toString().toLowerCase().includes(query.toLowerCase());
        }
        return false;
    });
}***
```

*😄漂亮！*

# *第二:通用排序器！*

*本着与`**genericSearch**`函数相同的思路，我们来写一个可以接受任何类型`**T**`的`**genericSort**`函数。在`**genericSearch**`被用于`**filter**`数组函数的情况下，`**genericSort**`函数当然会被应用为数组`**sort**`调用的回调。因此，这需要一个比较器函数，接受类型为`**T**`的“a”和“b”对象，以及当前活动的排序器:*

```
***export function genericSort<T>(
  objectA: T,
  objectB: T,
  sorter: ISorter<T>
) {
...
}***
```

*其中 ISorter(也是通用的)是一个帮助接口，帮助我们跟踪活动的过滤器属性，以及排序是否应该是降序的:*

```
***export default interface ISorter<T> {
    property: Extract<keyof T, string | number | Date>;
    isDescending: boolean;
}***
```

*请再次注意，我们在这里使用了`**keyof T**`语法，但是只提取了那些与`**>**`和`**<**`比较器操作预期功能相同的类型(对我们来说是`**string**` s、`**number**` s 和`**Date**`s——您自己的应用程序中可能有更多类型！)我们可以如下使用它:*

```
***const result = () => {
    if (objectA[property] > objectB[property]) {
        return 1;
    } else if (objectA[property] < objectB[property]) {
        return -1;
    } else {
        return 0;
    }
}***
```

*最后，如果降序排序是`**true**`，我们对`**result**`的值取反，并返回它:*

```
***return sorter.isDescending ? result() * -1 : result();***
```

*总之，我们的`**genericSort**`函数看起来像这样:*

```
***import ISorter from "../interfaces/ISorter";****// comparator function for any property within type T
// works for: strings, numbers, and Dates (and is typed to accept only properties which are those types)
// could be extended for other types but would need some custom comparison function here
export function genericSort<T>(
  objectA: T,
  objectB: T,
  sorter: ISorter<T>
) {
  const result = () => {
    if (objectA[sorter.property] > objectB[sorter.property]) {
        return 1;
    } else if (objectA[sorter.property] < objectB[sorter.property]) {
        return -1;
    } else {
        return 0;
    }
  }** **return sorter.isDescending ? result() * -1 : result();
}***
```

# *第三:通用滤镜！*

*对于我们的最后一个功能，让我们实现通用过滤。在`**genericSearch**`是一个`**filter**`回调的情况下，它有点特殊，比较用户输入的值。对于`**T**`(在我们的例子中是`**IWidget**`)的每个给定属性，我们将允许用户选择他们是否想要查看该属性的*真实*或*虚假*的所有项目。*

# *真理？福尔西。啊？🤔*

*`**T**`的任何属性都可以有任何类型。为了避免编写基于各种类型的花哨过滤函数，我退回到 JavaScript 对 *falsy* 和 *truthy* 值的评估——换句话说，一个给定值在布尔语句中使用时的评估结果。概括一下，最常见的 [JavaScript 原语](https://developer.mozilla.org/en-US/docs/Glossary/Primitive)的 *falsy* 值如下:*

*TypeFalsey 值`**objectundefined**`、`**null**`、`**NaNstring""number0booleanfalse**`(咄！😂)*

*其中每种类型的任何其他值在布尔评估中将评估为`**true**`。*

*我意识到为用户提供每个属性的真与假的选项可能有些过头了。您可以决定对某些属性只提供一个过滤器。这取决于您正在过滤的实际项目以及您希望 UI 中包含的内容。为了完整性和您的方便，我已经实现了。😄*

*也就是说，我们可以预期我们的`**genericFilter**`需要什么样的签名。我们需要将出现在`**filter()**`回调中的`**T**`类型的对象，以及活动过滤器本身:*

```
***export function genericFilter<T>(object: T, filters: Array<IFilter<T>>) {
...
}***
```

*其中`**IFilter**`是一个帮助界面(也是通用的),帮助跟踪我们正在过滤的属性，以及用户是否选择查看它们真实或虚假的一面:*

```
***export default interface IFilter<T> {
    property: keyof T;
    isTruthyPicked: boolean;
}***
```

*然后，我们要确保选择的每个*过滤器都适用于我们当前正在过滤的项目。这是 JavaScript 的 `[**Array.prototype.every()**](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/every)` [函数](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/every)的完美用例。**

*回到 JavaScript 的 truthy / falsy 评估并使用`**Array.prototype.every()**`,`**genericFilter**`的实际过滤逻辑非常容易理解:*

```
***return filters.every((filter) => {
    return filter.isTruthyPicked ? object[filter.property] :     !object[filter.property];
});***
```

*回到每个属性的 truthy 和 falsy 选项:我生成了一对单选按钮，用户可以根据它们的 truthy 或 falsy 值显式地过滤对象。*

*例如，对于我们的`**IWidget**`的`**title**`属性，用户可以明确地选择显示所有`**title**`为真的结果。标记为“is falsy”的单选按钮当然会提供相反的结果(显示小部件，其中`**title**`是一个空字符串——到目前为止，在我的模拟数据中只有一个在[示例存储库](https://github.com/princefishthrower/react-typescript-generic-search-sort-and-filter)中)。或者，当没有为给定属性选择单选按钮时，当然对该属性的过滤没有影响。*

*您可能还想实现一个 clear all 按钮，该按钮将删除在`**genericSearch**`中使用的`**filters**`数组(有状态的，更多细节见下一节)中的所有项目，但是我将把这个任务留给您。😄*

*总之我们的`**genericFilter**`函数看起来是这样的:*

```
***import IFilter from "../interfaces/IFilters";****// filter n properties for truthy or falsy values on type T (no effect if no filter selected)
export function genericFilter<T>(object: T, filters: Array<IFilter<T>>) {
  // no filters; no effect - return true
  if (filters.length === 0) {
    return true;
  }** **return filters.every((filter) => {
    return filter.isTruthyPicked ? 
    object[filter.property] : 
    !object[filter.property];
  });
}***
```

# *把一切都联系起来*

*因此，我们有(在我看来)非常酷的通用`**genericSearch**`、`**genericSort**`和`**genericFilter**`函数。让我们把它们接到我们的阵列上。*

*对于没有过滤的标准列表呈现(姑且称之为`**widgets**`带`**Array<IWidget>**` ) *，你应该这样做:**

```
***widgets.map(
    item => return <SomeComponentToRenderYourWidget {...object}/>
)***
```

*为了挂接我们的函数，我们应该这样做:*

```
***import { genericSearch } from "./utils/genericSearch";
import { genericSort } from "./utils/genericSort";
import { genericFilter } from "./utils/genericFilter";
import IWidget from './interfaces/IWidget';****...****return (
    <>
        {widgets
            .filter((widget) =>
                genericSearch<IWidget>(widget, ["title", "description"], query)
            )
            .sort((widgetA, widgetB) =>
                genericSort<IWidget>(widgetA, widgetB, activeSorter)
            )
            .filter((widget) => 
                genericFilter<IWidget>(widget, activeFilters)
            ).map(widget => 
                return <SomeComponentToRenderYourWidget {...widget}/>
            )
        }
    </>
)***
```

*通过向`**genericSearch**`函数提供`**<IWidget>**`类型，如果在`**properties**`数组中传递的任何字符串在`**IWidget**`中不存在，TypeScript 将对我们大喊。同样的还有`**genericFilter**`和`**genericSort**`。这里不再有讨厌的运行时错误！*

*即使我们忘记了`**IWidget**`中的某个属性是`**object**`或者某个无法排序或搜索的其他类型，我们也知道这些属性不会对搜索结果有任何影响，因为我们在`**search**`和`**sort**`函数中进行了类型检查(在这种情况下我们返回`**false**`)。*

*我们的`**filter**`函数是*函数，所以*是泛型的，我们根本不用担心，并且可以在这里传递所有类型的属性，这要归功于 JavaScript 的真实和虚假的功能。*

# *重要警告*

*事实上，在上面的代码片段中，我遗漏了一些步骤来获得一个完全运行的应用程序。您需要您的`**query**`、`**activeSorter**`和`**activeFilters**`变量是有状态的，我们当然必须实际实现`**<SomeComponentToRenderYourWidget/>**`组件来呈现每个`**widget**`中的值。不过，这些都是在[示例存储库中](https://github.com/princefishthrower/react-typescript-generic-search-sort-and-filter)实现的。*

# *这应该成为节点包吗？😍*

*如果有兴趣的话，通过进一步的清理和选项设置，以及一些微调，我相信这可以移植到一个开源项目和 Node.js 包中——尽管我确信还有其他的过滤/排序/搜索包和解决方案。请在评论中告诉我！*

# *塞:在我的课程中了解更多信息！*

*您可以在本文中了解到所有细节，并通过我课程中的完整视频课程,**“高级类型脚本:通用搜索、排序和过滤”**可在 Skillshare 和 Udemy 上获得:*

*[](https://skl.sh/3oGQMbr) [## 高级类型脚本:通用搜索、排序和过滤| Chris Frewin | Skillshare

### 本课程将讲述如何在 TypeScript 中使用泛型。在本课程中，我们'将从复习…

skl.sh](https://skl.sh/3oGQMbr) [](https://www.udemy.com/course/advanced-typescript-generic-search-sorting-and-filtering/?referralCode=22441D8B6B06045473D2) [## 高级类型脚本:通用搜索、排序和过滤

### Chris Frewin 已经是一名专业的完整堆栈软件工程师超过七年，并为更多的人编程…

www.udemy.com](https://www.udemy.com/course/advanced-typescript-generic-search-sorting-and-filtering/?referralCode=22441D8B6B06045473D2) 

要免费预览前几节课，请查看 YouTube 上的播放列表:

# 谢谢你！

我希望你喜欢这个帖子。我非常喜欢 TypeScript 及其泛型能力的力量。我希望这个帖子对你有用！

干杯！🍺

-克里斯*