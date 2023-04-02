# 使用。map()，。过滤器()和。适当减少()👩🏽‍💻

> 原文：<https://javascript.plainenglish.io/using-map-filter-and-reduce-properly-50e07f80c8b2?source=collection_archive---------8----------------------->

![](img/081887765e931289e4b4cf977b8cf6b0.png)

Photo by [Safar Safarov](https://unsplash.com/@codestorm?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

Map、reduce 和 filter 都是 JavaScript 中的数组方法。每一个都将遍历一个数组，并执行转换或计算。每个函数都将根据函数的结果返回一个新数组。在本文中，您将了解为什么以及如何使用它们。

*请注意，这篇文章很可能适用于您可能使用的任何其他编程语言，因为这些概念存在于许多其他语言中*

# 。地图()

`.map()`函数只是帮助你用 iterable 中的每个值执行一组语句，并返回修改后的值。例如，如果您有一个 people 对象数组，并且您希望将它们的租金乘以 2。使用 foreach 的典型方法是:

```
let people = [
 { id: 101, name: ‘Michelle’, rent:1200 },
 { id: 124, destination: ‘Bob’, rent:2400 },
 { id: 151, destination: ‘Paul’, rent:500 },
 { id: 171, destination: ‘Peter’, rent:3899 }
]let newArray =[];people.forEach(person => {
 newArray.push(person.rent * 2);
});console.log(newArray)
***//*[2400, 4800, 1000, 7798]**
```

有多种方法可以实现这一点。您可能想通过创建一个空数组，然后使用`.forEach()`、`.for(...of)`或一个简单的`.for()`来实现您的目标。

因此，使用`.map()`功能，你可以简单地执行如下所示的相同任务。

```
const rentArray = people.map(person=>person.rent *2);console.log(rentArray)
**//[2400, 4800, 1000, 7798]**
```

**Person** 是用于访问数组中每个元素的变量。箭头`=>`后的语句是我们回调的主体。由于主体只有一条语句，我们可以省略花括号和关键字`return`。所以基本上上面的代码和下面的一样。

```
let rentArray = people.map(person => {
 return person.rent * 2;
});
```

那么`.map()`是如何工作的呢？基本上，它有两个参数，一个回调和一个可选的上下文(在回调中被认为是`this`),我在前面的例子中没有用到。回调针对数组中的每个值**运行，并且**返回结果数组中的每个新值**。**

# 。过滤器()

`.filter()`功能如其名。如果你有一个数组，但是你只想要其中的一些元素？这就是`.filter()`的用武之地。例如，如果您有一批带有装运 ID 和装运目的地的装运，并且您想要一批只发往美国的装运，典型的做法是；

```
let shipments=[
 { id: 101, destination: ‘USA’ },
 { id: 124, destination: ‘UK’ },
 { id: 151, destination: ‘UAE’ },
 { id: 171, destination: ‘USA’ },
 { id: 201, destination: ‘UK’ },
] let newShipments = [];shipments.forEach(shipment => {
 if (shipment.destination == "USA") {
  newShipments.push(shipment);
 }
});console.log(newShipments)
**//[{id:101,destination:"USA"},{ id:171, destination:"USA"}]**
```

因此，使用`.filter()`功能，您可以简单地执行如下所示的相同任务。

```
const newShipment = 
 shipments.filter(shipment=> shipment.destination =='USA');console.log(newShipment)
**//[{id:101,destination:"USA"},{ id:171, destination:"USA"}]**
```

基本上，如果回调函数**返回 true** ，当前元素**将在结果数组**中。如果它返回 false，那就不是。`filter()`构建一个新数组，从不改变/变异旧数组，它只是迭代旧数组。

# 减少()

`.reduce()`方法对数组的每个成员执行 **reducer** 函数(您提供的),产生一个输出值。就像`.map()`，`.reduce()`也为数组的每个元素运行回调。这里不同的是，reduce 将回调的结果(累加器)从一个数组元素传递给另一个数组元素。

累加器几乎可以是任何东西(整数、字符串、对象等)。)并且在调用`.reduce()`时必须实例化或传递。

例如，假设你有一个整数数组，你想得到它们的和。

```
const numbers = [ 2, 4, 6, 8, 10 ];numbers.forEach(number => sum += number);console.log(sum)
**//30**
```

我们可以使用`**.reduce();**`来执行这个操作

```
const sum = numbers.reduce((acc, number) => acc + number, 0);console.log(sum)
**//30**
```

请注意，我已经将初始值设置为 0。如果有必要，我也可以使用现有的变量。在对数组的每个元素运行回调之后，reduce 将返回累加器的最终值(在我们的例子中是:`30`)。

# 我们结合一下。map()，。filter()，。减少()

我们举个简单的例子。我们有如下一组学生。

```
const students = [
 {
  id: 5, 
  name: “Perone Skywalker”,
  average: 78.9,
  major: ‘Physics’,
 },
 { 
  id: 82,
  name: “Micheal Wren”,
  average: 73,
  major: ‘Biology’,
 },
 {
  id: 22, 
  name: “Zeb De Leone”,
  average: 59,
  major: ‘Physics’,
 },
 {
  id: 15,
  name: “Chidi Bridger”,
  average: 73,
  major: ‘Physics’,
 },
 {
  id: 11,
  name: “Caleb Canop”, 
  average: 33,
  major: ‘Chemistry’, 
 },
 {
  id: 47,
  name: “Neo Plims”, 
  average: 79,
  major: ‘Biology’,
 }
];
```

现在的目标是得到学物理的学生的总平均成绩。

首先我们需要过滤掉学物理的学生。

```
const physicsStudents = students.filter( student=>student.major ==’Physics’);console.log(physicsStudents)
**// [{...}, {...}, {...}] (Perone, Zeb and Chidi)**
```

所以我们有一个由 3 名学生组成的学习物理的团队。现在我们要得到这些学生的平均值。我们可以利用。map()来实现。

```
const average = physicsStudents.map(student => student.average);console.log(average);
**//[ 78.9, 59, 73]**
```

现在我们可以了。减少()并得到总数。

```
const totalAverage = average.reduce((acc, score) =>{
 return acc + score;
}, 0);console.log(totalAverage);
**//210.9**
```

现在我们可以做一次了。

```
const totalAverage = students
 .filter(student => student.major ==”Physics”)
 .map(student =>student.average)
 .reduce((acc, score) => acc + score, 0);console.log(totalAverage)
**//210.9**
```

完成了！👊🏼

试着用`.map()`、`.reduce()`、`.filter()`来代替你的`for`循环。如您所见，使用这些函数可以减少大量代码行。

希望我让这些函数非常容易理解..🤟🏽

继续编码！❤️👩🏽‍💻

![](img/8fa3b0fe054a13c3766d159ba831adc4.png)