# 详细了解 JavaScript 中的 filter()、map()和 reduce()

> 原文：<https://javascript.plainenglish.io/take-a-good-look-at-filter-map-and-reduce-in-javascript-18d72f483ada?source=collection_archive---------6----------------------->

## 深入了解 JavaScript 中的 filter()、map()和 reduce()方法。

在 JavaScript 中，我们将这三个方法作为 Array.prototype 方法的一部分，但是它们之间有什么区别，它们到底是做什么的？所以，让我们深入了解一下吧！

![](img/f56e1e362ba7d983f8dda609070bc0a0.png)

Photo by [Mazhar Zandsalimi](https://unsplash.com/@m47h4r?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 1.所以让我们从 filter()方法开始:

filter()方法应用于一个数组，然后创建一个新数组，在新数组上，它根据我们提供的函数过滤第一个数组。例如，让我们假设我们有一个名为 *oldArray* 的数组，它包含一些元素，我们只想保留其中的一些特定成员，并将它们存储在一个名为 *newArray 的新数组中，这样*我们就可以使用 filter()方法来实现这个目的:

```
//ES5
var oldArray = [10, 20, 30, 40, 50];
var newArray = oldArray.filter(function(number){
    return number > 35
});
console.log(newArray) // [40, 50];
```

我们也可以在 **ES6** 中使用箭头功能，如下所示:

```
// ES6
const oldArray = [10, 20, 30, 40, 50];
const newArray = oldArray.filter(number=> number > 35);
console.log(newArray); // [40, 50]
```

请记住，你也可以在这里定义并调用一个函数

过滤方法有两个基本要素:

*   它**创建了一个新的数组，**，所以如果我们记录上面的*旧数组*，我们仍然有原来的数组。
*   我们传递给 filter 方法的函数实际上抛出了一个 **True/False，**所以这个元素是否会成为 newArray 的一部分。

*filter()方法主要用于当我们拥有完整的数据，并且我们只想显示我们想要的一组特定项目时；例如，我们有一个书店中所有书籍的列表，我们想只显示物理书。*

# 2.map()方法:

这个方法得到一个数组，然后**按照函数的意愿转换**该数组的元素，然后创建一个新的数组并将它们全部存储在新的数组中。假设我们有一个数字数组，出于某种原因，我们想把它的所有元素都加倍；我们可以使用 *forEach* 循环来获取所有元素，然后将它们加倍，**但是**我们也可以使用**map()方法**。

因此，让我们重新检查旧数组，看看如何使用 map()方法:

```
//ES5
var oldArray = [10, 20, 30, 40, 50];
var newArray = oldArray.map(function(number){
    return number * 2
});
console.log(newArray) // [20, 40, 60, 80, 100];
//ES6
const oldArray = [10, 20, 30, 40, 50];
const newArray = oldArray.map(number => number * 2);
console.log(newArray) // [20, 40, 60, 80, 100];
```

与 forEach 相比，map()方法的重要之处在于，forEach 循环**实际上并不**返回任何东西，而且**还会改变原始数组。**相比之下**，**map()方法**创建一个新数组。**

# 3.reduce()方法:

reduce()方法获得一个数组，**将它简化为一个值**。例如，假设我们有一个数字数组，我们想得到数字的总数，我们可以创建一个新变量并将其设置为零，通过使用 For 循环，我们可以将数组中的每个元素添加到我们刚刚创建的变量中。或者我们可以只提供一个**缩减器函数**给 *reduce()方法*，如下所示:

```
//ES5
var myArray = [1, 2, 3, 4];
var reducedAmount = myArray.reduce(function(total, currentValue){
    return total + currentValue
});
console.log(reducedAmount); // 10
//ES6
const myArray = [1, 2, 3, 4];
const reducedAmount = myArray.reduce(
  (accumulator, currentValue) => accumulator + currentValue);
console.log(reducedAmount); // 10
//here we have reduced myArray to a single number value
```

![](img/2261076f7712b0c07b5a77bd9feee2af.png)

Photo by [Mohammad Rahmani](https://unsplash.com/@afgprogrammer?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 如何像专业人士一样使用 reduce()方法:

我们提到 reduce()获取一个数组并将其简化为单个值，但是要记住这个**值**也可以是一个*对象*或者一个新的*数组*。我们还可以为这个值设置一个初始数量，然后减少一个数组，并将其与默认值一起存储。例如，假设我们有一个数组。我们想把它简化成一个新的数组*(如前所述，我们可以把一个数组简化成一个值，这个值也可以是一个数组)。*

我们还想在新数组中添加一些**不在*旧数组*中的**元素，这样我们就可以简单地传递一个包含我们想要的元素的数组，然后将*旧数组*归入其中。我知道这听起来可能有点不清楚，所以让我们看看它的代码。在前面的例子中，我们得到了一个数组元素的总量，但是现在我们想把总量加到我们想要的一个**初始值**上，这时我们可以把另一个*参数*传递给 reduce()方法，这是`accumulator`的开始状态，如下面的代码所示，我们把 10 作为第二个参数。

```
const oldArray = [1, 2, 3, 4];
let sum = oldArray.reduce((accumulator,currentValue) => {
 return accumulator + currentValue;
},10); 
console.log(sum); // 20 ((1+2+3+4)+10)
```

## reducer 函数有四个参数:

*   `accumulator`
*   `currentValue`
*   `currentIndex`
*   `array`

因此，让我们跳到另一个例子中来看看它们的作用:假设我们有一个数字数组，并想求出该数组的平均值。事实上，我们也可以使用 for 循环，但是让我们看看如何使用 reduce 方法来实现这一点:

```
const numArray = [10, 20, 30];
const average = numArray.reduce(
  (accumulator, currentValue, currentIndex, array) => {
    accumulator += currentValue;
    return currentIndex === array.length - 1 ? 
      accumulator / array.length : accumulator
  }
}); 
// we have not provided an initial value
console.log(average); // 20
```

**请记住**如果没有提供*初始值*，reduce 方法将从索引 **1** 开始执行其回调函数，并跳过第一个索引。但是如果提供了*初始值*，它将从索引 **0** 开始。如果数组为空并且没有提供*初始值*，将抛出*类型错误*。

对于最后一个技巧，不要同时使用 map()方法和 filter()方法，您可以只使用 reduce()方法来完成这项工作，因为这种方法遍历数组**一次**而不是两次。

# 分享给你的朋友！拍手声👏最多 50 次。

请不要犹豫与我分享你的想法和主意。你可以在 Twitter 上找到我，或者通过访问我的作品集在其他社交媒体上找到我。

[](https://www.hmousavi.dev/) [## 侯赛因穆萨维-软件开发人员

### 你好。👋🏻发现侯赛因穆萨维的空间，找出我的作品。请随时与我联系，还有更多！

www.hmousavi.dev](https://www.hmousavi.dev/) 

**从我这里读更多:**

[](https://itnext.io/a-complete-guide-to-the-api-first-approach-ecd796dd0f10) [## API-优先方法完全指南

### 在您的软件开发生涯中，开发应用程序可能采用的许多方法之一是…

itnext.io](https://itnext.io/a-complete-guide-to-the-api-first-approach-ecd796dd0f10) [](https://medium.com/codex/multiple-interceptors-in-angular-e0880b2f7d91) [## 多截击机在角

### Angular 提供的一个令人惊奇的特性是拦截器，但是拦截器能做什么，我们能不能…

medium.com](https://medium.com/codex/multiple-interceptors-in-angular-e0880b2f7d91) [](https://medium.com/angular-in-depth/husky-6-lint-prettier-eslint-and-commitlint-for-javascript-project-d7174d44735a) [## 用于 JavaScript 项目的 husky 6 Lint(appellite+eslint)和 commitlint

### 编程是一项团队工作，所以我们必须确保我们的代码库是干净的，对团队中的每个人都是可用的…

medium.com](https://medium.com/angular-in-depth/husky-6-lint-prettier-eslint-and-commitlint-for-javascript-project-d7174d44735a) [](https://medium.com/angular-in-depth/angular-forms-reactive-form-including-angular-material-and-custom-validator-9ef324cc3b08) [## 角形(反应式)包括角形材料和自定义验证器

### 表单是每个角度项目的主要部分，在这篇文章中，我们想实现一个反应式的角度表单，带有一个…

medium.com](https://medium.com/angular-in-depth/angular-forms-reactive-form-including-angular-material-and-custom-validator-9ef324cc3b08) [](/how-to-implement-dark-light-themes-in-a-next-js-app-using-context-hook-tailwindcss-336558dd4579) [## 如何使用上下文挂钩在 Next.js 应用程序中实现暗/亮主题

### 初始化一个 Next.js 应用程序，然后使用上下文钩子和 TailwindCSS 实现暗/亮主题切换

javascript.plainenglish.io](/how-to-implement-dark-light-themes-in-a-next-js-app-using-context-hook-tailwindcss-336558dd4579)