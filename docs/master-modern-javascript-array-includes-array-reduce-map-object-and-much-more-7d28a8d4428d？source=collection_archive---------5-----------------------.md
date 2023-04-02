# 掌握现代 JavaScript——Array includes、Array reduce、Map object 等等

> 原文：<https://javascript.plainenglish.io/master-modern-javascript-array-includes-array-reduce-map-object-and-much-more-7d28a8d4428d?source=collection_archive---------5----------------------->

![](img/06e670e46f81a3a847d6ec088b13ce78.png)

Photo by [Julien Pouplard](https://unsplash.com/@pouplardjulien?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

在过去的几年里，JavaScript 语言有了很多更新。如果你想提高你的编码技能，这些更新是非常有用的。

因此，让我们来看看 JavaScript 中添加的一些东西，你需要熟悉这些东西来提高你的技能并获得高薪工作。

> ***注:*** *这是来自* [*精通现代 JavaScript*](https://modernjavascript.yogeshchavan.dev/) *一书的内容最后的简短预览。在实际的书中有更多的内容。*

如果你错过了，请查看我以前的帖子以获得更多预览内容。

所以让我们开始吧。

# 数组.原型.包含

ES7 增加了这个函数，它检查数组中是否存在元素，并返回布尔值`true`或`false`。

```
// ES5 Code
const numbers = ["one", "two", "three", "four"];console.log(numbers.indexOf("one") > -1); // true
console.log(numbers.indexOf("five") > -1); // false
```

使用数组`includes`方法的相同代码可以写成如下所示:

```
// ES7 Code
const numbers = ["one", "two", "three", "four"];console.log(numbers.includes("one")); // true
console.log(numbers.includes("five")); // false
```

所以使用数组`includes`方法使得代码简短易懂。

在比较不同的值时，`includes`方法也很方便。

看看下面的代码:

```
const day = "monday";if(day === "monday" || day === "tuesday" || day === "wednesday") {
  // do something
}
```

上面使用`includes`方法的代码可以简化如下:

```
const day = "monday";if(["monday", "tuesday", "wednesday"].includes(day)) {
  // do something
}
```

所以在检查数组中的值时，`includes`方法非常方便。

# Array.reduce

`Array.reduce`的语法如下:

```
Array.reduce(callback(accumulator, currentValue[, index[, array]])[, initialValue])
```

`reduce`方法对数组的每个元素执行 **reducer** 函数(您提供的),产生一个输出值。

**`**reduce**`**方法的输出总是单值。它可以是对象、数字、字符串或数组等。这取决于你希望** `**reduce**` **方法产生什么样的输出，但它总是一个单一的值。****

**假设，你想得到数组中所有数字的和，那么你可以使用 reduce 方法。**

```
const numbers = [1, 2, 3, 4, 5];const sum = numbers.reduce(function(accumulator, number) { 
 return accumulator + number;
}, 0); console.log(sum); // 15
```

**`reduce`方法接受一个回调函数，该函数接收
`accumulator`、`number`、`index`和`array`作为值。**

**在上面的代码中，我们只使用了`accumulator`和`number`。**

**`accumulator`只是一个用来标识初始值的名字。**

**`accumulator`将包含用于数组的`initialValue`。`initialValue`决定`reduce`方法返回的数据的返回类型。**

**`number`是回调函数的第二个参数，它将在循环的每次迭代中包含数组元素。**

**在上面的代码中，我们为`accumulator`提供了`0`作为`initialValue`。**

**所以回调函数第一次执行时，`accumulator + number`将是`0 + 1 = 1`，我们将返回值`1`。**

**所以下次回调函数运行时，`accumulator + number`将会是`1 + 2 = 3` ( `1`这里是上一次迭代返回的前一个值
，`2`是数组中的下一个元素)。**

**当回调函数下一次运行时，`accumulator + number`将会是`3 + 3 = 6`(这里的前 3 是上一次迭代返回的值，下一个`3`是数组中的下一个元素)，它将会继续，直到`numbers`数组中的所有元素都没有被迭代。**

**因此`accumulator`将像静态变量一样保留上一次操作的值。**

**在上面的代码中，不需要`0`的`initialValue`，因为数组的所有元素都是整数。**

***其他有用的* `*reduce*` *方法的例子你会在书中找到。***

# **地图**

**`Map`是 ES6 中添加的一种新类型的对象，用于保存键值对。**

**它是这样创建的:**

```
const map = new Map();
```

**我们使用`set`方法给地图添加值。**

```
const user1 = { name: 'David', age: 30 };
const user2 = { name: 'Mike', age: 40 };const map = new Map();map.set('user1', user1);
map.set('user2', user2);
```

**我们还可以使用类似数组的语法向映射添加值。所以上面的代码可以这样写:**

```
const user1 = { name: 'David', age: 30 };
const user2 = { name: 'Mike', age: 40 };const map = new Map([['user1', user1], ['user2', user2]]);
```

**如果我们试图打印地图，我们将得到一个 map 类型的对象。**

```
console.log(map); // [object Map] { ... }
```

**但是我们可以使用`for...of`循环来遍历地图。**

```
for(element of map) {
 console.log(element);
}/* output['user1', { name: 'David', age: 30 }]
['user2', { name: 'Mike', age: 40 }]*/
```

**为了从地图中获取特定的元素，我们可以使用由`Map`提供的`get`方法。**

```
console.log(map.get('user1')); // { name: 'David', age: 30 }
console.log(map.get('user2')); // { name: 'Mike', age: 40 }
```

***你可以在书中找到* `*Map*` *提供的其他有用的方法。***

> ***与* `*Map*` *相关的一个非常受欢迎但不那么容易的编程挑战，在第一轮编码面试中经常被问到，以筛选出你会在带有代码解释的书中找到的候选人。***

# **结束点**

**《掌握现代 JavaScript》这本书涵盖了成为现代 JavaScript 技能专家所需要知道的一切。**

****通过一次性购买，您将在每次新发布的 ESNext 中免费获得该书的更新版本。****

**这本书包含了所有的概念，这些概念是学习 React 的先决条件，也是成为现代 JavaScript 专家和更好地使用 React 所必需的。**

**这是你面对任何 JavaScript/React 面试所需要的唯一指南，在这些面试中，ES6+特性是成功面试的必备要素。**

****只剩下最后几个小时了，这本书限时 43 折。所以现在就抓住它。****

**这本书再也不会有这么大的折扣了。**

**通过点击下面的链接获得这本书。**

**[**精通现代 JavaScript**](https://modernjavascript.yogeshchavan.dev/)**

**快乐学习！**

****别忘了直接在你的收件箱** [**这里**](https://yogeshchavan.dev/) **订阅我的每周时事通讯，里面有惊人的技巧、诀窍和文章。****