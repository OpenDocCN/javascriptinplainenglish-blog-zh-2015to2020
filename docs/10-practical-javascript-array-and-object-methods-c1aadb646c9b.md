# 10 多种实用的 JavaScript 数组和对象方法

> 原文：<https://javascript.plainenglish.io/10-practical-javascript-array-and-object-methods-c1aadb646c9b?source=collection_archive---------3----------------------->

## 每个开发人员都应该知道如何使用这些方法

![](img/771bf95d360861edc66776027fbc433b.png)

Photo by [Alex Bello](https://unsplash.com/@anderjb?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

下面列出了一些有用的方法，可以帮助你更容易地使用 JavaScript 数组和对象。

## **过滤器()**

`filter()`方法创建一个数组，其中填充了满足特定条件的所有数组元素。就像罐头上说的那样！

*示例:*

```
let array = [
  { name: "James Lee", age: 2},
  { name: "Nat Pete", age: 5 },
  { name: "Dan Blob", age: 43 },
  { name: "Sam Smith", age: 17 },
  { name: "Stan Lee", age: 22 }
];let newArray = array.filter(a => a.**age** > 18); 
// [{ name: "Dan Blob", age: 43 }, { name: "Stan Lee", age: 22 }]
```

## 地图()

`map()`方法创建一个新的数组，其中填充了调用数组中每个元素的函数的结果。

```
let array = [
  { name: "James Lee", age: 2},
  { name: "Nat Pete", age: 5 },
  { name: "Dan Blob", age: 43 },
  { name: "Sam Smith", age: 17 },
  { name: "Stan Lee", age: 22 }
];// Get an array with just ages
let ageArray = array.map(a => a.**age**); 
// [2, 5, 43, 17, 22];// Add one to everyones age and keep as an object array
let newArray = array.map(obj=> ({ ...obj, age: obj.age + 1}));
```

## **有的()**

`some()`方法测试数组中是否至少有一个元素通过了由提供的函数实现的测试。这将返回一个布尔值。

我看这个方法用得还不够。很多开发人员最终会创建一个循环或者使用`findIndex()`然后检查它不是-1。

*示例:*

```
let array = [
  { name: "James Lee", age: 2},
  { name: "Nat Pete", age: 5 },
  { name: "Dan Blob", age: 43 },
  { name: "Sam Smith", age: 17 },
  { name: "Stan Lee", age: 22 }
];let over18 = array.some(a => a.age > 18); // true
let over50 = array.some(a => a.age > 50); // false
```

## 每隔()

与`some()`相似，但是每个元素都需要通过所提供函数的测试。

*例子:*

```
let array = [
  { name: "James Lee", age: 2},
  { name: "Nat Pete", age: 5 },
  { name: "Dan Blob", age: 43 },
  { name: "Sam Smith", age: 17 },
  { name: "Stan Lee", age: 22 }
];let over18 = array.every(a => a.age > 18); // false
let over50 = array.every(a => a.age > 50); // false
let over1 = array.every(a => a.age > 1); // true
```

## findIndex()

`findIndex()`方法用于返回给定数组中满足测试函数的第一个元素的索引。否则，返回-1。

如果你想改变数组中的某些东西，使用`findIndex()`而不是`some()`。

*例如:*

```
let array = [
  { name: "James Lee", age: 2},
  { name: "Nat Pete", age: 5 },
  { name: "Dan Blob", age: 43 },
  { name: "Sam Smith", age: 17 },
  { name: "Stan Lee", age: 22 }
];
let firstOver18 = array.findIndex(a => a.age > 18); // 2array[firstOver18].age += 1; // New Age 44
```

## 排序()

方法对数组中的元素进行排序，并返回排序后的数组。伟大的排序数字或字母顺序

*例如:*

```
let array = [
  { name: "James Lee", age: 2},
  { name: "Nat Pete", age: 5 },
  { name: "Dan Blob", age: 43 },
  { name: "Sam Smith", age: 17 },
  { name: "Stan Lee", age: 22 }
];// Sorts by age
array.sort((a, b) => a.age - b.age);// Sorts by name
array.sort((a, b) => a.name.localeCompare(b.name));
```

## 推送()

可能是用的最多的数组方法。方法将零个或多个元素添加到数组的末尾，并返回数组的新长度。

```
let array = [
  { name: "James Lee", age: 2},
  { name: "Nat Pete", age: 5 },
  { name: "Dan Blob", age: 43 },
  { name: "Sam Smith", age: 17 },
  { name: "Stan Lee", age: 22 }
];array.push({name: "New Guy", age: 35}); // Returns 6
// This will now be at the bottom of the array
```

## 切片()

`slice()`方法复制数组的给定部分，并将复制的部分作为新数组返回。它**不改变**原来的数组。

它接受两个参数:

*   **From:** 从元素索引开始对数组切片
*   **直到:**分割数组，直到另一个元素索引

```
let array = [
  { name: "James Lee", age: 2},
  { name: "Nat Pete", age: 5 },
  { name: "Dan Blob", age: 43 },
  { name: "Sam Smith", age: 17 },
  { name: "Stan Lee", age: 22 }
];const newArray = array.slice(0, 2);// **newArray** = 
// [{"name":"James Lee","age":2},{"name":"Nat Pete","age":5}]// **array** will not change
```

## 拼接()

类似于`slice()`的方法复制一个数组的给定部分，并将复制的部分作为一个新数组返回(移除)，但是**会改变原来的数组。**

参数:

*   **指标:**必选参数。这个参数是开始修改数组的索引。
*   **delete_count:** 要从起始索引中删除的元素数。
*   **items_list:** 由逗号运算符分隔的新项目列表，将从起始索引插入。

```
let array = [
  { name: "James Lee", age: 2},
  { name: "Nat Pete", age: 5 },
  { name: "Dan Blob", age: 43 },
  { name: "Sam Smith", age: 17 },
  { name: "Stan Lee", age: 22 }
];const newArray = array.slice(0, 2);// **newArray** = 
// [{"name":"James Lee","age":2},{"name":"Nat Pete","age":5}]// **array** = 
// [{"name":"James Lee","age":2},{"name":"Nat Pete","age":5}]
```

这将取决于您在减少阵列时应该使用的创建内容。

开发人员使用的另一种方法叫做`delete`要小心，因为这不会重新索引数组或更新其长度。这使得它看起来好像是未定义的。

## Object.keys()

`Object.keys()`方法返回一个给定对象自己的可枚举属性名的数组，其迭代顺序与普通循环相同。

当你想遍历一个对象的键和值时，`Object.keys()`非常有用。当您并不总是知道对象的属性，但仍然希望显示带有键的值时，这很有用。

```
let person = { firstName: "James", lastName: "Lee", age: 28};let keys = Object.keys(person); // ["firstName","lastName","age"];// Iterate through the keys
Object.keys(person).forEach(key => {
    let value = person[key];
    console.log(`${key}: ${value}`);
});
// returns
// firstName: James
// lastName: Lee
// age: 28
```

## Object.values()

`Object.values()`方法返回给定对象自己的可枚举属性值的数组。

```
let person = { firstName: "James", lastName: "Lee", age: 28};let values = Object.values(person); 
// returns ["James","Lee",28]
```

## Object.entries()

`Object.entries()`创建一个对象的键/值对的嵌套数组。

与`Object.keys()`类似，我们可以使用`forEach()`方法循环处理结果。

```
let person = { firstName: "James", lastName: "Lee", age: 28};let entries = Object.entries(person);
// returns: [["firstName","James"],["lastName","Lee"],["age",28]]// Iterate through the keys
entries.forEach(entry => {
    let key = entry[0];
    let value = entry[1];console.log(`${key}: ${value}`);
});// returns
// firstName: James
// lastName: Lee
// age: 28
```

希望你喜欢阅读！