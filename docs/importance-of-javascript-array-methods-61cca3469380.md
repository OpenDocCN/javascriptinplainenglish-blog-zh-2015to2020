# JavaScript 数组方法的重要性

> 原文：<https://javascript.plainenglish.io/importance-of-javascript-array-methods-61cca3469380?source=collection_archive---------2----------------------->

## 学习 JavaScript 中数组方法的重要功能和概念

![](img/d583187c8dbcf68506ae8728c09dc8fa.png)

Photo by Arif Riyanto on Unsplash

今天我们将了解每个开发人员都必须知道的数组的重要功能。首先让我们了解一点数组。数组只不过是元素的集合，它包含许多内置的属性和方法来解决任何任务。现在让我们深入正题。

*   一些()
*   每隔()
*   扁平()
*   过滤器()
*   forEach()
*   findIndex()
*   查找()
*   排序()
*   concat()
*   平面地图()

# 一些()

**some()** 方法用于测试数组中至少有一个元素通过了由**回调**函数实现的测试。当使用参数执行**回调**函数时，可以为**这个**赋值。

定义:

```
arr.some(callback(element[, index[, array]])[, thisArg])
```

示例:

```
const a = [1, 2, 3, 5, 8].some(item => item > 5)
const b = [1, 2, 3, 4, 5].some(item => item > 5)

console.log(a)
console.log(b)

---------
Output
---------
> true
> false
```

# 每隔()

**every()** 方法类似于 **some()** 方法。它测试数组中的所有元素是否通过了由**回调**函数实现的测试。

定义:

```
arr.every(callback(element[, index[, array]])[, thisArg])
```

示例:

```
const a = [10, 9, 8, 7, 6].every(item => item > 5)
const b = [7, 6, 5].every(item => item > 5)

console.log(a)
console.log(b)

---------
Output
---------
> true
> false
```

# 扁平()

**flat()** 方法用于创建一个新数组，所有的子数组都连接到其中。默认情况下，平坦级别始终设置为级别 1。

定义:

```
arr.flat([depth])
```

示例:

```
const arr1 = [1, 2, [3, 4]]
console.log(arr1.flat())

const arr2 = [1, 2, [3, 4, [5, 6]]]
console.log(arr2.flat())

const arr3 = [1, 2, [3, 4, [5, 6]]]
console.log(arr3.flat(2))

const arr4 = [1, 2, [3, 4, [5, 6, [7, 8, [9, 10]]]]]
console.log(arr4.flat(Infinity))

---------
Output
---------
> (4) [1, 2, 3, 4]
> (5) [1, 2, 3, 4, Array(2)]
> (6) [1, 2, 3, 4, 5, 6]
> (10) [1, 2, 3, 4, 5, 6, 7, 8, 9, 10]
```

请注意，如果您想要重复执行，请将 **infinity** 值作为参数传递给它。

# 过滤器()

**filter()** 方法也用于创建一个新数组，所有元素都通过测试，由**回调**函数实现。

定义:

```
arr.filter(callback(element[, index, [array]])[, thisArg])
```

示例:

```
const array = [-3, -2, -1, 0, 1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 11, 12, 13]

function isPrime(num) {
  for (let i = 2; num > i; i++) {
    if (num % i == 0) {
      return false
    }
  }
  return num > 1
}

console.log(array.filter(isPrime))

---------
Output
---------
> (6) [2, 3, 5, 7, 11, 13]
```

# ForEach()

**forEach()** 方法用于对每个数组元素只执行一次提供的方法。

定义:

```
arr.forEach(callback(currentValue [, index [, array]])[, thisArg])
```

示例:

```
const array = [1, 2, 3, 4, 5]
array.forEach((item) => console.log(item))

---------
Output
---------
1
2
3
4
5
```

# Findindex()

**findindex()** 方法用于返回回调函数提供的数组中第一个元素的索引。当没有元素传入测试时，它总是返回-1。即使索引没有赋值， **findindex ()** 方法也会执行**回调**函数。

定义:

```
arr.findIndex(callback( element[, index[, array]] )[, thisArg])
```

示例:

```
function isPrime(num) {
  for (let i = 2; num > i; i++) {
    if (num % i == 0) {
      return false
    }
  }
  return num > 1
}

console.log([4, 6, 8, 9, 12].findIndex(isPrime))
console.log([4, 6, 7, 9, 12].findIndex(isPrime))

---------
Output
---------
-1
2
```

# 查找()

**find()** 方法也类似于 **findindex()** 方法。不同之处在于它返回由**回调**函数提供的第一个元素的值。如果没有元素满足**回调**函数，则返回 undefined。

定义:

```
arr.find(callback(element[, index[, array]])[, thisArg])
```

示例:

```
const inventory = [
  {name: 'apples', quantity: 2},
  {name: 'bananas', quantity: 0},
  {name: 'cherries', quantity: 5}
]

const result = inventory.find( ({ name }) => name === 'cherries' )

console.log(result)

---------
Output
---------
> {name: "cherries", quantity: 5}
```

# 排序()

**sort()** 方法用于将数组元素排序到正确的位置，并返回排序后的数组。默认排序始终是升序。这种方法的复杂性和性能不是恒定的。这取决于实现。

定义:

```
arr.sort([compareFunction])
```

示例:

```
const numbers = [4, 2, 5, 1, 3]
const numbers2 = numbers.sort((a, b) => a - b)

console.log('numbers', numbers)
console.log('numbers2', numbers2)
console.log(numbers === numbers2)

---------
Output
---------
numbers > (5) [1, 2, 3, 4, 5]
numbers2 > (5) [1, 2, 3, 4, 5]
true
```

# Concat()

concat()方法用于将两个或多个数组连接到一个新数组中。

定义:

```
const new_array = old_array.concat([value1[, value2[, ...[, valueN]]]])
```

示例:

```
const letters = ['a', 'b', 'c']
const numbers = [1, 2, 3]

console.log(letters.concat(numbers))

---------
Output
---------
> (6) ["a", "b", "c", 1, 2, 3]
```

# 平面图()

**flatmap()** 方法用于对数组中的每个元素应用一个函数，并将平坦的结果返回到数组中。它将 **flat()** 和 **map()** 结合在一个功能中。

定义:

```
arr.flatMap(callback(currentValue[, index[, array]])[, thisArg])
```

示例:

```
const array = [[1], [2], [3], [4], [5]]

const a = array.flatMap(arr => arr * 10)

// With .flat() and .map()
const b = array.flat().map(arr => arr * 10)

console.log('flatMap', a)
console.log('flat&map', b)

---------
Output
---------
flatMap (5) [10, 20, 30, 40, 50]
flat&map (5) [10, 20, 30, 40, 50]
```

# 结论

我希望你喜欢这个故事，并且知道一些关于 JavaScript 数组方法的新知识。一般来说，JavaScript 数组包含一些很好的方法来简化我们的工作。通过了解这些概念，它将提高我们项目的性能。

谢谢你的阅读！