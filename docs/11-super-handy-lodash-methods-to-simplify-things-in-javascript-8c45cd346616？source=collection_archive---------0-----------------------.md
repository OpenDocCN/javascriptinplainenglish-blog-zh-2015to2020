# 简化 JavaScript 的 11 个超级方便的 Lodash 方法

> 原文：<https://javascript.plainenglish.io/11-super-handy-lodash-methods-to-simplify-things-in-javascript-8c45cd346616?source=collection_archive---------0----------------------->

## 有时他们会拯救你的一天

![](img/0da2e3e992c4dba5d1a7d55dade216f4.png)

Photo by [Katie Rodriguez](https://unsplash.com/@katertottz?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

Lodash 是每个 JavaScript 程序员都应该知道的最好的实用程序库之一。

在本文中，我将通过示例向您展示 11 个有用的 Lodash 函数。

让我们开始吧。

# 1.过滤列表

当我为一个电子商务网站开发搜索功能时，我经常使用这种方法。搜索结果应根据选定的标准显示。

洛达什使它变得容易。假设您想要显示一个小说书籍列表，那么:

```
_.filter(books: { genre: ‘fiction’ });
```

当您必须处理复杂的条件时，您可以将第二个参数作为函数传递。

下面的例子向你展示了如何过滤价格超过 7.5 美元的小说

```
_.filter(books: function(book) {
  return book.genre === ‘fiction’ && book.price > 7.5;
});
```

# 2.生成随机数

似乎生成随机的东西是我们必须处理的常见任务之一。

如果您想用纯 JavasScript 生成一个从 1 到 10 的随机数，您可以这样做:

```
Math.floor(Math.random() * 10) + 1;
```

还能更简单吗？是的。

在洛达什，只是:

```
_.random(1, 10);
```

对于浮点数，例如从 2.5 到 5.5，您可以:

```
_.random(2.5, 5.5);
```

# 3.随时间循环

我发现将**乘以**函数与**随机**函数结合起来生成一个随机数数组非常有用。

当然，你可以在的时候用**做**或者**。然而，代码应该比**乘以**要短。**

```
function getRandomNumber() {
  return _.random(1, 10);
}let randomNumbers = _.times(10, getRandomNumber);console.log(randomNumbers); // example: [9, 5, 2, 6, 2, 1, 5, 3, 1, 8]
```

# 4.克隆一个对象

我曾经艰难地克隆了一个 JavaScript 对象。这并不简单。你也可能面临这个问题。

说到 Lodash，这个任务简直是小菜一碟。

例如，您想要创建另一个 book 对象，该对象具有给定 book 的相同值:

```
let book = {
  name: ‘JavaScript: The Good Parts’,
  price: 13.5
};let clonedBook = _.clone(book);
```

很简单对吧。

但是在克隆对象列表时要小心:

```
let books = [{
  name: ‘JavaScript: The Good Parts’,
  price: 13.5
}, {
  name: ‘Learn JavaScript Visually’,
  price: 15
}];let clonedBooks = _.clone(books);
```

这里有什么问题？

书籍和克隆书籍中的相应对象现在仍然持有相同的引用。我是说如果你换**本书【0】。名字**对**学习反应原生**那么克隆的 Books[0]也应该是**学习反应原生**。

有点微妙的问题，是吧？

我们如何解决它？

这时候深度克隆功能就来了。

所以与其说:

```
let clonedBooks = _.clone(books);
```

现在，我们将使用:

```
let clonedBooks = _.cloneDeep(books);
```

问题就解决了。

# 5.比较两个对象

如果要比较两个物体，一般会怎么做？比较每个属性或使用 JSON.stringify

你试过 Lodash 的 isEqual 吗？除非您必须处理深度嵌套的对象，否则您可能看不到它的重要价值。

只需一个简单的调用，isEqual 就会将比较进行到更深的层次。

```
let book1 = {
  name: ‘JavaScript: The Good Parts’,
  price: 13.5
};let book2 = {
  name: ‘JavaScript: The Good Parts’,
  price: 13.5
};console.log(_.isEqual(book1, book2)); // true
```

# 6.选择属性

当您希望基于现有对象的属性形成新对象时，此功能非常有用。

例如，您想要创建一个具有两个属性**名称**和**价格**的新产品。你所要做的就是挑选房产，剩下的交给 Lodash。

```
let product = {
  name: ‘Learning React Native’,
  category: ‘Book’,
  price: 15,
  discount: 0.3
};let newProduct = _.pick(product, [‘name’, ‘price’]);console.log(newProduct); // { name: ‘Learning React Native, price: 15 }
```

您也可以通过使用**省略**功能以相反的方式来解释该任务:

```
let product = {
  name: ‘Learning React Native’,
  category: ‘Book’,
  price: 15,
  discount: 0.3
};let newProduct = _.omit(product, [category, discount]);console.log(newProduct); // { name: ‘Learning React Native, price: 15 }
```

# 7.获取/设置属性值

如果使用一个对象的未定义属性会发生什么？它将抛出如下错误:

```
let book = {
  name: ‘Learning JavaScript’
};let discountPrice = book.price.discount; // Uncaught TypeError: Cannot read property ‘discount’ of undefined
```

这一次我们需要使用 **get** 函数。如果属性未定义，将返回默认值:

```
let book = {
  name: ‘Learning JavaScript’
};let discountPrice = _.get(book, ‘book.price.discount’, 0); // 0
```

# 8.检查变量是否为空

当您想要检查对象、贴图、集合或集合是否为空时，可以使用此函数。

下面我们来看看:

```
let book1 = {};
console.log(_.isEmpty(book1)); // true;let book2 = { name: ‘Learning JavaScript };
console.log(_.isEmpty(book2)); // false;let books1 = [{
  name: ‘Learning JavaScript’
}, {
  name: ‘Learning React Native’
}];
console.log(_.isEmpty(books1)); // false;let books2 = [];
console.log(_.isEmpty(books2)); // true;let nullBook = null;
console.log(_.isEmpty(nullBook)); // true;let undefinedBook = undefined;
console.log(_.isEmpty(undefinedBook)); // true;
```

# 9.对收藏进行排序

例如，如果您想按价格升序搜索图书列表，请执行以下操作:

```
let books = [{
  name: ‘JavaScript’,
  price: 10
}, {
  name: ‘C++’,
  price: 9
}, {
  name: ‘Python’,
  price: 14
}, {
  name: ‘Golang’,
  price: 12
}];let sortedBook = _.sortBy(books, [‘price’]);console.log(sortedBook); // [{ name: ‘C++’, price: 9 }, { name: ‘JavaScript’, price: 10 }, { name: ‘Golang’, price: 12 }, { name: ‘Python’, price: 14 }]
```

如果要控制排序顺序，可以使用 **orderBy** 功能。

```
let sortedBook = _.orderBy(books, [‘price’], [‘desc’]);let sortedBook = _.orderBy(books, [‘price’], [‘asc’]);
```

或者以组合的方式:

```
let sortedBook = _.orderBy(books, [‘price’, ‘discount’], [‘asc’, ‘desc’]);
```

# 10.延迟调用函数

假设你正在开发一个类似 Google search suggestion 的功能——当用户在搜索框中输入内容时，它会下拉一个建议条目列表。

你应该使用**去抖**功能在一段时间后执行任务，而不是为每个输入的字符调用 API:

```
function showSuggestedTerms() {
  // Call API and then show the result
}let searchBox = document.getElementById(‘search-box’);searchBox.addEventListener(‘keyup’, _.debounce(showSuggestedTerms, 300);
```

上面的 **showSuggestedTerms** 函数会在用户停止输入 300ms 后被调用。

# 11.合并数组

你可以使用 **concat** 方法来合并数组:

```
let array1 = [1, 5, 3, 1];
let array2 = [7, 25, 21];
let array3 = [11, 3, 3, 2];let mergedArray = _.concat(array1, array2, array3);
console.log(mergedArray); // [1, 5, 3, 1, 7, 25, 21, 11, 3, 3, 2];
```

如果您希望合并数组的元素值是唯一的，您可以使用 **union** 函数:

```
let mergedArray = _.union(array1, array2, array3);
console.log(mergedArray); // [1, 5, 3, 7, 25, 21, 11, 2]
```

以上功能不是全部，只是我大部分时间使用的功能。

我喜欢 Lodash。它节省了我很多时间，也让我的代码更漂亮。

你觉得这个强大的库怎么样？

[](https://medium.com/javascript-in-plain-english/how-to-write-effective-javascript-code-that-every-programmer-loves-to-maintain-a490b42b9b9f) [## 如何编写每个程序员都喜欢维护的有效 JavaScript 代码](https://medium.com/javascript-in-plain-english/how-to-write-effective-javascript-code-that-every-programmer-loves-to-maintain-a490b42b9b9f)