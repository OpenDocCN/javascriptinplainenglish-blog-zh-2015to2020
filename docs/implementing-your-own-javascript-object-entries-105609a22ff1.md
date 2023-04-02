# 实现自己的 JavaScript Object.entries

> 原文：<https://javascript.plainenglish.io/implementing-your-own-javascript-object-entries-105609a22ff1?source=collection_archive---------7----------------------->

![](img/24dbd7ba1ab443025aa8593a78673691.png)

Photo by [Artem Sapegin](https://unsplash.com/@sapegin?utm_source=medium&utm_medium=referral)

JavaScript Object.entries()方法将对象转换为数组的数组。对象中的每个键/值对都被转换成一个数组，所有这些数组都保存在一个数组中。

例如，如果一个对象有两个键/值对，如下所示，

```
const obj = {fruit:’apple’, drink:’coffee’}Object.entries(obj) //[ [ ‘fruit’, ‘apple’ ], [ ‘drink’, ‘coffee’ ] ]
```

因为我们得到了一个数组作为输出，所以我们可以很容易地对它应用 map、filter 和 reduce 方法。

我们也可以将 Object.entries 应用于原语

```
// StringObject.entries(‘apple’) 
// [ [ ‘0’, ‘a’ ], [ ‘1’, ‘p’ ], [ ‘2’, ‘p’ ], [ ‘3’, ‘l’ ], [ ‘4’, ‘e’ ]]
```

将它应用于 Number 或 Boolean 将导致一个空数组。

我们可以使用任何方法迭代数组。这里我使用的是地图，同样我们也可以在地图上使用 for，forEach。

```
const obj = {fruit:’apple’, drink:’coffee’}let objArray = Object.entries(obj)objArray.map(entry=>{const [key, value] = entry; //array destructuringconsole.log(`${value} is a ${key}`)})//apple is a fruit
 //coffee is a drink
```

现在让我们看看如何创建我们自己的 Object.entries 方法

```
//takes an object as inputObject.myOwnEntries = obj => {
   let objKeys=Object.keys(obj); //an array of Object keys
   let i = objKeys.length; // i to iterate over the object
   let resArray = []; //empty array to hold the results
   while(i>0) //using while loop {
      i — ; //iterating backwards
      resArray[i]=[objKeys[i],obj[objKeys[i]]] //creating the resultant array }

   return resArray;}const obj = {fruit:’apple’, drink:’coffee’}Object.entries(obj) //[ [ ‘fruit’, ‘apple’ ], [ ‘drink’, ‘coffee’ ] ]Object.myOwnEntries(obj) //[ [ ‘fruit’, ‘apple’ ], [ ‘drink’, ‘coffee’ ] ]
```

反过来也是可能的。我们可以使用 Object.fromEntries 方法从数组的数组中创建一个对象。

```
let myEntries= [ [ 'fruit', 'apple' ], [ 'drink', 'coffee' ] ]Object.fromEntries(myEntries)//{fruit: "apple", drink: "coffee"}
```

我们可以实现自己的 formEntries 方法，如下所示，很简单。

```
const obj={}myEntries.forEach(a=>obj[a[0]]=a[1])console.log(obj) //{fruit: “apple”, drink: “coffee”}
```