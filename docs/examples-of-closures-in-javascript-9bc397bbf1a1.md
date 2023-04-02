# JavaScript 中的闭包示例

> 原文：<https://javascript.plainenglish.io/examples-of-closures-in-javascript-9bc397bbf1a1?source=collection_archive---------3----------------------->

![](img/f9fbdebb7a4ac13f9070646d0c03ab04.png)

闭包这个词听起来很复杂。您几乎肯定已经理解了闭包——让我们看看一些例子。

# 实施例 1

```
function outer() {
  const a = 23;
  function inner() {
    console.log(a);
  }
  inner();
}outer();
//=> 23// The `inner` function is said to have closure over the variable `a`
```

在上面的例子中，`inner`函数可以访问变量`a`，因此`inner`被认为对变量 a 有闭包。

# 示例 2

我们可以利用闭包来有效地隐藏我们可能不想用我们的程序公开的东西。

这里有一个(不切实际的)例子来证明这一点:

```
function outer(number) {
  const secretNumber = 23;
  function inner(num) {
    console.log(num * secretNumber);
  }
  return inner(number);
}outer(10);
//=> 230
```

这里，`outer`函数返回另一个可以访问变量`secretNumber`的函数。

虽然上面的例子可能有点简单，而且无效(任何人都可以通过调用`outer(1)`来猜测`secretNumber`是什么)，但是它展示了如何使用闭包来使事情变得私有。

# 示例 3:可重用的函数

我们可以利用闭包的力量，使函数在不同的上下文中可以重用。

```
function adder(firstNumber) {
  function add(secondNumber) {
    console.log(firstNumber + secondNumber);
  }
  return add; 
}const addFive = adder(5);
addFive(10);
//= > 15const addTen = adder(10);
addTen(100);
// => 110
```

请注意，上面的加法器函数返回`add`函数，但不调用它(就像这样:`add()`)。这意味着当我们用`firstNumber`参数调用`adder`时(例如:`const addFive = adder(5)`)，它会返回一个现在知道`firstNumber`值的函数，我们可以调用它(例如:`addFive(10)`)来获得最终结果。

# 示例 5:可重用闭包的一个更实际的示例

我发现上面的`adder`函数非常有助于理解闭包的这种用法，但是直到我看到一些真正的生产代码，我才真正“了解”它是如何被实际使用的。

这里有一个例子，希望能让事情变得更清楚。

***注*** *:我正在用包* `*axios*` *取数据——你不需要知道* `*axios*` *是如何工作的，就能理解下面的例子*

```
// Reusable data fetcher function function dataFetcher(url) {
  return async function getData(path) {
    try {
      const endpoint = path ? `${url}/${path}` : url;
      const response = await axios.get(endpoint);
      return response.data;
    } catch (error) {
      console.log(error);
    }
  };
} // Reusable UK police data fetcher function async function ukPoliceForceDataFetcher(path) {
  try {
    const policeForces = dataFetcher('[https://data.police.uk/api/forces'](https://data.police.uk/api/forces'));
    const result = await policeForces(path);
    console.log('result', result);
  } catch (error) {
    console.log(error);
  }
};// EXAMPLES OF FUNCTION RE-USE// Example 1: Invoking it without a path parameter
ukPoliceForceDataFetcher(); // Example 2: Invoking it with 'leicestershire'
ukPoliceForceDataFetcher('leicestershire');// Example 3: Invoking it with 'essex' 
ukPoliceForceDataFetcher('essex');
```

1.  我创建了一个接受 URL 的通用`dataFetcher`函数，并返回一个接受附加路径参数的函数。我用它来动态改变路径，以获得不同情况下的结果。
2.  我创建了一个名为`ukPoliceForceDataFetcher`的更具体的数据获取器，它使用步骤 1 中的`dataFetcher`函数，并传入英国警察部队 API 的 url(我选择这个是因为它不需要任何认证)
3.  `ukPoliceForceDataFetcher`函数返回另一个函数，该函数可以将一个区域作为路径参数，并返回该区域的相关细节。如果没有提供区域，则默认返回所有区域的名称和 id。

上面的例子显示了当我们想要获得不同地区的数据时，我们如何能够重用`ukPoliceForceDataFetcher`函数，但是使用通用的`dataFetcher`函数也使我们能够重用它。让我们看一个工作示例。

# 示例 6:重用通用数据提取器函数

```
// Here's the genetic dataFetcher function that we used to create our ukPoliceDataFetcher abovefunction dataFetcher(url) {
  return async function getData(path) {
    try {
      const endpoint = path ? `${url}/${path}`: url;
      const response = await axios.get(endpoint);
      return response.data;
    } catch (error) {
      console.log(error);
    }
  };
}// Now, here's another re-use of it. Same function, but now we're using the [https://jsonplaceholder.typicode.com/todos](https://jsonplaceholder.typicode.com/todos') API.async function getToDos(path) {
  try {
    const placeHolderUrl = '[https://jsonplaceholder.typicode.com/todos'](https://jsonplaceholder.typicode.com/todos');
    const todos = dataFetcher(placeHolderUrl);
    const result = await todos(path);
    console.log(result);
  } catch (error) {
    console.log(error);
  }
}getToDos(); // Gets the entire to do list
getToDos('1'); // Gets the to do with id: 1
getToDos('23'); // Gets the to do with id: 23
```

我们可以通过传递其他参数，比如`queryString`、`request body`和`path`，来构建更多的内容，但是最重要的是总体概念。

如果你有什么建议或者问题，请在下面留言！